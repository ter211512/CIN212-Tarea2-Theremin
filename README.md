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
