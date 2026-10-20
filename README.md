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
