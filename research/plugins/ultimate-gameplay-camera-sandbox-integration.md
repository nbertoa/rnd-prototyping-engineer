# Ultimate Gameplay Camera — integración real en el Sandbox UE 5.8

**Fecha:** 20 de octubre de 2026  
**Proyecto:** Blank C++ + Third Person Feature Pack; `L_EnvironmentTest`  
**Estado actual:** **Cambio entre presets Near / Medium / Far confirmado por el usuario en PIE mediante `Berta.UGC.Next` (dos cambios sucesivos informados).** No hay todavía confirmación del packaging Windows Development de **esta integración UGC**, ni backup de cámara posterior al número 06.

> **Procedencia:** reconstrucción de los pasos ejecutados y de los resultados expresamente comunicados en la conversación del 20-10-2026; contrastada con el código fuente de `BertaUltimateGameplayCameraExt` en `nbertoa/ue5-bertadevkit` y con la [investigación de cámaras](camera-systems-comparison.md). No se leyó el proyecto Unreal local, el `.uasset` ni el Output Log íntegro. La configuración exacta de los assets y la asignación final en el Editor deben tratarse como **reportadas o inferidas de la secuencia de pruebas**, no como inspección independiente. No atribuir un problema de la extensión a UGC sin evidencia.

## 1. Qué estamos intentando validar

Integrar **Ultimate Gameplay Camera (UGC)** como proveedor de cámara **jugable** en el Sandbox, conservando el personaje y Controller de GAS Companion, `CharacterMovement` nativo, Nice Interaction V2 y Enhanced Input, y experimentar con **tres encuadres definidos por datos** sin crear tres actores de cámara ni tres `CameraComponent`.

El Sandbox no es un producto final: los tres estilos **son una prueba de la API de presets y de la extensión**, no una decisión definitiva de interfaz de jugador.

Arquitectura objetivo de esta etapa:

```text
GameMode existente
└── Third Person PlayerController Blueprint existente
    ├── Parent: GSCModularPlayerController   (conservar)
    ├── AC_PC_Interaction                     (conservar)
    ├── Player Camera Manager Class
    │   └── BP_SandboxUGCCameraManager       (hijo de BP_UGCCameraManager)
    │       └── Default Camera Data: DA_Camera_Medium
    └── BertaUGCCameraCycleComponent
        └── CameraPresets: [DA_Camera_Medium, DA_Camera_Near, DA_Camera_Far]
            
BP_ThirdPersonCharacter existente
├── Parent: GSCModularCharacter              (conservar)
├── CameraBoom / FollowCamera existentes
└── GAS Jump / Sprint / Stamina / HUD

UGC PlayerCameraManager + UGC Camera Data Assets
→ modifican configuración/encuadre según presets
```

**Matices importantes:** `BP_SandboxUGCCameraManager` es el **nombre que se indicó crear**; el usuario terminó reportando éxito después de las instrucciones para crear/configurar/asignar ese hijo, pero no aportó captura final del nombre de clase ni del array. Los nombres Near/Medium/Far son nuestra convención y se deben confirmar si se quiere una reproducción byte-for-byte del proyecto real. El diagrama representa la **configuración de trabajo prevista y coherente con la secuencia**, no un volcado de assets.

## 2. Baseline y condiciones antes de UGC

Se partió de un checkpoint **`06_NiceInteraction_Validated`**, cuyo backup completo y packaging Windows Development fueron confirmados por el usuario, con:

- `BP_ThirdPersonCharacter` heredando de `GSCModularCharacter`.
- Third Person PlayerController existente heredando de `GSCModularPlayerController`, sin crear otro Controller.
- GAS Companion: Jump, Sprint, Stamina, regeneración y `WB_HUD_Minimalist`, validados en paquete antes de UGC.
- Nice Interaction System V2: `AC_PC_Interaction` en ese mismo Controller y `AC_GS_Interaction` en `BP_SandboxGameState`; cubo y campfire interactuables con `E` y botón cuadrado de PlayStation.
- Entorno Ultra Dynamic Sky + SSFS y prueba independiente Chameleon ya respaldados.
- UGC y la extensión `BertaUltimateGameplayCameraExt` habilitados (reportado por el usuario); también Black Eye y su extensión, **sin integrarlos todavía en runtime**.

No cambiar GameMode, Pawn, CharacterMovement, GAS ni el sistema de interacción para resolver un problema que pertenece a configuración o selección de cámara.

## 3. Cronología precisa: configuración, pruebas, fallos y resolución

### 3.1 Se seleccionó directamente el Camera Manager del plugin

En **Class Defaults** del **Third Person PlayerController existente**, propiedad **Player Camera Manager Class**, se seleccionó `BP_UGCCameraManager` del fabricante.

El usuario compiló/guardó y probó PIE en `L_EnvironmentTest`. **Confirmó:**
- Seguimiento y control de cámara correctos con mouse/joystick.
- Movimiento del personaje normal.
- Incluso percibió que la cámara se veía mejor.

Ejecutó `Berta.UGC.Dump` y confirmó que el **Camera Manager activo era UGC**. **No se conservó salida completa** ni conteo de modifiers.

**Lectura técnica:** tener una instancia UGC de Camera Manager no prueba todavía que los **Data Assets propios** estén seleccionados, ni que cada modifier/colisión avanzada funcione.

### 3.2 Se agregó el componente de extensión

Se agregó `BertaUGCCameraCycleComponent` como componente al **mismo Third Person PlayerController**. No se crearon entradas de Enhanced Input adicionales. La extensión tiene comandos de consola propios, por lo que la primera validación no necesitaba tocar controles de gamepad.

### 3.3 Se crearon y documentaron Data Assets

Se creó un `Data Asset` seleccionando la clase **`UGC_CameraDataAssetBase`**, con el nombre `DA_Camera_Medium`, en la carpeta de trabajo `/Game/Gameplay/Camera`.

Se recibieron **cinco fotos iniciales** y **cinco capturas digitales posteriores** del panel **Details**. Todos los campos/valores visibles, incluido `Camera Arm`, `FOV`, `Collisions`, `Dithering`, `Movement Follow`, `Angle Constraints`, `Focus` y `Add Ons`, están transcritos en [la referencia detallada del Camera Data Asset](ultimate-gameplay-camera-data-asset-reference.md).

Después se creó al menos un segundo preset `Far`; más tarde se solicitaron valores de Near y Far. **El usuario informó que probó dos cambios con Next y funcionaron.**

### 3.4 Primer fallo: editar un Data Asset no cambiaba visualmente la cámara

**Síntoma comunicado:** cambiar parámetros del asset no producía diferencias; la cámara se mantenía igual. El propio usuario preguntó si faltaba algo en Third Person Character.

**Error en la guía anterior:** se le indicó crear/asignar assets a `CameraPresets` **sin haber configurado su selección inicial mediante un manager propio**. Además, se confundió inicialmente la confirmación de que el Camera Manager estaba activo con la confirmación de que **nuestro preset** estaba activo. Son dos verificaciones distintas.

**Hecho de código:** `BertaUGCCameraCycleComponent` **no aplica ningún preset por sí solo al agregarlo**: `BeginPlay()` valida y reconcilia el índice de uno ya activo; el cambio ocurre mediante llamadas explícitas a `SelectCamera`, `SelectNextCamera` o `SelectPreviousCamera`, que utilizan `PopCameraDataHead` y `PushCameraData`.

### 3.5 Segundo fallo: Next devolvía false, índice -1

Se probó en consola:

```text
Berta.UGC.Next
Berta.UGC.Dump
```

El usuario reportó en Output Log: **`success=false` y `storedCycleIndex=-1`**. **No conservamos la línea completa con nombre del Controller ni el contenido íntegro del Dump.**

Se pidió verificar la asignación del array; el usuario probó también con **dos assets** y no funcionó.

**Diagnóstico parcial posterior:** el Dump mostró que estaba activo **un Camera Data Asset del fabricante**, distinto de los assets propios que se habían creado. Se probó la hipótesis de añadir ese asset del fabricante al array en la primera posición. **El usuario confirmó que eso tampoco resolvió el error.**

**Corrección a nuestras conclusiones previas:** era demasiado concluyente afirmar que el error se debía únicamente a tener un preset activo fuera del ciclo. El código tiene múltiples caminos de `false`. La referencia a un preset del fabricante era una **pista real**, no una prueba definitiva de causa raíz; y añadirlo al array **falló en esa sesión**.

### 3.6 Qué dice realmente el código de BertaDevKit

Fuentes revisadas:
- [`BertaUGCCameraCycleComponent.cpp`](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/Source/BertaUltimateGameplayCameraExt/Private/Components/BertaUGCCameraCycleComponent.cpp)
- [`BertaUGCConsoleCommands.cpp`](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/Source/BertaUltimateGameplayCameraExt/Private/Console/BertaUGCConsoleCommands.cpp)
- [`BertaUGCCameraDebugLibrary.cpp`](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/Source/BertaUltimateGameplayCameraExt/Private/Debug/BertaUGCCameraDebugLibrary.cpp)
- [README de la extensión](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/README.md)

**Condiciones que pueden impedir Next/Select:**

1. El Controller dueño del componente no es válido.
2. Su `PlayerCameraManager` no es `AUGC_PlayerCameraManager` o derivado.
3. El array `CameraPresets` está vacío, tiene un `None`/asset inválido o referencias duplicadas.
4. Hay un `GetCurrentCameraDataAsset()` activo que **no coincide por identidad de objeto** con ninguno de los elementos del array.
5. El índice solicitado está fuera del array.
6. Después de `PushCameraData`, el manager no conserva el asset de destino como el actual.

`SelectNextCamera` primero verifica (2), (3) y (4), y luego llama a `SelectCamera`. Una selección exitosa actualiza `CurrentCameraIndex`. `INDEX_NONE` se muestra como **-1**.

**Limitación real de diagnósticos de nuestra extensión:** la línea final de `Berta.UGC.Next` contiene solo `success` e índice guardado. Algunos rechazos en `ValidatePresets` se registran como warning; un array vacío no genera aviso específico; un preset ajeno durante `SelectAdjacentCamera` retorna `false` **sin log en ese camino**. `Berta.UGC.Dump` incluye `ActivePreset` y `CycleActiveIndex` (además de datos de Pawn/SpringArm/modifiers), pero no enumera todos los presets del array ni la condición exacta que rechazó Next. **Esto constituye deuda de observabilidad de BertaDevKit; no se demostró un bug de UGC**.

La auditoría del editor **Tools → Audit Ultimate Gameplay Camera** comprueba los presets configurados y otras reglas estáticas, pero no resuelve todos los rechazos runtime. Se recomendó ejecutarla; **el usuario no informó resultados de esa auditoría**, por lo que no debemos escribir que pasó.

### 3.7 El dato decisivo vino de una integración anterior

El usuario recordó un proyecto anterior donde había creado **un Camera Manager Blueprint hijo del `BP_UGCCameraManager`** y configurado allí **`Default Camera Data`**. Señaló que en esta integración faltaba ese paso.

Se acordó y recomendó:

1. Crear **Child Blueprint Class** de `BP_UGCCameraManager`, bajo el nombre `BP_SandboxUGCCameraManager`.
2. En **Class Defaults** de ese hijo, configurar **`Default Camera Data = DA_Camera_Medium`**.
3. Asignar ese hijo en el **Third Person PlayerController existente → Class Defaults → Player Camera Manager Class**, reemplazando la referencia directa al manager del plugin.
4. Compilar y guardar.
5. Mantener `BertaUGCCameraCycleComponent` en el Controller y su array con los presets que realmente usaremos.

**Estado de evidencia:** estas instrucciones se dieron; el usuario no compartió captura de la asignación ni transcripción paso a paso de su ejecución, pero **inmediatamente después informó que el cambio entre presets mediante Next funcionaba**. Esto es evidencia funcional consistente con esa corrección. No se puede reconstruir si también hizo otro ajuste no mencionado.

**Conclusión operativa:** usar un **manager propio derivado del del proveedor** y fijar explícitamente un Camera Data Asset inicial es el camino probado en esta conversación. **Causa raíz exacta del primer `false`: no demostrada.** Es razonable que el cambio haya alineado el preset activo con el conjunto permitido por el cycle; no afirmar que esa haya sido la única causa sin Dump antes/después.

### 3.8 Confirmación de funcionamiento de Next

Tras recibir los valores Near/Far, el usuario expresó: **“Perfecto, ahí funcionó haciendo next en las dos.”**

Estado atribuible con certeza:
- Al menos **dos avances por `Berta.UGC.Next`** produjeron cambios de cámara perceptibles para el usuario en PIE.
- La combinación UGC + extensión de ciclo deja de estar bloqueada y la función de **selección de estilos** está probada en esta sesión.
- No hay todavía captura ni log posterior que certifique índices concretos o el orden exacto de esos dos avances. Tampoco se probó `Previous`, una asignación de botón de gamepad, empaquetado UGC ni el retorno desde Black Eye.

## 4. Presets de prueba: valores prescritos

**Convención de nombres propuesta:** `DA_Camera_Medium`, `DA_Camera_Near` y `DA_Camera_Far`. Si el asset ya creado se llama `DA_Camera_Close` en el proyecto, usar el nombre real: el objetivo es la distancia, no una renombrada forzosa. No duplicar CameraComponents: son Data Assets de configuración.

| Parámetro del Data Asset | Medium (capturas) | Near (valores propuestos) | Far (valores propuestos) |
| --- | ---: | ---: | ---: |
| Min Arm Length | 100 | 80 | 350 |
| Max Arm Length | 250 | 120 | 450 |
| Min FOV | 90 | 80 | 95 |
| Max FOV | 125 | 90 | 105 |
| Arm Range Blend Time | 0.5 s | 0.5 s | 0.5 s |
| FOVRange Blend Time | 0.5 s | 0.5 s | 0.5 s |

- Los valores **Medium** son de las capturas legibles de Details (y correspondían visualmente a la cámara previa según el usuario).
- Los valores **Near** y **Far** se proporcionaron como **presets experimentales sugeridos** y el usuario dijo que los cambios con Next funcionaban; **no inspeccionamos los uassets finales para certificar numéricamente lo que quedó guardado**.
- Se recomendó **duplicar Medium**, cambiar **solo** los cuatro campos de distancia/FOV y mantener blend `0.5`; offsets, curvas, colisión, Focus y seguimiento, sin modificaciones en esta prueba.
- Los límites min/max de distancia y FOV no representan necesariamente la longitud/FOV exactos del POV actual. El manager/modifiers, normalizaciones y SpringArm determinan el resultado. Para probar de forma controlada conviene distinguir **preset activo** de **valor final observado** con `Berta.UGC.Dump`.
- Son tres **estilos de una cámara jugable**, no tres cámaras físicas. No está decidida todavía la UX final; no se agregaron botones.

## 5. Procedimiento reproducible (nuevo proyecto con el mismo stack)

### A. Infraestructura

1. Partir del Third Person Character y PlayerController **ya existentes**; no copiar el GameMode ni personajes de la demo UGC.
2. Habilitar UGC y `BertaUltimateGameplayCameraExt`. Verificar compatibilidad instalada y compilar.
3. Crear en Content Browser los `UGC_CameraDataAssetBase` bajo una carpeta de proyecto, al menos `DA_Camera_Medium`; duplicarlo para los otros dos estilos y cambiar únicamente los cuatro campos de la tabla.
4. **Antes de probar los presets**: hacer hijo Blueprint de `BP_UGCCameraManager` del fabricante, por ejemplo `BP_SandboxUGCCameraManager`, y en **Class Defaults** seleccionar `Default Camera Data = DA_Camera_Medium`. Este es el paso que se omitió en la primera secuencia.
5. En el **Third Person PlayerController Blueprint existente** (`GSCModularPlayerController`), establecer `Player Camera Manager Class = BP_SandboxUGCCameraManager`. No cambiar su Parent Class ni retirar `AC_PC_Interaction`.
6. Agregar `BertaUGCCameraCycleComponent` al mismo Controller; en `CameraPresets` poner **assets válidos, distintos y pertenecientes al ciclo**. Orden inicial recomendado: `[DA_Camera_Medium, DA_Camera_Near, DA_Camera_Far]`. No meter un preset de ejemplo del fabricante solamente como parche si el manager ya utiliza el nuestro. Reutilizar el `Default Camera Data` propio en el ciclo para que Next no lo considere ajeno.
7. Compilar/guardar y usar el mismo GameMode que ya apunta a ese Controller.

### B. Comprobación controlada en PIE

1. Iniciar `L_EnvironmentTest` y probar movimiento y Look.
2. Ejecutar `Berta.UGC.Dump`. Verificar **Manager**, **ActivePreset** y **CycleActiveIndex**. Esperado con el orden anterior: Medium activo e índice de ciclo `0` (esto último es **expectativa del código**, no un log archivado).
3. Ejecutar `Berta.UGC.Next`, comprobar `success=true`, cambio visual y nuevo `ActivePreset` con `Berta.UGC.Dump`.
4. Repetir Next para el segundo cambio, luego `Berta.UGC.Previous` para validar reversibilidad. Los dos **Next** están reportados como funcionando; `Previous` sigue sin confirmación.
5. Alternativamente `Berta.UGC.Select 0`, `Berta.UGC.Select 1`, `Berta.UGC.Select 2` fuerzan índices concretos, si se necesita aislar problemas.
6. **No asignar todavía cruceta ni inputs extra**. Primero completar la comprobación por consola y evitar conflictos con las entradas actuales de GAS Companion y Nice Interaction.

### C. Cuando Next falle de nuevo

1. Copiar la **línea íntegra** de `Berta.UGC.Next` y de `Berta.UGC.Dump` (especialmente `ActivePreset`, `CycleActiveIndex`, `SpringArm`, `Modifiers`).
2. Comprobar que el `PlayerController` en el log es el que realmente usa el GameMode de la escena.
3. Verificar que **`Player Camera Manager Class` es el hijo UGC configurado** y que el `Default Camera Data` es un asset **incluido en el array**.
4. Verificar que los elementos de `CameraPresets` están asignados y **no tienen None ni duplicados**.
5. Si no basta, correr `Tools → Audit Ultimate Gameplay Camera` y copiar advertencias (la auditoría no reemplaza logs runtime).
6. Solo si todavía falla, instrumentar `BertaUGCCameraCycleComponent` para registrar **qué rama concreta devuelve false**. No añadir cámaras nuevas ni editar Third Person Character por conjetura.
7. Si el comando retorna `true` pero el encuadre no cambia: comprobar `ActivePreset` y `FinalPOV`, y luego inspeccionar SpringArm/modifiers aplicables; esto es **otro problema** y no debe mezclarse con el fallo de selección.

## 6. Errores de nuestra asistencia y lecciones

| Error o salto de conclusión | Qué pasó | Qué hacer en futuras integraciones |
| --- | --- | --- |
| Se empezó por el manager base del proveedor, sin fijar Default Camera Data propio | UGC Manager funcionaba, pero nuestro asset no era el activo | Configurar **primero un hijo del manager con default propio**, después probar el ciclo. |
| Se asumió que seleccionar una clase de Camera Manager equivalía a aplicar un Data Asset | El usuario editó valores sin cambios visuales | Verificar **ActivePreset**, no solamente la clase `Manager`. |
| Se sugirió crear presets extra antes de resolver `Success=false` | Añadir Far no arregló nada | Resolver selección/validación antes de ampliar la matriz de presets. |
| Se atribuyó `false` al preset ajeno con certeza excesiva | Añadir el asset vendor al ciclo **no arregló** el problema | Tratarlo como hipótesis; capturar causa concreta en runtime. |
| Se siguió pidiendo modificar arrays sin salida diagnóstica suficiente | `storedCycleIndex=-1` no revela el punto de retorno | Usar Dump/Audit, y mejorar logs en la extensión si reaparece. |
| Riesgo de tocar Third Person Character prematuramente | La primera falla pertenecía a selección/configuración de presets | Separar **activación de Camera Data** de los cambios de SpringArm que harán falta para otras features. |
| Se asumió que tener plugin activo prueba todas sus features | Solo se validó control básico y dos cambios de estilo | No marcar colisión predictiva, Focus, dithering, GAS state camera, Black Eye ni package hasta probarlos. |

## 7. Sobre Character, colisiones y funciones pendientes

**No hay evidencia de que se haya reemplazado el `CameraBoom` nativo por el `BP_UGCSpringArmComponent`.** Para UGC en UE 5.5+ el informe de investigación describe ese componente como la ruta documentada de colisión predictiva; **no mezclar** instrucciones antiguas del Collision Modifier deprecado. Esta es una comprobación futura **separada**, y debe revisarse con los componentes que realmente existen en `BP_ThirdPersonCharacter`. No duplicar Character ni crear componentes por inercia.

La existencia de campos de `Penetration Avoidance Feelers`, `Dithering`, `Yaw/Pitch Follow` o `Focus` en el Data Asset **no acredita su funcionamiento**. Del mismo modo, todavía no hay puente GAS para disparar preset Sprint por Gameplay Tag ni integración cinematográfica Black Eye.

## 8. Estado de validación y siguiente gate

| Elemento | Estado |
| --- | --- |
| UGC/BertaUGC habilitados | Confirmación verbal del usuario |
| `BP_UGCCameraManager` original como manager activo | Comprobado en PIE mediante Dump en primera prueba |
| Cámara libre + movimiento + Look | Confirmado en PIE |
| `BertaUGCCameraCycleComponent` en Controller existente | Confirmado por el usuario |
| `DA_Camera_Medium` creado y campos documentados | Confirmado por capturas |
| Camino de manager hijo + Default Camera Data | Recomendado, seguido de éxito funcional; asignación exacta no auditada |
| Cambio de estilo por Next (dos veces) | **Confirmado por el usuario en PIE** |
| Previous / selección de índice / gamepad para alternar | Pendiente |
| Fallo inicial `success=false, storedCycleIndex=-1` | Reproducido verbalmente y luego superado funcionalmente; **causa raíz estricta no certificada** |
| Regresión completa GAS + Nice Interaction **después** de cambiar presets | Pendiente de confirmación |
| UGC Dynamic Spring Arm, colisión predictiva, Focus, dithering | Pendientes |
| UGC Windows Development packaged build | **Pendiente** |
| Backup 07 de UGC | **No confirmado; no crear ni dar por hecho** |

**Siguiente paso mínimo sugerido:** comprobar `Berta.UGC.Previous` y/o el Dump de `ActivePreset`, luego verificar que Jump/Sprint/HUD/Interacción siguen funcionando mientras se alternan presets. Después revisar la ruta de colisión UGC, packaging y backup 07. No automatizar ninguna transición que todavía no se verificó.

## 9. Referencias

- [Índice de documentación oficial del proyecto](plugin-documentation-index.md).
- [Comparación técnica Black Eye vs. Ultimate Gameplay Camera](camera-systems-comparison.md).
- [Inventario de campos y valores de `DA_Camera_Medium`](ultimate-gameplay-camera-data-asset-reference.md).
- [BertaUltimateGameplayCameraExt README](https://github.com/nbertoa/ue5-bertadevkit/blob/main/BertaDevKitHost/Plugins/BertaUltimateGameplayCameraExt/README.md).
- [UGC Camera Modifiers y presets — documentación del proveedor](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/camera-modifiers-5).
- [UGC System Setup — documentación del proveedor](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/system-setup-3).
- [UGC Dynamic Spring Arm — documentación del proveedor](https://docs.superhuman.com/@aurora-devs/documentation-ultimate-gameplay-camera/new-dynamic-spring-arm-36).
