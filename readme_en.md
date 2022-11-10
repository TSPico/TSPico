<h1 align="center">TS Pico</h1>

<img src="/static/english.png" alt="English" width="50"> 
<h3 align="center">Connecting a TS-2068 (and probably other Z80-based microcomputers) to a Raspberry Pico, to load/save programs and sending/receiving arbitrary data
</h3>

---

After over a year of development, many failed prototypes, and a seriously damaged TS 2068 on the road, the TS-Pico interface is finally ready to be presented in public. It allows connecting a TS 2068 (and, with some modifications, many other Z80-based systems) to a Raspberry Pico (R-Pico) to load and save programs (in .TAP and .TZX formats) and savestates (.SNA, .Z80). The interface also performs ROMs switching, both main (HOME) ROM and extended (EX) ROM, with ROMS stored in an external flash memory (absolutely **NO INTERNAL MODIFICATION** to the original hardware or case opening is required), and by the same mechanism, simulation of cartridges in .DCK format 

The hardware is completely generic, consisting primary of a R-Pico (any variation should work), some 74xx family discrete ICs, a flash memory for storing ROMs and DCK images, some resistors, capacitors, switches/jumpers and little more. The most tricky components (at least in Argentina) would be the rear bus connector, and two 74LVC245 bus control and level adapter ICs. Obviously, the PCB is also needed. Its design is part of this project and will be available freely. In case the storage provided by the R-Pico flash (2MB or 16MB depending on the model) is not enough for storing .TAP, .SNA, .DCK etc, a SD card can be added, although it is not required. Total cost should be around $20-$30 US dollars

As for the software, the control program is completely implemented natively in MicroPython (uPython) and PIO (Parallel Input/Output) on the R-Pico, and modified LOAD, SAVE and some more on the TS 2068 ROM (Z80 assembler)

Communication between TS-R-Pico can also be controlled by a BASIC/Assembler program running on the TS side, thus sending and receiving commands/responses from the microcontroller. By its intermediate, the TS could also acces the PC/notebook attached to the R-Pico, in order to download files or otherwise exchange information

Flash memory on the interface can be programmed from the R-Pico, by a specific uPython program, in order to add customized ROMs and cartridge images. With a 4Mbit 39SF040 flash, up to 16 TS 2068 images slots are available (two 16K blocks each), 32 Spectrum/TC2048 images, or any combination of the two. Additionally, up to 4 64K cartridge images can be stored to be used at boot time. Any ROM and cartridge image can be selected arbitrarily by manipulating the flash upper address lines (A14-A18 depending on the case)  

Right now there are some fine timing issues to be addressed; other than that the functionality is completed. See videos below for clarification. Also see the links at the end of the document for further info

These are the most prominent features of the TS Pico interface:

- Bidirectional communication between TS 2068 and a R-Pico.
- Load and save programs, both BASIC and machine language, from .TAP and .TZX. Right now, storing is only supported for .TAP format, although an external utility (also a uPython program) will be able to convert from .TAP to .TZX
- As far as I can tell, this is **THE ONLY** interface that EXTERNALLY switches any or both TS 2068 ROMs, HOME and EX-ROM. **ABSOLUTELY NO MODIFICATION** of the TS-2068 hardware is required 
- The same board can be used to update the ROMs and DCK images stored on the Flash, used by the TS 2068. These ROMs or DCKs can be stored on the R-Pico or on an external SD card
- High data transfer speed. A full program can be loaded in around one second
- Open plaftorm, modifiable and programmable by the users community. All interface control program are developed in uPython/PIO asm and Z80 assembler
- Both items above combined, imply that specific routines can be added later. For example, one could think in a "page-in/page-out" routine that would dump entire memory regions between two specific address, to or from the R-Pico. The programmer would have at its disposal memory dumps limited only by the amount of external flash/SD memory available. Imagine a Jet Set Willy with (nearly) infinite levels!
- A routine for exchange of commands/responses between the TS and the R-Pico is to be developed, that would in turn allow future functions, such as Internet access, or a network of similar devices and interfaces
- Also a NMI processing routine is to be developed, to allow state save in .SNA format for future restoring.
- The /WAIT line can be programatically controlled by the R-Pico, to pause the processor during extensive IO operations (mainly intended for the above features), or any other purpose
- A Kempston joystick and a reset button are incorporated

The management of the interface can be, as now, interacting with the R-Pico via the USB connection and the attached PC/notebook (I used Thonny for development and testing purposes), or using a front-end, either in BASIC, assembler or a combination of the two. This front-end program is pending of development as now.

<h3 align="center">
VIDEOS
</h3>

---

These are two videos showing the interface in action. Please see the videos references for further reference


VIDEO #1:
https://youtu.be/ASCslXNy0ak



VIDEO #2:
https://youtu.be/zoI_xMJVHv8

<h3 align="center">
PROJECT LICENSE. CREDITS
</h3>

---

The whole project is completely (c) **2022 RICARDO JAVIER CALCAGNO**, for **FREE DISTRIBUTION** of all its components for private and recreational use only. Any commercial purposes is **COMPLETELY PROHIBITED**, unless specifically permitted by the author. Also, this work **MUST BE MENTIONED** in any further development based on it. This interface was made possible by, and is largely based on, the following work:

- The main contribution was from the work made in early 2000s by Dr Beep, for its PC-ZX interface, originally developed for the ZX Spectrum. It was the inspiration for this project. You can access it here:
	  
https://trastero.speccy.org/cosas/JL/DrBeep/PC-ZX.html

The logic circuit, the modified SAVE/LOAD Z80 assembler routines were taken from here, and the original Pascal program was ported to uPython, with obvious modifications, to support loading .TAP, .SNA and .Z80 files. Also the routine that prints data from the R-Pico (PC) on the TS (ZX) was adapted from here. This originally allowed printing the directory of files, mounting, renaming or deleting files, etc. This functionality is yet to be developed for the TS Pico

- The idea of using a R-Pico for communication was taken from “Life with David”:
	  
https://www.youtube.com/c/LifewithDavid1/videos

mainly the episodes "Captain Pico – The PIO chronicles” 1 to 5, for using PIO as a high speed communication line between the two buses

- A complete introduction to PIO can be found in:
https://www.youtube.com/watch?v=yYnQYF_Xa8g

- For uPython execution optimization purposes, and consecuently speeding up data transfer, this video was used:
https://www.youtube.com/watch?v=hHec4qL00x0

- Flash writing routines should be similar to the ones on this project:
https://github.com/gfoot/picoprom

- .TAP <-> .TZX file format conversion were/will be adapted from:
https://github.com/shred/tzxtools
	


<h3 align="center">
GENERAL DESCRIPTION
</h3>

---

Este es un esquema en bloques de la interface:
(*a completar*)

Los accesos a determinadas áreas de memoria (ROM, EXROM, etc) y determinadas operaciones (RD, WR, MREQ, IORQ) se decodifican para así intercalar datos provenientes de la Flash (para accesos ROM, EXROM o DOCK) o desde un archivo de almacenamiento, en caso de lectura o escritura de datos. 

El cálculo de temporizaciones es especialmente crítico, ya que pequeñas desviaciones ocasionan errores  en la carga, lectura y accesos a las ROMs alojadas en la flash.

La interface hace un uso intensivo de los PIOs del R-Pico para dos fines principales:


- “Conmutación” de las ROMs internas (y las imágenes de cartucho) de la TS con las imágenes almacenadas en la Flash; para ello se manipulan las direcciones superiores de la Flash (A18-A15), poniendo en 0 o 1 respectivamente las mismas en función del acceso a memoria decodificado. Esta conmutación de ROMs externas debería permitir la implementación, por ejemplo, de acceso a unidades de disco (Zebra, Oliger, LarKen, FDD, etc.)

- Para la transmisión/recepción de octetos desde/hacia la TS, principalmente en las funciones SAVE y LOAD (implementadas), el módulo de comandos/respuesta o futuras operaciones de I/O entre la TS y el R-Pico (a implementar)

- En ambos casos, las señales de control (MREQ, IORQ, RD, WR, etc) también son accedidas por las máquinas de estado (SMs) de los PIOs para su correcto procesamiento

El resto de la interface, como dijimos antes,  consiste en integrados discretos para la decodificación de las señales de control, la adaptación de voltajes entre las señales de la TS (5V) y el R-Pico (3.3v), y el manejo y asignación del bus de datos. Toda la lógica de procesamiento de las señales de control, la asignación de las ROMs/DCKs de la flash y la transferencia de datos (SAVE/LOAD etc) se implementan mediantes programas uPython, totalmente modificables y configurables por el usuario.

<h3> </h3>
<h3 align="center"> DETALLES TECNICOS </h3>

---

Como la interface es un prototipo y un trabajo en progreso, la lista de componentes podría variar, pero sirve como orientativa de la sencillez de la misma:

- 1 (un) Raspberry Pico (cualquier versión puede servir; con la que tiene 16MB de Flash no sería necesario en principio contar con una SD.
- 1 (un) integrado Flash, por ejemplo de la familia 39SF (en el prototipo se utilizó un 39SF040 de 4Mbits). Este puede ser sustituido en el diseño por integrados de tipo EEPROM, aunque su costo puede llegar a ser superior
- Integrados de la familia 74xx: 74LS04, 74LS32, 74LS27, 74LS257: uno cada uno. Para la programación de la flash se necesitan dos 74HCT595 (serial/paralelo); y para la adaptación de niveles lógicos dos 74LVC245. Para el joystick es necesario un 74LS366
- Si se desea almacenar los archivos .TAP, .TZX, .SNA, .Z80, .DCK etc en una SD Card, es necesario el módulo lector y las bibliotecas para uPython (sdcard.py)
- Se necesita también la placa impresa, y el conector de borde de 2x32 conectores.
- Zócalos y pines para los integrados mencionados anteriormente
- Un número de capacitores de desacople (0.1 uF), interruptores y jumpers


<h3 align="center"> ESTADO DEL PROYECTO </h3>

---

Esta planilla muestra el grado de avance de los distintos componentes del proyecto:


*(a completar)*



<h3 align="center"> FUTURO </h3>

---

Algunas características que se podrían agregar a futuro:

- Una versión con componentes SMD, para reducir el tamaño
- Implementación de funciones de vuelco de regiones de memoria desde/hacia SD (símil memoria virtual)
- Emulación de unidades de disco (Zebra, Oliger, LarKen, FDD, etc.)
- *(a completar)*



<h3 align="center"> MAS INFORMACION </h3>

---

Hasta que habilite la sección de Discusión y la WiKi del proyecto en esta misma página, dejo una entrada a los dos grupos de discusión donde se podrá consultar más información actualizada, y también dejo un correo electrónico por cualquier consulta puntual:


- Entrada grupo Timex Sinclair 2068 (en inglés): *(a completar)*
- Entrada foro Retrocomputacion: *(a completar)*
- Consultas por correo electrónico: tspico.info@gmail.com


(c) 2022 - RICARDO JAVIER CALCAGNO
