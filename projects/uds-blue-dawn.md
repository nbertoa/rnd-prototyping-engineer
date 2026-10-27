# Ultra Dynamic Sky — Blue Dawn (`L_Sky_BlueDawn`)

**Fecha de registro:** 2026-10-27  
**Proyecto:** Sandbox Prototype, Unreal Engine 5.8  
**Fuente técnica de controles:** documentación Ultra Dynamic Sky 9.7 del usuario, [referencia anotada](../research/plugins/ultra-dynamic-sky-9-7-complete-reference.md).  
**Estado:** **look visual aprobado por el usuario en el Editor** (2026-10-27). No se verificaron automáticamente archivos locales de Unreal, capturas, versión instalada de UDS, assets de Configuration Manager, FPS ni paquete ejecutable.

## Objetivo visual

Escena de **hora azul, justo antes del amanecer**: cielo azul profundo con una transición violeta suave, nubes volumétricas relativamente oscuras y escenario suficientemente legible. Más oscuro y frío que `L_Sky_Sunset`, pero sin llegar a la oscuridad de `L_Sky_ClearNight`.

**Nivel indicado:** `L_Sky_BlueDawn`. El usuario confirmó haber configurado el look y que le gusta. El nombre/ruta exacta del nivel guardado en el proyecto no fue inspeccionado por herramienta; verificar in-editor si el nombre coincide.

## Valores entregados durante la sesión

Son **valores propuestos para esta escena**, no presets oficiales ni valores exportados por Unreal. El usuario confirmó el resultado visual y el último cambio de luminosidad; **no se inspeccionó un volcado de todos los Details para confirmar cada campo**.

### Basic Controls

| Campo de UDS | Valor propuesto |
|---|---|
| `Time of Day String` | `05:40` |
| `Dawn Time` | `600` (ingresado como escala 0–2400; verificar posición solar real) |
| `Sky Mode` | `Volumetric Clouds` |
| `Color Mode` | `Sky Atmosphere` |
| `Project Mode` | `Game / Real-time (Non VR)` |
| `Cloud Coverage` | `0.35` |
| `Fog` | `0.10` |
| `Saturation` | `0.90` |
| `Contrast` | `1.05` |
| `Overall Intensity` | `0.85` |
| `Night Brightness` | `0.70` |
| `Lighting Brightness → Dawn/Dusk` | **`0.85` — ajuste final confirmado por el usuario** |

El último valor reemplaza la propuesta inicial de **`0.65`**. El usuario reportó que, después de aumentar de `0.65` a `0.85`, consiguió la iluminación deseada y le gusta el resultado. **No reemplazar silenciosamente este valor por el anterior**.

### Sky Atmosphere — colores RGBA (valores lineales en Details)

| Campo | R | G | B | A |
|---|---:|---:|---:|---:|
| `Rayleigh Scattering → Dawn/Dusk` | 0.08 | 0.24 | 0.75 | 1.0 |
| `Sunset/Sunrise Absorption` | 0.08 | 0.10 | 0.20 | 1.0 |
| `Twilight Absorption` | 0.02 | 0.06 | 0.15 | 1.0 |

Los colores son **entradas propuestas** que producen un resultado dependiente del resto del setup, la exposición y las condiciones solares. No equivalen directamente a un código hexadecimal ni garantizan el mismo color de captura en otro nivel.

### Clouds, Simulation y Exposure

| Campo | Valor |
|---|---|
| `Cloud Speed` | `0` |
| `Randomize Cloud Formation On Run` | `false` |
| `Clouds Move With Time of Day` | `false` |
| `Cloud Phase` | conservar formación elegida en la escena; valor numérico no reportado |
| `Animate Time of Day` | `false` |
| `Use System Time` | `false` |
| `Simulate Real Sun` | `false` |
| `Manually Position Sun Target` | `false` |
| `Exposure Bias → Dawn/Dusk` | `-0.3` **solo si este campo existe y fue aplicado**; no confirmado |

Todos los demás campos, incluidos `Sky Light` y el `Volumetric Fog` detallado, permanecen sin valores específicos documentados. No copiar aquí el anterior `Base Fog Density` doblado para una prueba indoor, salvo que esté efectivamente presente en el nivel.

## Resultado confirmado

- **2026-10-27:** el usuario indicó que había configurado la hora azul y quería que se viera algo más iluminada.
- Se propuso aumentar exclusivamente `Lighting Brightness → Dawn/Dusk` de `0.65` a `0.85`.
- El usuario reportó: «Perfecto, eso lo logré configurar y me gusta mucho cómo queda».
- **Estado:** preset visual **aceptado por el usuario**, no validación técnica completa. No inferir FPS, Play-In-Editor, build packaged o configuración guardada como asset.

## Próxima acción — preservar el resultado

**Recomendado:** guardar en Unreal el mapa y generar una configuración nativa con el **UDS Configuration Manager** (propuesta de nombre: `UDS_CFG_BlueDawn`; ruta real pendiente). Según el manual 9.7, los Configuration Overrides vuelven no editables los controles manuales mientras están asignados. Guardar primero la configuración trabajada y comprobar que puede reaplicarse.

Opcional antes de cerrar hito: captura desde cámara fija y listado de detalles con los valores finales, prueba PIE, `stat fps`/`stat gpu` y empaquetado en Windows. No marcar estos pasos como realizados sin la confirmación correspondiente.

## Relación con otros looks

- Atardecer cálido: `L_Sky_Sunset`, look preparado en una sesión anterior, [plan original](uds-sunset-clear-night.md).
- Hora azul: **`L_Sky_BlueDawn`**, resultado visual aprobado.
- Noche despejada: `L_Sky_ClearNight`, objetivo del plan original, **no hay confirmación de que esté terminado**.

**Decisión de arquitectura:** conservar cada look en su mapa y, si necesitamos compartirlo, con configuración nativa de UDS. Por ahora **no** desarrollar presets propios en BertaDevKit.
