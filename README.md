# Material de Sistemas de Control
## Introducción al Control y al Control Clásico

### ¿Qué es el control?
***Definición de control:*** Es un proceso de modificación y ajuste de la entrada de un sistema para obtener una salida deseada. Se basa en la idea de reducir o eliminar la discrepancia entre el valor real de una variable (temperatura, velocidad, presión) y el valor deseado o referencia. <br>
Este proceso se aplica a un sistema o planta las cuales pueden ser:

***Sistema en lazo abierto:*** La señal de control se aplica sin ningún tipo de retroalimentación. El sistema no tiene información sobre su estado actual, por lo que su comportamiento depende únicamente de las entradas aplicadas.

- *Ventaja:* es simple y rápido.
- *Desventaja:* no tiene capacidad para adaptarse a cambios o perturbaciones en el sistema. Ejemplo: Un sistema de riego controlado por tiempo.

***Sistema en lazo cerrado:*** La señal de control se ajusta según la retroalimentación de la salida del sistema. 
- *Ventaja:* Mejora la precisión, ya que el sistema puede corregir automáticamente su comportamiento.
- *Ejemplo:* El termostato en un sistema de calefacción, que ajusta la calefacción en función de la temperatura medida.

## Elementos de un sistema de control

- ***Planta:*** El sistema que se va a controlar. Puede ser cualquier proceso físico, como un motor eléctrico, un sistema térmico o un sistema hidráulico.
- ***Sensor:*** Mide la variable controlada (temperatura, posición o velocidad). La medición debe ser precisa para que el controlador ajuste correctamente la señal de control.
- ***Controlador:*** Se encarga de calcular y enviar la señal de control adecuada al actuador, basándose en la retroalimentación obtenida del sensor.
- ***Actuador:*** El actuador ejecuta la señal de control, modificando la planta. Podría ser un motor, una válvula o un calentador, entre otros.

## Modelado de Sistemas y Funciones de Transferencia
Todo sistema físico (una bomba, un horno, un motor, una válvula) responde con cierto retardo ante una señal de entrada. Para poder diseñar un controlador, necesitamos una representación matemática que describa cómo responde ese sistema.

El **modelado** es el proceso de crear una representación abstracta de un *sistema físico* o *proceso*, utilizando herramientas matemáticas. Este modelo permite predecir cómo se comportará el sistema bajo diferentes condiciones y es fundamental para diseñar el controlador. <br>
El modelado nos permite:

1.  *Predecir* la respuesta temporal del sistema ante una acción de control o una perturbación (cambio externo).
2.	*Diseñar* la base matemática para diseñar, analizar y sintonizar un controlador (como el PID).
3.	*Simular* el controlador en un entorno virtual antes de implementarlo en el sistema físico real.

### Formas para expresar el modelado
***Ecuaciones Integro Diferenciales (ED):*** Forma original del modelo, basadas en leyes físicas (Leyes de Newton para sistemas mecánicos, Leyes de Kirchhoff para eléctricos, balances de energía o masa para químicos). La transformada de Laplace nos permite convertir esas ecuaciones integro-diferenciales en expresiones algebraicas, más fáciles de manipular y entender.

<p align="center">
  Transformada de Laplace: $L{f(t)}=F(s)=>\int_0^\infty f(t)^{-st}dt$
</p>

**Ejemplo:** Para un sistema masa-resorte-amortiguador, se aplica la Segunda Ley de Newton para obtener una ED de segundo orden.
Función de Transferencia $G(s)$ es la representación más usada en control clásico. Se obtiene aplicando la Transformada de Laplace a la ecuación diferencial lineal:
<p align="center">
$$G(s) = \frac{\text{Salida}(s)}{\text{Entrada}(s)}$$ <br>
</p>

En el diagrama de bloques, esta función representa al Proceso/Planta.

Con el modelo obtenido podemos estudiar la ***respuesta temporal del sistema***, es el comportamiento de su salida (variable a controlar) a lo largo del tiempo, luego de que se le aplica una señal de entrada específica. <br>
Es la forma en que el sistema reacciona dinámicamente. Para el análisis, se usan entradas de prueba estándar, siendo la más común la *Entrada Escalón Unitario*.

A esta respuesta temporal la analizaremos en dos partes:

### 1. Respuesta Transitoria
Es el comportamiento inicial y dinámico del sistema, que ocurre inmediatamente después de un cambio en la entrada.

*Características:* Suele estar marcada por picos, oscilaciones y cambios rápidos. Esta parte eventualmente desaparece. <br>
*Parámetros importantes (para sintonizar el PID):*
- Tiempo de Levantamiento ($t_r$): Tiempo para ir del 10% al 90% del valor final. (Relacionado con la velocidad de la respuesta).
- Sobrepaso Máximo ($M_p$): La máxima excursión por encima del valor final deseado.(Relacionado con la estabilidad del sistema).
- Tiempo Pico ($t_p$): Tiempo en el que se alcanza el primer sobrepaso.

### 2. Respuesta en Estado Estable
Es el comportamiento del sistema después de que ha transcurrido un tiempo largo, cuando la respuesta transitoria ha desaparecido. 

*Características:* La salida se estabiliza cerca del valor deseado. <br>
*Parámetros importantes:*
- Error en Estado Estacionario ($e_{ss}$): La diferencia final y permanente entre el valor de entrada y la salida. (El objetivo del término Integral del PID es eliminar este error).
- Tiempo de estabilizacion ($t_s$): El tiempo requerido para que la respuesta se mantenga dentro de un pequeño porcentaje (usualmente 2% o 5%) del valor final.

Comprender el modelado $G(s)$ y la respuesta temporal es lo que permite entender cómo los términos P, I, y D del controlador afectan directamente la velocidad, la estabilidad y el error del sistema. para esto debemos enteder como es la respeusta temporal de un sistema.

## Sistemas de Primer Orden Control

Los sistemas de primer orden por definición son aquellos que tienen un solo polo y están representados por ecuaciones diferenciales ordinarias de primer orden, Quiere decir que el máximo orden de la derivada es 1. Considerando el caso de las ecuaciones diferenciales lineales de primer orden, con coeficientes constantes y condición inicial cero, tenemos: <br>
$a_1\frac{dy(t)}{dt}+ a_0y(t)= b_0 u(t)$ con: $y(0)=0$ <br>

sea nuestra ecuación diferencial: <br>
$a \frac{dh(t)}{dt}=k_1H(t-0)\alpha(t-0)-k_2h(t)$ <br>

Aplicando la transformada de Laplas: <br>
$AsH(s) = k_1e^(-\Theta s)\alpha(s) - k_2H(s)$ <br>

$\frac{H(s)}{\alpha(s)}=\frac{k_1}{As+k_2}e^{(-\Theta s)}$ <br>

$\frac{H(s)}{\alpha(s)}=\frac{ \frac{k_1}{k_2}}{ \frac{A}{k_2}s+1}e^{(-\Theta s)}$ <br>

$\frac{H(s)}{\alpha(s)}=\frac{K}{\tau s+1}e^{(-\Theta s)}$

## Respuesta de un Sistema de Primer Orden

si aplicamos la anti transformada de Laplace a la ecuación transerencia: <br>
$\frac{H(s)}{\alpha(s)}=\frac{K}{\tau s+1}e^{(-\Theta s)}$ <br>

Obtenemos <br>
$h(t)=AK(1-e^{(-\frac{t}{\tau})})$ <br>

Ahora podemos graficarla: <br>
<img width="377" height="278" alt="image" src="https://github.com/user-attachments/assets/21a7d099-79d7-4b4c-91f7-20c1feeb1dcb" />

La identificación de polos y ceros de primer orden es esencial para el diseño y análisis de sistemas de control. Los polos y ceros determinan las características de estabilidad y tiempo de establecimiento del sistema.

Todo sistema de primer orden posee un polo el cual rige la dinámica del mismo. Si ese polo se encuentra cerca del eje imaginario, hace que la respuesta del sistema sea mucho más lenta (o sea que su estado transitorio va a demorar más tiempo)

Si el polo se encuentra lejos del eje imaginario, la respuesta del sistema sera rápida (estado transitorio rápido).

Al final, el polo deja de tener efecto, y la variable observada en la exponencial de la ecuación temporal $h(t)$ se va a volver cero, lo que implica que el sistema habrá entrado en su régimen permanente.

En la siguiente figura se puede evidenciar la respuesta transitoria y permanente de un sistema de primer orden.

<img width="274" height="280" alt="image" src="https://github.com/user-attachments/assets/c3abaa91-e55e-4019-a7f7-ce0fa8687977" /> <br>
<img width="377" height="278" alt="image" src="https://github.com/user-attachments/assets/c22a0e6d-3e97-4401-ae64-144808efd876" />

El estado estacionario del sistema es cuando la dinámica deja de variar y alcanza un equilibrio. Este valor de equilibrio puede ser encontrado a través del teorema del valor final. <br>
$h_e=\lim_{t\to\infty}AK(1-e^{(-\frac{t}{\tau})})$

ásicamente, el estacionario de los sistemas dinámicos de primer orden se encuentra en la ganancia estática de un sistema multiplicado por la magnitud de la entrada.

*τ = Constante de tiempo del sistema:* Tiempo que le toma al sistema en llegar al 63.2% del estado estable. <br>
*Tiempo de Establecimiento:* Tiempo que le toma al sistema en llegar al estado estable y se calcula como: <br>
$t_s=4τ$

La formula de tiempo de establecimiento es una herramienta valiosa para calcular el tiempo requerido para que un sistema alcance su estado estable.

## Sistema realimentado
Para poder controlar la respuesta del sistema necesitamos saber cómo está la salida para ello debemos muestrearla y de alguna manera modificar la entrada para lograr que la salida sea el valor deseado (seteado) esto da comienzo al concepto de lazo cerrado en el que encontramos los siguientes conceptos importantes:

- 1.	*Error ($e(t)$):* Es el motor del control. Se calcula constantemente: $$e(t) = SP - PV(t)$$ El objetivo del controlador es llevar y mantener este error a cero.
- 2.	*Retroalimentación (Feedback):* Es el proceso de tomar la medición de la salida (PV) y enviarla de regreso al punto de comparación para generar el error. Es lo que hace que un sistema de lazo cerrado se autocorrija.
- 3.	*Acción de Control:* Es la señal generada por el controlador (el PID) que se envía al actuador para corregir el error. El controlador PID logra esto a través de tres acciones (P, I, D), que es la base para las clases que planeas.

## Polos, ceros y estabilidad
La estabilidad de un sistema depende de los polos de su función de transferencia (raíces del denominador)

- Si todos los polos tienen parte real negativa, el sistema es estable.
- Si algún polo tiene parte real positiva, el sistema es inestable.
- Si hay polos en el eje imaginario, el sistema oscila.

En la práctica, la estabilidad se observa en la respuesta temporal: si la salida se estabiliza, es estable; si oscila o diverge, no.

# Conclusiones prácticas
- La función de transferencia es la herramienta más poderosa del control clásico.
- Permite estudiar estabilidad, tiempo de respuesta y comportamiento sin experimentar físicamente.
- Casi todos los controladores industriales se diseñan con base en un modelo $G(s)$ aproximado de la planta.

## Control PID
Veamos como controlar una plata: supongamos que se trata de un tanque de agua.

Para poder entender el concepto de este controlador, vamos a tomar como base el control de nivel de un tanque, que es un proceso simple, pero muy común en industrias, y lo importante es que gracias a ese proceso entenderemos el funcionamiento del control PID. A continuación se muestra el diagrama de proceso del tanque:

<img width="457" height="261" alt="image" src="https://github.com/user-attachments/assets/3fff6292-84d9-42f4-9960-3b4f372b3f27" />

El tanque tendrá dos válvulas, la válvula de entrada sera nuestro elemento final de control, es decir el control actúa sobre esta válvula para aumentar o disminuir el nivel en el tanque, la válvula de salida, $a_s$, sera una perturbación, y vamos a considerarla como si ella se mantuviera en una abertura fija. Ahora partimos de la transferencia de la planta, llamemoslo proceso (carga del tanque), nuestro control actuara sobre la señal de entrada (abrir la válvula de carga del tanque) de nuestro proceso y siempre tomaremos una realimentación unitaria (ganancia del lazo de realimentación unitaria o igual a 1)

<img width="463" height="153" alt="image" src="https://github.com/user-attachments/assets/ca56d23c-e279-40d3-99a6-d785fae8a45a" />

Podemos ahora verlo de la otra forma donde el control PID le da la orden de apertura y cierre de la válvula, $u(t)$, y notemos que el sensor de nivel siempre le está mandando información al controlador de cual es el nivel que hay en el interior del tanque, $y(t)$, el control PID hace una comparación entre la señal del sensor y el setpoint o referencia que nosotros mismos le colocamos, $r(t)$, y el calcula un error, $e(t)=r(t)-y(t)$, y con base a ese error el controlador actúa sobre el proceso para intentar volver cero el error y de esa forma llegar a la referencia.

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
