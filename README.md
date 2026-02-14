Code Comments: Cómo funciona la interrupción del temporizador
La interrupción del temporizador funciona de manera periódica. El hardware tiene un temporizador que cuenta ciclos de reloj, y cuando llega a un valor previamente configurado, genera automáticamente una señal de interrupción (IRQ).
Cuando se genera esta IRQ, el procesador detiene temporalmente la ejecución normal del programa. En ese momento, la CPU guarda el contexto actual (registros, contador de programa, etc.) para poder continuar después sin perder información.
Luego, el procesador salta a la rutina de atención de interrupción. Esta función es la encargada de atender específicamente la interrupción del timer. Por ejemplo, puede incrementar un contador de ticks del sistema, actualizar el estado del sistema o ejecutar alguna lógica relacionada con planificación.
Después de ejecutar el código correspondiente, la interrupción se reconoce o limpia para que no se vuelva a disparar inmediatamente. Finalmente, la CPU restaura el contexto guardado y continúa ejecutando el programa donde se había quedado.

Function Documentation: Descripción de cada función modificada/agregada
Durante el laboratorio se trabajó en distintas funciones distribuidas en las capas del sistema:
Función de inicialización del temporizador:
Esta función configura el temporizador del hardware con una frecuencia específica. Además, habilita la línea de interrupción correspondiente para que el procesador pueda recibir las señales del timer. Básicamente, aquí se deja listo el hardware para empezar a generar interrupciones periódicas.
Rutina de atención de interrupción:
Esta función se ejecuta automáticamente cuando ocurre la interrupción del temporizador. Su responsabilidad es manejar el evento: puede incrementar un contador global, ejecutar lógica del sistema operativo o realizar alguna acción definida en os.c. También debe limpiar o reconocer la interrupción para que el sistema pueda seguir funcionando correctamente.
Función despachadora de IRQ:
Esta función se encarga de identificar qué tipo de interrupción ocurrió. Es decir, revisa el origen de la IRQ y llama al handler correspondiente. Sirve como intermediaria entre el hardware y la lógica del sistema operativo.
Funciones en os.c:
Estas funciones pertenecen a la capa más alta (nivel tipo sistema operativo). Aquí se implementa la lógica principal del sistema. No interactúan directamente con el hardware, sino que reciben los eventos procesados por la capa de manejo de interrupciones.

Architecture Explanation: Cómo el sistema maneja las IRQ desde el hardware hasta el handler
La arquitectura del laboratorio está dividida en tres capas bien definidas:
Capa de Configuración y Programa Ficticio (nivel SO)
Esta es la capa más alta y contiene únicamente el archivo os.c. Aquí se implementa la lógica del sistema, simulando el comportamiento de un sistema operativo. Esta capa no accede directamente al hardware, sino que depende de la capa intermedia.
Capa de Manejo de Interrupciones e Interfaz con el Hardware
Esta capa actúa como puente entre el hardware y el sistema operativo. Se encarga de configurar las interrupciones, registrar los handlers y recibir las señales del hardware. Es la que traduce la señal física de la interrupción en una llamada de función que el sistema puede manejar.
Capa de Hardware
Es el nivel más bajo. Aquí se inicializa la raíz del programa y se encuentra el temporizador físico. Este hardware es el que genera las interrupciones periódicamente.
stema sea más organizado, modular y fácil de mantener.
