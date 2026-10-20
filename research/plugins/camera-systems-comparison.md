# Black Eye Camera vs. Ultimate Gameplay Camera

**Investigación R&D — 20 de octubre de 2026.** Proyecto: Sandbox Prototype, Unreal Engine 5.8. Alcance: arquitectura, sustitución, coexistencia y plan de validación. No se modificó ni ejecutó el proyecto Unreal.

## 1. Decisión

**Recomiendo B: Ultimate Gameplay Camera como único sistema de cámara del Sandbox, sujeto a superar una validación funcional y de packaging.** Es una decisión de arquitectura sustentada en documentación; todavía no es una certificación de integración.

Los argumentos decisivos son:

1. El problema actual es una cámara jugable sobre Third Person + CharacterMovement + GAS Companion. UGC documenta comportamientos específicos para ese problema: seguimiento de movimiento, pendientes y caída; restricciones angulares suaves; lock-on con pérdida de línea de visión; cambios de encuadre por estado; y prevención predictiva de penetración. Black Eye tiene una cámara Third Person real en v2, pero no documenta equivalentes completos para varias de esas políticas.
2. UGC centraliza las modificaciones mediante un Camera Manager y presets de datos, conservando Character, Controller y movimiento actuales. Requiere cambiar la clase de Camera Manager y, para su colisión actual, sustituir el SpringArm. No requiere sustituir el Character ni el PlayerController por los de una demo.
3. Black Eye es superior en composición procedural de planos, múltiples sujetos, Cross Camera, selección automática de planos y blending consciente de composición. Son ventajas reales, pero todavía no constituyen requisitos confirmados del Sandbox que justifiquen una segunda infraestructura.
4. No hay integración oficial localizada entre ambos. Mezclar sus dos managers no es una configuración soportada. Utilizar actores Black Eye con el manager de UGC es una hipótesis técnicamente razonable para necesidades cinematográficas futuras, con limitaciones importantes; no hace falta adoptarla ahora.

**Descarto A para la base actual:** convertir Black Eye en sustituto funcional completo de UGC exigiría cubrir políticas de gameplay que UGC ya entrega. No lo descarto como producto ni lo reduzco a herramienta de cinematics: su v2 tiene Orbit, colisión, input, prioridad y manager propios.

**Descarto C por ahora:** tener ambos comprados y tener extensiones BertaDevKit para ambos no demuestra necesidad de operarlos juntos. La combinación agrega contratos de entrada/salida, suspensión de modificadores, restauración de input, ControlRotation y estado de cámara. Se reabre únicamente cuando un prototipo necesite una capacidad exclusiva de Black Eye y Sequencer/cámaras nativas resulten insuficientes.

La recomendación no significa que UGC reemplace todo Black Eye, ni que deba ser una dependencia obligatoria de cada proyecto futuro. Es el proveedor inicial preferido para la cámara de gameplay del laboratorio. Un producto centrado en dirección procedural, diálogos de sujetos variables o realización multicámara puede justificar Black Eye como sistema único.

## 2. Evidencia y límites

### 2.1 Qué se consultó

- Estado del Sandbox y documentación central desde GitHub, rama main, árbol raíz observado `1f2f673642aa793ddcfbfd297962d9ce0a566b3e` [R1, R2].
- Manual **Black Eye v2.0**, recuperado directamente por el conector Google Drive: aproximadamente 83 mil caracteres; metadatos de modificación del documento: 29 de septiembre de 2026. Se revisaron las secciones de arquitectura, Orbit, Follow/LookAt, Cross, Shot List, Trigger Volumes, migración y rendering [B1–B8].
- Manual web de UGC: portada, System Setup, Introduction, Camera Modifiers y sus once páginas enumeradas —incluida Collision Modifier obsoleta—, FAQ e índice de Add-ons. El navegador de Work permitió recuperar contenido técnico que antes no se había podido leer [U1–U13].
- Fab en navegador con JavaScript: compatibilidad por motor/plataforma y changelogs de ambos productos [F1, F2].
- Hilo oficial de Black Eye en Epic Forums: anuncios, respuestas del fabricante y reportes de usuarios [T1].
- Tutoriales oficiales localizados desde las fichas; se verificaron enlaces, pero no se obtuvo una transcripción completa ni se verificó cada operación mostrada [V1–V3].
- Código público y README de las extensiones existentes de BertaDevKit, incluyendo ciclo de presets UGC, auditoría estática y switcher Black Eye [D1–D4].

Los PDFs adjuntos de Moonville no describen estos plugins y no se usaron como evidencia sobre sus cámaras. Moonville continúa fuera de la integración actual.

### 2.2 Qué NO se pudo comprobar

No hay proyecto Unreal ni fuentes comerciales instaladas accesibles en este workspace. No se inspeccionaron los grafos reales de los productos, sus descriptores locales, sus Build.cs completos, ni el código de sus algoritmos. Los identificadores de APIs comerciales citados provienen del manual o de consumidores públicos de esas APIs; eso no equivale a revisar su implementación.

Coda/Superhuman mostró ocasionalmente una invitación de registro; otras páginas fueron legibles públicamente. Algunas propiedades viven en imágenes, contenido diferido o videos. Se recuperó el cuerpo técnico principal, pero **no se presenta como una exportación íntegra ni como lectura de todos los detalles visuales del manual**. La FAQ se pudo complementar mediante su contenido oficial indexado. Los add-ons abren documentos embebidos separados; se confirmó su separación del producto base y su propósito, sin auditar todas sus subpáginas.

Firecrawl devolvió falta de créditos. La investigación continuó mediante Google Drive, búsqueda web y navegador. No se usaron sitios de descarga no autorizada como prueba de versiones, mantenimiento o código fuente.

### 2.3 Convención de certeza

- **D — documentación oficial:** el fabricante o Epic declara el mecanismo.
- **R — repositorio:** código o documentación actual del proyecto/BertaDevKit.
- **U — reporte del usuario:** comportamiento informado por Nico; no observado en esta sesión.
- **T — testimonio externo:** experiencia particular; no demuestra comportamiento general.
- **I — inferencia técnica:** conclusión razonada a partir de contratos documentados.
- **H — hipótesis:** requiere experimento.

**Ninguno de los dos plugins está funcionalmente probado en este Sandbox.** Instalación, compatibilidad declarada, compilación y comportamiento validado son estados diferentes.

## 3. Estado real y restricciones del Sandbox

La base actual usa `BP_ThirdPersonCharacter` derivado de `GSCModularCharacter`, un Third Person PlayerController Blueprint ya derivado de `GSCModularPlayerController`, CharacterMovement nativo y Enhanced Input. Jump, Sprint, Stamina y HUD tienen validación de Windows Development informada por el usuario; existen backups 04 de Jump y 05 de JumpSprint [R1].

La parte final del registro de Nice Interaction todavía deja packaging pendiente. **El mensaje actual de Nico lo actualiza: Windows Development ya pasó.** Se conserva ese resultado como U, sin modificar el registro anterior ni inventar la existencia del backup 06. Su creación sigue sin confirmar.

Nice Interaction utiliza `AC_PC_Interaction` en el Controller existente, `AC_GS_Interaction` en GameState y `LookAtTrace`. La interacción con E y Square funciona según el registro; el cambio automático del glyph está diferido. Ningún plugin de cámara necesita resolver ese problema de UI.

Restricciones de la elección:

- Conservar las clases y la herencia GAS existentes.
- No reemplazar CharacterMovement, ASC, habilidades ni GameState.
- Cambiar solo la infraestructura de cámara necesaria.
- Preservar Move, Look, Jump, Sprint y las acciones de interacción.
- No crear otro PlayerController por seguir literalmente una demo.
- No promover un framework de cámara nuevo a BertaDevKit antes de demostrar reutilización.

La guía de GAS Companion exige conservar un Controller modular o derivado para su integración de input [E3]. Cambiar la propiedad `PlayerCameraManagerClass` del Controller existente es conceptualmente distinto de cambiar su clase padre.

## 4. Arquitectura de Black Eye v2

### 4.1 Rigs de cámara independientes

La v2 reconstruye sus actores como extensiones de las cámaras de Unreal y usa `CineCameraComponent`. El manual identifica bases C++ y componentes Follow/LookAt. Entre sus clases documentadas están:

- `ABlackEyeCineCameraActorBase`: base de los rigs.
- `ABlackEyeCineCameraActor`: Follow + LookAt general.
- `ABlackEyeOrbitCineCameraActor`: Third Person orbital.
- `ABlackEyeCrossCineCameraActor`: encuadre relativo a dos sujetos/grupos.
- `ABlackEyeShotListCineCameraActor`: contenedor de cámaras.
- `UBlackEyeOrbitFollowComponent`: seguimiento y entrada orbital.
- `ABlackEyeCameraManager`, `UBlackEyeCameraPriorityStack`, `UBlackEyeCameraBlendList`, `UBlackEyeCameraSubsystem` e `ICameraWithTagAndPriority`: selección, registro y blending.

Estos son identificadores documentados, no una lista completa de módulos auditados [B1, B2].

**Follow calcula traslación; LookAt calcula orientación y encuadre.** Permiten sujetos Actor, componente o hueso; damping, offsets, zonas muertas y composición en pantalla. El Follow general puede combinar sujetos; Orbit permite un solo sujeto de Follow, pero comparte el LookAt más flexible [B3, B4].

Los actores pueden colocarse en el nivel o generarse desde el manager. El manual recomienda deferred spawning cuando se asignan targets por C++, para evitar una evaluación inicial con objetivos incorrectos. Hay autoasignación al Pawn del jugador y actualización de ControlRotation opcional [B2].

### 4.2 Manager, prioridad y blending

El manager amplía el contrato de PlayerCameraManager. Selecciona ViewTargets y puede mezclar propiedades conceptuales de composición, además de transform/FOV. Admite interrupción y blends concurrentes; la profundidad máxima por defecto documentada es cuatro [B2].

La pila ordena cámaras por prioridad, con valores más altos primero, y resuelve cambios al final del frame para evitar más de un cambio de cabeza por frame. Los Gameplay Tags identifican cámaras. Los Blend Lists eligen transición por par origen/destino, wildcard de origen/destino o valor por defecto. Los Trigger Volumes añaden/quitan solicitudes [B2, B7].

**Esto no conecta automáticamente con GAS.** Que ambos usen Gameplay Tags no significa que un Activation Owned Tag haga push de una cámara. Falta la lógica que traduzca cambios de estado del juego a solicitudes del manager.

La interfaz permite registrar cámaras o actores externos. El changelog 2.0.7 extiende explícitamente esa posibilidad a cualquier Actor y menciona otros sistemas de cámara [F1]. Eso facilita su inclusión como ViewTarget; **no demuestra que Black Eye pueda ejecutar el pipeline privado de otro Camera Manager**.

### 4.3 Input y relación con el personaje

Orbit vive fuera del Character. Enlaza input cuando pasa a ser ViewTarget y usa Enhanced Input por defecto. Debe configurarse con las acciones Look que realmente utiliza el proyecto; 2.0.7 admite varias acciones [B4, F1].

Para input personalizado se documentan `BindPlayerInput_Implementation(APawn*)`, `UnbindPlayerInput_Implementation(APawn*)` y `AddOrbitInput`. No requiere heredar el Character de una clase Black Eye. El manual desaconseja adjuntar uno de sus actores como hijo del SpringArm del personaje: Follow puede comportarse incorrectamente [B2, B4].

La integración debe definir un solo propietario efectivo del Look y la relación entre vista y ControlRotation. Dejar simultáneamente la lógica de cámara Third Person anterior y la de Orbit sin revisar sus escrituras es una fuente de riesgo, no una incompatibilidad probada.

### 4.4 Capacidades de dirección

Cross Camera coloca la cámara con respecto a la línea entre dos sujetos/grupos y combina damping del brazo virtual y de cámara. Es especialmente útil para diálogos de personajes con distinta posición o tamaño [B5].

Shot List ofrece:

- **Blend:** secuencia de cámaras con espera y transición.
- **Cloud:** elección de cámara con vista despejada; prioridad o aleatoriedad, tiempo mínimo de plano y parámetros de sondeo.
- **Anidación:** Cloud de cámaras que a su vez son Shot Lists Blend.

Cloud soluciona selección de un plano visible; no equivale a prevención predictiva continua de penetración del Orbit [B6].

Sequencer, seguimiento de huesos, foco óptico/DoF, bake hacia una cámara enlazada, Camera Switcher y Save In Play son parte importante de su valor de autoría [B3, B6, B8].

## 5. Arquitectura de Ultimate Gameplay Camera

### 5.1 Cámara del Pawn y pipeline de modificadores

UGC mantiene el modelo Character/Pawn → SpringArm → CameraComponent, coordinado por su PlayerCameraManager. Su base `UGC_CameraModifier` extiende `UCameraModifier`. Los modificadores se ejecutan secuencialmente por prioridad: cero primero, 255 último [U2, U3].

El manual llama al manager `BP_UGCCameraManager`; el consumidor C++ de BertaDevKit confirma la clase nativa `AUGC_PlayerCameraManager` y los presets `UUGC_CameraDataAssetBase` [D2, D3]. Son capas de la misma familia, no una obligación de adoptar un Controller de demo.

Los puntos de extensión documentados incluyen:

- `ProcessControlRotation`.
- `ProcessBoomDistanceAndFOV`.
- `ProcessBoomOffset`.
- `ProcessTurnRate`.

No todo se aplica únicamente al POV final: hay trabajo sobre la rotación de control y sobre propiedades del boom. Por eso **añadir un UGC modifier a cualquier Camera Manager no demuestra disponer de UGC completo** [U3].

El manual requiere que el Camera Manager derive del suyo. Se pueden añadir/quitar modificadores en defaults o en runtime. Para customizaciones se documentan hijos de `BP_UGCModifierBase` y llamadas al padre en los overrides [U3, U12].

### 5.2 Configuración y estados

Los Camera Data Assets agrupan configuración de la cámara y sus modifiers. Se activan mediante `PushCameraData`; `PopCameraDataHead` devuelve al estado previo. El manager/modifiers interpolan la transición. También existen setters específicos [U3].

**Una pila de presets no equivale a arbitraje completo de estados concurrentes.** Sprint, Aim y una notificación de animación pueden superponerse. Si cada sistema hace Pop de la cabeza sin verificar qué entrada posee, puede quitar el estado equivocado. El código de BertaDevKit ya evita que su ciclo de presets interfiera con un preset temporal ajeno y distingue Pop de cabeza de eliminación por asset [D2].

Toda modificación de FOV, longitud y offsets debe pasar por el manager/modifier correspondiente. La FAQ advierte que las escrituras externas pueden ser sobrescritas o generar jitter. También advierte que Focus no debe activarse simultáneamente con Yaw Follow/Pitch Follow [U4, U12].

### 5.3 Movimiento y input

Con CharacterMovement estándar, la integración no exige una sustitución del sistema de movimiento. Para Pawns con movimientos no estándar existe `UGC_PawnMovementInterface`, con información como velocidad, caída, suelo y strafe [U12]. No hay razón documentada para introducirlo como reemplazo de CharacterMovement en este Sandbox.

UGC documenta mouse/gamepad y Enhanced Input mediante `UGC_CameraTurnRateModifier` para que el pipeline pueda modificar la sensibilidad. El SpringArm debe usar `UsePawnControlRotation`. Hay también una interfaz de Pawn de input identificable en el consumidor de BertaDevKit, cuya necesidad concreta se debe contrastar con los assets instalados; su auditoría no la considera universalmente obligatoria [U3, U12, D3].

Hay que revisar la ruta Look existente antes de añadir el input modifier. No se deben crear acciones Move/Jump/Sprint nuevas ni sustituir el Mapping Context de GAS para instalar una cámara.

### 5.4 Colisión actual

Para UE 5.5 y posteriores, el antiguo Collision Modifier está obsoleto. Su reemplazo documentado es `BP_UGCSpringArmComponent`, con `DoCollisionTest` habilitado, prevención de penetración y predictive avoidance [U9].

Usa feelers con sphere sweeps y offsets angulares, inspirados en Lyra, para anticipar obstáculos; ofrece tiempos distintos de entrada/salida, pesos de mundo/Pawn, radios y exclusión por tag. `TraceTag CameraPen` permite visualizar las consultas [U10].

**No seguir las instrucciones antiguas que desactivan DoCollisionTest.** Esa receta corresponde al modifier deprecado y entra en contradicción con la ruta actual.

Dithering es otro mecanismo: consultas LOS y overlaps más materiales preparados para ocultación. Requiere materiales Masked y funciones/parametrización compatibles. El dithering circular usa posición del jugador en una Material Parameter Collection [U11]. No es automático sobre cualquier material y debe probarse cuidadosamente con vistas locales múltiples.

## 6. Matriz de funcionalidades

**Clasificación:** N = mecanismo incorporado directamente; C = capacidad implementada que necesita configuración/assets/eventos de proyecto; X = extensión personalizada para la capacidad descrita; ND = no documentada en las fuentes examinadas. ND no prueba ausencia. **A** identifica un add-on separado; no se atribuye al producto base. C no implica que la integración ya esté hecha.

### 6.1 Gameplay, control y obstáculos

| Capacidad concreta | Black Eye v2 | UGC base | Diferencia práctica y fuente |
| --- | --- | --- | --- |
| Third Person libre | C: Orbit | C: boom + modifiers | Ambos implementados; modelo de ownership diferente [B4, U2–U3]. |
| First Person completo | X/ND como modo completo; integración de cámara UE FPS documentada | ND como framework FPS completo | No confundir aceptar una cámara FPS con resolver meshes, clips y transición FPS/TPS [B2, U1]. |
| Follow de Actor/componente/hueso | C | C: sigue Pawn mediante boom; Focus calcula objetivo | Black Eye tiene Follow procedural más general [B3, U4]. |
| Órbita y freelook | C: input propio de Orbit | C: ControlRotation del jugador | Black Eye cambia dónde vive la cámara; UGC conserva boom [B4, U2]. |
| Damping posicional por eje y orientación | C: Follow/LookAt/Orbit | C: interpolaciones; lag heredado del boom, validar configuración | No hay medición que permita declarar cuál se siente mejor [B3–B4, U3]. |
| Zonas muertas de encuadre | C: screen dead zones + Follow Dwell | ND como sistema equivalente | Ventaja Black Eye para seguimiento de sujetos con animación [B3]. |
| Recentrado tras inactividad | C: delay + speed | C: Yaw Follow con tiempo/ángulo y Pitch Follow | No son el mismo algoritmo ni la misma política [B4, U5–U6]. |
| Follow de dirección de movimiento | C parcial: heading relativo al sujeto y recentrado | C: Yaw Movement Follow explícito | UGC evita equiparar orientación del Actor con su velocidad [B4, U5]. |
| Adaptación a pendientes y caída | X para política equivalente | C: Pitch Follow | Ventaja UGC; Black Eye Up Reference no equivale a leer la pendiente [B4, U6]. |
| Up arbitrario / referencia orbital rotada | C: World/Subject/Custom | ND como capacidad equivalente del base | Black Eye documenta orbits orientables; advierte posibles problemas de gimbal [B4]. |
| Límites de pitch | C: min/max orbital | C: Angle Constraints | Ambos; UGC documenta desaceleración suave [B4, U7]. |
| Límites relativos de yaw y desaceleración | ND/X para equivalencia | C: Angle Constraints | UGC resuelve explícitamente esa política [U7]. |
| Colisión que acerca cámara al sujeto | C: sondeo Orbit + deflexión | C: Dynamic Spring Arm | Los dos; no atribuir la colisión Orbit a todos los rigs Black Eye [B4, U10]. |
| Anticipación con múltiples feelers | ND/X | C | Ventaja documentada UGC, sin benchmark comparativo [U10]. |
| Suavizado de salida de colisión | C: restore time | C: blend in/out | UGC documenta ambos tiempos; Black Eye además limita deflexión [B4, U10]. |
| FOV en función de colisión | C: curva de deflexión | ND como política específica equivalente | Ventaja Black Eye documentada [B4]. |
| Ocultación de obstáculos/propio personaje | ND/X | C: Dithering + materiales | Ventaja UGC, con trabajo real de materiales [U11]. |
| FOV y distancia por estado | C: parámetros/rigs y selección | C: Camera Data Assets | Los dos requieren traducir estados del juego [B2, U3]. |
| FOV según pitch | C: curva Orbit normalizada 0..1 | C: modifier con curvas -1..1 | Ambos; no reutilizar la misma curva sin adaptar dominio [B4, U8]. |
| Distancia según pitch | X/ND como curva dedicada en manual Orbit | C: Pitch to Arm Length | Orbit Radius editable no prueba curva incorporada [B4, U8]. |
| Zoom con rueda/botón listo para el proyecto | X: binding/política | X: binding/política | Tener FOV/distancia configurables no entrega controles de zoom terminados. |
| Shoulder y cambio de hombro | C para encuadre; X para política de cambio | C: Arm Offset; X para input/estado | UGC documenta explícitamente el caso [B3–B4, U9a]. |
| Aim camera / apuntado | C encuadre, X integración arma/estado | C presets/Focus, X integración arma/estado | Ninguno prueba sistema de armas, retícula o balística completo. |
| Look-at objetivo | C: LookAt independiente de Follow | C: Focus | Ambos; objetivos/composición de Black Eye son más generales [B3, U4]. |
| Lock-on: LOS, umbral temporal y distancia | X sobre LookAt para esa política | C: Focus | UGC documenta ruptura por LOS, min/max distancia y mitigación de objetivo cercano [U4]. |
| Adquisición y cambio de enemigo completos | X | X: Target Method configurable/custom | El modifier de Focus no es un sistema completo de selección de combate [U4]. |
| Respeto/suspensión de input durante Focus | C: componer/configurar Orbit, validar interacción | C: Ignore Camera Input | UGC documenta el contrato explícito [B4, U4]. |
| Enhanced Input | C: Look Actions; múltiples desde 2.0.7 | C: ruta Look y TurnRateModifier | No se presupone que los mappings de demo sean los del Sandbox [F1, U3]. |
| Gamepad | C: acciones Enhanced Input | C: documentado mouse/gamepad | Ambos deben probar sensibilidad, inversión y ausencia de doble input [B4, U12]. |
| GAS / GAS Companion directo | X: puente de estado | X: puente de estado | No se localizó integración oficial específica de ninguno. |
| Notify de animación para FOV/offset/distancia | X: invocar API desde eventos propios | C: AnimNotifyStates incluidos para UE5.5+ | UGC incluye ANS_UGCCameraDataAsset y overrides temporales [U13]. |

### 6.2 Dirección, herramientas y reutilización

| Capacidad concreta | Black Eye v2 | UGC base | Diferencia práctica y fuente |
| --- | --- | --- | --- |
| Cambios contextuales de comportamiento | C: prioridad/tags/triggers | C: push/pop de presets; X trigger de mundo | Black Eye entrega infraestructura de cámara contextual más completa [B2, B7, U3]. |
| Pila de cámaras con prioridad numérica | N/C | ND equivalente; tiene pila de settings | Una pila de presets no sustituye una pila de ViewTargets [B2, U3]. |
| Blend de ajustes dentro de un modo | C | C | Diferente unidad de configuración: rigs vs presets/modifiers. |
| Blend de composición entre cámaras | C: manager Black Eye | ND; blending UE convencional | Ventaja Black Eye; usar otro manager pierde esa garantía [B2]. |
| Interrupciones/múltiples blends activos | C: documentado, profundidad 4 por defecto | ND como contrato equivalente | No inferir equivalencia por ver una transición suave [B2]. |
| Composición multiobjetivo ponderada | C | X para framing equivalente | Focus Target Method puede producir un punto, pero no es un solver de encuadre multiobjetivo [B3, U4]. |
| Cross Camera para dos grupos/sujetos | C | ND/X | Exclusiva documentada Black Eye [B5]. |
| FOV dinámico por tamaño del sujeto | C | ND/X | Black Eye usa bounds/viewport size; no equivale a FOV por pitch [B3]. |
| Foco óptico/DoF hacia hueso/componente | C | ND específico; capacidades nativas UE aparte | No confundir Focus lock-on UGC con enfoque de lente [B3, F1]. |
| Look-ahead por velocidad | C: LookAt | A: Movement Anticipation | Add-on no incluido en base UGC [B3, U14]. |
| Secuencia de rigs con hold/blend sin Sequencer | C: Shot List Blend | X o Sequencer nativo | No es Camera Animation aditiva [B6]. |
| Selección de plano despejado/aleatorio | C: Shot List Cloud | ND/X | Distinto problema de colisión del boom [B6]. |
| Shot Lists anidadas | C | ND/X | Ventaja Black Eye para dirección procedural [B6]. |
| Switching multicámara en vivo | C: Camera Switcher | X o herramientas UE | No es un sistema de replay grabado ni replicación de director [B8]. |
| Sequencer / gameplay a cinematic | C | C: guía FAQ | Ambos; tienen limitaciones y configuración diferente [B8, U12]. |
| Camera Animation aditiva + callbacks | ND como wrapper equivalente; Sequencer UE disponible | C: Animation Camera Modifier | UGC documenta parámetros y eventos de fin/interrupción/ease-out [U3a]. |
| MRQ/MRG con blend conceptual propio | C, con limitación de previsualización | ND específico | Ventaja Black Eye de rendering; no prueba playback igual al scrub [B8]. |
| Bake a cámara enlazada | C | ND específico | Black Eye exige componentes compatibles [B3]. |
| Save In Play y panel especializado | C/N | ND equivalente; runtime tuning documentado | Black Eye ahorra trabajo de iteración y persistencia [B1]. |
| Diagnóstico | N: CVars, pila, blends, cámara activa | N/C: debug global/por modifier | Hay herramientas adicionales BertaDevKit opcionales [B1–B2, U12, D1–D3]. |
| Reutilización de configuración | C: hijos BP, curvas, Blend Lists, spawn settings | C: Data Assets y modifiers hijos | Ambos; distintas dependencias de datos. |
| Camera Shake nativo UE | I: compatible a verificar con manager elegido | I: compatible a verificar con manager elegido | No es una feature específica garantizada por estos manuales [E1–E2]. |
| Shake dinámico según movimiento | ND/X | A: Dynamic Shake | No contar el add-on como base UGC [U14]. |
| Speed a FOV/longitud | X mediante eventos/curvas de proyecto | A: Speed to Arm Length and FOV | El preset Sprint del base no es respuesta continua a velocidad [U14]. |
| Cámara guiada por spline | ND como solver dedicado del manual revisado | A: Spline | Sequencer nativo puede resolver tomas concretas sin otro producto [U14]. |
| Cámara de combate especializada | C/X: Cross/LookAt + lógica de combate | A: Dynamic Combat Camera | Focus base sí está incluido; el add-on de combate es separado [U4, U14]. |
| Multiplayer | C: targeting por jugador, arreglos de spawn y posesión documentados | D: funcionamiento local declarado | No equivale a replicar estados compartidos o a prueba de red [B2, F1–F2]. |
| Rendimiento comparado | ND: sin benchmark controlado | ND: sin benchmark controlado | Más C++ o menos actores no demuestra mejor rendimiento. |

## 7. Sustitución: qué se conserva y qué se pierde

### 7.1 ¿Black Eye puede reemplazar completamente UGC?

**No como equivalencia funcional documentada.** Sí puede reemplazar la cámara Third Person básica y muchas necesidades contextuales sin UGC: Orbit, LookAt, recentering, FOV, damping, colisión, tags y transiciones ya están incorporados.

La brecha aparece al pedir la misma política de gameplay: prevención predictiva multifeeler, dithering de oclusores, pendientes/caída, límites angulares suaves relativos, lock-on con pérdida de LOS/distancia y notify states listos. Algunas se pueden construir con APIs Black Eye y Unreal; construirlas es X, no una funcionalidad que el producto ya entregue.

Si el juego es exploración Third Person sencilla y dirección procedural sofisticada, esas brechas podrían no importar y Black Eye único sería defendible. En el Sandbox, cuyo objetivo es validar gameplay e integrar sistemas con mínimo código, cubrirlas por anticipado pierde frente a UGC.

### 7.2 ¿UGC puede reemplazar completamente Black Eye?

**Tampoco.** UGC cubre mejor la cámara controlada por jugador y ofrece Camera Animation y blending convencional a cinematics. Eso alcanza para muchos prototipos, sin necesitar el solver de composición Black Eye.

No se documentan equivalentes completos para Cross Camera, framing ponderado de varios sujetos con FOV por tamaño, Cloud/nesting de Shot Lists, blending de composición, Save In Play ni panel de dirección. Crear un modifier que calcule esas cosas sería implementar otro sistema.

Por eso B es una elección por requisitos actuales y coste de complejidad, no una declaración de superioridad universal.

### 7.3 Comparación de las capacidades compartidas

| Necesidad | Implementación y flexibilidad preferida | Configuración y trabajo personalizado | Juicio |
| --- | --- | --- | --- |
| Cámara Third Person libre | UGC para movimiento estándar; Black Eye para órbita/Up/composición más general | UGC mantiene boom; Black Eye mueve ownership a actor y enlaza Look | UGC para esta base; calidad visual por medir. |
| Colisión jugable | UGC tiene anticipación y controles más específicos documentados | Reemplazo del SpringArm y ajuste de feelers; Black Eye requiere menos conceptos para push-in simple | UGC más completo para interiores, no rendimiento demostrado. |
| Suavizado | Black Eye detalla damping y composición; UGC integra seguimiento/políticas | Ambos requieren tuning con el mismo recorrido | Sin ganador de calidad hasta comparar video/frame rate. |
| Lock-on | UGC para límites/LOS/input; Black Eye para composición de grupos | Ambos necesitan suministro/adquisición de target | Menos lógica personalizada UGC en lock-on de gameplay. |
| Aim/shoulder | UGC tiene Arm Offset y presets explícitos | Ambos necesitan input/estado y prueba contra obstáculos | UGC más directo; ninguno reemplaza gameplay de armas. |
| Sprint y otros estados | UGC: preset; Black Eye: rig/propiedades/pila | Ambos necesitan puente GAS | UGC coincide con el estado del Pawn; Black Eye gana si el estado cambia el tipo de plano. |
| Cámaras contextuales de mundo | Black Eye: triggers, prioridad, reglas de blend | UGC requiere trigger y política de presets propia | Black Eye más completo y menos glue en esta categoría. |
| Cinematic convencional | Ambos pueden usar Sequencer UE | UGC ya documenta CanBlend/LockPreviousCamera; Black Eye requiere evitar tracks que compitan con Follow | UGC suficiente para el caso mínimo; Black Eye más flexible para composición dinámica. |
| Reutilización y extensión | UGC: cadena modular y datos; Black Eye: rigs/componentes y dirección | Mantener hijos y configuración propios en ambos | UGC preferido para gameplay, Black Eye para autoría procedural. |
| UE5.8 y mantenimiento | Ambos declaran soporte; Black Eye documenta cambios mejor | Verificar versiones locales y packaging antes de adjudicar facilidad real | No hay evidencia para declarar integración efectiva más rápida en este Sandbox. |

## 8. Coexistencia: posible, pero no adopción por defecto

### 8.1 Dos managers no se suman

Un PlayerController selecciona una clase de PlayerCameraManager. Los productos presentan familias de herencia propias y pipelines distintos [B2, U3, E2]. No existe herencia múltiple de estas dos clases UObject que permita conservar automáticamente sus implementaciones.

Elegir Black Eye Manager y “añadir UGC modifiers” conserva potencialmente modificadores nativos aislados, pero no demuestra mantener el procesamiento de boom, preparación de Pawn, presets y funciones del manager UGC. Elegir UGC Manager no incorpora la pila/blending conceptual Black Eye.

Tampoco hay evidencia oficial de que modificar el mismo CameraComponent/ControlRotation desde ambos simultáneamente sea soportado. La FAQ UGC advierte justamente contra múltiples escritores [U12].

### 8.2 Combinación razonable si aparece una necesidad exclusiva

**H: UGC como único manager y cámara jugable; actores Black Eye no-Orbit como ViewTargets temporales para planos procedurales.**

Las responsabilidades serían:

- UGC: input de gameplay, boom, colisión, presets de locomoción/aim.
- Black Eye: Follow/LookAt/Cross/Shot List de un plano temporal.
- Unreal/una única operación de reveal: entrar, salir y recuperar la cámara.
- Lógica del proyecto: política explícita para suspender/restaurar modifiers que interfieran, input y ControlRotation.

Aquí se renuncia al manager/pila y al high-level blending de Black Eye. El paso de UGC a Black Eye usaría blending estándar de ViewTarget; **no hereda por existir un rig Black Eye el blending conceptual del manager Black Eye**.

BertaDevKit ya tiene `BertaBlackEyeCameraExt`, que usa el Controller/manager existente y SetViewTargetWithBlend, sin requerir el manager Black Eye [D1, D4]. Eso reduce la necesidad de construir glue nuevo; **no demuestra que el contrato UGC respete toda la sesión ni que sus modifiers se suspendan correctamente**.

La extensión captura retorno y ControlRotation, maneja políticas de cambio externo y elimina sus propias contribuciones de bloqueo. Sus eventos temporizados no miden convergencia visual. No es un árbitro global de cámaras ni integración oficial UGC–Black Eye.

También existe un camino inverso de investigación: Black Eye como manager y un Actor externo registrado mediante su interfaz. Esto es soporte oficial de ViewTargets externos [B2, F1], pero no una receta oficial para preservar el manager UGC. No lo propongo como arquitectura del Sandbox.

### 8.3 Conflictos concretos a probar

| Contrato | Riesgo | Condición mínima para coexistencia |
| --- | --- | --- |
| Ownership de vista | Stack Black Eye, Sequencer y reveal piden distintos destinos | Un manager final y política explícita de interrupción/retorno. |
| ControlRotation | Orbit/autoalineación, Focus y input ajustan la misma rotación | Suspensión por modo y restauración correcta, sin escritura competitiva por frame. |
| Input | Look del Character y Orbit quedan enlazados | Evitar Orbit en la primera prueba combinada; contar bindings y revisar salida/posesión. |
| FOV/offset/colisión | UGC modifica una cámara externa o el boom anterior durante un plano | Inspeccionar ejecución real de modifiers; suspender únicamente los incompatibles. |
| Blending | Doble interpolación, outgoing lock o retorno con salto | Probar entrar, interrumpir, salir y encadenar transiciones. |
| Estados GAS | Sprint termina durante el reveal y reaparece un preset vencido | El retorno refleja estado actual, no solo una instantánea anterior. |
| Destrucción/posesión | Desaparecen Pawn, target o cámara capturada | Fallback seguro y limpieza de solicitudes/input por propietario. |
| Multiplayer local | Jugador equivocado o estado visual compartido | Prueba separada por LocalPlayer; revisar MPC de dithering. |

La documentación de la extensión Berta advierte que el código Black Eye 2.0.7 inspeccionado anteriormente no tenía un override actor EndViewTarget que eliminara bindings de Orbit. **Esta sesión no revisó el source comercial y no reprodujo un defecto.** Es una pista estática de validación, no un bug demostrado ni motivo para parchear el producto [D1].

**Conclusión de coexistencia:** no es imposible ni necesariamente frágil si se acota a planos especializados. Pero todavía no aporta un requisito activo que compense el coste adicional. No ejecutar C preventivamente.

## 9. Compatibilidad con el Sandbox

| Parte existente | UGC | Black Eye | Estado de evidencia |
| --- | --- | --- | --- |
| BP_ThirdPersonCharacter → GSCModularCharacter | Conservar herencia; usar su CameraComponent y boom compatible | Conservar herencia; cámaras externas siguen al Pawn | D de arquitectura + I de compatibilidad conjunta; sin prueba Sandbox. |
| Controller → GSCModularPlayerController | Conservar clase y AC_PC_Interaction; asignar manager UGC | Conservar clase; asignar manager Black Eye si se adopta el sistema completo | No hace falta un nuevo Controller [B2, U3, E3]. |
| CharacterMovement | Mantener; proveedor de movimiento estándar | Mantener; sujeto de seguimiento | I fuerte; no sustituir por un framework de movimiento. |
| GA_Jump | Sin dependencia directa de cámara; Pitch Follow puede reaccionar a caída | Follow/damping siguen al sujeto | Compatibilidad inferida; comprobar aterrizaje y control aéreo. |
| GA_Sprint/Stamina | Traducir estado a preset, sin tocar coste/drain/MaxWalkSpeed | Traducir estado a selección/propiedades de cámara | X de glue, no integración oficial directa. |
| Nice Interaction | Conservar componentes y LookAtTrace | Igual; cambios de perspectiva afectan percepción/interacción | Validar cómo obtiene la traza su origen/dirección en assets reales. |
| Enhanced Input | Revisar Look y TurnRateModifier; no duplicar contextos de GAS | Configurar Look Actions reales y eliminar rutas de cámara competitivas | D de mecanismo; mapa exacto local no inspeccionado. |
| HUD y stamina | No se prevé necesidad de modificación | Igual | I; regresión obligatoria en PIE y paquete. |
| UE5.8 Windows | Declarado en Fab | Declarado y añadido explícitamente en changelog | D de soporte; sin build funcional de cámara en este Sandbox. |

No copiar la instrucción de un ejemplo Black Eye que pone GameMode Override en None sin analizar el mapa: en este Sandbox podría desactivar la configuración que ya funciona. Tampoco usar su Setup Wizard sin revisar el resultado: el changelog 2.0.3 reconoce una corrección del wizard que afectaba la conservación de clases/datos existentes [F1].

Con UGC, **sí hay una modificación de componentes** para obtener la colisión actual: sustituir el SpringArm por el del plugin, conservando el CameraComponent y transfiriendo configuración válida. Si CameraBoom fuera un componente nativo heredado no reemplazable en Blueprint, habría que determinar el ajuste mínimo en esa clase; no crear otro Character por defecto. La estructura exacta sigue pendiente de inspección.

## 10. Mantenimiento, código y rendimiento

### 10.1 Versiones y soporte observados

| Aspecto | Black Eye | UGC |
| --- | --- | --- |
| Motor declarado en Fab | 5.4–5.8 | 5.1–5.8 |
| Plataforma de desarrollo declarada | Windows, macOS, Linux | Windows |
| Plataforma relevante para este experimento | Windows | Windows |
| Última actualización de ficha | 9 septiembre 2026: videos de galería | 24 junio 2026 |
| Última versión de código identificada oficialmente | 2.0.7, 19 agosto 2026 | La ficha no explica versión en sus notas; no adoptar un número de mirrors |
| Soporte UE5.8 explícito en changelog | 19 junio 2026 | Rango de versiones de ficha; notas no informativas |
| Calidad de changelog | Detallado: fixes y cambios API | Entradas observadas mayormente sin descripción |
| Canales de ayuda | Discord, web y respuestas públicas en Epic Forums | Discord, tutoriales y documentación web |
| Prueba de respuesta efectiva actual | No se contactó soporte | No se contactó soporte |

La última fecha de ficha no equivale siempre a una actualización del código. Black Eye permite distinguirlo; UGC deja menos trazabilidad pública [F1–F2].

Black Eye documenta correcciones relevantes en 2.0.5–2.0.7: jitter por orden de evaluación, cambios a cámaras UE, Standalone, spawn multiplayer, deflexión/DoCollisionTest e input múltiple. Es evidencia favorable de mantenimiento activo y también una razón para verificar la versión local, no una promesa de que ya no existen fallos [F1].

UGC tiene migración importante de colisión en UE5.5+ y nuevos AnimNotifyStates para esas versiones. Las páginas antiguas siguen accesibles; hay riesgo de mezclar instrucciones [U9–U10, U13].

### 10.2 Source y dependencia del proveedor

Ambos exponen clases C++ y Blueprints; UGC declara mayor parte de lógica en Blueprint y bases C++. Black Eye v2 documenta migración de bases a C++ y referencias a headers/source. BertaDevKit consume APIs de ambos, lo que evidencia superficies extensibles [B1, F2, D1–D4].

**No se confirmó aquí la integridad del código fuente incluido en cada instalación.** Antes de adoptarlos, revisar Source/Public/Private, Build.cs, uplugin y capacidad de recompilar la versión local. Tener headers y acceso a Blueprint no demuestra disponer de toda implementación o de una política futura de soporte.

Black Eye 1.x→2.x exigió cambios de nombres/includes, reparenting y reconstrucción de Shot Lists viejas. 2.0.3 cambió firmas de input; 2.0.6 pasó UCLASS a MinimalAPI. No suponer que toda subclase C++ seguirá enlazando tras una actualización sin revisión [B1, F1].

Para ambos, conservar hijos/configuración en carpetas propias; no editar assets originales ni forkar el proveedor antes de encontrar un defecto concreto. Mantener versión y backup por hito. Evitar llamadas a APIs comerciales distribuidas por todas las habilidades; el puente debe ser pequeño y local. BertaDevKit conserva integraciones opcionales, sin volver obligatorios los productos en el toolkit base.

### 10.3 Dependencias que sí se conocen y que faltan

- Black Eye: cámaras/CineCamera de UE; Enhanced Input por defecto en Orbit; Gameplay Tags para su stack; APIs editor y rendering para herramientas especializadas.
- UGC: CameraManager/CameraModifier nativos, boom/CameraComponent, Enhanced Input en la ruta documentada y Camera Animation/Sequencer para animaciones.
- Ninguna fuente revisada establece GAS Companion o Nice Interaction como dependencia obligatoria.
- **Lista transitiva exacta de plugins/módulos: pendiente del uplugin/Build.cs instalado.** No asumir que una función opcional de cine/render es dependencia de todo el runtime.

### 10.4 Rendimiento

No hay benchmark A/B confiable que permita ordenar ambos. La afirmación “AAA” no es una métrica. Tampoco lo es un porcentaje histórico de optimización del changelog Black Eye.

UGC añade ejecución secuencial de modifiers, feelers y eventualmente overlaps/dithering/materiales. Black Eye evalúa rigs/sujetos, LookAt, blends y, en Cloud, visibilidad de candidatos. Más candidatos activos o probes pueden aumentar coste. Es una inferencia de sus mecanismos, no una medición.

Medir misma escena, misma resolución/frame rate, mismo recorrido y preset visual comparable; warmup, varias repeticiones y CPU de cámara/consultas con Unreal Insights. Comparar frame time mediano y p95, número/coste de queries y calidad visible. No comparar solo FPS de mapas de demo distintos. Si no hay problema observable o presupuesto concreto, no optimizar.

### 10.5 Experiencias externas

En el hilo de Black Eye, un usuario reportó fallo de packaging con un mapa de ejemplo en noviembre de 2025; el autor informó un fix en 1.1.10.4.1. Otro pidió un plano sobre hombro adaptable y el autor propuso Cross Camera en junio de 2026 [T1]. Son interacciones concretas y útiles, no validación del Sandbox ni estimación de tasa de bugs.

Se buscaron experiencias externas UGC. Buena parte de los resultados eran reseñas replicadas por agregadores o mensajes de verificación de Discord, sin proyecto reproducible ni versión actual. No se usaron para establecer calidad técnica ni soporte 5.8. Las calificaciones Fab no demuestran corrección ni rendimiento. No se localizó un case study independiente, reproducible y contemporáneo que comparara ambos en la misma escena.

## 11. Riesgos principales de la opción B

1. **Varios escritores o modifiers incompatibles.** FOV/boom/ControlRotation externos pueden competir; Focus excluye Yaw/Pitch Follow. Mitigación: un punto de solicitud y presets que activen explícitamente combinaciones válidas.
2. **Receta de colisión equivocada.** El modifier antiguo y el spring arm nuevo requieren configuraciones distintas. Mitigación: versión instalada primero; ruta actual con DoCollisionTest activado y pruebas de esquinas/techo.
3. **Pila de presets sin ownership.** Sprint termina bajo Aim o durante un notify y elimina el estado equivocado. Mitigación: no hacer PopHead indiscriminado; confirmar semántica real de eliminación y probar solapamientos/cancelaciones.
4. **Regresión de input/rotación/movement.** El Look modifier, UsePawnControlRotation y offsets pueden cambiar percepción y movimiento relativo a vista. Mitigación: conservar acciones y validar mouse/gamepad antes de añadir automatismos.
5. **Dithering modifica materiales.** Afecta el pipeline visual y puede compartir estado mediante MPC. Mitigación: fuera de la primera integración; aplicar a un único material de prueba después.
6. **Mantenimiento poco trazable.** Changelog UGC no explica cambios y el manual mezcla rutas de motor. Mitigación: congelar versión local, documentar diffs relevantes y repetir regresión al actualizar.
7. **Dirección procedural futura fuera del alcance.** UGC no entrega Cross/Cloud ni composición multicámara equivalente. Mitigación: usar UE/Sequencer para necesidades simples y reevaluar Black Eye solo contra una escena concreta.

## 12. Plan mínimo de integración y validación — no ejecutado

### Fase 0: cerrar baseline e inspeccionar

1. Confirmar creación del backup 06 posterior al paquete Nice Interaction exitoso; si no existe, crearlo antes de cambiar cámara. No confundir ese backup con el futuro hito de cámara.
2. Registrar versiones locales de ambos plugins, UE5.8 exacta y módulos runtime/editor. Comprobar que no se selecciona una clase deprecated.
3. Inspeccionar CameraBoom, FollowCamera, ruta Look, Mapping Contexts y PlayerCameraManagerClass del Controller existente. Buscar setters de FOV/boom/ControlRotation y plugins que ya los toquen.
4. Revisar el tutorial oficial y los grafos instalados para cerrar los detalles de input que no eran legibles en texto. No copiar GameMode/Controller/Character de demo.

### Fase 1: UGC mínimo, sin funciones extras

- Trabajar sobre el baseline respaldado y una copia del nivel de prueba; mantener Character, Controller, GameState y GameMode existentes.
- Crear únicamente una configuración/hijo de Camera Manager UGC propia y asignarla en el Controller actual. Usar un hijo existente si ya está creado.
- Mantener FollowCamera; sustituir/configurar CameraBoom con el SpringArm actual del plugin y revisar transforms, offsets y UsePawnControlRotation.
- Reutilizar Look y sus contextos reales, añadiendo el TurnRateModifier donde corresponda según versión. No duplicar bindings de GAS/interacción.
- Un solo preset inicial: cámara libre y colisión. Focus, automatismos, dithering y cámara Sprint desactivados inicialmente.

**Gate:** Move/Look con mouse y gamepad, sensibilidad consistente y ninguna rotación duplicada. La cámara no atraviesa un muro simple ni salta al iniciar PIE.

### Fase 2: regresión y un comportamiento de estado

- Ejecutar Jump y Sprint exactamente como antes: coste, drain, regen, cancelación por cero, restauración de velocidad, nueva activación y HUD.
- Ejecutar cubo/campfire con E y Square. Comprobar que la traza corresponde a la vista final, también tras un cambio de offset.
- Añadir un único preset Sprint con encuadre/FOV perceptible y traducir `Berta.State.Movement.Sprinting` a solicitudes locales de cámara. Usar una sola ruta simétrica de entrada/salida; no tocar la lógica de movimiento ni stamina.
- Probar liberación de input, agotamiento y cancelación; la cámara debe volver a exploración en todos los casos.
- Añadir Yaw/Pitch Follow uno por vez, únicamente si mejora control. Probar pendiente, caída y movimiento lateral/strafe; el input manual debe recuperar prioridad.

### Fase 3: prueba discriminante de cámara

Usar un recorrido corto con muro, esquina convexa/cóncava, puerta y techo bajo; caminar, sprintar y rotar rápido. Repetir a 30, 60 y 120 FPS cuando sea posible. Observar penetración, pops, oscilación, tiempo de recuperación, comodidad y coste CPU.

Probar Focus con un objetivo: estático, móvil, muy cercano, fuera de rango y oculto brevemente. Verificar exclusión de Yaw/Pitch Follow y recuperación de freelook. No hace falta un sistema de combate completo para esta prueba.

Solo después probar un cambio de hombro/preset Aim y una superposición Sprint→Aim→fin de Sprint→fin de Aim. El resultado final debe corresponder al estado actual, sin preset vencido.

### Fase 4: cinematic y ciclo de vida

Una Level Sequence mínima con cámara UE, ida/vuelta y una interrupción. Seguir la instrucción UGC de CanBlend/LockPreviousCamera cuando aplique. Probar respawn/posesión si el proyecto los necesita; no ampliar a red antes de que sea un requisito.

Dithering queda como prueba separada de un solo material. Camera shake nativo también se valida aparte; no instalar un add-on para demostrar que el manager acepta shake de Unreal.

### Fase 5: paquete y evidencia

Windows Development con el nivel correcto como Game Default Map; reinicio del Editor y prueba del ejecutable. Repetir Move/Look, interacción, Jump/Sprint/Stamina/HUD, colisión y retorno de cinematic. Registrar versiones, cambios exactos, video corto y fallos.

**Aceptación:** sin regresiones funcionales, sin doble input, sin cámara atascada tras cancelación/retorno y sin dependencia accidental de assets de editor/demo. El hito de cámara recibe el próximo número de backup disponible después de confirmar 06. Hasta entonces, ninguna casilla se marca probada.

### Experimento mínimo que puede cambiar la decisión

La documentación permite elegir B como dirección. Faltan evidencia local de integración, comparación perceptual y coste real. Para impugnar la elección sin integrar dos sistemas simultáneamente:

1. Desde el mismo baseline, realizar dos pruebas aisladas y reversibles: UGC libre + colisión; Black Eye Orbit libre + colisión.
2. Mantener clases GAS y mappings equivalentes; no añadir extensiones custom que oculten una brecha.
3. Repetir el mismo recorrido de obstáculos y una transición a cámara contextual con retorno. Cronometrar el trabajo de integración y registrar cambios de clases/componentes y problemas.
4. Si UGC no puede conservar la base sin modificaciones sustanciales o su cámara falla repetidamente donde Black Eye funciona, revisar A. Si ambos pasan, las políticas de gameplay ya implementadas favorecen B.
5. Solo si una escena narrativa real requiere Cross/Cloud, comparar UGC + Sequencer nativo contra Black Eye. Si el primero cumple con poco trabajo, mantener B. Si Black Eye resuelve una necesidad decisiva, evaluar A antes que C.
6. C requiere además una prueba de entrar/interrumpir/salir de un rig Black Eye bajo UGC Manager, restaurando cámara/input/estado y pasando packaging. Sin ese resultado, no declararlo soportado.

**Estado final de esta investigación:** recomendación B; integración y experimentos pendientes; ningún Blueprint, asset Unreal ni plugin comercial modificado.

## 13. Referencias y acceso a secciones

Fecha de consulta de todas las fuentes: 20 de octubre de 2026. Las etiquetas identifican exactamente el documento y la sección que sostiene cada afirmación. Para el Google Doc no se inventan anchors: usar la pestaña/sección indicada dentro del enlace oficial.

### Black Eye

- **B1.** [Manual oficial Black Eye v2.0](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “What’s new in v2.0!”, “Installation”, “DEBUG Draws”, “Known Issues”, “Upgrading 1.x cameras”, “Black Eye Panel”. Leído mediante Google Drive.
- **B2.** [Mismo manual — BEC for Gameplay](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “Black Eye Camera Manager”, “High Level Blending”, “Priority Stack & its API”, “Blend Lists”, “Adding Priority Stack support to UE Cameras & Actors”, “How to Initialize Black Eye Cameras in your Game”.
- **B3.** [Mismo manual — BEC Camera](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “Follow”, “Look”, “Composition”, “Dynamic FoV”, “Focus”, “Linked Camera - camera bake”.
- **B4.** [Mismo manual — BEC Orbit Camera](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “Input Binding”, “Enhanced Input”, “Custom Input Binding”, “Orbit Control”, “Follow”, “Customization”, “Collision”.
- **B5.** [Mismo manual — BEC Cross Camera](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “Cross Camera Perpendicular Follow”, “Cross Camera LookAt”.
- **B6.** [Mismo manual — BEC Shot List](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “Blend mode”, “Cloud mode”, “Nested Shot Lists”, controles de ambos modos.
- **B7.** [Mismo manual — BEC Trigger Volume](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit): “How it works”, ejemplo TightSpace.
- **B8.** [Mismo manual — MRQ / MRG Rendering y Camera Switcher](https://docs.google.com/document/d/11WFd3UGSwaFpYBnzZJyAOWoq0fzvk6B4jLuuA2K95FI/edit), más “Known Issues” para conflictos de Transform tracks y spawnables.

### Ultimate Gameplay Camera

Los enlaces Coda proporcionados redirigen a Superhuman Docs. Los siguientes destinos fueron observados en el navegador:

- **U1.** [Portada y capacidades](https://coda.io/@aurora-devs/documentation-ultimate-gameplay-camera); [System Setup](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/system-setup-3): tutoriales y notas de integración ALS/Lyra.
- **U2.** [Introduction](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/introduction-6): arquitectura, orden de modifiers y offsets del boom.
- **U3.** [Camera Modifiers](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/camera-modifiers-5): “BP UGC Camera Manager”, puntos Process*, “UGC Camera Data Asset”, push/pop.
- **U3a.** [Animation Camera Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/animation-camera-modifier-23): creación, reproducción, parámetros, callbacks y Reset Type.
- **U4.** [Focus Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/focus-modifier-22): settings, target method y problema de objetivo cercano.
- **U5.** [Yaw Follow Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/yaw-follow-modifier-9): seguimiento, thresholds y exclusión con Focus.
- **U6.** [Pitch Follow Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/pitch-follow-modifier-16): default pitch, pendiente y caída.
- **U7.** [Angle Constraints Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/angle-constraints-modifier-17): límites relativos y desaceleración.
- **U8.** [Pitch to Arm Length and FOV Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/pitch-to-arm-length-and-fov-modifier-18): dominios de curvas y rangos.
- **U9a.** [Arm Offset Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/arm-offset-modifier-21): Socket/Target offsets y blend.
- **U9.** [Collision Modifier — DEPRECATED](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/collision-modifier-deprecated-19): cambio de ruta desde UE5.5.
- **U10.** [Dynamic Spring Arm](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/new-dynamic-spring-arm-36): setup, feelers, settings y diagnóstico.
- **U11.** [Dithering Modifier](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/dithering-modifier-20): consultas, materiales, MPC y funciones.
- **U12.** [FAQ](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/faq-14): jitter/escrituras externas, debugging, cámara cinematográfica, mouse/gamepad y custom movement. Complementada con el [contenido oficial Coda indexado](https://coda.io/@aurora-devs/documentation-ultimate-gameplay-camera/faq-14).
- **U13.** [AnimNotifyState Modifiers](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/new-animnotifystate-modifiers-37): FOV, longitud, offsets y ANS_UGCCameraDataAsset.
- **U14.** [Add-ons](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/add-ons-25): confirma distribución separada de Dynamic Combat Camera, Speed, Shake, Movement Anticipation y Spline. Las capacidades no se atribuyen a UGC base.

### Fichas, tutoriales y experiencias

- **F1.** [Black Eye — Fab](https://www.fab.com/listings/41696440-c0c3-42dd-a4bd-51f463912af0): formatos/compatibilidad y botón “Registro de cambios”; entradas 19 junio, 24 junio, 16 julio, 19 agosto y 9 septiembre de 2026.
- **F2.** [UGC — Fab](https://www.fab.com/listings/4d3a1a73-a38d-4704-b552-8b543f24fd25): formatos/compatibilidad y “Registro de cambios”; última fecha 24 junio de 2026; notas sin detalle.
- **V1.** [Black Eye v2: START HERE](https://www.youtube.com/watch?si=k8cccPphV49h9R4d&v=vs6yjL-l_FQ&feature=youtu.be), enlazado desde Getting Started en Fab. No confundir con el tutorial anterior a v2.
- **V2.** [How to install Ultimate Gameplay Camera on Unreal Engine 5](https://www.youtube.com/watch?si=Q6DrctFgfSHnJcLe&v=wa_N5Q0MQ-w&feature=youtu.be), tutorial oficial enlazado desde Fab.
- **V3.** [Black Eye — Seamless Gameplay Cutscene Transitions](https://youtu.be/lJ_1NAYtdtg), enlace oficial desde Fab; no se auditó toda su reproducción.
- **T1.** [Black Eye — Epic Developer Community Forums](https://forums.unrealengine.com/t/black-eye-technologies-black-eye-cameras/2436923): respuestas 11–12 sobre packaging; 14–16 sobre Cross Camera. Reportes históricos, no pruebas de esta versión local.

### Unreal, GAS Companion y repositorios

- **E1.** [Epic — UCameraModifier](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/UCameraModifier): contrato de modificador, asociación a manager y prioridades.
- **E2.** [Epic — APlayerCameraManager](https://dev.epicgames.com/documentation/unreal-engine/API/Runtime/Engine/APlayerCameraManager): ViewTarget y ApplyCameraModifiers. [ProcessViewRotation](https://dev.epicgames.com/documentation/unreal-engine/API/Runtime/Engine/UCameraModifier/ProcessViewRotation): retorno puede detener modifiers posteriores.
- **E3.** [GAS Companion — Getting Started](https://gascompanion.github.io/quick-start/): “Setup Map and Game Mode”, clases modulares y Enhanced Input.
- **R1.** [Estado actual del Sandbox](../../projects/sandbox-prototype.md). Distinguir registro histórico de actualizaciones comunicadas en esta investigación.
- **R2.** [Índice oficial de documentación](plugin-documentation-index.md).
- **D1.** [BertaBlackEyeCameraExt README](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaBlackEyeCameraExt/README.md): “Requirements”, retorno, policies, input y limitaciones.
- **D2.** [BertaUGCCameraCycleComponent.cpp](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/Source/BertaUltimateGameplayCameraExt/Private/Components/BertaUGCCameraCycleComponent.cpp): uso real de manager, presets y stack.
- **D3.** [BertaUGCAudit.cpp](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/Source/BertaUltimateGameplayCameraExtEditor/Private/Audit/BertaUGCAudit.cpp): inspección estática, posibles escritores y alcance limitado.
- **D4.** [BertaBlackEyeCameraSwitcherComponent.cpp](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaBlackEyeCameraExt/Source/BertaBlackEyeCameraExt/Private/BertaBlackEyeCameraSwitcherComponent.cpp): uso de SetViewTargetWithBlend sin manager Black Eye obligatorio.

Este informe es una síntesis propia y referencias de investigación; no redistribuye manuales completos, código ni assets comerciales.
