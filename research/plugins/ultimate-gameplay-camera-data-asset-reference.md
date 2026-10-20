# Ultimate Gameplay Camera — referencia de campos del Camera Data Asset (UE 5.8)

**Registro:** 2026-10-20 · Sandbox Prototype UE 5.8 · fuente: cinco fotografías iniciales y cinco capturas de pantalla directas posteriores del panel **Details** del asset `DA_Camera_Medium` compartidas por el usuario en la conversación. Las segundas imágenes permitieron confirmar valores previamente ambiguos.  
**Clase seleccionada al crear el asset:** `UGC_CameraDataAssetBase`.  
**Ruta de trabajo acordada:** `/Game/Gameplay/Camera/DA_Camera_Medium` (ruta indicada en la conversación; **no inspeccionada** directamente en los archivos del proyecto).

> **Alcance y certeza.** Esto es una **transcripción visual de la interfaz de la versión instalada**, no una lista de valores por defecto certificados del fabricante, ni una inspección de source/reflection, ni una garantía de que las funciones estén activas en runtime. Los valores corresponden a lo mostrado en pantalla en las capturas, con cambios previos del usuario desconocidos. Las opciones atenuadas se registran igualmente y no se asume que surtan efecto. `None` significa referencia/selector vacío; `[colapsado]` significa que no se ven subcampos. Se conservaron como aproximados únicamente los decimales de los límites angulares que siguen siendo demasiado pequeños para certificar su valor exacto. **No extrapolar propiedades no visibles.**

## 1. Camera Arm

### Arm Length Settings
| Propiedad exacta en Details | Valor visible |
| --- | --- |
| Min Arm Length | `100.0` |
| Max Arm Length | `250.0` (confirmado en captura nueva) |
| Arm Range Blend Time | `0.5` |
| Arm Range Blend Curve | `None` |

### Arm Offset Settings
| Propiedad | Valor visible |
| --- | --- |
| Arm Socket Offset | Vector (X `0.0`, Y `40.0`, Z `0.0`); **Y positivo confirmado** |
| Arm Socket Offset Blend Time | `0.5` |
| Arm Socket Offset Blend Curve | `None` |
| Arm Target Offset | Vector (X `0.0`, Y `0.0`, Z `58.0`); **Z confirmado en captura nueva** |
| Arm Target Offset Blend Time | `0.5` |
| Arm Target Offset Blend Curve | `None` |

Las filas individuales `X`, `Y`, `Z` aparecen expandidas para ambos vectores.

### Arm Lag Settings
| Propiedad | Valor visible |
| --- | --- |
| Override Spring Arm Component Settings | desactivado |
| Enable Camera Lag | activado |
| Enable Camera Rotation Lag | desactivado |
| Camera Lag Speed | `8.0` (campo atenuado) |
| Camera Rotation Lag Speed | `10.0` (campo atenuado) |
| Camera Lag Max Distance | `0.0` (campo atenuado) |

## 2. Camera FOV

### FOVSettings
| Propiedad | Valor visible |
| --- | --- |
| Min FOV | `90.0` |
| Max FOV | `125.0` |
| FOVRange Blend Time | `0.5` |
| FOVRange Blend Curve | `None` |

El encabezado en la interfaz aparece como `FOVSettings` (sin espacio) y se distribuye entre las fotos 1 y 2.

## 3. Camera Collisions

### Collision Settings
| Propiedad | Valor visible |
| --- | --- |
| Penetration Blend In Time | `0.05` |
| Penetration Blend Out Time | `0.5` |
| Prevent Penetration | activado |
| Do Predictive Avoidance | activado |
| Penetration Avoidance Feelers | array de **11 elementos**, índices `[0]` a `[10]`; cada uno indica `4 members`; **contenido interno no desplegado** |
| Ignore Camera Collision Tag | `UGC_IgnoreCameraCollision` |

No registrar valores inventados de los 44 subcampos de Feelers. La existencia de opciones de colisión en el Data Asset no prueba por sí sola qué `SpringArmComponent` está usando el Character.

## 4. Camera Dithering

### Dithering Settings
| Propiedad | Valor visible |
| --- | --- |
| Material Opacity Parameter Name | `Opacity` |
| Update Material Player Position | desactivado |
| Material Player Position Parameter Name | `PlayerLocation` (campo atenuado) |
| Material Dither Minimum | `0.1` |
| Dither In Speed | `10.0` |
| Dither Out Speed | `10.0` |
| Ignore Dithering Tag | `UGC_IgnoreDithering` |
| Dither Child Actors | activado |
| Dither Attached Components | activado |
| Dither Line Of Sight | desactivado |
| Dither LOSChannel | `Camera` (atenuado) |
| LOSProbe Size | `5.0` (atenuado) |
| Collision Time Threshold | `0.0` (atenuado) |
| Dither Overlaps | activado |
| Dither Owner | activado |
| Dither Overlap Channel | `Camera` |
| Sphere Collision Radius | `15.0` |

La captura 2 continúa al comienzo de la 3. **No afirmar dithering funcional:** hay requisitos de materiales y ejecución que no se han probado.

## 5. Camera Movement Follow

### Pitch → Pitch Follow Settings
| Propiedad | Valor visible |
| --- | --- |
| Enable Pitch Movement Follow | activado |
| Pitch Follow Speed | `10.0` |
| Pitch Follow Time Threshold | `2.0` |
| Reset Threshold Timer when No Movement | desactivado |
| Pitch Follow Angle Threshold | `5.0` |
| Resting Pitch | `-10.0` |
| Trigger when Falling | activado |
| Pitch Follow Time Threshold when Falling | `0.5` |
| Min Distance from Ground to Trigger when Falling | `500.0` |
| Speed Multiplier when Falling | `6.0` |
| Trigger on Slopes | activado |
| Slope Min Incline | `25.0` |
| Slope Follow Weight | `1.0` (atenuado) |

### Yaw → Yaw Follow Settings
| Propiedad | Valor visible |
| --- | --- |
| Enable Yaw Movement Follow | activado |
| Reset Threshold Timer when No Movement | desactivado |
| Yaw Follow Speed | `50.0` |
| Yaw Follow Time Threshold | `2.0` |
| Yaw Follow Angle Threshold | `10.0` |

## 6. Camera Angle Constraints

### Pitch Constraints
| Propiedad | Valor visible |
| --- | --- |
| Constrain Pitch | desactivado |
| Pitch Constraint Tolerance | `10.0` |
| Local Min Pitch | aproximadamente `-90` grados (el campo muestra un valor cercano a `-89.99999`; decimales exactos no certificados) |
| Local Max Pitch | aproximadamente `90` grados (el campo muestra un valor cercano a `89.99999`; decimales exactos no certificados) |
| Pitch Constraints Blend Time | `0.5` |
| Pitch Constraints Blend Curve | `None` |

### Yaw Constraints
| Propiedad | Valor visible |
| --- | --- |
| Constrain Yaw | desactivado |
| Yaw Constraint Tolerance | `10.0` |
| Local Min Yaw | `0.0` |
| Local Max Yaw | aproximadamente `360` grados (el campo muestra un valor cercano a `359.99999`; decimales exactos no certificados) |
| Yaw Constraints Blend Time | `0.5` |
| Yaw Constraints Blend Curve | `None` |

## 7. Camera Focus

### Focus Settings
| Propiedad | Valor visible |
| --- | --- |
| Enabled | desactivado |
| Interp Speed | `10.0` |
| Rotation Offset | vector/rotador; mostrado con componentes `0.0°`, `0.0°`, `0.0°` y subestructura colapsada |
| Ignore Camera Input | desactivado |
| Rotate Yaw Only | desactivado |
| Stop if Blocked LOS | desactivado |
| Blocked LOS Time Threshold | `3.0` (atenuado) |
| Min Distance from Target | `100.0` |
| Max Distance from Target | `1000.0` |
| Experimental Sanitize if Too Close | desactivado |
| Focus Target Method | `None` |

## 8. Pitch to Arm Length and FOVCurv

### Pitch to Arm and FOVCurve Settings
| Propiedad | Valor visible |
| --- | --- |
| Enabled | activado |
| Pitch to Arm Length Curve | `None` |
| Pitch to FOVCurve | `None` |

Se preserva la grafía visible (`FOVCurv`, `FOVCurve`) hasta poder inspeccionar nombres internos. **Enabled marcado con ambas curvas `None` no prueba un comportamiento de curva activo.**

## 9. Add Ons

| Propiedad | Valor visible |
| --- | --- |
| Add Ons Settings | Array: **0 elementos** |

No se desplegaron tipos/clases de elementos porque la lista está vacía.

## 10. Estado del experimento y próximos pasos

**Confirmaciones del usuario anteriores a las capturas:**
- En el Third Person PlayerController existente (padre `GSCModularPlayerController`) seleccionó `BP_UGCCameraManager` como `Player Camera Manager Class`.
- En PIE el movimiento y el control de cámara siguen funcionando; el usuario observó mejora subjetiva de la cámara.
- Comprobó con `Berta.UGC.Dump` que el Camera Manager activo es el de UGC; no se copió el texto completo del dump.
- Agregó `BertaUGCCameraCycleComponent` al mismo PlayerController.
- Creó `DA_Camera_Medium`, basado en `UGC_CameraDataAssetBase`, y compartió primero cinco fotografías y luego cinco capturas directas más nítidas; las segundas confirmaron **Max Arm Length 250.0**, **Arm Socket Offset Y +40.0** y **Arm Target Offset Z 58.0**.
- Nice Interaction v2: Windows Development y backup completo `06_NiceInteraction_Validated` confirmados.

**Actualización posterior (20-10-2026):** este párrafo de pendientes iniciales quedó **superado parcialmente**. El usuario confirmó **dos cambios visibles** mediante `Berta.UGC.Next` después de recomendar un Camera Manager hijo de `BP_UGCCameraManager` con `Default Camera Data` propio; no hubo inspección de los assets finales. Se definieron variantes **Near** (Arm 80–120, FOV 80–90) y **Far** (Arm 350–450, FOV 95–105), ambas con blends 0.5 segundos, partiendo de Medium. **Fuente principal y cronología de fallos/solución:** [integración de Ultimate Gameplay Camera](ultimate-gameplay-camera-sandbox-integration.md). **Sigue pendiente:** `Previous`, verificación completa de GAS + Nice Interaction después de cambiar presets, comprobación de Dynamic Spring Arm/collision avanzada y packaging de UGC. **No está confirmado un backup 07.**

## Reglas para retomar

1. Usar **estos nombres literales del panel Details** y verificar en la versión instalada antes de proponer nodos o campos adicionales.
2. No interpretar todos los valores de esta captura como “defaults del proveedor” ni asumir que cada opción visible funciona sin su modifier/spring arm.
3. No volver a pedir capturas generales de este Data Asset. El signo del Socket Offset Y ya está confirmado; si alguna precisión decimal del límite angular o contenido de una curva/feeler es imprescindible, consultar **solo ese campo**.
4. Usar el registro arquitectónico en [camera-systems-comparison.md](camera-systems-comparison.md) y el estado del proyecto en [sandbox-prototype.md](../../projects/sandbox-prototype.md) como documentos complementarios.
5. **Referencia histórica de imagen:** diez imágenes del Details de `DA_Camera_Medium` compartidas en el chat del 2026-10-20 (cinco fotografías y cinco capturas directas). Las imágenes no se copian al repositorio; esta referencia textual preserva la observación.
