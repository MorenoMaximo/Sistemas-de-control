# Material de Sistemas de Control
## Introducción al Control y al Control Clásico

### ¿Qué es el control?
***Definición de control:*** Proceso que modifica y ajusta de la entrada de un sistema para obtener una salida deseada. Pueden ser:

***Sistema en lazo abierto:*** La señal de entrada se aplica sin realimentación. El sistema no tiene información sobre su estado actual, por lo que su comportamiento depende únicamente de las entradas aplicadas.

- *Ventaja:* es simple y rápido.
- *Desventaja:* no tiene capacidad para adaptarse a cambios o perturbaciones en el sistema. Ejemplo: Un sistema de riego controlado por tiempo.

***Sistema en lazo cerrado:*** La señal de control se ajusta según la realimentación de la salida del sistema. 
- *Ventaja:* Mejora la precisión, ya que el sistema puede corregir automáticamente su comportamiento.
- *Ejemplo:* El termostato en un sistema de calefacción, que ajusta la calefacción en función de la temperatura medida.

## Elementos de un sistema de control

- ***Planta:*** El sistema que se va a controlar.
- ***Sensor:*** Mide la variable controlada.
- ***Controlador:*** Calcula y envía la señal al actuador, basándose en la realimentación obtenida del sensor.
- ***Actuador:*** Ejecuta la señal, modificando la planta. (motor, válvula, calentador, entre otros).

## Modelado de Sistemas y Funciones de Transferencia
"Todo sistema físico (una bomba, un horno, un motor, una válvula) responde con cierto retardo ante una señal de entrada. Para poder diseñar un controlador, necesitamos una representación matemática que describa cómo responde ese sistema."

El **modelado** es el proceso de crear una representación abstracta de un *sistema físico* o *proceso*, utilizando herramientas matemáticas. <br>
El modelado nos permite:

1.  ***Predecir*** la respuesta temporal del sistema ante una acción de control o una perturbación (cambio externo).
2.	***Diseñar*** la base matemática para diseñar, analizar y sintonizar un controlador (como el PID).
3.	***Simular*** el controlador en un entorno virtual antes de implementarlo en el sistema físico real.

### Formas para expresar el modelado
***Ecuaciones Integro Diferenciales (ED):*** Forma original del modelo, basadas en leyes físicas (Leyes de Newton para sistemas mecánicos, Leyes de Kirchhoff para eléctricos, balances de energía o masa para químicos). Laplace nos permite convertir esas ecuaciones en expresiones algebraicas, más fáciles de manipular y entender.

<p align="center">
  Transformada de Laplace: $L{f(t)}=F(s)=>\int_0^\infty f(t)^{-st}dt$
</p>

Con el modelo obtenido podemos estudiar la ***respuesta temporal del sistema***, es el comportamiento de su salida a lo largo del tiempo, luego de que se le aplica una señal de entrada específica. <br>
Para el análisis, se usan entradas de prueba estándar, siendo la más común la *Entrada Escalón Unitario*.

A esta respuesta temporal la analizaremos en dos partes:

### 1. Respuesta Transitoria
Es el comportamiento inicial y dinámico del sistema, que ocurre inmediatamente después de un cambio en la entrada.

***Características:*** Suele estar marcada por picos, oscilaciones y cambios rápidos. Eventualmente desaparece. <br>
***Parámetros importantes (para sintonizar el PID):***
- Tiempo de Levantamiento ($t_r$): Tiempo para ir del 10% al 90% del valor final. (Relacionado con la velocidad de la respuesta).
- Sobrepaso Máximo ($M_p$): La máxima excursión por encima del valor final deseado.(Relacionado con la estabilidad del sistema).
- Tiempo Pico ($t_p$): Tiempo en el que se alcanza el primer sobrepaso.

### 2. Respuesta en Estado Estable
Es el comportamiento del sistema después de que transcurrió un tiempo, *cuando la respuesta transitoria ha desaparecido*. 

***Características:*** La salida se estabiliza cerca del valor deseado. <br>
***Parámetros importantes:***
- Error en Estado Estacionario ($e_{ss}$): La diferencia final y permanente entre el valor de entrada y la salida. (El objetivo del término Integral del PID es eliminar este error).
- Tiempo de estabilizacion ($t_s$): El tiempo requerido para que la respuesta se mantenga dentro de un pequeño porcentaje (usualmente 2% o 5%) del valor final.

## Sistemas de Primer Orden Control

Sistemas que tienen un solo polo y están representados por ED de primer orden, es decir, el máximo orden de la derivada es 1.

### Respuesta de un Sistema de Primer Orden

La identificación de polos y ceros de primer orden es esencial para el diseño y análisis de sistemas de control. Los polos y ceros determinan las características de estabilidad y tiempo de establecimiento del sistema.

Todo sistema de primer orden posee un polo el cual rige la dinámica del mismo. 
- Si el polo está **cerca** del eje imaginario, la respuesta del sistema será lenta (estado transitorio lento)
- Si el polo está **lejos** del eje imaginario, la respuesta del sistema será rápida (estado transitorio rápido).

Al final, el polo deja de tener efecto, lo que implica que el sistema habrá entrado en su régimen permanente.

El estado estacionario de los sistemas dinámicos de primer orden se encuentra en la ganancia estática de un sistema multiplicado por la magnitud de la entrada.
<p align="center">
  $\text{Valor Estable} = \text{Ganancia Estática}.\text{Entrada}$
</p>

*τ = Constante de tiempo del sistema:* Tiempo que le toma al sistema en llegar al 63.2% del estado estable. <br>
*Tiempo de Establecimiento:* Tiempo que le toma al sistema en llegar al estado estable y se calcula como: $t_s=4τ$

*La formula de tiempo de establecimiento es una herramienta valiosa para calcular el tiempo requerido para que un sistema alcance su estado estable.*

## Sistema realimentado
Para controlar esta respuesta necesitamos saber cómo está la salida, para ello se muestrea y se modifica la entrada para lograr que la salida sea el valor deseado. En este sistema encontramos conceptos importantes:
1.	*Error ($e(t)$):* El objetivo del controlador es llevar y mantener este error a cero.
2.	*Retroalimentación (Feedback):* Proceso de tomar la medición de la salida y enviarla de regreso al punto de comparación para generar el error. Hace que un sistema de lazo cerrado se autocorrija.
3.	*Acción de Control:* Señal generada por el controlador (PID) que se envía al actuador para corregir el error.

## Polos, ceros y estabilidad
La estabilidad de un sistema depende de los polos de su función de transferencia (raíces del denominador)
- Si todos los polos tienen parte real negativa, el sistema es estable.
- Si algún polo tiene parte real positiva, el sistema es inestable.
- Si hay polos en el eje imaginario, el sistema oscila.

En la práctica, la estabilidad se observa en la respuesta temporal: si la salida se estabiliza, es estable; si oscila o diverge, no.

## Control PID
Veamos como controlar una plata: supongamos que se trata de un tanque de agua.

Para poder entender el concepto de este controlador, vamos a tomar como base el control de nivel de un tanque, que es un proceso simple, pero muy común en industrias, y lo importante es que gracias a ese proceso entenderemos el funcionamiento del control PID. A continuación se muestra el diagrama de proceso del tanque:
<p align="center">
<img width="457" height="261" alt="image" src="https://github.com/user-attachments/assets/3fff6292-84d9-42f4-9960-3b4f372b3f27" />
</p>

El tanque tendrá dos válvulas, la válvula de entrada, $a_e$, será nuestro elemento final de control, el control actúa sobre esta válvula para aumentar o disminuir el nivel en el tanque, la válvula de salida, $a_s$, será una perturbación, y vamos a considerarla como si ella se mantuviera en una abertura fija. 

Ahora partimos de la transferencia de la planta, la carga del tanque, nuestro control actuara sobre la señal de entrada (abrir la válvula de carga del tanque) y siempre tomaremos una realimentación unitaria (ganancia del lazo de realimentación unitaria o igual a 1)
<p align="center">
<img width="463" height="153" alt="image" src="https://github.com/user-attachments/assets/ca56d23c-e279-40d3-99a6-d785fae8a45a" />
</p>

Podemos ahora verlo de la otra forma donde el control PID le da la orden de apertura y cierre de la válvula, $u(t)$, y notemos que el sensor de nivel, $y(t)$, siempre le está mandando información al controlador de cual es el nivel que hay en el interior del tanque. El control PID hace una comparación entre la señal del sensor y el setpoint o referencia que nosotros mismos le colocamos, $r(t)$, y calcula el error, $e(t)=r(t)-y(t)$, y con base a ese error el controlador actúa sobre el proceso para intentar volver cero el error y de esa forma llegar a la referencia.

# Control P (Control Proporcional)
Comencemos entendiendo los beneficios que le aporta un bloque de proporcional al controlador, para eso vamos a suponer que en nuestro lazo de control unicamente contamos con un control Proporcional. Este controlador unicamente calcula un valor proporcional al error actual que existe en el proceso de lazo cerrado:

<img width="510" height="220" alt="image" src="https://github.com/user-attachments/assets/0b25eb3c-85c5-4bce-b21b-c2abd974db18" />

Y con base a ese valor proporcional se lo aplica al sistema. En este caso a nuestra valvula de entrada del tanque: <br>
$a_e=K_pe_H$

$a_e=K_p(H_r-H)$

Considere para nuestro caso $a_e$ es la abertura de la válvula de entrada, $H_r$ es la referencia, o sea cuánto nivel nosotros queremos en el tanque y $H$ es el nivel actual dentro del tanque. <br>
Se puede notar que cuando el error es muy grande, la válvula abre mucho más y cuando el error disminuye la abertura de la válvula va cerrando. <br>
Observemos este comportamiento a través del siguiente gráfico:

<img width="338" height="247" alt="image" src="https://github.com/user-attachments/assets/af3e128d-d316-480d-8c40-8107dedf816e" />

La pendiente de la rampa viene dado por el valor de $K_p$, entre mas grande sea este valor, más inclinada será la rampa. El grado de libertad del controlador puede ser observado en esa rampa y es conocido como la banda proporcional, entre más alto sea el valor de $K_p$ menos banda proporcional tengo, y esto hace que la válvula se abre y se cierre instantáneamente, lo cual puede ser perjudicial para nuestro elemento final de control. La banda proporcional viene dado por: <br>
$BP= \frac{100}{K_p}%$

Ahora, analizando el diagrama de lazo cerrado del sistema de control, sabemos que la ecuación del lazo cerrado viene dado por: <br>
$T=\frac{CP}{(1+CP)}$

Donde el modelo del tanque puede ser representado por un sistema de primer orden: <br>
$P=\frac{K}{\tau s+1}$

Que solo tendrá el comportamiento de su ganancia estática, quiere decir que el lazo cerrado en estado estacionario viene dado por: <br> 
$T_s=\frac{K_pK}{1+K_pK}$

A partir de esa ecuación es fácil ver que un control proporcional por si solo no elimina el error, es decir no llega hasta la referencia, porque en el denominador le estamos sumando un uno. Notemos que si aumentamos mucho la Ganancia proporcional $K_p$ el lazo cerrado va a tender a 1, es decir va a estar muy cerca de la referencia, sin embargo, como ya lo habíamos anticipado antes, eso va a dejar el sistema con una banda proporcional muy pequeña.

Para entender esto, vamos a colocar una acción proporcional bastante grande, $K_p=500$, asi la banda proporcional viene dado por: <br>
$BP= \frac{100}{500}%=0.2%$

<img width="332" height="279" alt="image" src="https://github.com/user-attachments/assets/587fc107-0ef9-40a4-883f-7ef60f1fd20b" />

Vemos que la banda proporcional es bastante reducida, lo que implica un comportamiento de abertura y cierre de la válvula son muy rápidos, esto no es bueno para ningún sistema mecánico, perjudicial a nuestra válvula.

## Error en estado estacionario

Cuando sólo usamos un control proporcional para eliminar el error en estado estacionario es muy común encontrar en los controladores PID industriales un BIAS, que permite adicionar al lazo de control un valor adicional para alcanzar la referencia.

<img width="381" height="139" alt="image" src="https://github.com/user-attachments/assets/a7202def-5b53-4343-bef5-01129509b556" />

Con este Bias, la rampa del controlador proporcional es desplazada del origen, con esto la banda proporcional puede ser graficada como:

<img width="390" height="294" alt="image" src="https://github.com/user-attachments/assets/f4358395-bf03-449e-bdae-20be891a5e21" />
