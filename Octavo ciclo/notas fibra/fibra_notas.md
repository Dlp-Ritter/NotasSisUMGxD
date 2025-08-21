# Notas para la presentacion sobre fibra optica

## 1. ¿Qué es la Fibra Óptica?

La fibra óptica es fundamentalmente un **"tubo de luz"** - un medio de transmisión que utiliza **pulsos de luz** para transportar información a través de hilos de vidrio o plástico ultra delgados.

### Principio Físico Fundamental: Reflexión Total Interna

La fibra funciona gracias a un fenómeno óptico llamado **reflexión total interna**:

1. **Núcleo** (core): Material con índice de refracción alto
2. **Revestimiento** (cladding): Material con índice de refracción menor
3. **Resultado**: La luz queda "atrapada" dentro del núcleo, rebotando en las paredes sin escapar

![[5.png ]]
## 2. Como funciona la conversion de Bits a Fotones

### **- Conversión Eléctrica → Óptica (Transmisión)**

**En el transmisor:**

1. **Datos digitales** llegan como señales eléctricas (0s y 1s)
2. Un **driver circuit** amplifica y acondiciona la señal
3. La señal eléctrica alimenta la **fuente de luz** (LED o láser) - Se ve luego.
4. La fuente de luz se **enciende y apaga** rápidamente siguiendo los bits:
    - **Bit 1** = Luz ON (fotones emitidos)
    - **Bit 0** = Luz OFF (sin fotones o nivel muy bajo)

### **- Propagación por la Fibra**

- Los **pulsos de luz** viajan por reflexión total interna
- Cada pulso representa información binaria
- La luz mantiene su patrón de encendido/apagado a lo largo de kilómetros

### **- Conversión Óptica → Eléctrica (Recepción)**

**En el receptor:**

1. Un **fotodiodo** (detector de luz) recibe los pulsos luminosos
2. Cada fotón que llega **genera electrones** (efecto fotoeléctrico)
3. Los electrones forman **corriente eléctrica**
4. Un **amplificador transimpedancia** convierte corriente en voltaje
5. Circuitos digitales **reconstruyen** los 0s y 1s originales

## 3. Tecnologías de Fuentes de Luz

### LED (Light Emitting Diode)
- **Longitudes de onda**: 850 nm, 1310 nm
- **Características**: Luz incoherente, espectro amplio
- **Distancias**: Cortas a medias (hasta 2 km)
- **Costo**: Menor
- **Aplicaciones**: Redes LAN, multimodo principalmente

### LASER (Light Amplification by Stimulated Emission of Radiation)
- **Tipos**: DFB, VCSEL, FP
- **Longitudes de onda**: 850, 1310, 1550 nm principalmente
- **Características**: Luz coherente, espectro estrecho
- **Distancias**: Largas distancias
- **Costo**: Mayor
- **Aplicaciones**: Monomodo y multimodo de alta velocidad

## 4. Materiales de Fabricación del Núcleo

### Vidrio
- **Material**: Dióxido de silicio
- **Ventajas**: Menor atenuación, mayor ancho de banda, transmisión a largas distancias
- **Desventajas**: Más frágil, costoso de fabricar
- **Aplicaciones**: Telecomunicaciones, redes de alta velocidad, aplicaciones críticas

![[2.jpg | 370]]
### Plástico
- **Material**: PMMA (polimetacrilato de metilo) o policarbonato
- **Ventajas**: Más flexible, resistente a golpes, fácil instalación, menor costo
- **Desventajas**: Mayor atenuación, menor ancho de banda, distancias cortas (máximo 100m)
- **Aplicaciones**: Redes domésticas, automotriz, aplicaciones industriales de corta distancia

![[3.jpg | 370]]
## 2. Estructura del Cable de Fibra Óptica

### Componentes por capas (de interior a exterior):
1. **Núcleo**: Por donde viaja la luz
2. **Revestimiento (Cladding)**: Refleja la luz hacia el núcleo
3. **Recubrimiento primario**: Protección UV y humedad
4. **Recubrimiento secundario**: Protección mecánica adicional
5. **Elementos de refuerzo**: Kevlar o fibra de vidrio
6. **Chaqueta exterior**: Protección final contra factores ambientales

![[1.jpg | 330]]
## 3. Tipos de Fibra Óptica

### Monomodo (Single Mode - SM) -- 
- **Diámetro del núcleo**: 9 micrómetros
- **Características**: Un solo modo de propagación de luz
- **Distancias**: Hasta 40 km o más sin repetidores
- **Aplicaciones**: Redes WAN, enlaces de larga distancia, backbones
- **Ventajas**: Mayor ancho de banda, menor dispersión
- **Estándar**: ITU-T G.652, G.653, G.654, G.655, G.657

### Multimodo (Multi Mode - MM)
- **Diámetro del núcleo**: 50 o 62.5 micrómetros
- **Características**: Múltiples modos de propagación
- **Distancias**: Hasta 2 km típicamente
- **Aplicaciones**: Redes LAN, centros de datos, edificios
- **Tipos**:
  - **OM1**: 62.5/125 μm - hasta 275m a 1000 Mbps
  - **OM2**: 50/125 μm - hasta 550m a 1000 Mbps
  - **OM3**: 50/125 μm optimizada para laser - hasta 300m a 10 Gbps
  - **OM4**: 50/125 μm mejorada - hasta 400m a 10 Gbps
  - **OM5**: 50/125 μm para SWDM - soporte para múltiples longitudes de onda

## 4. Pulido de conecoteres
## ¿Qué es el Pulido en Fibra Óptica?

El pulido es el proceso de preparar la superficie del extremo de la fibra donde se produce la conexión óptica. Una superficie mal pulida causa:

- Pérdidas de inserción altas
- Reflexiones excesivas
- Degradación de la señal
- Problemas de compatibilidad
## Tipos de Pulido de Conectores
### UPC (Ultra Physical Contact)
- **Ángulo**: 0° (perpendicular)
- **Pérdida de retorno**: -50 dB típica
- **Color identificación**: Azul o verde
- **Aplicaciones**: Sistemas digitales, no críticos en reflexión

### PC (Physical Contact)
- **Ángulo**: 0° con mejor acabado que UPC
- **Pérdida de retorno**: -40 dB típica
- **Aplicaciones**: Sistemas menos críticos

### APC (Angled Physical Contact)
- **Ángulo**: 8° con respecto al eje de la fibra
- **Pérdida de retorno**: -60 dB o mejor
- **Color identificación**: Verde
- **Aplicaciones**: Sistemas analógicos, CATV, aplicaciones críticas en reflexión
- **Ventaja**: Minimiza las reflexiones enviándolas fuera del núcleo

### **FLAT (Flat Polish)** - obsoleto ya no se ocupa
- **Superficie**: Completamente plana, sin curvatura
- **Ángulo**: 0° (perpendicular)
- **Pérdida de retorno**: -14 a -20 dB (la peor)
- **Características**: No hay contacto físico directo entre fibras, queda un pequeño gap de aire
- **Color identificación**: Sin color específico estándar
- **Aplicaciones**: Sistemas muy antiguos, aplicaciones no críticas
- **Desventajas**: Altas pérdidas de inserción, muchas reflexiones
- **Estado**: **Prácticamente obsoleto**

![[4.png ]]

## 5. Transceptores

### Clasificación por Multiplexación
- **Sin multiplexación**: Un canal por fibra
- **Con multiplexación (WDM/DWDM)**: Múltiples canales en una fibra

### Tipos por Forma Factor
- **SFP**: Small Form-factor Pluggable (1 Gbps)
- **SFP+**: Enhanced SFP (10 Gbps)
- **XFP**: 10 Gigabit SFP
- **QSFP+**: Quad SFP+ (40 Gbps)
- **QSFP28**: 100 Gbps
- **CFP/CFP2/CFP4**: 100G+ aplicaciones

### Tipos por Alcance
- **SR (Short Range)**: 150m - 300m
- **LR (Long Range)**: 10 km
- **ER (Extended Range)**: 40 km
- **ZR (Ultra Long Range)**: 80 km+

## 8. Conectores

### LC (Lucent Connector)
- **Características**: Pequeño, tipo push-pull
- **Factor de forma**: 1.25 mm
- **Aplicaciones**: Alta densidad, SFP/SFP+
- **Ventajas**: Compacto, fácil instalación

### SC (Subscriber Connector)
- **Características**: Cuadrado, tipo push-pull
- **Factor de forma**: 2.5 mm
- **Aplicaciones**: Telecomunicaciones, aplicaciones estándar
- **Ventajas**: Robusto, confiable

### Otros Conectores Importantes
- **ST**: Bayoneta, común en multimodo
- **FC**: Rosca, aplicaciones críticas
- **MTP/MPO**: Múltiples fibras (12, 24 fibras)

## 9. Convertidores de Medios

### Definición Ampliada
Dispositivos que convierten señales entre diferentes medios de transmisión.

### Tipos
- **Fibra a Cobre Ethernet**: Más común
- **Transceptores ópticos**: Integrados en switches
- **Repetidores ópticos**: Amplificación de señal
- **Convertidores de protocolo**: Diferentes estándares

### Aplicaciones
- Extensión de redes LAN
- Integración de sistemas legacy
- Aislamiento eléctrico
- Inmunidad electromagnética

## 10. Cables Prearmados

### DAC (Direct Attach Cable)
- **Material**: Cable de cobre con conectores ópticos
- **Características**: 
  - Baja latencia
  - Menor costo que fibra
  - Consumo energético reducido
  - Distancias limitadas (1-7 metros típicamente)
- **Aplicaciones**: Conexiones dentro del rack, top-of-rack
- **Tipos**: Pasivo (no amplificado) y Activo (con amplificación)

### AOC (Active Optical Cable)
- **Material**: Fibra óptica con transceptores integrados
- **Características**:
  - Mayor distancia que DAC (hasta 300m)
  - Inmune a interferencia electromagnética
  - Plug-and-play
  - Más costoso que DAC
- **Aplicaciones**: Conexiones entre racks, centros de datos

## 11. Multiplexación por División de Longitud de Onda

### Lambda (λ) - Concepto Ampliado
- **Definición**: Cada longitud de onda específica utilizada para transmitir información
- **Medición**: Nanómetros (nm)
- **Estándares ITU**: Grid de frecuencias específicas

### Tipos de Multiplexación
- **CWDM (Coarse WDM)**: 18 canales, espaciado 20 nm
- **DWDM (Dense WDM)**: 40+ canales, espaciado 0.8 nm
- **LWDM (LAN WDM)**: Para centros de datos
- **SWDM (Short WDM)**: Multimodo con múltiples λ

### Ventanas de Transmisión
- **Primera ventana (850 nm)**: Multimodo, corta distancia
- **Segunda ventana (1310 nm)**: Dispersión cero
- **Tercera ventana (1550 nm)**: Mínima atenuación
- **Banda L (1625 nm)**: Aplicaciones especiales

## 12. Parámetros Técnicos Importantes

### Atenuación
- **Medición**: dB/km
- **Valores típicos**: 0.2-0.4 dB/km en 1550 nm (monomodo)

### Dispersión
- **Cromática**: Diferentes λ viajan a diferentes velocidades
- **Modal**: En multimodo, diferentes modos

### Ancho de Banda
- **Monomodo**: Prácticamente ilimitado
- **Multimodo**: Limitado por dispersión modal

### Apertura Numérica (NA)
- **Definición**: Capacidad de recolección de luz
- **Valores**: 0.1-0.2 (monomodo), 0.2-0.3 (multimodo)
