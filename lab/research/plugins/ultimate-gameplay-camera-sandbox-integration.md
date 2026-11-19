# Ultimate Gameplay Camera — integración real en el Sandbox UE 5.8

**Fecha:** 20 de octubre de 2026  
**Proyecto:** Blank C++ + Third Person Feature Pack; `L_EnvironmentTest`  
**Estado actual (actualizado 2026-10-24):** presets Medium/Near/Far y UGC Dynamic Spring Arm validados visualmente en PIE; **Camera Focus probado por el usuario en aplicación empaquetada y reportado funcionando**. Backup 07 confirmado verbalmente (checkpoint anterior a la migración del Spring Arm; ruta/nombre local sin inspeccionar). Yaw Follow aislado no validado; abstracción reversible Exploración/Focus pendiente. Las secciones anteriores describen el estado histórico y las actualizaciones posteriores prevalecen.

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

### Receta rápida para reproducir exactamente esta comparación

En `/Game/Gameplay/Camera` (ruta de trabajo propuesta), crear `DA_Camera_Medium` como **`UGC_CameraDataAssetBase`**, duplicarlo para `DA_Camera_Near` y `DA_Camera_Far` y **modificar solamente Min/Max Arm Length y Min/Max FOV** de la tabla anterior. Conservar **Arm Range Blend Time y FOVRange Blend Time en 0.5**. No alterar offsets, colisión, Focus ni movimiento para esta comparación.

Configurar el **hijo de `BP_UGCCameraManager`** usado en el PlayerController existente con **`Default Camera Data = DA_Camera_Medium`**, y este orden de `CameraPresets` en `BertaUGCCameraCycleComponent`:

```text
CameraPresets[0] = DA_Camera_Medium  ← mismo asset inicial del Camera Manager
CameraPresets[1] = DA_Camera_Near
CameraPresets[2] = DA_Camera_Far
```

Prueba desde consola durante PIE:

```text
Berta.UGC.Dump        # comprobar Manager / ActivePreset / CycleActiveIndex
Berta.UGC.Next        # Medium -> Near
Berta.UGC.Next        # Near -> Far
Berta.UGC.Previous    # Far -> Near (aún pendiente de confirmación explícita)
```

**Evidencia:** dos cambios por Next fueron confirmados visualmente. El orden concreto de las transiciones es el **esperado para la configuración propuesta**, no una captura del log final. Los valores Near y Far son un **ejemplo de tuning propuesto**, no un preset oficial ni mediciones del POV final. Este ejemplo también figura en la [documentación MkDocs de BertaUltimateGameplayCameraExt](https://nbertoa.github.io/ue5-bertadevkit/bertaultimategameplaycameraext/).

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

---

## Update — successful packaged build and next checkpoint (2026-10-20)

The user confirmed that **packaging completed and the packaged application works correctly** after the two successful camera-cycle transitions in PIE. This supersedes this document's earlier **UGC packaging pending** entries; those older entries are preserved as historical descriptions of what was pending at the time. Build variant was not supplied, and we have no full packaged-build feature checklist or saved logs.

**Recommended backup:** create a new complete-project restore point **`07_UGC_CameraPresets_Validated`** alongside `06_NiceInteraction_Validated`, not overwriting an earlier snapshot. **Not yet confirmed as created.** The user's local absolute backup path was never inspected.

**Next technical experiments to discuss:** enable and test current `BP_UGCSpringArmComponent` collision in a controlled obstacle course; map GAS Sprint state to UGC presets without fighting manual cycling and with correct teardown; independently validate Focus/lock-on and return to player look. In BertaDevKit prioritize auditing `Default Camera Data` against `CameraPresets` and improving the diagnostic reason for `success=false`, rather than reimplementing native UGC camera modifiers. These are candidates only.

**Postscript:** the Dynamic Spring Arm experiment was subsequently carried out in PIE; see the later migration section below. The packaged success referred to in this older entry predates this migration.


---

## Update — Dynamic Spring Arm migration and predictive collision (2026-10-20)

**Validation scope:** steps below were carried out **in PIE after checkpoint 07 and the earlier packaged UGC baseline**. The user has **not yet reported a new packaged-build verification after replacing the Spring Arm**. This section supersedes the older **"not yet inspected/replaced"** status of the default Character Spring Arm and the "collision predictive pending" notes above; those were accurate *at the time* and are retained as history.

### A. Before the change — confirmed original component

User-provided screenshot of the existing **`BP_ThirdPersonCharacter` → `CameraBoom`** showed the native Unreal **`SpringArmComponent`**, with these visible values:

| Native CameraBoom property | Observed value |
| --- | --- |
| Target Arm Length | `400` |
| Socket Offset | `(0, 0, 0)` |
| Target Offset | `(0, 0, 0)` |
| Use Pawn Control Rotation | Enabled |
| Inherit Pitch / Yaw / Roll | Enabled / Enabled / Enabled |
| Do Collision Test | Enabled |
| Probe Size | `12` |
| Probe Channel | `Camera` |
| Enable Camera Lag | Disabled |
| Enable Camera Rotation Lag | Disabled |

The user's `FollowCamera` originally had **Parent Socket = `SpringEndpoint`** under `CameraBoom`; this socket is critical and must be explicitly preserved when reparenting.

**Important:** `Target Arm Length = 400` on the component is the editable baseline in the Blueprint, **not a measurement of final runtime camera distance**; UGC Camera Data Assets can drive distances/FOV through the active manager.

### B. How the Spring Arm was migrated — executed steps

1. In the existing `BP_ThirdPersonCharacter` (parent still `GSCModularCharacter`), **Add Component → `BP_UGCSpringArmComponent`**, from the licensed UGC plugin.
2. Initially **retain `CameraBoom` and `FollowCamera`** to make the migration reversible; the new UGC Spring Arm was placed under the same parent (`CapsuleComponent`), not attached to `CameraBoom`.
3. Copy/replicate the important baseline properties from the native Spring Arm to the new UGC Spring Arm: Arm Length 400, offsets zero, Use Pawn Control Rotation true, Inherit Pitch/Yaw/Roll true, Do Collision Test true, Probe Size 12, Probe Channel Camera, both Camera Lags disabled. **Leave UGC-specific predictive-collision settings at their initial values** for the first experiment.
4. **Move `FollowCamera`** from the old `CameraBoom` to the **new UGC Spring Arm**. Crucially, in `FollowCamera` Details choose **Parent Socket = `SpringEndpoint`** *again*, even though this was already selected under the old arm; check its **relative Location/Rotation = `(0,0,0)`**. The socket name was explicitly confirmed in the user discussion.
5. Compile and save.

Temporary topology during migration:

```text
BP_ThirdPersonCharacter
└── CapsuleComponent
    ├── CameraBoom                      (native SpringArm; old)
    └── BP_UGCSpringArmComponent        (UGC Spring Arm; new)
        └── FollowCamera                (Parent Socket = SpringEndpoint)
```

### C. Issue discovered: two simultaneous Spring Arms

**Actual observation:** with both arms present and `FollowCamera` attached to the new one, the user reported that the framing suddenly looked **more centered on the character** and that cycling Near/Medium/Far changed distance **very little** compared to before. No `Berta.UGC.Dump` output identifying UGC's `GetOwnerSpringArmComponent()` was captured at this point.

**Reasoned hypothesis, not a proved internal cause:** the UGC manager might still have found/manipulated `CameraBoom` (the old native arm), or multiple arm references might have made UGC's applied offsets/ranges inconsistent with the actual camera attachment. It would be an overclaim to assert a specific internal selection algorithm without a before/after Dump or source-level runtime trace.

**What actually fixed the visible symptoms:** the **user deleted `CameraBoom`**, leaving **only `BP_UGCSpringArmComponent`** with `FollowCamera` under `SpringEndpoint`. The user then reported **"funciona bien"**; camera framing and preset differences became correct again.

Final intended component topology:

```text
BP_ThirdPersonCharacter  (GSCModularCharacter)
└── CapsuleComponent
    └── BP_UGCSpringArmComponent      (only Spring Arm)
        └── FollowCamera
            Parent Socket: SpringEndpoint
            Relative Location: (0, 0, 0)
            Relative Rotation: (0, 0, 0)
```

**Reproducible operational rule for this tested Sandbox configuration:** **do not leave the old `CameraBoom` in place after the migration.** This project was visually incorrect with two Spring Arms and improved after deleting the old one. Do not generalize that two Spring Arms are *always* invalid in every Unreal Pawn without further evidence. Preserve the original baseline in **backup 07**, rather than recreating the obsolete boom inside the final Character.

### D. PIE smoke test performed — what passed and what is still inferred

After the user removed `CameraBoom`:
- **General camera and existing preset switching:** user reported the camera **works well** again.
- **Obstacle response:** user moved near a wall and rotated the view; observed **smoother/interpolated movement** while collision adjusted the camera. Confirmed perceptually in PIE; no numeric timing captured.
- **Predictive corner avoidance:** user repeated a slow camera rotation near the **corner of a wall** and explicitly reported **camera anticipation of the corner**. This is good **visual functional evidence** for UGC's predictive avoidance in the configured setup, but is not a technical trace of specific feeler sweeps or a performance benchmark.
- No additional manual changes to predictive feeler settings were reported; leave default settings as tested.
- **Not yet demonstrated:** an instrumented trace of feelers (`TraceTag CameraPen`), exhaustive concave/convex corners, occlusion materials/dithering, 30/60/120 FPS benchmark, gamepad regression after the change, or the **packaged application rebuilt after the Spring Arm replacement**.

### E. Recommended checklist to reproduce in another project

1. Validate a working UGC Player Camera Manager and cycle (initial `Default Camera Data` must be one of the assets listed in `CameraPresets`); see section 5 above.
2. Inspect/document the actual native `CameraBoom` settings and the `FollowCamera` parent socket.
3. Add `BP_UGCSpringArmComponent` as a new component alongside the old one for the short migration phase, preserving rotation/collision settings and the UGC-specific defaults.
4. Reparent the existing `FollowCamera` to the new component, explicitly restoring **`Parent Socket = SpringEndpoint`** and relative Location/Rotation `(0,0,0)`.
5. **Remove old `CameraBoom` before accepting the migration**. Verify the Character has **one effective Spring Arm** supporting the camera; if a native inherited component cannot be deleted from a Blueprint, stop and inspect the parent-class ownership rather than creating a competing camera setup.
6. Compile/save. Test Near/Medium/Far distances and vertical composition, collision against a wall and anticipation at a corner; record evidence with `Berta.UGC.Dump` if helpful.
7. Perform post-change regressions of Jump/Sprint/stamina, Nice Interaction E/Square, mouse/gamepad Look and UI; then **repackage Windows** to validate the change before creating any new packaged checkpoint. Existing checkpoint 07 is the *pre-SpringArm-migration* baseline.

**BertaDevKit documentation:** [`BertaUltimateGameplayCameraExt` MkDocs](https://github.com/nbertoa/ue5-bertadevkit/blob/main/docs/bertaultimategameplaycameraext/index.md) contains the portable setup notes. These steps configure the **vendor's native Spring Arm Blueprint**; they are **not** evidence of a new BertaDevKit runtime camera feature or a change to the extension's C++ code.


---

## Update — Camera Focus y Yaw/Pitch Follow, packaged smoke test (2026-10-24)

**Proveniencia:** investigación reproducida en conversaciones 20–24 octubre 2026, capturas de Blueprints UGC 1.92/UE 5.8, e informe de viabilidad local aportado por Codex. No se recibieron archivos `.uasset` ni logs completos del Sandbox. Distinguir lo observado de lo propuesto; no atribuir nuevas funciones a BertaDevKit.

### A. Pruebas de Yaw Movement Follow (sin validación)

- Captura del `BP_SandboxUGCCameraManager` (hijo de `BP_UGCCameraManager`): array Default Modifiers de 13 elementos, incluyendo `UGC_YawFollowModifier`, `UGC_PitchFollowModifier`, `UGC_FocusCameraModifier`; distinto de `UGC_PitchToArmLengthAndFOVModifier`.
- CharacterMovement: `Orient Rotation to Movement=true`, `Use Controller Desired Rotation=false`; Pawn `Use Controller Rotation Yaw=false`.
- El usuario ensayó A/D y desplazamiento diagonal sin mover Look durante varios segundos; **no observó giro automático de cámara**. Activó Debug de Yaw, pero no obtuvo salida visible. Cambiar `Yaw Follow Time Threshold` de `2.0` a `0.0` no resolvió; se recomendó restaurar 2.0, pero no se inspeccionó su valor final.
- `Berta.UGC.Dump` reportó `UGC=11 enabled / 0 disabled` y `External=3 enabled / 0 disabled` (contadores agregados; no demuestran cada modifier). **Yaw Follow automático permanece sin validar**.

### B. Ejemplo de Focus y target

`Camera Focus → Focus Target Method` ofrecía `None`, `BP_FocusTargetMethod_Test1`, `BP_FocusTargetMethod_Test2`. En la captura de `BP_FocusTargetMethod_Test1 → GetTargetLocation`, el Blueprint hace `Get All Actors with Tag` (`FocusTest1`), comprueba que el array no esté vacío, toma `GET 0`, obtiene la localización del actor y devuelve sus X/Y con **Out Target Location Z = 0.0 explícito** y el actor como `Return Value`. El comentario del proveedor indica **demo only**, y que una solución real pasaría la localización del objetivo para hard-lock/aim assist/conversaciones. Se propuso asignar **Actor Tag** `FocusTest1` a un cubo del Sandbox; no se confirmó qué target exacto se utilizó en el packaged test final.

### C. Causa exacta del bloqueo Focus vs Follow

Advertencia real de UGC: `Trying to focus the camera on a target while yaw/pitch follow modifiers are enabled. This is not allowed.`

Captura de `UGC_FocusCameraModifier`: condición **`IsYawMovementFollowEnabled()` OR `IsPitchMovementFollowEnabled()`**. Las dos funciones del manager inspeccionadas leen **FollowSettings de las instancias internas**, no el enabled del `UCameraModifier`:

| Consulta de UGC | Retorna true si… |
| --- | --- |
| `IsYawMovementFollowEnabled()` | `Enable Yaw Movement Follow` |
| `IsPitchMovementFollowEnabled()` | `Enable Pitch Movement Follow` **OR** `Trigger when Falling` **OR** `Trigger on Slopes` |

**Hallazgo crítico:** poner a false solo el flag principal de Pitch no desactiva Pitch Follow para el chequeo de Focus; hay otros dos booleanos. `Disable Modifier` y `Remove Camera Modifier` (este último devolvió true) **no resolvieron** el cartel; tampoco alcanzó con desactivar solo los principales desde Data Asset/`Set Settings`. Un `Set Settings` en Pitch podía hacer que una consulta inmediata devolviera false para el flag editado, pero Focus continuaba viendo otros flags Pitch activos. Al desactivar las **cuatro condiciones** efectivas desde Blueprint, el usuario logró hacer funcionar Focus. No afirmar que `Remove Camera Modifier` falló como API, solo que no cumplió el chequeo de Focus.

### D. Solución nativa descubierta: `ToggleYawAndPitchFollow`

En la categoría **Camera Follow** del **`BP_UGCCameraManager` original** el usuario encontró la función Blueprint heredada `ToggleYawAndPitchFollow`. La captura muestra `Sequence`, validación de referencias `UGCPitchFollowModifier`/`UGCYawFollowModifier`, y `Set Members in Camera Pitch Follow Settings` / `Set Members in Camera Yaw Follow Settings` **sobre las settings efectivas**.

Sus **cuatro entradas exactas**, observadas en el graph:

1. `Enable Pitch Movement Follow`
2. `Trigger Pitch Follow when Falling`
3. `Trigger Pitch Follow on Slopes`
4. `Enable Yaw Movement Follow`

**No es un toggle automático pese al nombre:** aplica los booleanos provistos. La llamada `ToggleYawAndPitchFollow(false, false, false, false)` suspende las cuatro condiciones necesarias para evitar el conflicto. **No guarda snapshot ni restaura** los valores previos por sí sola. Si se implementa Exploración ↔ Focus reversible, guardar la combinación original ANTES de la primera suspensión y restaurarla exactamente; las suspensiones repetidas no deben reemplazar el snapshot con cuatro `false`.

Se discutieron funciones propias `SetPitchFollowSuspended`, `SuspendMovementFollow`, `RestoreMovementFollow`, `SetFocusActive`, `SetCameraMode` y un bool `bPitchFollowSuspended`, con imagen instructiva. **Su existencia/validación final en el Sandbox no se confirmó**. Tampoco se comprobó la implementación de un Data Asset exclusivo de Focus ni binding R3; no anunciar un sistema completo de lock-on.

### E. Límite de `BertaUltimateGameplayCameraExt` C++ (informe de Codex)

El usuario aportó control de viabilidad sobre **UGC 1.92 / UE 5.8**. En `Camera/Data/UGC_CameraData.h` existen `FCameraYawFollowSettings::bEnableYawMovementFollow` y `FCameraPitchFollowSettings::{bEnablePitchMovementFollow,bTriggerWhenFalling,bTriggerOnSlopes}`, pero son settings de Camera Data o estructuras, no accessors C++ tipados a **copias efectivas de modifiers Blueprint**. `UUGC_CameraModifier` no expone esos getters/setters; `AUGC_PlayerCameraManager` ofrece `GetCurrentCameraDataAsset`, `PushCameraData`, `PopCameraDataHead`, `FindCameraModifierOfClass`, y UE `ForEachCameraModifier` permite enumeración, pero no acceso tipado a settings internos. Las funciones `SetYawMovementFollowSettings`, `SetPitchMovementFollowSettings`, `IsYawMovementFollowEnabled`, `IsPitchMovementFollowEnabled` viven en Blueprint manager, y `OnCameraDataStackChanged` es `BlueprintNativeEvent`, no multicast suscribible para el componente propuesto.

Codex **detuvo la implementación por contrato de viabilidad**: sin componente C++ nuevo, bridge externo, cambios a repo, build ni tests para esta feature. Se prefiere consumir `ToggleYawAndPitchFollow` desde el Blueprint heredado del Sandbox antes de construir un bridge frágil. No presentar funcionalidad runtime Follow como añadida a BertaDevKit.

### F. Confirmación nueva: Focus en packaged build

**2026-10-24: el usuario confirmó explícitamente «Ya probé en packaging el focus y funciona».** Registrar **UGC Camera Focus: smoke test exitoso en aplicación empaquetada, según usuario**. Esta declaración reemplaza el histórico «Focus pendiente», pero no detalla plataforma/configuración exacta de ese build, target method final, input, el mecanismo exacto de activación, logging, ni si el retorno al modo exploración con restauración de settings quedó validado. Tampoco constituye por sí sola prueba de colisión predictiva, regresión GAS/Interacción o ciclo de presets en ese build. Backup 07 fue creado antes de reemplazar el Spring Arm; **no se informó nuevo backup** tras los últimos experimentos.

### G. Estado y próximos gates

- **Confirmado:** presets Near/Medium/Far en PIE; Spring Arm UGC con respuesta visual a pared/esquina en PIE; lógica de validación Yaw/Pitch conocida; `ToggleYawAndPitchFollow` disponible en Blueprint UGC; Focus **funciona en packaging según el usuario**.
- **No demostrado:** Yaw auto-follow; suspensión/restauración idéntica repetible; `SetCameraMode` completo o input target-lock; cambio de preset durante Focus; regreso a Look normal; automatización; nueva compilación o tests de BertaDevKit; nueva copia de respaldo.
- **Próxima prueba aconsejada:** en `BP_SandboxUGCCameraManager`, usar la función heredada para apagar cuatro flags, activar/desactivar Focus y restaurar los cuatro originales; comprobar con `IsYawMovementFollowEnabled` / `IsPitchMovementFollowEnabled`. Luego revisar interacción con ciclo Medium/Near/Far, GAS, Nice Interaction, mouse/gamepad, Spring Arm y un packaged build que ejerza específicamente la transición Focus ON/OFF. No editar los Blueprints propietarios ni inventar API C++.


---

## Update — reviewed official UGC PDF and confirmed Blueprint helpers (2026-10-26)

El usuario agregó a las fuentes del proyecto ChatGPT el manual combinado **`Ultimate Gameplay Camera Documentation.pdf`**, 65 páginas, elaborado mediante exportación de páginas oficiales Aurora Devs; se revisaron texto completo y capturas principales. **Fuente no incluida en GitHub público** por su licencia. Lectura original por secciones, nombres de los nodos, matriz, prioridades y límites: [Ultimate Gameplay Camera — manual y feature matrix](ultimate-gameplay-camera-feature-matrix.md).

**Revisión importante del diseño de Focus:** la captura oficial de p. 20 documenta los nodos de **BP_UGCCameraManager** `Set Camera Focus Settings`, **`Toggle Camera Focus`** (bool Enabled) y **`Set Camera Focus Targeting Method`** (Focus Method Class, Targeting Method Ref). No crear funciones propias para replicar el toggle ni asumir que Focus requiere siempre un Data Asset extra. La clase target-method debe **heredar de `UGC_IFocusTargetMethod`**, sobrescribir `GetTargetLocation` y **llamar al Parent: Get Target Location** (p. 22). El método retorna ubicación y opcionalmente actor enfocado, que se usa para line of sight. Esto permite definir un objetivo dinámico **si el proyecto entrega explícitamente su actor a la estrategia**, paso aún no implementado/probado: el PDF no describe un setter directo de `AActor*` en `BP_UGCCameraManager`. Probar dos actores y cambio de foco sin `GetAllActorsWithTag` antes de diseñar un sistema de lock-on completo.

**Otro hallazgo:** FAQ p. 57 contiene el nodo Blueprint real `Toggle All UGCModifiers Debug` (bool Enabled) en el manager, distinto del nombre de debug que buscamos erróneamente en una sesión anterior. No confundir el `Berta.UGC.Dump` C++ (agregado) con el debug detallado del proveedor.

**Cobertura incompleta:** el PDF no incluye el tutorial de System Setup ni la guía detallada del Dynamic Spring Arm; de este último hay una validación PIE del Sandbox independiente. Videos embebidos no preservados.

**Continuidad:** la validación de Focus en packaged reportada el 24-10 sigue vigente. No agregar validaciones de Focus dinámico, regreso completo a exploración, packaged predictive collision, cambios al plugin BertaDevKit o nuevos backups que el usuario no confirmó.

## Update — Camera Focus Target Lock funcional con GAS Companion y Berta ActorTag (2026-10-26)

**User-confirmed en sesión actual:** el usuario terminó la integración de selección de objetivo dinámica en el Sandbox y reportó «Todo funciona». El prototipo implementado difiere de nuestra propuesta de Ability persistente: **`GA_CameraFocusToggle` se activa por pulsación, toma una decisión de target y termina**, mientras UGC/Character conservan el estado de Focus entre pulsaciones.

**Input:** `IA_FocusToggle` mapeada a Middle Mouse Button y R3 del gamepad dentro de Input Mapping Context existente, conectada vía flujo GAS Companion a la Gameplay Ability. La Ability fue configurada por el usuario con tag de identidad (no se reconfirmó aquí el nombre exacto que eligió para Asset Tags; no inventarlo).

**Detección:** el usuario probó `Multi Sphere Trace By Channel` y después prefirió **`Sphere Overlap Actors` + comparación angular `Dot Product`** para priorizar el actor más centrado en dirección de visión. Los candidatos se filtran mediante la nueva etiqueta exacta **`Berta.ActorTag.Targeting.Focusable`** introducida en el **plugin base** BertaDevKit y aplicada manualmente a `Actor → Tags`; la comparación nativa puede ser `FGameplayTag.GetTagName() → Actor Has Tag`. **No son Gameplay Tags almacenados en el actor**, ni se necesita ASC/interfaz/componente en el target. El usuario confirmó haber marcado actores y haber configurado la Ability para seleccionar un candidato válido; no hay un grafo final publicado, por lo que orden/radio/canal/valor exacto de umbral **no están independientemente inspeccionados**.

**Toggle final implementado, según descripción del usuario:**

| Objetivo actual | Resultado de nueva búsqueda | Acción |
| --- | --- | --- |
| Ninguno | A | Activar Focus en A |
| A | Ninguno | Quitar Focus anterior |
| A | A | Quitar Focus anterior |
| A | B, diferente | Cambiar Focus a B |

El usuario dijo además que **funcionan** el regreso a cámara normal/restauración de Yaw/Pitch Follow, la pérdida o destrucción de objetivo y el cambio entre objetivos sin comportamiento problemático, respondiendo al checklist de revisión. Esto se registra **como validación funcional informada por el usuario**, no como auditoría de nodos/código, traza de logs ni prueba de red.

**UGC:** integración apoyada en la clase propia **`BP_SandboxFocusTargetMethod`** derivada de `UGC_IFocusTargetMethod` y los métodos del vendor Camera Manager para activar/desactivar Focus; `GetTargetLocation` recibe `In Owner = BP_ThirdPersonCharacter` (observado por el usuario en breakpoint). El sistema aprovecha las funciones nativas `Toggle Camera Focus` y `ToggleYawAndPitchFollow`; este último necesita cuatro flags `false` durante Focus y restauración de originales para evitar incompatibilidad. No afirmar que existe extensión C++ nueva ni copiar al repositorio fuente comercial de UGC.

**Alcance del hito:** funcional en el test local del usuario. **No se confirmó todavía un nuevo packaged build con `GA_CameraFocusToggle`, ni un nuevo backup posterior al checkpoint 07**. El packaged Focus anterior de 2026-10-24 correspondía a una etapa previa y **no verifica** automáticamente este Target Lock nuevo. **Próximo gate:** probar package Windows Development la secuencia A → None, A → A, A → B, target destruido y restauración de Follow, y después tomar nuevo restore point.

## Update — Target Lock empaquetado confirmado (2026-10-26)

**User-confirmed:** después de solicitar expresamente la **prueba del nuevo Target Lock en la build empaquetada**, el usuario respondió «funciona». Por el contexto inmediato, esto confirma **smoke test exitoso en build empaquetada del flujo nuevo `GA_CameraFocusToggle`** y supera el estado «packaging pendiente» de la sección anterior. La funcionalidad incluye Focus dinámico implementado con GAS Companion, Actor Tags de BertaDevKit y el Camera Manager de UGC. No se aportaron logs, versión exacta del paquete ni checklist con el resultado individual de cada caso (A→None, A→A, A→B, target destruido); conservar el detalle de cobertura de PIE/local reportado previamente, sin inventar una batería detallada de packaged tests.

**Pendiente:** **nuevo backup/restore point posterior al checkpoint 07**. No se confirmó haberlo creado. No se modificó el código de BertaDevKit por esta actualización documental.
