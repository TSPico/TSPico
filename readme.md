# <p align="center"> TS Pico </p>

## <img src="/static/english.png" alt="Español" width="50"> </img>     [English version here](readme_en.md)

<img src="/static/spanish.png" alt="Español" width="50"> 

## <p align="center"> Interconexión de una TS-2068 (y probablemente otros microcomputadores basados en Z80) con un Raspberry Pico,para cargar/grabar programas y enviar/recibir datos arbitrarios </p>

Luego de más un año de desarrollo, varios prototipos fallidos y una TS 2068 seriamente averiada en el camino, la interface TS-Pico finalmente está en condiciones de ser presentada en público. La misma permite vincular una TS 2068 (y, con modificaciones, seguramente muchos otros sistemas basados en Z80) a un Rasberry Pico (R-Pico) para almacenamiento y carga de programas (.TAP y .TZX) e imágenes (.SNA, .Z80). La interface además permite la conmutación de ROMs tanto pricipal (HOME ROM) como extendida (EX-ROM), con imágenes almacenadas en una memoria flash externa (no es necesaria NINGUNA MODIFICACIÓN INTERNA al hardware original), y por el mismo método, la simulación de cartuchos en formato .DCK. 


El hardware de la interface es totalmente genérico, consistiendo básicamente en un R-Pico (cualquier variedad en principio serviría), algunos integrados discretos de la línea 74xx, una memoria Flash para almacenar las ROMs y las imágenes de cartucho, algunas resistencias, capacitores, interruptores y poco más. Lo más complicado de conseguir (al menos en Argentina) es el conector de bus trasero, y dos integrados 74LVC245 de control de bus y adaptación de niveles lógicos. Obviamente, también se necesita la placa impresa, cuyo diseño forma parte del proyecto. En caso de que el almacenamiento provisto por la flash del R-Pico (2MB o 16MB dependiendo del modelo) no sea suficiente para almacenar los archivos (.TAP, .SNA, .DCK etc), se puede adicionar una tarjeta SD, aunque no es imprescindible. El costo total no debería superar los 20-30 dólares.

	
En cuanto al software, la totalidad del programa de control de la TS-Pico está desarrollada en uPython (MicroPython) y PIO (Parallel Input/Output) en el R-Pico, y rutinas LOAD y SAVE modificadas en la ROM de la TS 2068 (assembler Z80). 

	
La comunicación TS-Raspberry también se podría realizar mediante un programa (en Assembler o BASIC) corriendo en la TS, y así enviar comandos y recibir respuestas del microcontrolador. Además. por intermedio del R-Pico, se puede a su vez acceder a la PC o notebook conectada al mismo, para descargar archivos. 

	
La memoria Flash de la interface podrá ser programada desde el R-Pico, mediante un programa específico en uPython; para agregar imágenes ROM customizadas y programas en cartucho. Con una memoria Flash de 4 Mbits, se podrán almacenar hasta 16 imágenes ROM de TS 2068 (16K para HOME y 16K para EX-ROM), 32 imágenes de Spectrum o TC2048 (solamente 16K de ROM), o una combinación de ambos tipos. Además de ello, se podrán almacenar hasta 4 imágenes de cartucho para ser accesibles al momento de booteo.

	
Salvo algunas cuestiones de ajuste fino en algunas temporizaciones, el grueso del desarrollo está casi terminado. A continuación las características más importantes de la TS-Rpi:

- Comunicación bidireccional entre la TS 2068 y un R-Pico.
- Carga y almacenamiento de programas, tanto BASIC como lenguaje máquina, desde archivos .TAP y .TZX. Actualmente, el almacenamiento solo es posible en formato .TAP, aunque una utilidad externa (también en uPython) podrá convertir el .TAP a .TZX
- Hasta donde sé, es LA UNICA interface que permite la conmutación EXTERNA de ambas ROMs TS 2068, tanto HOME como EX-ROM. No es necesaria ABSOLUTAMENTE NINGUNA modificación del hardware original de la TS 2068
- La misma interface sirve para actualizar las ROMs e imágenes DCK almacenadas en la Flash, usadas por la TS 2068. Estas ROMs o DCKs pueden estar almacenadas en el R-Pico o en una tarjeta SD
- Alta velocidad de operación. La velocidad de carga de un programa completo es de alrededor de un segundo.
- Plataforma abierta, modificable y programable por la comunidad de usuarios. Los programas de control de la interface están programados en uPython/asm PIO y assembler Z80
- Los dos puntos mencionados antes, implican que a futuro se puedan incorporar rutinas específicas. Por ejemplo se podría pensar en una rutina de “page-in, page-out” de memoria desde y hacia el  R-Pico. El programador podría tener a disposición vuelcos de memoria solamente limitados por la capacidad de almacenamiento en la SD o la flash del R-Pico. Imaginemos un Jet Set Willy, con niveles casi infinitos!
- Se prevé una rutina de intercambio de comandos/respuesta entre la TS y el R-Pico, que a su vez servirá para integrar funciones adicionales futuras, como ser acceso a Internet, o una red de comunicación con otros micros e interfases similares
- También se prevé el desarrollo de una rutina de procesamiento de interrupción (NMI) para salvar el estado de la máquina en formato .SNA y recuperarlo posteriormente.
- La línea /WAIT puede ser controlada por el R-Pico, para poner en pausa el procesador durante operaciones de IO extensas, o para cualquier otro propósito que se desee
- Interface de Joystick estilo Kempston y botón de reset incorporados.
 

La administración de la interface puede darse, como actualmente, interactuando con el R-Pico mediante la conexión USB y la PC/notebook (para el desarrollo se utilizó Thonny por ejemplo), o bien utilizando un front-end, programado en BASIC y lenguaje máquina. Este front end está pendiente de desarrollo actualmente.


## <p align="center">  VIDEOS </p>


A continuación, un par de videos de la placa en funcionamiento. Para más detalles, ver las referencias en el mismo video


VIDEO #1:
https://youtu.be/ASCslXNy0ak



VIDEO #2:
https://youtu.be/zoI_xMJVHv8


## <p align="center"> CREDITOS </p>

El proyecto en su totalidad es (c) 2022 RICARDO JAVIER CALCAGNO, de LIBRE DISTRIBUCION para su uso recreacional y privado. El uso comercial se encuentra TERMINANTEMENTE PROHIBIDO, salvo expresa autorización de su autor. Asimismo, DEBE CITARSE este trabajo en cualquier desarrollo posterior basado en el mismo.
El desarrollo de la interface fue posible, y se basó en gran medida, en los siguientes proyectos:

- El grueso del desarrollo se basa en el trabajo realizado a comienzos de 2000 por Dr Beep, para su interface PC-ZX, originalmente pensada para el ZX Spectrum. El proyecto puede consultarse aquí:
	  
https://trastero.speccy.org/cosas/JL/DrBeep/PC-ZX.html

De allí se tomaron y adaptaron el circuito, las rutinas SAVE y LOAD en assembler Z80, y se adaptó el programa Pascal a uPython, para la carga de .TAPs, .SNAs y .Z80s. También la rutina de impresión de información en la TS (ZX) originada en el R-Pico (PC). Esto permitía consultar el directorio de información, montar, renombrar o borrar archivos, etc. Esta funcionalidad se incorporará en un futuro a la interface

- La idea de utilizar un R-Pico para implementar la comunicación se tomó de “Life with David”:
	  
https://www.youtube.com/c/LifewithDavid1/videos

principalmente los episodios “Captain Pico – The PIO chronicles” 1 a 5, para el uso de PIO como forma de comunicación de alta velocidad entre los buses

- Una introducción más completa a PIO se encuentra en:
https://www.youtube.com/watch?v=yYnQYF_Xa8g

- Para optimizar la velocidad de ejecución de uPython y consecuentemente la transferencia de datos, se utilizó este video:
https://www.youtube.com/watch?v=hHec4qL00x0

- Las rutinas de escritura en Flash mediante R-Pico serán similares a las que se pueden encontrar aquí:
https://github.com/gfoot/picoprom

- Las rutinas de conversión de .TAP a .TZX (y viceversa) fueron adaptadas de:
https://github.com/shred/tzxtools
	


## <p align="center"> DESCRIPCION GENERAL </p>


Este es un esquema en bloques de la interface:
(*a completar*)

Los accesos a determinadas áreas de memoria (ROM, EXROM, etc) y determinadas operaciones (RD, WR, MREQ, IORQ) se decodifican para así intercalar datos provenientes de la Flash (para accesos ROM, EXROM o DOCK) o desde un archivo de almacenamiento, en caso de lectura o escritura de datos. 

El cálculo de temporizaciones es especialmente crítico, ya que pequeñas desviaciones ocasionan errores  en la carga, lectura y accesos a las ROMs alojadas en la flash.

La interface hace un uso intensivo de los PIOs del R-Pico para dos fines principales:


- “Conmutación” de las ROMs internas (y las imágenes de cartucho) de la TS con las imágenes almacenadas en la Flash; para ello se manipulan las direcciones superiores de la Flash (A18-A15), poniendo en 0 o 1 respectivamente las mismas en función del acceso a memoria decodificado. Esta conmutación de ROMs externas debería permitir la implementación, por ejemplo, de acceso a unidades de disco (Zebra, Oliger, LarKen, FDD, etc.)

- Para la transmisión/recepción de octetos desde/hacia la TS, principalmente en las funciones SAVE y LOAD (implementadas), el módulo de comandos/respuesta o futuras operaciones de I/O entre la TS y el R-Pico (a implementar)

- En ambos casos, las señales de control (MREQ, IORQ, RD, WR, etc) también son accedidas por las máquinas de estado (SMs) de los PIOs para su correcto procesamiento

El resto de la interface, como dijimos antes,  consiste en integrados discretos para la decodificación de las señales de control, la adaptación de voltajes entre las señales de la TS (5V) y el R-Pico (3.3v), y el manejo y asignación del bus de datos. Toda la lógica de procesamiento de las señales de control, la asignación de las ROMs/DCKs de la flash y la transferencia de datos (SAVE/LOAD etc) se implementan mediantes programas uPython, totalmente modificables y configurables por el usuario.

## <p align="center">  DETALLES TECNICOS </p>


Como la interface es un prototipo y un trabajo en progreso, la lista de componentes podría variar, pero sirve como orientativa de la sencillez de la misma:

- 1 (un) Raspberry Pico (cualquier versión puede servir; con la que tiene 16MB de Flash no sería necesario en principio contar con una SD.
- 1 (un) integrado Flash, por ejemplo de la familia 39SF (en el prototipo se utilizó un 39SF040 de 4Mbits). Este puede ser sustituido en el diseño por integrados de tipo EEPROM, aunque su costo puede llegar a ser superior
- Integrados de la familia 74xx: 74LS04, 74LS32, 74LS27, 74LS257: uno cada uno. Para la programación de la flash se necesitan dos 74HCT595 (serial/paralelo); y para la adaptación de niveles lógicos dos 74LVC245. Para el joystick es necesario un 74LS366
- Si se desea almacenar los archivos .TAP, .TZX, .SNA, .Z80, .DCK etc en una SD Card, es necesario el módulo lector y las bibliotecas para uPython (sdcard.py)
- Se necesita también la placa impresa, y el conector de borde de 2x32 conectores.
- Zócalos y pines para los integrados mencionados anteriormente
- Un número de capacitores de desacople (0.1 uF), interruptores y jumpers


## <p align="center">  ESTADO DEL PROYECTO </p>


Esta planilla muestra el grado de avance de los distintos componentes del proyecto:


*(a completar)*



## <p align="center">  FUTURO </p>


Algunas características que se podrían agregar a futuro:

- Una versión con componentes SMD, para reducir el tamaño
- Implementación de funciones de vuelco de regiones de memoria desde/hacia SD (símil memoria virtual)
- Emulación de unidades de disco (Zebra, Oliger, LarKen, FDD, etc.)
- *(a completar)*



## <p align="center">  MAS INFORMACION </p>


Hasta que habilite la sección de Discusión y la WiKi del proyecto en esta misma página, dejo una entrada a los dos grupos de discusión donde se podrá consultar más información actualizada, y también dejo un correo electrónico por cualquier consulta puntual:


- Entrada grupo Timex Sinclair 2068 (en inglés): *(a completar)*
- Entrada foro Retrocomputacion: *(a completar)*
- Consultas por correo electrónico: tspico.info@gmail.com


(c) 2022 - RICARDO JAVIER CALCAGNO
