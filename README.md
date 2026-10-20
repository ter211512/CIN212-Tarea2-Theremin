| Componente | Pin Componente | Pin Arduino UNO | Configuración |
| :--- | :--- | :--- | :--- |
| **Pulsador Grave** | Pin 1 / Pin 2 | D2 / GND | `INPUT_PULLUP` interno |
| **Pulsador Medio** | Pin 1 / Pin 2 | D3 / GND | `INPUT_PULLUP` interno |
| **Pulsador Agudo** | Pin 1 / Pin 2 | D4 / GND | `INPUT_PULLUP` interno |
| **Buzzer Pasivo** | Positivo (+) / Negativo (-) | D8 / GND | Salida `tone()` |
| **Sensor HC-SR04** | VCC / GND | 5V / GND | Alimentación |
| **Sensor HC-SR04** | TRIG | D9 | Salida (`OUTPUT`) |
| **Sensor HC-SR04** | ECHO | D10 | Entrada (`INPUT`) |
