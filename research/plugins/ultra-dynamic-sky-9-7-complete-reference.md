# Ultra Dynamic Sky 9.7 — manual completo: índice técnico y decisiones de R&D

**Fuente primaria:** PDF del usuario, `Ultra Dynamic Sky 9.7 Documentation.pdf`, **139 páginas**, título interno «Ultra Dynamic Sky — Version 9.7 Documentation», fechado/exportado el 26-10-2026. **Revisado completo** (texto de las 139 páginas y repaso de su composición visual). El documento incluye **Ultra Dynamic Sky (UDS)**, **Ultra Dynamic Weather (UDW)**, **Common Issues** y **Technical Notes**. Los enlaces a videos incrustados aparecen como tarjetas/capturas; su contenido audiovisual no forma parte de las páginas exportadas y **no se examinó como video**.

**No almacenar el PDF de terceros en este repositorio público**. Este archivo es una síntesis técnica con referencias a páginas, no una reproducción del manual ni una prueba de que el UDS instalado en el Sandbox sea 9.7. La versión **del documento** es 9.7; la versión **instalada** sigue sin verificarse desde Unreal. Ante nombres distintos, prevalecen el panel Details/tooltips y el botón `UDS Documentation` de la instancia real.

**Caso de uso actual:** [dos niveles estáticos: atardecer y noche clara](../../projects/uds-sunset-clear-night.md). **Estado:** investigación documental completa; ajustes en Unreal y benchmarks pendientes. Cualquier «configuración recomendada» en estas notas es una **hipótesis de look**, no un preset numérico del fabricante.

## 1. Mapa íntegro de contenidos (páginas del PDF, 1-based)

| Bloque | Páginas | Qué cubre |
|---|---:|---|
| Sky / Setup y Basic Controls | 1–5 | Instalación en nivel, siete Sky Modes, dos Color Modes, tres Project Modes y Feature Level |
| Clouds | 6–10 | Volumetric, 2D Dynamic, Static, Voxel, capas extra, movimiento, Cloud Wisps, authoring |
| Lighting | 10–14 | Componentes de luz, Sun/Moon, Cloud Shadows, Sky Light, Exposure, estáticas |
| Time | 14–17 | Time of Day, Animate Time, funciones y widgets |
| Sun and Moon | 17–20 | Trayectoria, manual targets, sol, fases lunares |
| Fog | 21–23 | Densidad, color, volumétrica, material global y polvo |
| Sky Color / Simulation | 24–27 | Rayleigh/Absorption, Simplified Color, sol/luna/estrellas reales, coordenadas y fecha |
| Cinematics | 27–31 | Sequencer, Movie Render Queue, Path Tracer, loop de nubes |
| Level integration | 31–35 | Lights Day/Night Toggle, eventos horarios, material functions |
| Night Sky | 35–38 | Stars, Aurora, Sky Glow, Light Pollution, Space Layer |
| Tools / Additional Sky Features | 39–52 | Configuration Manager, modifiers, save state, lens flare, interiores, cloud painter, rayos, agua, PP, audio, widgets |
| Performance / Maintenance | 52–58 | Modos GPU, escalabilidad, mobile, upgrades y herencia de Blueprints |
| Weather setup/state | 58–71 | UDW, presets, cambio/clima aleatorio, estaciones, regiones, tormentas |
| Weather effects | 72–77 | Partículas, colisiones, spawning, rayos y triggers |
| Material effects | 77–95 | Wetness, snow, dust, DLWE V3, foliage, vidrio, viento, máscaras |
| Sound & sampling | 95–103 | Audio, oclusión, MetaSound, weather/temp, eventos, Actor Weather Status |
| Wind & cinematic weather | 103–108 | Wind, gusts, debris, physics, camera, Sequencer |
| Additional Weather | 108–120 | Puddles, nieve spline, agua, wind fog, arcoíris, gotas, frost, calor, vehículos |
| Common Issues | 120–130 | Lighting/exposure, skylight, cloud/render artifacts, Niagara, VR, colisiones |
| Technical Notes | 130–139 | Arquitectura de Blueprints UDS/UDW, caches, actualización, Niagara, audio, réplica |

## 2. Setup y controles base — p. 1–6

- El fabricante indica remover las luces y actores de cielo previos: `DirectionalLight`, `SkyLight`, `ExponentialHeightFog`, `SkyAtmosphere`, `VolumetricCloud`; colocar `Ultra_Dynamic_Sky` preferentemente a **altura de suelo**. Existe conversión asistida si se coloca UDS antes de retirar un setup simple. Si persiste iluminación baked, reconstruir lighting.
- `Basic Controls → Time of Day` es el control horario; `Time of Day String` acepta formatos como `3PM` y `15:45`. La API de lectura `Get Time of Day` expone una escala **0–2400**, no 0–24.
- `Cloud Coverage` y `Fog` controlan el estado meteorológico básico en UDS **si UDW no está presente**. Con UDW, su `Weather State` puede tomar control de estos valores.
- Para art direction global: `Saturation`, `Contrast`, `Overall Intensity`, `Night Brightness`, `Lighting Brightness` separadas para **Day**, **Dawn/Dusk**, **Night**.
- `Dusk Time` y `Dawn Time` especifican en el modo básico el cruce del horizonte. **Son ignorados si `Simulate Real Sun` está activado.**
- `Sky Mode` 9.7 tiene **siete** opciones: `Volumetric Clouds`, `Static Clouds`, `2D Dynamic Clouds`, `No Clouds`, `Volumetric Aurora`, `Space`, `Voxel Clouds`. Las tres opciones ausentes de la nota preliminar eran `Volumetric Aurora`, `Space` y `Voxel Clouds`. `Space` quita nubes, atmósfera, suelo y color atmosférico y **no** corresponde a una noche terrestre.
- `Color Mode`: `Sky Atmosphere` (scattering físico del motor) o `Simplified Color` (LUT y valores más editables, menor costo).
- `Project Mode`: `Game / Real-time (Non VR)` para prototipos jugables; `Cinematic / Offline` **solo** para render offline, aplica calidad extra y puede degradar severamente el rendimiento interactivo; `Virtual Reality` si hay estéreo.
- `Feature Level` debe coincidir con el target de rendering; opciones mobile de fallback y mapeo por platform name.

## 3. Cloud shaping y composición — p. 6–10 y 46–49

- `Volumetric Cloud Rendering Mode` es la exposición del cvar `r.volumetricrendertarget.mode`; se puede priorizar performance, definición o intersecciones con meshes. `View Sample Scale` y `Shadow Sample Scale` cambian costo/ruido.
- `Bottom Altitude` cambia solo altura de nubes; **no elevar el actor completo** para lograrlo. `Volumetric Clouds Scale` escala campo. `Cloud Profile Authoring Tool` permite generar la LUT de perfil de formas; `Volumetric Cloud Painter` pinta áreas locales de cobertura sobre el valor global.
- `Additional Cloud Layers`: capas extra arriba/abajo del campo principal, con `Inherited Cloud Coverage` y `Independent Cloud Coverage`; **solo la capa principal proyecta cloud shadows**. Más capas aumentan GPU cost.
- `Static Clouds` usa una textura barata con iluminación aproximada; `Static Clouds Authoring Tool` permite capturar volumétricas a textura. `2D Dynamic Clouds` son materiales en esfera, sin geometría de nubes 3D. `Voxel Clouds` comparte gran parte del setup volumétrico.
- `Cloud Movement`: `Cloud Speed`, `Cloud Direction`, `Cloud Phase`, `Randomize Cloud Formation On Run`, `Clouds Move With Time of Day`. Para una formación fija reproducible: speed 0, desactivar randomización y movimiento por hora, **ajustar Cloud Phase** hasta encontrar la composición.
- `Cloud Wisps`: velo de nubes altas que puede seguir visible **en todos los Sky Modes salvo Space**. Una noche con `No Clouds` aún puede tener wisps: verificar `Cloud Wisps Opacity`, incluida la opacidad para baja/alta coverage, antes de declarar «completamente despejada».
- `Volumetric Cloud Painter`: contenedor `UDS Cloud Paint Container`; `Save to Textures` es imprescindible para persistir las celdas a `UDS_LevelData` junto al mapa. Activar/desactivar containers en runtime es posible; no hace falta para dos vistas fijas.
- `Volumetric Cloud Light Rays` (p.49): efecto de rayos con **tarjetas aditivas Niagara** por aberturas de nubes pintadas, distinto del fog volumétrico y del screen-space bloom. `Individual Cloud Light Rays` aumenta potencialmente el número de ray cards y el costo.

## 4. Luz y exposición — p. 10–14

- Usar **componentes internos** `Sun`, `Moon`, `Sky Light` por defecto. Actores de luz externos solo si alguna integración lo exige. En esos casos copiar `Cloud Scattered Luminance Scale` en directional actors y alinear `Source Type` del skylight actor con modo UDS.
- `Cloud Shadows` existen incluso en `No Clouds`: **no asumir que desaparecen automáticamente** con ese Sky Mode. En volumétricas reflejan la capa 3D; en otros modos usan texturas 2D. Intensidad/softness dependen de coverage, fog y dust; si hay sombras en una noche despejada, comprobar esta categoría.
- `Sky Light Mode`: `Capture Based` (habitualmente captura en tiempo real), `Custom Cubemap`, `Cubemap with Dynamic Color Tinting` (eficiente y multiplataforma). `Sky Light Color Multiplier` por hora puede **superar 1**, incrementando intensidad además de tintar.
- `Exposure → Apply Exposure Settings` maneja `Exposure Compensation Curve`, `Exposure Bias` por hora/tiempo, `Exposure Metering Mode` (incluye Manual) y `Exposure Brightness Range` (Min/Max EV100). **PPV/cámara pueden sobreescribir/conflictuar**: diagnosticar en `Show → Visualize → HDR (Eye Adaptation)` y viewport `Game Settings` activo.
- Sun/Moon/SkyLight mobility debe modificarse mediante valores expuestos en Details de UDS. Un sol static/stationary impide cambiar Time of Day en runtime; los dos niveles estáticos no necesitan esto, pero el baseline es movable.

## 5. Hora, órbitas y simulación — p. 14–27

- Para cada mapa de look fijo: `Animate Time of Day` **apagado**; `Simulation → Use System Time` **apagado**. `Time of Day String` es buen input inicial; el resultado efectivo depende del horizonte, la simulación, `Dusk Time` y las orientaciones.
- Sin simulación real: `Sun Yaw`, `Sun Pitch`, `Sun Vertical Offset`; análogos de Moon, más `Moon Orbit Offset`. Al activar `Simulate Real Sun`/`Moon`, esos controles de trayectoria quedan reemplazados; `Manually Position Sun/Moon Target` **tiene prioridad sobre la simulación**.
- El widget de dirección manual aparece como **diamante azul** sobre el actor seleccionado; dirección = vector desde UDS al target. Útil si la composición requiere sol/luna en lugar exacto, pero preferir hora real en primera pasada para mantener consistencia.
- `Sun Scale` determina también el tamaño angular usado para directional shadows/reflejos, con `Sun Source Angle Scale`; hay `Sun Softness`, Color e Intensity; `Sun Lens Flare` es efecto propio del paquete (p.44).
- `Moon Phase`, `Render Moon Phases`, `Moon Texture`, `Moon Phase Normal`. `Simulate Real Moon` dirige fase y posición; se puede deshabilitar fases si la textura no las necesita.
- `Simulate Real Sun`, `Simulate Real Moon` y `Simulate Real Stars` son independientes. `Simulate Real Stars` cambia textura de estrellas tiled por **mapa panorámico 360° de mayor resolución**. Simulación utiliza latitud/longitud, UTC, DST, año/mes/día y `North Yaw`; no habilitar simulación astronómica sin requerimiento.
- Con Simulate Real Sun, velocidad del ciclo depende de `Simulation Speed`, no de `Day Length`/`Night Length`. `Use System Time` usa UTC del reloj y el Time Zone, aun sin el resto de opciones de simulación.
- Funciones nativas: `Set Time with Time Code`, `Set Date and Time`, `Set Time of Day With String`, `Transition Time of Day`, `Get Time of Day`, `Get Time Code`, `Get Date Time`, `Is It Daytime?`, widgets de reloj.
- Dispatchers: Sunrise, Sunset, Midnight, Hourly, Current Hour Changed, Every Minute, Custom Time; distinguir eventos al saltar horas, offset de sunrise/sunset y su `Time Event Dispatcher Check Period`. `Light Day/Night Toggle` y material functions `Day to Night Float/Color`, `Active Sun or Moon Vector` ya existen (p.31–35).

## 6. Color del cielo, cielo nocturno y fog — p. 21–25 y 35–38

- `Sky Atmosphere → Rayleigh Scattering`: color del cielo **independiente** para día, noche y dawn/dusk; `Sunset/Sunrise` y `Twilight` controlan absorción para reforzar color de atardecer/twilight. `Sky Atmosphere Overcast Luminance` afecta cielos muy nubosos.
- `Simplified Color`: variables de cielo y de 2D/Static Clouds, `Directional Intensity` curve, `Directional Light Scattering` curve y LUT de sol/luna. **Colores de nubes volumétricas** se ajustan en `Volumetric Clouds`, no en los ajustes de 2D/Static del modo simplificado.
- `Fog Density`: base density + aportes de `Cloud Coverage`, `Fog`, `Dust Amount`; height falloff base y variantes cloudy/foggy/dusty; start distance depende de density y umbrales. Evitar trasladar al cielo abierto la densidad duplicada para test indoor.
- `Fog Color`: el project setting `Support Sky Atmosphere Affecting Height Fog`, habitualmente ON, permite que la atmósfera gobierne gran parte del color de fog. Si se necesita control directo siempre, puede desactivarse **con una justificación**. Con `Simplified Color` todas las propiedades Fog Color se aplican.
- `Volumetric Fog`: extinction/distancia y `Sun/Moon Volumetric Scattering Intensity`; `Global Volumetric Material` admite ruido 3D, ground fog con mesh distance fields, weather masks y subacuático. `Dust` puede modificar color/densidad y lo controla UDW si existe.
- `Stars` (p.35–36): `Tiling Stars`, `Real Stars`, color, intensity, atmospheric twinkling. Una cantidad de estrellas realistas visualmente se logra equilibrando su intensidad contra `Exposure`, sky glow y luna, no simplemente maximizando un único valor.
- `Sky Glow` (p.36–37): `Night Sky Glow` agrega iluminación de fondo uniforme y algo de luz a las nubes desde arriba; `Light Pollution` simula iluminación urbana y alumbra nubes desde abajo. Para **noche clara remota**, empezar con Light Pollution bajo, pero no inventar un valor obligatorio.
- `Aurora` 2D versus `Volumetric Aurora` (esta última reemplaza nubes 3D). `Space Layer` permite planetas, lunas y nebulosa como decals en cielo, exige **DBuffer Decals** (Mobile DBuffer si aplica). Son opciones de otros experimentos, no requerimientos de nuestros dos looks.

## 7. Herramientas nativas para reutilizar y animar — p. 27–52

- `Configuration Manager` (p.39–41), lanzable desde menú Ultra Dynamic Sky en toolbar o editor utility: guarda configuración **completa**, la aplica a actores y puede exportarse a **Project** (solo proyecto) o **Engine Content** (compartido entre proyectos de misma versión de UE). Configs anteriores pueden cargarse, pero no traen nuevos campos agregados en versiones posteriores: conviene resalvar según el caso.
- `Apply Sky Configuration` (UDS), `Apply Weather Configuration` (UDW); al aplicar ambas, **Weather primero** según el manual.
- `Configuration Overrides` en `Basic Controls → Advanced`: un actor puede tomar todos sus valores de un **config asset compartido**. **Mientras hay override activo, no se pueden editar manualmente los valores UDS/UDW del actor en Details**; quitarlo para hacer ajustes. Al iniciar runtime, aplica override y luego se pueden modificar variables normales.
- `Configuration Buttons` agrega botones editor para probar varias configs de forma inmediata sin sistemas propios.
- `Sky Modifiers` son **otra cosa distinta**: assets editados con `Sky Modifier Editor`, alteran subconjuntos float/color y PP, superponen/fadean; funciones `Add Modifier`, `Remove Modifier`, `Set Modifier State`, `Clear Modifiers`, `Set Unmodified Property Values`; hay `Time of Day Specific Modifiers`. No crear un modificador si basta con dos configs estáticas.
- `Create UDS and UDW State for Saving`/`Apply Saved UDS and UDW State` empaquetan/restauran estado de tiempo y weather, distinto de configurar **todos** los parámetros de un look.
- `Sequencer`: keyframe Time of Day con animación autónoma apagada; UDW manual state si está presente; para trayectoria reproducible de nubes: `Cloud Speed=0`, `Randomize Cloud Formation On Run=false`, `Clouds Move With Time of Day=false`, animar `Cloud Phase`. Variables no expuestas pueden requerir `Expose to Cinematics` y reapply static properties.
- `Movie Render Queue` pide `Cinematic / Offline`, volver a `Game / Real-time` tras capturar. Path Tracer usa skylight cubemap en lugar del sky nativo; opciones `Adjust for Path Tracer` y `Render Height Fog In Path Tracer Using Post Process`.
- Para loops volumétricos (p.30–31): restricciones concretas: speed 0, direction cardinal, Formation Change Speed 1, 3D Noise Scale 1, Formation Texture Scale 1, Macro Variation 0, 3D Noise Vertical Movement 0, animate Cloud Phase 0–100. **No hace falta para cielos fijos**.
- `Interior Adjustments` y `Player Occlusion` (p.45–46): basados en trazas y/o `UDS Occlusion Volume`; afectan fog/iluminación, clima, temperatura y audio. Cuidado al comparar un cielo exterior desde interior del Third Person playground.
- `Screen Space Light Shafts` es bloom basado en imagen/profundidad solo al mirar hacia fuente luminosa; **no es** volumetric fog ni `Volumetric Cloud Light Rays`.
- Agua: `Global Water Level`, water body classes, caustics, máscaras subacuáticas; post-processing array puede mezclar por hora/clima y podría cruzarse con Chameleon y PPV; el orden y prioridad visual deberán verificarse.
- `AmbientSound-Time and Weather Controlled`, `UDS Onscreen Controls` y dos clock widgets ofrecen UI/audio básicos reutilizables sin custom tooling.

## 8. Rendimiento, compatibilidad y mantenimiento — p. 52–58

- Impacto de GPU principal: `Sky Mode`. `Volumetric Clouds` más costoso; `2D Dynamic Clouds` y `Static Clouds` más baratos; `No Clouds` mínimo. `Simplified Color` puede ahorrar algo de costo de sky material.
- Cloud view/shadow samples son un trade-off costo/ruido; multi-layer y two-layers suben costo; variación de alta frecuencia también. El modo cloud para buenas mesh intersections cuesta distinto del predeterminado para fondo.
- `Sky Light → Cubemap with Dynamic Color Tinting` reduce costo de captura en tiempo real. `Volumetric Fog` tiene **costo considerable**; Unreal Shadow Quality Low/Medium lo deshabilita por defecto, y esto **puede afectar** la presentación en packaged.
- `Half Rate Tick`: por defecto encima de un umbral de **45 fps**, UDS/UDW hace tick cada dos frames. Cambiarlo puede causar stutter si se baja demasiado. `Use Sky Mode Scalability Map` permite cambiar automáticamente calidad de cielos según Effects Quality. `Directional Light → Lights Update` controla frecuencia de cambios/invalidaciones.
- Mobile: `Feature Level = Mobile` y fallback compatibles, no tratar mobile como una versión igual del cielo de escritorio. VR: usar `Project Mode → Virtual Reality`.
- Actualizaciones sobrescriben assets originales; **customizaciones en child Blueprints**, no modificar clases del proveedor. Configs y Sky Modifiers nuevos suelen permanecer al actualizar; verificar changelog y respaldar. El manual distingue flujo en ruta original `Content/UltraDynamicSky` y ruta movida; recomienda actualizar desde launcher, no Fab plugin.

## 9. Ultra Dynamic Weather — p. 58–120, cubierto para conocimiento futuro

**No es obligatorio añadir `Ultra_Dynamic_Weather` a los dos mapas estáticos.** Solo integrar si necesitamos clima activo. Para el caso actual, evitar ceder el control de `Cloud Coverage`/`Fog` antes de fijar ambos looks.

- **Setup (58–61):** UDW necesita actor UDS y lo detecta. `Weather State` incluye Cloud Coverage, Fog, Wind Intensity, Rain, Snow, Dust, Thunder/Lightning; `Material State` Wetness, Snow Coverage, Dust Coverage.
- **Presets / Change Weather (61–63):** `Blueprints/Weather_Effects/Weather_Presets`; presets extensibles, `Change Weather` realiza transición duradera.
- **Random / Seasons (63–66):** Random Interval, Daily, Hourly; mapas de probabilidades por estación y climates; Season float 0–4, derivable de fecha UDS, offsets/meteorological seasons; material functions `Sample UDW Season`, `Season Color Blend`.
- **Manual / Override (66–69):** Weather vacío usa `Manual Weather State`, overrides individuales, `Weather Override Volume` por spline, transition width y prioridad; método `Custom Volume Behavior` en child para lógica extra. Material state también se representa espacialmente.
- **Radial Storms (69–71):** tormentas móviles circulares con nubes localizadas visibles desde fuera en Sky Mode volumétrico; spawn/fade/move/destrucción y weather presets. También permiten zonas de cielo despejado.
- **Partículas (72–76):** rain/splash, snow, dust, wind debris; `Particle Collision Mode` Simple (trazas CPU, Visibility default), Distance Field (GPU, requiere mesh distance fields), No Collision; spawn alrededor de cámara; `Weather Particles Kill Sphere` solo uno activo por vez; `Flash Lightning` y `UDW_Lightning Strikable Actor Interface`.
- **Material Weather (77–95):** `Surface Weather Effects` para props/arquitectura, `Dynamic Landscape Weather Effects V3` para paisajes/suelos, `Foliage Weather Effects` para follaje, `Glass Window Rain Drips`, `Foliage Wind Movement`, `Water Surface Rain Ripples`. DLWE Interaction component deja huellas y ondas; Nanite Tessellation opcional; son costosos en materials y deben activarse con static bools selectivos.
- **Material state persistence (78–79):** `Simulate Changing Material State Over Time` separa acumulación gradual (nieve/humedad) de valores instantáneos del preset. Para keyframes precisos en Sequencer conviene **desactivar** simulación.
- **Masking (91–95):** material functions `Sample UDW Material State`, `Sample UDW Wind`, Parameter Collection; `Weather Mask Brush` y `Weather Mask Projection Box` pintan/proyectan máscaras a targets `UDS_LevelData`, deben guardarse. Volumen de influencia alrededor del punto de control.
- **Audio (95–99):** weather sound effects, oclusión por `Player Occlusion`, `UDS Occlusion Portal`, audio bus, `UDS_Environment_Sound` data assets con MetaSound; formato salida MetaSound **5.1** con mapeo custom y `UDS_Weather_AudioBus` para entorno direccional. Input params `Time`, `Weather`, `Wind`, `Weather Interp`, `Time Interp`; `Change Environment Sound`.
- **Sampling / actor reaction (99–103):** `Get Cloud Coverage`, `Get Current Temperature`, Local Weather State, display name, volumes de temperatura, `UDW Thermometer`; event dispatchers de lluvia/nieve/polvo/niebla/nubosidad. `Actor Weather Status` estima exposición local, temperatura y material parameters Wet/Raining/Snowy/Dusty/Wind Intensity.
- **Wind (103–106):** direction y variation, debris, gusts, Wind Directional Source para cloth/SpeedTree, `Wind Physics Force` para rigid bodies, `Wind Camera Shake` con oclusión.
- **Weather cinematics / extras (106–120):** Manual Weather State keyframed, eventos de Lightning Flash, `Puddle Fluid Volume` y sus interacciones simuladas, `Snow Pile Spline`, Water Level, `Post Process Wind Fog`, Rainbow con condiciones de sol bajo y rain/fog, Screen Droplets/Frost, Heat Distortion, weather display widget, `Custom Weather Behavior` en data asset child, dripping Niagara, freezing breath, Vehicle Window Weather (UV sin overlap, Allow CPU Access, wipers), Rain Drip Spline, icicles, Weather Occlusion Volume.

## 10. Troubleshooting documentado — p. 120–130

| Síntoma | Comprobación precisa según 9.7 |
|---|---|
| Nivel completamente negro al agregar UDS | PPV/cámara exposición conflictiva, viewport Game Settings, probar `Apply Exposure Settings` off |
| Ambiente no actualiza al avanzar hora | `Sky Light Mode` incompatible/captura en plataforma, luz baked remanente; probar dynamic color tint |
| Lumen tarda en cambiar ambiente | PPV → Lumen GI → Update Speed (trade-off GPU) |
| Variable activa tarda 1–2 s en verse | Interpolación cache intencional; `Hard Reset Cache`, `Max Property Cache Period` |
| Variable estática cambia pero sin efecto | Llamar función `Static Properties - <feature>` apropiada |
| Sombras nubes no afectan agua single-layer | `Single Layer Water Uses Light Function Atlas`; `Correct Specular Scale For Low Angle Cloud Shadows` |
| Volumétricas chocan duro con geometría | Cambiar Rendering Mode priorizando `Mesh Intersections` |
| Translucencia atraviesa nube | Material `Apply Fogging`, `Apply Cloud Fogging`, quizá `Compute Fog Per Pixel` |
| Ruido nubes cuando cámara está dentro | Más `View Sample Count Scale`, reducir `Tracing Max Distance`, ajustar `Close View Sample Ratio` |
| Nubes ghosting en timelapse | Otro Volumetric Cloud Rendering Mode (historial más corto) |
| Nubes oscuras/grises anormales | Actores de luz duplicados, `Cloud Scattered Luminance Scale` en custom lights |
| Línea dura de fog/cielo o cielo negro al bajar cámara | Ubicación ground del UDS / componente Sky Atmosphere bajo punto más bajo de cámara |
| Volumetric fog pixelado o ausente | Scalability `Shadow Quality`, no parámetros de UDS |
| Planetas/space layer ausentes | DBuffer decals no activados o plataforma incompatible |
| Partículas UDW ausentes | Collision invisible, show flags, Niagara Full Rebuild; Simulate editor mode no actualiza cámara |
| Weather particles no colisionan | Simple collision/Visibility/Query Only vs Global Distance Field, según modo |
| VR errores de ojos diferentes | `Project Mode = Virtual Reality` |

## 11. Arquitectura real de los Blueprints — p. 130–139

- **UDS:** `Startup Sky` (Construction Script + BeginPlay/restarts), `Update Static Variables` para estáticas, `Update Active Variables` para variables animadas, `Cache Properties`, `Monitor for Changes`, `Increment Cache Timer`, dispatcher de apply functions prefijadas `AP`. Cache interpola valores old/new para ahorrar cómputo y puede demorar cambios visibles.
- La orientación solar/lunar se calcula en `Cache Sun and Moon Orientation`, con `Approximate Real Sun Moon and Stars` si corresponde; los rotadores `Sun World Rotation`/`Moon World Rotation` son intermediarios y la luz puede actualizarse a menor frecuencia. `Increment Cloud Movement Cache` y `Update Cloud Movement` separan posiciones de nubes; `UDS_VolumetricClouds_MPC` distribuye parámetros; `Get Current Base Fog Colors` produce componentes de fog/ambient.
- **UDW:** `Start Weather System`, `Update Current Global and Local Weather State`, `Monitor Local Weather Changes`, `Update Active Variables`, `Update Static Variables`; dispatchers `Construct Global Weather` y `Construct Local Weather`. Preset, random y transición configuran global; Weather Override Volumes, Radial Storms, altura de nubes configuran local.
- Niagara: emitters de colisión CPU/GPU construyen trayectorias reutilizadas; rayos separado entre `Lightning Strike` y `Obscured Lightning`. Audio MetaSound mezcla global y direccional por `UDS_Weather_AudioBus`; `Material State Manager` integra los efectos de material en el tiempo.
- **Replicación multiplayer:** UDS replica Time of Day/Date y velocidades, sincroniza periódicamente; UDW replica fuentes de weather state global y por volúmenes/storms; clientes calculan local. La actualización visual de efectos no implica replicar cada partícula individualmente.

## 12. Decisiones para el Sandbox (fuente vs proyecto)

| Elección | Soporte explícito del manual | Decisión R&D |
|---|---|---|
| Dos configuraciones completas de cielo | Configuration Manager, p.39–41 | Crear **config assets distintos**, y mantener un mapa por look |
| Sun bajo, tonos cálidos de horizonte | Dusk Time, Sky Atmosphere Rayleigh/Absorption, p.3/24 | Diseñar sunset por altura solar + scattering; sin inventar EV/códigos RGB |
| Nubes cinematográficas estables | Cloud Phase/Movement, p.9; Volumetric, p.6–7 | Desactivar animación/random y fijar fase tras componer |
| Rayos entre nubes | Painter y Volumetric Cloud Light Rays, p.46–49 | Opcional **después** del look base; medir GPU |
| Noche estrellada despejada | Stars, Sky Glow, p.35–37; No Clouds, p.4 | No Clouds + verificar Cloud Wisps + Stars + contraste/exposure |
| Estrellas reales | Simulation p.25; Stars p.35–36 | Probar Simulate Real Stars **independientemente** de Sun/Moon |
| Noche legible sin perder oscuridad | Night Brightness / Lighting Brightness (Night), p.3; Moon/Sky Light/Exposure p.11–13 | Ajustes separados, no levantar indiscriminadamente todo el sky |
| Fog y SSFS | UDS Fog p.21–23; SSFS por integración local | Revalidar densidad contra baseline; SSFS fuera del alcance del manual UDS |
| Chameleon y PPV | Post Processing p.50; Exposure p.13–14 | Chequear stacking/overrides en el Sandbox, no añadir tres controles simultáneos |
| Juego vs render offline | Project Mode p.5/29/53 | Game/Real-time para level; Cinematic/Offline solo export de video |

**Fuentes de estado, distintas del manual:** [integración UDS probada en Sandbox](ultra-dynamic-sky.md), [SSFS](screen-space-fog-scattering.md), [estado de Sandbox](../../projects/sandbox-prototype.md).

**Fuera de alcance explícito del PDF:** no provee una receta universal numérica de «sunset espectacular» o «noche clara espectacular» para una escena concreta, ni mide FPS en nuestra plataforma, ni confirma que la versión realmente instalada en el proyecto sea 9.7. El resultado requiere ajuste visual, capturas A/B y prueba empaquetada.