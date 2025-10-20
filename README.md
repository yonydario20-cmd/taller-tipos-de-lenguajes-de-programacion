# 🚀 Taller Diferencias entre Lenguajes 
**Estudiante:** *Yony Dario Plazas Nemeguen*  
**Materia:** Sistemas Digitales  
**Fecha:** *14/10/2025*  

---

## 1. Comparativa entre Lenguaje de Programación y Lenguaje de Descripción de Hardware

| Característica | Lenguaje de Programación (Software) | Lenguaje de Descripción de Hardware (HDL) |
|----------------|--------------------------------------|-------------------------------------------|
| **Ejemplos** | C, C++, Python, Java | VHDL, Verilog, SystemVerilog |
| **Propósito** | Escribir instrucciones secuenciales que se ejecutan en un procesador. | Describir el comportamiento y estructura del hardware digital. |
| **Nivel de abstracción** | Alto: orientado a algoritmos y lógica secuencial. | Bajo/medio: orientado a lógica digital, temporización y señales. |
| **Ejecución** | El código se compila y se ejecuta sobre una CPU o microcontrolador. | El código se sintetiza en compuertas lógicas dentro de una FPGA o PLD. |
| **Paralelismo** | Limitado, depende del procesador (multi-hilo). | Altamente paralelo: todas las señales pueden cambiar simultáneamente. |
| **Tiempo** | Controlado por el reloj del procesador. | Controlado por relojes y eventos definidos por el diseñador. |
| **Depuración** | Debug paso a paso, uso de printf, IDEs. | Simulación temporal, análisis de señales (waveforms). |
| **Uso típico** | Control de alto nivel, comunicación, procesamiento de datos. | Diseño de sistemas digitales, controladores, filtros, procesadores. |

**Conclusión:**  
Un lenguaje de programación describe *qué hacer*, mientras que un lenguaje de descripción de hardware define *cómo se comporta físicamente el circuito* que lo hace.

---

## 2. Comparativa entre Microcontrolador, Microprocesador, FPGA y PLD

| Dispositivo | Descripción | Ventajas | Desventajas | Ejemplo |
|--------------|--------------|-----------|--------------|----------|
| **Microcontrolador (MCU)** | Integra CPU, memoria y periféricos en un solo chip. | Bajo consumo, económico, ideal para sistemas embebidos. | Limitada capacidad de cómputo y memoria. | ESP32, ATmega328 |
| **Microprocesador (MPU)** | CPU sin periféricos, requiere memoria y módulos externos. | Alto rendimiento y capacidad de procesamiento. | Mayor consumo y complejidad. | Intel i7, ARM Cortex-A53 |
| **FPGA** | Matriz de compuertas programables mediante HDL. | Alta velocidad, paralelismo, personalización total del hardware. | Costoso y más complejo de programar. | Xilinx Basys2, Altera Cyclone |
| **PLD** | Dispositivo lógico programable más simple (PAL, GAL, CPLD). | Configurable, menor costo que FPGA. | Menor capacidad lógica y flexibilidad. | Altera MAX7000, XC9500 |

**Conclusión:**  
Los microcontroladores dominan en aplicaciones embebidas por su simplicidad y costo. Las FPGA se usan cuando se requiere procesamiento paralelo o personalización a nivel de hardware.

---

## 3. Lenguajes más usados en pro de hardware

| Tipo de sistema | Lenguajes principales | Aplicación |
|------------------|-----------------------|-------------|
| **Programación de microcontroladores** | C / C++ / Python (MicroPython) | Control embebido, IoT, sensores. |
| **Programación de microprocesadores** | C / C++ / Ensamblador / Java | Sistemas operativos, control industrial. |
| **Descripción de hardware (FPGA/PLD)** | VHDL / Verilog / SystemVerilog | Diseño digital, controladores, DSP. |
| **Diseño mixto hardware/software** | C + VHDL / Verilog | Sistemas SoC, procesamiento acelerado. |

**Conclusión:**  
En sistemas embebidos predominan C y C++ por eficiencia y control del hardware. En diseño digital, VHDL y Verilog son los estándares industriales.

---

## 4. Propuesta de Solución Tecnológica Embebida  
### Proyecto: **Robot Delivery con ESP32**

#### a. Procesador o Microcontrolador (ESP32)
El sistema se basa en un **microcontrolador ESP32**, encargado de controlar motores, sensores y comunicación Wi-Fi. El robot puede transportar pequeños paquetes entre puntos designados, evitando obstáculos y notificando su estado por red.

**Componentes principales:**
- ESP32 DevKit V1  
- 2 motores DC + driver L298N o TB6612FNG  
- Sensor ultrasónico HC-SR04  
- Sensor IMU MPU-6050  
- Servomotor para apertura de compartimento  
- Batería Li-ion 11.1V  
- Conexión Wi-Fi para control remoto  

**Funcionamiento general:**
1. El usuario envía una orden desde una app o página web.  
2. El ESP32 recibe la instrucción por Wi-Fi.  
3. El robot calcula la ruta hacia el destino y avanza.  
4. Los sensores detectan obstáculos y el robot corrige su trayectoria.  
5. Al llegar, abre el compartimento para entregar el paquete.  
6. Envía una notificación de entrega exitosa.  

---

#### b. Programación de Hardware (Basys2 / FPGA)
Como alternativa, se puede implementar el **control PWM de motores** o el **contador de encoders** en una FPGA (Basys2) usando **VHDL**, mejorando la precisión del control en tiempo real.

**Ejemplo en VHDL (PWM básico):**
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity pwm_driver is
  Port (
    clk   : in  STD_LOGIC;
    duty  : in  INTEGER range 0 to 255;
    pwm_out : out STD_LOGIC
  );
end pwm_driver;

architecture Behavioral of pwm_driver is
  signal counter : INTEGER := 0;
begin
  process(clk)
  begin
    if rising_edge(clk) then
      if counter < duty then
        pwm_out <= '1';
      else
        pwm_out <= '0';
      end if;
      counter <= (counter + 1) mod 256;
    end if;
  end process;
end Behavioral;
```

---

## 5. Código Base (ESP32 - C++ / Arduino)

```cpp
#define TRIG 5
#define ECHO 18
#define M1A 25
#define M1B 26
#define EN1 27

void setup() {
  Serial.begin(115200);
  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);
  pinMode(M1A, OUTPUT);
  pinMode(M1B, OUTPUT);
  pinMode(EN1, OUTPUT);
}

long medirDistancia() {
  digitalWrite(TRIG, LOW); delayMicroseconds(2);
  digitalWrite(TRIG, HIGH); delayMicroseconds(10);
  digitalWrite(TRIG, LOW);
  long tiempo = pulseIn(ECHO, HIGH);
  return tiempo * 0.034 / 2;  // en cm
}

void loop() {
  long d = medirDistancia();
  if (d < 20) { // obstáculo detectado
    digitalWrite(M1A, LOW);
    digitalWrite(M1B, LOW);
  } else {
    digitalWrite(M1A, HIGH);
    digitalWrite(M1B, LOW);
    analogWrite(EN1, 180);
  }
  delay(100);
}
```
---

## 6. Ventajas y Desventajas de Cada Sistema

| Sistema | Ventajas | Desventajas |
|----------|-----------|-------------|
| **ESP32 (microcontrolador)** | Bajo costo, Wi-Fi/Bluetooth integrado, fácil programación, soporte IoT. | Limitado en procesamiento paralelo, menor precisión temporal. |
| **Basys2 (FPGA)** | Alta precisión temporal, paralelismo, personalización completa. | Mayor complejidad, requiere HDL y herramientas especializadas. |
| **Combinado (ESP32 + FPGA)** | Equilibrio entre control lógico y conectividad. | Mayor costo y tiempo de desarrollo. |

---

## 7. Conclusiones
- Los **lenguajes de programación** y los **lenguajes de descripción de hardware** cumplen funciones complementarias: uno controla, el otro define el hardware que controla.  
- En proyectos embebidos, los **microcontroladores** como el **ESP32** son más prácticos y económicos para prototipos funcionales.  
- Las **FPGA** aportan precisión y paralelismo, útiles en módulos de control rápido o procesamiento de señales.  
- La integración de ambos enfoques permite diseñar sistemas embebidos potentes, como este robot delivery inteligente.

---

## 8. Referencias
- GeeksforGeeks: *Difference Between HDL and Programming Language*  
- AllAboutCircuits: *What is a Hardware Description Language (HDL)*  
- Wikipedia: *Lenguaje de descripción de hardware*  
- Espressif Docs: *ESP32 Technical Reference Manual*  
- Xilinx Basys2 Reference Manual  
