# TSPico
Interface between TS 2068 and Raspberry Pico
Interface TS-Pico


- INTRODUCCION
- CREDITO
	Dr. Beep
	Life with David (Pico)
	Stacksmashing (Pico)
	Optimización uPython
	https://github.com/gfoot/picoprom (Flash)
	https://github.com/shred/tzxtools (tzx)

- DESCRIPCION GENERAL
- DETALLES TECNICOS



INTRODUCCION

	Luego de más de dos años de desarrollo, varios prototipos fallidos, una TS 2068 seriamente averiada y MUCHAS cosas aprendidas en el camino, la interface TS-Pico finalmente está en condiciones de ser presentada en público. La misma permite vincular una TS 2068 (y, con modificaciones, seguramente muchos otros sistemas basados en Z80) a un Rasberry Pico (R-Pico) para almacenamiento y carga de programas (.TAP y .TZX) e imágenes (.SNA, .Z80). La interface además permite la conmutación de ROMs tanto pricipal (HOME ROM) como extendida (EX-ROM), con imágenes almacenadas en una memoria flash externa (no es necesaria NINGUNA MODIFICACIÓN INTERNA al hardware original), y por el mismo método, la simulación de cartuchos en formato .DCK. 

	El hardware de la interface es totalmente genérico, consistiendo básicamente en una R-Pico (cualquier variedad en principio serviría), algunos integrados discretos de la línea 74xx, una memoria Flash para almacenar las ROMs y las imágenes de cartucho, algunas resistencias, capacitores, interruptores y poco más. Lo más complicado de conseguir (al menos en Argentina) es el conector de bus trasero, y dos integrados 74LVC245 de control de bus y adaptación de niveles lógicos. Obviamente, también se necesita la placa impresa, cuyo diseño forma parte del proyecto. En caso de que el almacenamiento provisto por la flash del R-Pico (2MB o 16MB dependiendo del modelo) no sea suficiente para almacenar los archivos (.TAP, .SNA, .DCK etc), se puede adicionar una tarjeta SD, aunque no es imprescindible. El costo total no debería superar los 20-30 dólares.

	En cuanto al software, la totalidad del programa de control de la TS-Pico está desarrollada en uPython (MicroPython) y asm PIO, en el R-Pico, y rutinas LOAD y SAVE modificadas en la ROM de la TS 2068 (assembler Z80). 

	La comunicación TS-Raspberry también se podría realizar mediante un programa (en Assembler o BASIC) corriendo en la TS, y así enviar comandos y recibir respuestas del microcontrolador. Además. por intermedio del R-Pico, se puede a su vez acceder a la PC o notebook conectada al mismo, para descargar archivos. 

	La memoria Flash de la interface podrá ser programada desde la R-Pico, mediante un programa específico en uPython; para agregar imágenes ROM customizadas y programas en cartucho. Con una memoria Flash de 4 Mbits, se podrán almacenar hasta 16 imágenes ROM de TS 2068 (16K para HOME y 16K para EX-ROM), 32 imágenes de Spectrum o TC2048 (solamente 16K de ROM), o una combinación de ambos tipos. Además de ello, se podrán almacenar hasta 4 imágenes de cartucho para ser accesibles al momento de booteo.

	Salvo algunas cuestiones de ajuste fino en algunas temporizaciones, el grueso del desarrollo está casi terminado. A continuación las características más importantes de la TS-Rpi:

    • Comunicación bidireccional entre la TS 2068 y una R-Pico.
    • Carga y almacenamiento de programas, tanto BASIC como lenguaje máquina, desde archivos .TAP y .TZX. Actualmente, el almacenamiento solo es posible en formato .TAP, para evitar que el programa uPython no sea excesivamente largo; aunque se pueden convertir a.TZX mediante utilidades externas. 
    • Hasta donde se sabe, es LA UNICA interface que permite la conmutación EXTERNA de ambas ROMs TS 2068, tanto HOME como EX-ROM. No es necesaria ABSOLUTAMENTE NINGUNA modificación del hardware original de la TS 2068
    • La misma interface sirve para actualizar las ROMs usadas por la TS 2068. 
    • Alta velocidad de operación. Un programa se carga en algo menos de un segundo
    • Plataforma abierta, modificable y programable por el usuario. Los programas de control de la interface están programados en uPython/asm PIO y assembler Z80
    • Como consecuencia de los dos puntos anteriores, se pueden incorporar por ejemplo rutinas de “page-in, page-out” de memoria desde y hacia la R-Pico. El programador podría tener a disposición vuelcos de memoria limitados solamente por la capacidad de almacenamiento en la SD o la flash del R-Pico. Imaginemos un juego de plataforma estilo Jet Set Willy, con niveles casi infinitos!
    • Accediendo a través del R-Pico, se podría intercambiar información con la PC o notebook conectada. Por ejemplo, acceso a Internet  
