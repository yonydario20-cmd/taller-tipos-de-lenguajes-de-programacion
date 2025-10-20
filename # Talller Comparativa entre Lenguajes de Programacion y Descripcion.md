
## Lenguaje de Programación vs Lenguaje de Descripción de Hardware

| Característica                | Lenguaje de Programación (C, Python) | Lenguaje de Descripción de Hardware (VHDL, Verilog) |
|-------------------------------|--------------------------------------|-----------------------------------------------------|
| Propósito                     | Crear software que corre en procesadores | Describir circuitos electrónicos y lógica digital   |
| Ejecución                     | Secuencial, sobre CPU/microcontrolador | Paralela, sintetizable en hardware                  |
| Ejemplo de uso                | Control de sensores, lógica de negocio | Diseño de ALUs, registros, FSMs                     |
| Herramientas                  | Compiladores (gcc, python)           | Simuladores, sintetizadores (Vivado, Quartus)       |
| Flexibilidad                  | Alta para algoritmos y lógica         | Alta para arquitectura digital personalizada         |

---

## Comparativa entre Microcontrolador, Microprocesador, FPGA, PLD

| Dispositivo        | Descripción | Ventajas | Desventajas | Ejemplo |
|--------------------|-------------|----------|-------------|---------|
| Microcontrolador   | CPU + periféricos integrados, bajo consumo | Fácil de programar, económico | Menor potencia de procesamiento | ESP32, Arduino |
| Microprocesador    | CPU, alto rendimiento, sin periféricos integrados | Alto rendimiento, multitarea | Mayor consumo, requiere periféricos externos | Intel i7, ARM Cortex-A |
| FPGA               | Matriz de lógica programable, reconfigurable | Altamente personalizable, procesamiento paralelo | Complejidad de diseño, costo | Xilinx Basys2, Altera DE1 |
| PLD                | Dispositivo lógico programable simple | Bajo costo, rápido para lógica sencilla | Limitado en complejidad | GAL, CPLD |

---

## Lenguajes más usados en pro de hardware

- **VHDL** y **Verilog**: Para descripción y síntesis de hardware digital (FPGA, PLD).
- **C/C++**: Para programación de microcontroladores y procesadores embebidos.
- **Python**: Usado en prototipos y sistemas embebidos con alto nivel de abstracción (Raspberry Pi).

---

## Propuesta de Solución Tecnológica Embebida

### a. Procesador o Microcontrolador (Ejemplo: ESP32)

**Aplicación:** Sensor de temperatura con envío por WiFi.

**Código ejemplo (C para ESP32):**
```c
#include <WiFi.h>
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);
  WiFi.begin("SSID", "PASSWORD");
  dht.begin();
}

void loop() {
  float temp = dht.readTemperature();
  Serial.println(temp);
  delay(2000);
}
```

**Ventajas:**  
- Fácil de programar.
- Integración con sensores y comunicación inalámbrica.
- Bajo costo y consumo.

**Desventajas:**  
- Limitado en procesamiento paralelo.
- Menor flexibilidad para lógica personalizada.

---

### b. Programación de Hardware (Ejemplo: Basys2 FPGA)

**Aplicación:** Sensor digital de temperatura con visualización en display.

**Código ejemplo (Verilog para Basys2):**
```verilog
module temp_display(
    input clk,
    input [7:0] temp_data,
    output [6:0] seg,
    output [3:0] an
);
// Lógica para mostrar temp_data en display de 7 segmentos
endmodule
```

**Ventajas:**  
- Procesamiento paralelo.
- Personalización total de la arquitectura.
- Alta velocidad.

**Desventajas:**  
- Mayor complejidad de diseño.
- Requiere herramientas de síntesis y simulación.
- Costo y consumo mayores que microcontroladores simples.

---

## Hallazgos y Documentación

- Los microcontroladores son ideales para aplicaciones de bajo costo, bajo consumo y fácil desarrollo.
- Las FPGAs permiten soluciones altamente personalizadas y rápidas, pero requieren mayor conocimiento y herramientas especializadas.
- Los lenguajes de descripción de hardware son esenciales para sistemas digitales avanzados, mientras que C/C++ sigue siendo el estándar para sistemas embebidos.
- La elección depende de los requisitos de procesamiento, flexibilidad y costo del proyecto.

---

## Ventajas y Desventajas de Cada Sistema

| Sistema           | Ventajas                                   | Desventajas                          |
|-------------------|--------------------------------------------|--------------------------------------|
| Microcontrolador  | Fácil desarrollo, bajo costo, integración  | Limitado en procesamiento paralelo   |
| Microprocesador   | Alto rendimiento, multitarea               | Mayor consumo, costo                 |
| FPGA              | Personalización, procesamiento paralelo    | Complejidad, costo, herramientas     |
| PLD               | Rápido para lógica simple, económico       | Limitado en complejidad              |

---