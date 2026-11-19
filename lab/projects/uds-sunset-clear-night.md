# Dos niveles Ultra Dynamic Sky — atardecer y noche clara (manual 9.7)

**Estado 2026-10-26:** planificación **actualizada tras revisar las 139 páginas del PDF original Ultra Dynamic Sky 9.7**. Todavía **no se crearon los dos mapas ni se validaron los settings dentro de Unreal**.  
**Sandbox:** UE 5.8 Blank C++, Third Person; UDS + SSFS verificados en `L_EnvironmentTest`, incluidos checkpoint de packaged build y backup `02_Environment_UDS_SSFS` reportados por el usuario. **UDS instalado: versión todavía no comprobada.**  
**Referencia técnica completa:** [índice anotado de la documentación 9.7, página a página](../research/plugins/ultra-dynamic-sky-9-7-complete-reference.md). La documentación oficial 9.7 es la fuente de los nombres/behaviors; **no define parámetros numéricos universales de look** para nuestro escenario.

> **Actualización 2026-10-27:** además de los dos looks originalmente planificados, se configuró y **aprobó visualmente** el look de amanecer azul `L_Sky_BlueDawn`. Valores propuestos y ajuste final de iluminación `Dawn/Dusk = 0.85` en el [registro de Blue Dawn](uds-blue-dawn.md). Este documento conserva el alcance original de sunset + clear night; no confundir con que `L_Sky_ClearNight` esté listo.

## Diseño de la investigación

Construir **dos mapas separados**, con **hora fija y apariencia persistente** (no un ciclo día/noche). Ambos duplicados **independientes** de `L_EnvironmentTest`. Preservar `L_EnvironmentTest`, `L_ChameleonTest` y el backup.

| | `L_Sky_Sunset` | `L_Sky_ClearNight` |
|---|---|---|
| Objetivo | Cielo dramático, sol cercano al horizonte, nubes 3D con siluetas iluminadas, cálido abajo/frío arriba | Cielo terrestre claro, fondo oscuro, estrellas definidas, luna e iluminación tenue y coherente |
| Sky Mode inicial | `Volumetric Clouds` | `No Clouds` |
| Tiempo | Tarde cercana al cruce del horizonte, posición visual primero | Noche profunda, sol debajo del horizonte |
| Cielo/color | `Sky Atmosphere` Rayleigh + `Sunset/Sunrise`/`Twilight` Absorption | `Stars` + `Sky Glow` + `Moon` + exposición |
| Atmósfera secundaria | Cobertura y fase de nubes, rayos opcionales, fog medido | Cloud Wisps mínimo/ausente, fog mínimo si quita transparencia |
| Assets posibles | `UDS_CFG_Sunset` | `UDS_CFG_ClearNight` |

`UDS_CFG_...` son **nombres propuestos**, no assets que ya existan. Para compartir presets entre niveles, el manual recomienda el Configuration Manager o `Configuration Overrides` (p.39–41).

## Preset A — `L_Sky_Sunset`

1. **Duplicar** `L_EnvironmentTest` a `L_Sky_Sunset` y comprobar que mantiene actor UDS, luces internas, fog y SSFS. No crear DirectionalLight, SkyLight, SkyAtmosphere, Fog o Cloud actores en paralelo: UDS ya tiene los componentes (p.1–2,10–12).
2. **Congelar el tiempo:** categoría `Animate Time of Day` → desactivar animación; `Simulation → Use System Time` → desactivar. `Basic Controls → Time of Day String` acepta `17:30`, etc. **Importante:** `Time of Day` como float va de 0 a **2400**, NO de 0 a 24 (p.14–17). Comenzar explorando hora de tarde, sin confundir hora de reloj con ángulo solar garantizado.
3. **Orientar el sol:** buscar que aparezca bajo y se vea bien desde cámara. `Basic Controls → Dusk Time` regula cruce del horizonte **si no** está activo `Simulate Real Sun`; en modo básico, `Sun Yaw`/Pitch/Vertical Offset permiten orientar su recorrido (p.3,17–19). Si alguna vez es imprescindible un encuadre exacto, considerar `Manually Position Sun Target`, pero no usar controles manuales y simulación simultáneamente sin intención.
4. **Volumétricas y silueta:** dejar `Sky Mode = Volumetric Clouds`; variar `Cloud Coverage`. `Volumetric Clouds → Bottom Altitude` cambia altura de capa, **no** mover actor UDS. `Volumetric Clouds Scale` escala masas. Para fijar una formación interesante: `Cloud Movement → Cloud Speed = 0`, `Randomize Cloud Formation On Run = false`, `Clouds Move With Time of Day = false`; ajustar `Cloud Phase` (p.6–10).
5. **Color real del atardecer:** empezar en `Color Mode = Sky Atmosphere`. `Sky Atmosphere → Rayleigh Scattering` tiene contribuciones de Day/Night/Dawn-Dusk; `Sunset/Sunrise` y `Twilight` controlan absorción y refuerzan los colores del horizonte (p.24). Comparar efecto al modificar **un grupo por vez**. Para color específico de nube volumétrica usar su propia categoría, no los sliders de nubes estáticas/2D de `Simplified Color`.
6. **Separar iluminación de cielo:** `Basic Controls → Lighting Brightness` del estado Dawn/Dusk afecta iluminación de escenario con independencia relativa del color del cielo (p.3). Verificar `Sky Light Mode` y `Sky Light Color Multiplier` antes de corregir con postproceso (p.12–13).
7. **Profundidad opcional:** `Additional Cloud Layers` suma capas (y GPU cost) pero **solo la principal proyecta sombras** (p.7). Si queremos rayos gigantes, empezar con `Volumetric Cloud Painter` para aperturas y, si justifica, `Volumetric Cloud Light Rays` / `Individual Cloud Light Rays` (p.46–49). **No es el mismo efecto** que `Screen Space Light Shafts`, Volumetric Fog o SSFS.
8. **Fog/exposición al final:** baseline tiene `Base Fog Density` duplicado de una prueba indoor; no convertir ese número en preset de exterior. UDS puede recibir fog color de Sky Atmosphere según project setting (p.21–23). Revisar `Exposure → Apply Exposure Settings`, `Exposure Bias`, auto exposure y el PPV existente; usar `Show → Visualize → HDR (Eye Adaptation)` y `Game Settings` del viewport (p.13–14). Comparar `r.SSFS 0` y `r.SSFS 1` **solo después** de fijar atmósfera base.

**Criterio de aceptación visual:** sol bajo, gradiente cálido/frío, nubes tridimensionales con relieve legible, composición reproducible al entrar en PIE, personaje/escenario legibles; sin iluminación quemada. Performance pendiente hasta medir.

## Preset B — `L_Sky_ClearNight`

1. **Duplicar `L_EnvironmentTest` de nuevo**, NO `L_Sky_Sunset`, para evitar heredar tuning difícil de rastrear.
2. Desactivar `Animate Time of Day` y `Use System Time`; elegir `Time of Day String` de noche profunda (punto de partida exploratorio `22:00` o `23:00`, **sin garantía** astronómica). Comprobar sol bajo horizonte y oscuridad real; `Dusk Time` cambia el cruce si no se usa simulación real.
3. `Basic Controls → Sky Mode = No Clouds` para máximo cielo. **Advertencia 9.7 importante:** `Cloud Wisps` se dibuja detrás de **todos** los Sky Modes salvo `Space`, así que comprobar `Cloud Wisps Opacity` con low/high `Cloud Coverage`; `No Clouds` solo no garantiza cielo sin velos (p.10). No usar `Space` para una noche terrestre: elimina atmósfera (p.4). `Cloud Shadows` puede operar incluso sin nubes (p.12): ajustar si deja patrones inesperados.
4. `Stars` regula color, intensidad, twinkling y la textura `Tiling Stars`. Probar `Simulation → Simulate Real Stars` para un mapa 360° de mayor resolución (usa `Real Stars`), **independiente** de `Simulate Real Sun/Moon` (p.25,35–36). No tratarlo como requerimiento obligatorio de exactitud astronómica.
5. `Sky Glow → Night Sky Glow` agrega brillo mínimo global/cenital; `Light Pollution` imita ciudad y luces desde abajo. Para un cielo remoto, mantener contaminación lumínica contenida y equilibrio contra las estrellas (p.36–37). `Moon → Moon Phase`/Moon Intensity y `Sky Light` pueden permitir leer terreno sin volver el cielo diurno. `Simulate Real Moon` también controla fase: dejarlo apagado si vamos por composición artística estable.
6. Separar controles **`Night Brightness`** (sky general), **`Lighting Brightness → Night`** (luz sobre escena), **`Sky Light Color Multiplier`** (ambiental), **`Moon`** (direccional), **`Exposure`** (adaptación de cámara). Evitar subirlos todos simultáneamente porque el resultado puede parecer crepúsculo de día (p.3,11–13,20).
7. Ajustar `Fog Density` y `Volumetric Fog` solo si el baseline deja una capa lechosa que borra estrellas/horizonte. SSFS aporta scattering de fog existente y podría no ser deseable en una noche despejada. Verificar `Exposure` con PPV y PIE antes de juzgar contraste.

**Criterio de aceptación:** cielo azul profundo/negro con estrellas diferenciadas, luna opcional, muy pocas nubes/wisps, siluetas y suelo visibles solo lo suficiente, exposición estable al caminar y entrar/salir de interior.

## Reutilizar configuraciones sin crear código

Cuando un look esté aprobado:

1. Abrir `Ultra Dynamic Sky → Configuration Manager` en toolbar (o editor utility `Blueprints`) y guardar configuración de cada actor a **Project**, no Engine Content, salvo que exista necesidad real de compartir entre proyectos (p.39–40).
2. Crear sendos config assets; anotar sus **rutas reales** al guardarlos.
3. `Configuration Overrides` en `Basic Controls → Advanced` permite enlazar un asset compartido para controlar el actor y aplicarlo al inicio de runtime. **No asignar override mientras seguimos editando sliders**: según el manual, los controles del actor se vuelven no editables mientras override está activo. El Configuration Manager puede aplicar una config sin un override permanente.
4. `Configuration Buttons` permite botones para intercambiar varias configs en editor (p.40–41). Si en el futuro hacen falta transiciones temporales, estudiar `Sky Modifiers` o `Apply Sky Configuration`; por ahora dos mapas y dos configs son suficientes.
5. No tocar los Blueprints originales de UDS para cambiar colores/hora. Si surge una feature genuinamente faltante, el manual aconseja derivar un child Blueprint (p.58).

## Validación incremental acordada

- **Paso 1 actual y único a ejecutar en Editor:** duplicar `L_EnvironmentTest` como `L_Sky_Sunset`, abrirlo y confirmar actor `Ultra_Dynamic_Sky` funcionando, sin alterar parámetros.
- **Pasos siguientes** solo tras confirmación: fijar horario → encuadrar sol → Cloud Phase/Coverage → scattering/color → fog/exposure → capturas y config asset. Repetir en `L_Sky_ClearNight` sin arrastrar tuning de sunset.
- Verificar en PIE y en packaged build, medir `stat gpu`/GPU timings o equivalente y comparar las opciones visuales de calidad. El manual avisa: `Cinematic / Offline` puede perjudicar mucho FPS, Volumetric Clouds son caros y Shadow Quality Low/Medium deshabilita Volumetric Fog por defecto (p.52–55).
- Si el actor/variable tarda 1–2 s en reaccionar, el cache es parte del diseño (`Hard Reset Cache` para reset, `Max Property Cache Period` para frecuencia; p.122). No confundir con rotura. Si algo está «negro», revisar exposición/PPV y viewport `Game Settings` (p.120–121).

| Verificación | Sunset | Clear night |
|---|---|---|
| Mapa duplicado y UDS funcionando | Pendiente | Pendiente |
| Versión instalada de UDS anotada | Pendiente | Pendiente |
| Time of Day fijo comprobado en PIE | Pendiente | Pendiente |
| Nubes/wisps/estrellas/luna afinados | Pendiente | Pendiente |
| Sky Atmosphere/Lighting/Exposure afinados | Pendiente | Pendiente |
| Fog y SSFS A/B | Pendiente | Pendiente |
| Screenshot con cámara controlada | Pendiente | Pendiente |
| UDS config asset guardado | Pendiente | Pendiente |
| Empaquetado probado | Pendiente | Pendiente |
| GPU/Scalability observado | Pendiente | Pendiente |

## Procedencia y límites

**Confirmado por el manual del fabricante:** nombres y función de los controles citados (PDF 9.7, páginas indicadas). **Confirmado por el usuario en octubre 2026:** baseline UDS/SSFS jugable/empaquetado y backup. **Hipótesis R&D:** look cálido/frío, noche remota, nombre de mapas/configs, orden de ajuste y qué parámetros tendrán el mejor resultado. **No demostrado:** settings finales, FPS, versión local UDS, el look logrado en pantalla.

El PDF 9.7 no suministra recetas numéricas exhaustivas para los cielos deseados: cualquier valor final de hora/intensidad/Cloud Phase/Rayleigh/EV deberá capturarse en la escena real.