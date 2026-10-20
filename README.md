| Componente | Pin Componente | Pin Arduino UNO | Configuración |
| :--- | :--- | :--- | :--- |
| **Pulsador Grave** | Pin 1 / Pin 2 | D2 / GND | `INPUT_PULLUP` interno |
| **Pulsador Medio** | Pin 1 / Pin 2 | D3 / GND | `INPUT_PULLUP` interno |
| **Pulsador Agudo** | Pin 1 / Pin 2 | D4 / GND | `INPUT_PULLUP` interno |
| **Buzzer Pasivo** | Positivo (+) / Negativo (-) | D8 / GND | Salida `tone()` |
| **Sensor HC-SR04** | VCC / GND | 5V / GND | Alimentación |
| **Sensor HC-SR04** | TRIG | D9 | Salida (`OUTPUT`) |
| **Sensor HC-SR04** | ECHO | D10 | Entrada (`INPUT`) |







| Rango Tonal | Frecuencia Mínima | Frecuencia Máxima | Rango Musical |
| :--- | :--- | :--- | :--- |
| **Grave** | 131 Hz | 262 Hz | Do3 a Do4 |
| **Medio** *(por defecto)* | 262 Hz | 523 Hz | Do4 a Do5 |
| **Agudo** | 523 Hz | 1047 Hz | Do5 a Do6 |


##Calculo y deduccion de formulas
La velocidad teorica de programacion del sonido en el aire a temperatura ambiente estandar (~20 °C) es :
$$v = 343\text{ m/s} = 0.0343\text{ cm/}\mu\text{s}$$
dado que el pulso ultra sonico emitido por transductor realiza una trayectoria de ida y vuelta hacia el objeto ($2d$):
$$2d = v \times t \implies d = \frac{t \times 0.0343}{2} = \frac{t}{58.3}\text{ cm}$$
en el firmware se emplea la constante de conversion `distancia = duracion / 58.2` y se establece un tiempo de espera acotado en `pulseIn(PIN_ECHO, HIGH, 25000)` para evitar el bloqueo del ciclo si no se recibe eco.

filtrado y zona util
-**Zona util:** definida entre 5.0 cm y 30.0 cm. Si no se detecta presencia o la distancia supera este umbral, el buzzer se apaga automaticamente con `noTone()`.
-**Filtro de ruido:** Se aplica un filtro de promedio movil con ventana de 5 lecturas para amortiguar fluctuaciones ambientales o jitter involuntario de la mano en reposo.
-**Mapeo inverso:** A menor distancia de la mano respecto al sensor, se genera una frecuencia mas aguda dentro de los limites del rango activo, emulando la fisica del theremin acustico.


## 1. Descripción del proyecto
Este proyecto consiste en el diseño e implementación de un theremin digital utilizando un Arduino UNO, un sensor ultrasónico, tres pulsadores y un buzzer pasivo.
El sistema permite generar tonos cuya frecuencia varía según la distancia entre la mano del usuario y el sensor. Mediante tres pulsadores se selecciona uno de los siguientes registros musicales: grave, medio y agudo.
El firmware utiliza antirrebote por software, filtrado de mediciones y funciones modulares para mejorar la estabilidad y la respuesta del instrumento.

## 2. Objetivos
- Medir la distancia de la mano utilizando un sensor ultrasónico.
- Convertir la distancia medida en una frecuencia audible.
- Seleccionar tres rangos tonales mediante pulsadores.
- Aplicar antirrebote por software a los botones.
- Filtrar mediciones para reducir las variaciones de distancia.
- Generar sonido mediante las funciones `tone()` y `noTone()`.
- Organizar el código y documentar el proyecto mediante GitHub.

## 3. Materiales
| Componente | Cantidad |
|---|---:|
| Arduino UNO R3 | 1 |
| Sensor ultrasónico HC-SR04 | 1 |
| Pulsadores momentáneos | 3 |
| Buzzer pasivo de 5 V | 1 |
| Protoboard | 1 |
| Cables de conexión | Varios |
| Cable USB compatible con Arduino | 1 |


## 4. Conexiones del circuito
| Componente | Pin Arduino |
|---|---|
| Pulsador grave | D2 |
| Pulsador medio | D3 |
| Pulsador agudo | D4 |
| Buzzer pasivo | D8 |
| HC-SR04 TRIG | D9 |
| HC-SR04 ECHO | D10 |
| Alimentación del sensor | 5V |
| Tierra del sensor | GND |

Los pulsadores utilizan la configuración `INPUT_PULLUP`. Por lo tanto, cada botón debe conectar su pin digital a GND cuando se presiona.
El buzzer debe ser pasivo para permitir generar diferentes frecuencias mediante `tone()`.
**Diagrama de conexiones:** agregar aquí una imagen del circuito real o un esquema que coincida exactamente con el montaje utilizado.

## 5. Rangos de frecuencia
| Registro | Frecuencia mínima | Frecuencia máxima |
|---|---:|---:|
| Grave | 131 Hz | 262 Hz |
| Medio | 262 Hz | 523 Hz |
| Agudo | 523 Hz | 1047 Hz |

La zona útil de distancia se establece entre 5 y 30 cm. Dentro de esta zona, acercar la mano aumenta la frecuencia y alejarla disminuye la frecuencia.
Fuera de la zona útil, el buzzer debe permanecer en silencio.

## 6. Funcionamiento del sistema
1. **Inicialización:** Arduino configura los pines y selecciona el registro medio por defecto.
2. **Selección del registro:** los tres pulsadores permiten cambiar entre grave, medio y agudo. El rango seleccionado permanece activo hasta presionar otro botón.
3. **Medición:** el HC-SR04 emite un pulso ultrasónico y mide el tiempo que tarda en regresar el eco.
4. **Conversión:** el tiempo de eco se transforma en una distancia expresada en centímetros.
5. **Filtrado:** se procesan las mediciones válidas para reducir el ruido del sensor.
6. **Mapeo:** la distancia filtrada se convierte en una frecuencia dentro de los límites del registro seleccionado.
7. **Generación del sonido:** `tone()` reproduce la frecuencia calculada y `noTone()` detiene el sonido cuando no hay una mano dentro de la zona útil.

## 7. Conversión de tiempo a distancia
La distancia se calcula a partir del tiempo de ida y vuelta de la onda ultrasónica:
`distancia_cm = tiempo_us × 0.0343 / 2`
El factor 0.0343 corresponde aproximadamente a la velocidad del sonido expresada en centímetros por microsegundo. Se divide entre dos porque la onda recorre la distancia hasta la mano y luego regresa al sensor.
Las lecturas inválidas o fuera de los límites definidos deben descartarse.

## 8. Compilación y carga del programa
1. Instalar Arduino IDE.
2. Descargar o clonar este repositorio.
3. Abrir el archivo `.ino` del proyecto.
4. Conectar el Arduino UNO mediante USB.
5. Seleccionar **Herramientas → Placa → Arduino UNO**.
6. Seleccionar el puerto correspondiente.
7. Presionar **Verificar** para compilar el programa.
8. Presionar **Subir** para cargar el firmware.
9. Probar los tres registros y la variación del tono con la distancia.

## 9. Organización del proyecto

```text
CIN212-Tarea2-Theremin/
├── README.md
├── codigo_fuente/
│   └── theremin.ino
└── docs/
    └── diagrama_conexiones.png
```

## 10. Pruebas de funcionamiento
Se deben verificar los siguientes aspectos:
- Cambio correcto entre los tres registros.
- Ausencia de cambios involuntarios por rebote de los pulsadores.
- Medición estable de la distancia.
- Variación del tono al acercar y alejar la mano.
- Silencio fuera de la zona útil.
- Compilación correcta y funcionamiento en el Arduino UNO.

Los resultados de estas pruebas se documentarán una vez realizadas.

## 11. Trabajo colaborativo
El desarrollo se gestionará mediante GitHub, registrando avances significativos en el historial de commits y utilizando Issues o GitHub Projects para distribuir las tareas entre los integrantes.

## 12. Autores
- Integrante 1: Walter Valencia
- Integrante 2: Lucas Marchant

## 13. Repositorio
https://github.com/ter211512/CIN212-Tarea2-Theremin
