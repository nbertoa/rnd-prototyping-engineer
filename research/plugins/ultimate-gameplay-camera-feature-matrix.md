
# Ultimate Gameplay Camera — Manual oficial y matriz de features (Sandbox UE 5.8)

**Fecha de revisión:** 2026-10-26. **Fuente:** PDF `Ultimate Gameplay Camera Documentation.pdf` (65 páginas), aportado a las fuentes del proyecto ChatGPT, a partir de las páginas de Aurora Devs (Superhuman/Coda). Se revisó el texto de las 65 páginas y se inspeccionaron visualmente sus páginas y gráficos Blueprint principales. El PDF de terceros **no se republica** aquí; se registran únicamente notas técnicas originales y enlaces. El PDF no certifica la versión global del plugin; la instalación real investigada en Sandbox es UGC 1.92/UE 5.8.

## Cobertura efectiva del PDF

| Páginas PDF | Tema |
| --- | --- |
| 1–4 | Portada, capacidades, índice |
| 5–6 | Introducción, cadena Camera Manager/Modifiers, offsets |
| 7–11 | Camera Modifiers, manager y Camera Data Assets |
| 12–16 | Animation Camera Modifier |
| **17–23** | **Focus Modifier y Focus Target Method** |
| 24–27 | Yaw Follow Modifier |
| 28–32 | Pitch Follow Modifier |
| 33–37 | Angle Constraints |
| 38–45 | Pitch to Arm Length and FOV |
| 46–48 | Arm Offset |
| 49–56 | Dithering |
| 57–65 | FAQ, debugging, API, troubleshooting, Pawn movement |

**Limitaciones importantes:** el índice web muestra *System Setup*, pero estas páginas no están incluidas en el PDF. La documentación específica del **Advanced/Dynamic Spring Arm** tampoco está incluida (solo mención de la capacidad); recurrir a esa guía por separado. Los videos embebidos no se pudieron conservar (aparecen como enlace no visualizable). Algunas capturas pueden corresponder a versiones anteriores: comprobar nodos en UGC 1.92 instalado. No confundir una función Blueprint del manager con una API C++ tipada para settings de instancias BP.

## Modelo de integración según el fabricante

- Las modificaciones de cámara pasan por un **Blueprint hijo del BP_UGCCameraManager**. Evitar escribir directamente Target Arm Length, FOV, Socket Offset, Target Offset o Control Rotation desde otros sistemas: puede causar jitter o ser sobrescrito (FAQ pp. 57–61).
- Los modifiers se ejecutan por prioridad ascendente: **0 primero, 255 último**. Un modifier posterior puede sobreescribir el anterior. El manual ejemplifica Arm Offset Modifier con prioridad 120.
- El proveedor aconseja usar el **Target Offset y Socket Offset**, no el transform relativo de localización/rotación del Spring Arm, que debe permanecer en cero (pp. 5–6).
- **Camera Data Assets** agrupan settings por estado del personaje. El manager tiene PushCameraData / PopCameraDataHead y la captura p. 20 muestra también Set Camera Data. BertaDevKit Camera Cycle utiliza la pila de UGC: distinguir cambiar preset y activar Focus.
- Debug general: captura oficial FAQ p. 57 muestra el nodo real **Toggle All UGCModifiers Debug** (bool Enabled, target UGC Player Camera Manager). Se obtiene el manager con **Get BPUGCCamera Manager** desde la BPI UGC Interface. En Class Defaults de cada modifier hay además Debug individual. Este nombre real corrige la búsqueda previa errónea de un supuesto "Show Camera Modifiers Debug" en Event Graph.

## Focus: API Blueprint oficial documentada (pp. 17–23)

La imagen de p. 20 de BP_UGCCameraManager muestra estos **nombres exactos de nodos**:

| Nodo | Función |
| --- | --- |
| **Toggle Camera Focus** | Entrada **Enabled**: activa/desactiva el foco explícitamente |
| **Set Camera Focus Settings** | Entrada Focus Settings (interp speed, offset, input, LOS, distancias, etc.) |
| **Set Camera Focus Targeting Method** | Entrada Focus Method Class, salida Targeting Method Ref; cambia clase de estrategia |

La prosa de p. 23 dice *SetFocusTargetMethod*, mientras que **el nodo ilustrado** es *Set Camera Focus Targeting Method*. Buscar primero el nombre real ilustrado en la instalación. Los Camera Data Assets son otra ruta para asignar Focus Target Method y settings, **no un requisito obligatorio** para prender/apagar Focus en gameplay.

**Objetivo dinámico prescrito por el proveedor:** crear Blueprint Class **derivada de UGC_IFocusTargetMethod**, sobrescribir **GetTargetLocation**. Las entradas visibles son In Owner (actor), Owner Location, View Point Location, View Point Rotation (p. 22). **Llamar siempre a Parent: Get Target Location**: la guía lo destaca como obligatorio. La implementación retorna **Out Target Location** y puede retornar referencia del **actor** enfocado; el actor devuelto permite comprobar *line of sight*. Asociar la clase en Camera Data o con **Set Camera Focus Targeting Method** (pp. 22–23).

El ejemplo real del Sandbox BP_FocusTargetMethod_Test1 buscaba GetAllActorsWithTag(FocusTest1) y el primer actor; no es una selección dinámica general. **Candidato de mínima integración:** el gameplay selecciona actor A/B y conserva referencia de target en un estado local del jugador; la clase UGC_IFocusTargetMethod obtiene la referencia desde su contexto Owner/Controller o desde Targeting Method Ref *después de validar cómo se accede en Blueprint 1.92*, verifica IsValid y devuelve posición/actor. No asumir un setter del target que el manual NO documenta. El primer smoke test es cambiar explícitamente entre dos actores en PIE, sin introducir IA ni lock-on completo.

**Focus settings** documentadas pp. 18–19: Enabled, Interp Speed, Rotation Offset, Ignore Camera Input, Rotate Yaw Only, Stop if Blocked LOS, Blocked LOS Time Threshold, Min/Max Distance from Target, Focus Target Method. El fabricante advierte que enfocar demasiado cerca puede provocar giros violentos; controlar distancia mínima/colisión (p. 23).

**Conflicto Focus/Follow:** el manual pp. 20, 25 y 30 prohíbe Yaw/Pitch Follow simultáneos con Focus. La investigación anterior del Sandbox identificó las condiciones internas precisas: Yaw Enabled; Pitch Enabled OR Falling OR Slopes. Además, el vendor **ya trae ToggleYawAndPitchFollow** con los cuatro flags; esa función se descubrió en el Blueprint instalado (no ilustrada en las páginas del PDF). Aplicar cuatro false antes de Focus. Para salir sin pérdida de estado, almacenar/restaurar la combinación original. **No se ha confirmado un sistema completo y reversible de modos**.

## Matriz de funciones / status / próximo experimento

| Función UGC | Manual/nodos confirmados | Sandbox | Siguiente comprobación |
| --- | --- | --- | --- |
| Camera Presets / datos | pp. 7–11 PushCameraData, PopCameraDataHead, Set Camera Data | Cambios Medium/Near/Far confirmados en PIE; paquete previo confirmado | Interacción del ciclo de presets con Focus |
| Dynamic Spring Arm / Predictive Collision | Capacidad mencionada pp. 1–2, **procedimiento ausente** | Migración y anticipación de esquina verificada en PIE | Test packaged específico de colisiones |
| **Camera Focus** | pp. 17–23 **Toggle Camera Focus**, **Set Camera Focus Targeting Method**, UGC_IFocusTargetMethod | **Funciona en packaged (usuario, 2026-10-24)** | Pasar y cambiar actor objetivo explícito + ON/OFF |
| Yaw/Pitch Follow | pp. 24–32 SetYawMovementFollowSettings / SetPitchMovementFollowSettings | Yaw Follow automático **no observado**; conflicto con Focus identificado | Restaurar las cuatro condiciones exactamente |
| Animation Camera | pp. 12–16 Play Camera Animation, params y callback async; ResetType | No probado | Animación corta y reset/interrupt |
| Angle Constraints | pp. 33–37 SetYawConstraints / SetPitchConstraints | No probado | Prueba de límites con transición |
| Pitch-driven Arm Length/FOV | pp. 38–45 SetPitchToArmLengthAndFOVCurves, SetFOVRange, SetArmLengthRange | Curve fields estaban None en Medium: sin efecto verificado | CurveFloat normalizada [-1,1] y mirar arriba/abajo |
| Arm Offset | pp. 46–48 SetArmSocketOffset / SetArmTargetOffset | Datos de offsets existentes, no shoulder swap dinámico | Cambiar hombro con blend |
| Dithering | pp. 49–56 SetCameraDitheringSettings, TraceTag CameraDithering | No probado | Material Masked + Overlap channel + opacity mask |
| Debug | FAQ p. 57 Toggle All UGCModifiers Debug | Berta.UGC.Dump probado; debug del modifier sin salida antes | Usar nodo general documentado |

**Dithering requiere configuración real**, no solo habilitar un booleano: el material debe ser Masked, usar MF_CameraDithering_Whole en Opacity Mask (o MF_CameraDithering_Circle, que requiere UpdateMaterialPlayerPosition), y el objeto debe **Overlap** el collision channel correspondiente (Line of Sight u Overlap de CameraComponent). Comando documentado: **TraceTag CameraDithering** (pp. 50–56).

**Angle constraints suave:** el manual pp. 36–37 pide agregar **UGC_CameraTurnRateModifier** al Look Input Mapping para desacelerar junto al límite; no cambiar Enhanced Input/GAS sin otra prueba.

**Animation Camera:** ResetType BackToStart / ResetToZero / ContinueFromEnd; en strafing UGC puede forzar ResetToZero; la animación del personaje sincronizada puede requerir Level Sequencer (pp. 13–16).

## Orden de investigación recomendado

1. **Focus Target explícito:** usar UGC_IFocusTargetMethod y helpers documentados. Priorizar la prueba de dos targets en PIE y packaged.
2. **Dithering:** prepara un material y canal de Overlap real; aporta valor nuevo distinto de predictive collision.
3. **Arm Offset / shoulder swap**, luego **GAS Sprint Camera**: integraciones pequeñas, reutilizan el manager y las abilities actuales.
4. Pitch Curve, Constraints, Animation Camera en sesiones independientes.

**Buy/Integrate Before Build:** reutilizar API Blueprint comercial probada, no duplicar el Focus Modifier o crear una extensión C++ no viable. No reproducir screenshots/texto sustancial del manual en GitHub público. Si se quisiera estudiar System Setup o Spring Arm exhaustivamente, guardar por separado esas páginas que faltan.

## Referencias oficiales por sección

- [Focus](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/focus-modifier-22) (pp. 17–23).
- [Camera Modifiers](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/camera-modifiers-5) (pp. 7–11).
- [Animation Camera](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/animation-camera-modifier-23) (pp. 12–16).
- [Yaw Follow](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/yaw-follow-modifier-9) (pp. 24–27).
- [Pitch Follow](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/pitch-follow-modifier-16) (pp. 28–32).
- [Constraints](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/angle-constraints-modifier-17) (pp. 33–37).
- [Pitch Arm/FOV](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/pitch-to-arm-length-and-fov-modifier-18) (pp. 38–45).
- [Offsets](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/arm-offset-modifier-21) (pp. 46–48).
- [Dithering](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/dithering-modifier-20) (pp. 49–56).
- [FAQ](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/faq-14) (pp. 57–65).
