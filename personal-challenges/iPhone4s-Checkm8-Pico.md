# 📱 iPhone 4s Passcode Bypass & Jailbreak (A5 SoC)

> **Choose your language / Selecciona tu idioma:**
> - [English Version](#english-version)
> - [Versión en Español](#versión-en-español)

---

# English Version

**Category:** Hardware Hacking / Mobile Forensics  
**Target:** iPhone 4s (iPhone 4,1) - SoC Apple A5  
**Exploit:** checkm8 (BootROM Vulnerability)  
**Hardware:** Raspberry Pi Pico (RP2040)  

---

## 1. Overview
This write-up documents the technical process of compromising an **iPhone 4s** security using the **checkm8** hardware exploit. The primary objective was to bypass the passcode via brute force and achieve a persistent jailbreak with **Cydia**, all while **preserving the device's user data without restoration or wiping.**

Unlike newer devices, the A5 chip requires extreme precision in USB timings, necessitating an external microcontroller like the **Raspberry Pi Pico**.

---

## 2. Technical Context (The "Why")
The **checkm8** exploit leverages a *Use-After-Free* (UAF) vulnerability in Apple's BootROM USB stack. 

### The A5 Chip Challenge
According to *Elcomsoft* research, the A5 SoC's USB controller is highly unstable when exploitation is attempted from a conventional OS (macOS/Linux). The **Raspberry Pi Pico** is used for:
* **Heap Grooming:** Manipulating the iPhone's memory with microsecond precision.
* **USB Bit-banging:** The RP2040 allows for "raw" malformed USB packets that bypass the latency inherent in standard PC USB controllers.

---

## 3. Environment & Tools
| Component | Tool / Hardware |
| :--- | :--- |
| **Exploit Host** | Raspberry Pi Pico (RP2040) |
| **Pwned DFU Firmware** | `checkm8-a5` (UF2 format) |
| **Bruteforce Script** | `iwannabrute` |
| **Post-Exploitation** | Legacy iOS Toolkit |
| **Package Manager** | Cydia |

<p align="center">
  <img src="https://github.com/user-attachments/assets/ab6693a0-f1b7-43fd-bbf8-5897ceb0e1c8" width="60%" alt="Cydia Success" />
  <br>
  <em>Materials for Exploit with Pi Pico</em>
</p>

---

## 4. Execution Flow

### Phase I: Pwned DFU Mode (The Hardware Entry)
1. **Preparation:** Flashed the Raspberry Pi Pico with `picom8.uf2` firmware.
2. **DFU Mode:** Placed the iPhone 4s into DFU mode.
3. **Exploitation:** Connected the iPhone to the Pico. The Pico detected the device and automatically executed the USB payload.
4. **Verification:** The Pico’s LED confirmed the device entered **Pwned DFU** state (Signature Check Disabled).

<p align="center">
  <img src="https://github.com/user-attachments/assets/6457258a-e149-4551-888b-76efa7970c6b" width="60%" alt="Cydia Success" />
  <br>
  <em>Exploit with Pi Pico</em>
</p>


### Phase II: Passcode Bruteforce
With the device accepting unsigned code:
1. **Ramdisk Deployment:** Uploaded an SSH ramdisk to the iPhone’s volatile memory.
2. **Bypassing Restrictions:** Operating outside the main iOS environment allowed us to disable "Auto-Wipe" and delay counters.
3. **iwannabrute:** Ran the brute force script to test the `0000-9999` range directly against the device's Keybag.
4. **Result:** Passcode successfully recovered.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7fcb6627-4892-4a55-815a-a2f90aa41a9b" width="45%" />
  <img src="https://github.com/user-attachments/assets/abe62b7e-d258-43b1-ada1-232048ee4307" width="45%" />
  <br>
  <em>Left: Execution of iwannabrute | Right: Obtencion of the device's pin</em>
</p>

### Phase III: Jailbreak & Cydia
1. **Legacy iOS Toolkit:** Connected the iPhone to the host PC and launched the toolkit.
2. **Exploit Path:** Selected the `Jailbreak` option for 32-bit devices.
3. **Cydia Installation:** The toolkit automated binary injection and Cydia deployment.
4. **Final State:** Device rebooted with full Root access and a functional **Cydia** app.

<p align="center">
  <img src="https://github.com/user-attachments/assets/10bf1042-c05d-4acd-8138-c140b1f97555" width="60%" alt="Cydia Success" />
  <br>
  <em>Jailbreaking</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ca7bc984-067a-42ac-aa09-9d53aa7274e9" width="60%" alt="Cydia Success" />
  <br>
  <em>End result: Root access and functional Cydia while maintaining 100% of the device's original data.</em>
</p>

---

## 5. Key Takeaways
* **Unpatchable:** As a BootROM vulnerability, it resides in the hardware's read-only memory, making it impossible for Apple to patch via software updates for the A5 SoC.
* **Hardware Superiority:** Using a microcontroller like the **Raspberry Pi Pico** increases the exploit success rate to nearly 100% on A5 devices by eliminating the USB stack latency inherent in standard PC operating systems.
* **Forensic Impact:** This workflow demonstrates that physical security is the weakest link when hardware-level flaws exist, allowing for total data extraction and security bypass on legacy devices.
* **Pico Versatility:** The use of the RP2040 as a payload injector proves to be a cost-effective and highly reliable alternative to expensive commercial forensic solutions.

---

## 6. Credits & References

* **Exploit Discovery:** [axi0mX](https://github.com/axi0mX) (checkm8).
* **Technical Research:** [Elcomsoft Blog](https://blog.elcomsoft.com/2022/05/checkm8-unlocking-and-imaging-the-iphone-4s/) - Analysis of A5 SoC USB timings and Raspberry Pi Pico implementation.
* **Jailbreak Tool:** [Legacy iOS Toolkit](https://github.com/LukeZGD/Legacy-iOS-Toolkit) by LukeZGD.
* **Bruteforce Tool:** [iwannabrute](https://github.com/platinumstufff/iwannabrute/tree/2.0) - iOS Security Research community.
* **Hardware Implementation:** `checkm8-a5` implementation for RP2040 microcontrollers.

## 7. Forensic Value
This methodology is highly relevant in digital forensics for the following reasons:
* **Data Integrity:** The process allows for **Physical Acquisition** without triggering the device's security wipe mechanisms.
* **Evidence Access:** By gaining Root access through Cydia/SSH, an examiner can extract sensitive databases such as `sms.db`, `CallHistory.storedata`, and third-party app data that are otherwise encrypted or inaccessible.
* **Legacy Recovery:** It provides a reliable path to recover information from legacy devices where the owner has forgotten the passcode.

---
> **Disclaimer:** This document was generated for educational and offensive security research purposes only.

![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

# Versión en Español

**Category:** Hardware Hacking / Mobile Forensics  
**Target:** iPhone 4s (iPhone 4,1) - SoC Apple A5  
**Exploit:** checkm8 (BootROM Vulnerability)  
**Hardware:** Raspberry Pi Pico (RP2040)  

---

## 1. Overview
Este Write-up documenta el proceso técnico para comprometer la seguridad de un **iPhone 4s** mediante el exploit de hardware **checkm8**. El objetivo principal fue saltar el código de bloqueo (Passcode) mediante fuerza bruta y realizar un jailbreak persistente con **Cydia**, **manteniendo toda la información del dispositivo intacta y sin necesidad de restaurarlo o borrarlo.**

A diferencia de otros dispositivos, el chip A5 requiere una precisión extrema en los *timings* USB, lo que hace necesaria la intervención de un microcontrolador externo como la **Raspberry Pi Pico**.

---

## 2. Technical Context (The "Why")
El exploit **checkm8** aprovecha una vulnerabilidad de tipo *Use-After-Free* (UAF) en la pila USB del BootROM de Apple. 

### El desafío del Chip A5
Según investigaciones de *Elcomsoft*, el controlador USB del SoC A5 es altamente inestable cuando se intenta explotar desde un sistema operativo convencional (macOS/Linux). La **Raspberry Pi Pico** se utiliza para:
* **Grooming del Heap:** Manipular la memoria del iPhone con precisión de microsegundos.
* **Bit-banging USB:** El RP2040 de la Pico permite enviar paquetes malformados de forma mucho más cruda y directa que un controlador USB de PC estándar.

---

## 3. Environment & Tools
| Componente | Herramienta/Hardware |
| :--- | :--- |
| **Host Exploit** | Raspberry Pi Pico (RP2040) |
| **Pwned DFU Firmware** | `checkm8-a5` (UF2 format) |
| **Bruteforce Script** | `iwannabrute` |
| **Post-Exploitation** | Legacy iOS Toolkit |
| **Package Manager** | Cydia |

<p align="center">
  <img src="https://github.com/user-attachments/assets/ab6693a0-f1b7-43fd-bbf8-5897ceb0e1c8" width="60%" alt="Cydia Success" />
  <br>
  <em>Materiales para explotación con la Pico</em>
</p>


---

## 4. Execution Flow

### Phase I: Pwned DFU Mode (The Hardware Entry)
1.  **Preparation:** Se flasheó la Raspberry Pi Pico con el firmware `picom8.uf2`.
2.  **DFU Mode:** Se puso el iPhone 4s en modo DFU (Device Firmware Update).
3.  **Exploitation:** Se conectó el iPhone a la Pico. La Pico detectó el dispositivo y ejecutó el payload USB automáticamente.
4.  **Verification:** El LED de la Pico confirmó que el dispositivo entró en estado **Pwned DFU** (Signature Check Disabled).

<p align="center">
  <img src="https://github.com/user-attachments/assets/6457258a-e149-4551-888b-76efa7970c6b" width="60%" alt="Cydia Success" />
  <br>
  <em>Explotación con la Pico</em>
</p>

### Phase II: Passcode Bruteforce
Con el dispositivo aceptando código no firmado, se procedió a la recuperación del acceso.
1.  **Ramdisk Deployment:** Se cargó un ramdisk SSH en la memoria volátil del iPhone.
2.  **Bypassing Restrictions:** Al operar desde fuera del sistema operativo iOS, se desactivaron las medidas de seguridad como el "Auto-Wipe" tras 10 intentos fallidos.
3.  **iwannabrute:** Se ejecutó el script de fuerza bruta para probar el rango `0000-9999` directamente contra el Keybag del dispositivo.
4.  **Result:** El código fue recuperado exitosamente.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7fcb6627-4892-4a55-815a-a2f90aa41a9b" width="45%" />
  <img src="https://github.com/user-attachments/assets/abe62b7e-d258-43b1-ada1-232048ee4307" width="45%" />
  <br>
  <em>Izquierda: Ejecución de iwannabrute | Derecha: Obtención del pin del dispositivo</em>
</p>

### Phase III: Jailbreak & Cydia
Para obtener persistencia y control total de la partición de sistema:
1.  **Legacy iOS Toolkit:** Se conectó el iPhone a la PC host y se ejecutó el toolkit.
2.  **Exploit Path:** Se seleccionó la opción `Jailbreak` dentro del menú del toolkit.
3.  **Cydia Installation:** El toolkit automatizó la inyección de los binarios del sistema de archivos y la instalación de Cydia.
4.  **Final State:** El dispositivo reinició con acceso Root completo y el gestor de paquetes **Cydia** funcional.

<p align="center">
  <img src="https://github.com/user-attachments/assets/10bf1042-c05d-4acd-8138-c140b1f97555" width="60%" alt="Cydia Success" />
  <br>
  <em>Proceso de Jailbreak</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/ca7bc984-067a-42ac-aa09-9d53aa7274e9" width="60%" alt="Cydia Success" />
  <br>
  <em>Resultado final: Acceso root y Cydia funcional manteniendo el 100% de los datos originales del dispositivo.</em>
</p>

---

## 5. Key Takeaways
* **Unpatchable:** Al ser una vulnerabilidad en el BootROM (ROM de máscara), Apple no puede parchearla mediante actualizaciones de software en el SoC A5.
* **Hardware Superiority:** El uso de microcontroladores como la **Raspberry Pi Pico** eleva la tasa de éxito del exploit casi al 100% en dispositivos A5, al eliminar la latencia de los controladores USB estándar de PC.
* **Forensic Impact:** Este flujo de trabajo permite la extracción total de datos y el bypass de seguridad en dispositivos legacy, demostrando que la seguridad física es el eslabón más débil si el hardware tiene vulnerabilidades a nivel de silicio.
* **Versatilidad de la Pico:** El uso del RP2040 como inyector de payloads demuestra ser una alternativa económica y eficiente a las soluciones comerciales forenses.

---

## 6. Credits & References
* **Exploit Discovery:** [axi0mX](https://github.com/axi0mX) (checkm8).
* **Technical Research:** [Elcomsoft Blog](https://blog.elcomsoft.com/2022/05/checkm8-unlocking-and-imaging-the-iphone-4s/) - Análisis sobre los timings del SoC A5 y el uso de la Raspberry Pi Pico.
* **Jailbreak Tool:** [Legacy iOS Toolkit](https://github.com/LukeZGD/Legacy-iOS-Toolkit) by LukeZGD.
* **Bruteforce Tool:** [iwannabrute](https://github.com/platinumstufff/iwannabrute/tree/2.0) de la comunidad de investigación de iOS.
* **Hardware Implementation:** Implementación de `checkm8-a5` para microcontroladores RP2040.

## 7. Valor Forense
Esta metodología es altamente relevante en la informática forense por las siguientes razones:
* **Integridad de los Datos:** El proceso permite una **Adquisición Física** sin activar los mecanismos de borrado de seguridad del dispositivo.
* **Acceso a Evidencia:** Al obtener acceso Root mediante Cydia/SSH, un examinador puede extraer bases de datos críticas como `sms.db`, `CallHistory.storedata` y datos de aplicaciones de terceros que de otro modo estarían cifrados o inaccesibles.
* **Recuperación de Legados:** Proporciona una vía confiable para recuperar información en dispositivos antiguos donde el propietario ha olvidado su código de acceso.
---
> **Aviso Legal:** Este documento fue generado con fines educativos y de investigación en seguridad ofensiva únicamente.


![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
