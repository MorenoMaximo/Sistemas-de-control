# Material de Sistemas de Control
## Introducción al Control y al Control Clásico

### ¿Qué es el control?
***Definición de control:*** Es un proceso de modificación y ajuste de la entrada de un sistema para obtener una salida deseada. Se basa en la idea de reducir o eliminar la discrepancia entre el valor real de una variable (temperatura, velocidad, presión) y el valor deseado o referencia. Este proceso se aplica a un sistema o planta las cuales pueden ser:

***Sistema en lazo abierto:*** La señal de control se aplica sin ningún tipo de retroalimentación. El sistema no tiene información sobre su estado actual, por lo que su comportamiento depende únicamente de las entradas aplicadas.

- Ventaja: es simple y rápido.
- Desventaja no tiene capacidad para adaptarse a cambios o perturbaciones en el sistema. Ejemplo: Un sistema de riego controlado por tiempo.

***Sistema en lazo cerrado:*** La señal de control se ajusta según la retroalimentación de la salida del sistema. Esto mejora la precisión, ya que el sistema puede corregir automáticamente su comportamiento. Ejemplo: El termostato en un sistema de calefacción, que ajusta la calefacción en función de la temperatura medida.

## Elementos de un sistema de control

- Planta: Es el sistema que se va a controlar. Puede ser cualquier proceso físico, como un motor eléctrico, un sistema térmico o un sistema hidráulico.
- Sensor: Mide la variable controlada, como la temperatura, la posición o la velocidad. La medición debe ser precisa para que el controlador ajuste correctamente la señal de control.
- Controlador: Se encarga de calcular y enviar la señal de control adecuada al actuador, basándose en la retroalimentación obtenida del sensor.
- Actuador: El actuador ejecuta la señal de control, modificando la planta. Esto podría ser un motor, una válvula o un calentador, entre otros.

## Modelado de Sistemas y Funciones de Transferencia

Todo sistema físico (una bomba, un horno, un motor, una válvula) responde con cierto retardo o dinámica ante una señal de entrada. Para poder diseñar un controlador, necesitamos una representación matemática que describa cómo responde ese sistema.

El modelado es el proceso de crear una representación abstracta de un sistema físico o proceso, utilizando herramientas matemáticas. Este modelo permite predecir cómo se comportará el sistema bajo diferentes condiciones y es fundamental para diseñar el controlador. EL modelado nos permite:

-	1.  Predecir la respuesta temporal del sistema ante una acción de control o una perturbación (cambio externo).
- 2.	Diseño: Es la base matemática para diseñar, analizar y sintonizar un controlador (como el PID).
- 3.	Simulación: Permite probar el controlador en un entorno virtual antes de implementarlo en el sistema físico real.

**Hay tres formas para expresar el modelado, pero veremos una de ellas**
***Ecuaciones Integro Diferenciales (ED):*** Es la forma original del modelo, basadas en leyes físicas (Leyes de Newton para sistemas mecánicos, Leyes de Kirchhoff para eléctricos, balances de energía o masa para químicos).La transformada de Laplace nos permite convertir esas ecuaciones integro-diferenciales en expresiones algebraicas, más fáciles de manipular y entender.

- Transformada de Laplace: $L{f(t)}=F(s)=>\int_0^\infty f(t)^{-st}dt$

**Ejemplo:** Para un sistema masa-resorte-amortiguador, se aplica la Segunda Ley de Newton para obtener una ED de segundo orden.
Función de Transferencia $G(s)$ es la representación más usada en control clásico. Se obtiene aplicando la Transformada de Laplace a la ecuación diferencial lineal: <br>
$$G(s) = \frac{\text{Salida}(s)}{\text{Entrada}(s)}$$ <br>
En el diagrama de bloques, esta función representa al Proceso/Planta.

Con el modelo obtenido podemos estudiar la *respuesta temporal del sistema*, es el comportamiento de su salida (Variable a Controlada) a lo largo del tiempo, luego de que se le aplica una señal de entrada específica. <br>
Es la forma en que el sistema reacciona dinámicamente. Para el análisis, se usan entradas de prueba estándar, siendo la más común la Entrada Escalón Unitario.

A esta respuesta temporal la analizaremos en dos partes:

### 1. Respuesta Transitoria
Es el comportamiento inicial y dinámico del sistema, que ocurre inmediatamente después de un cambio en la entrada.

*Características:* Suele estar marcada por picos, oscilaciones y cambios rápidos. Esta parte eventualmente desaparece.
**Parámetros importantes (para sintonizar el PID):**
- Tiempo de Levantamiento ($t_r$): Tiempo para ir del 10% al 90% del valor final.
(Relacionado con la velocidad de la respuesta).
- Sobrepaso Máximo ($M_p$): La máxima excursión por encima del valor final deseado.(Relacionado con la estabilidad del sistema).
- Tiempo Pico ($t_p$): Tiempo en el que se alcanza el primer sobrepaso.

### 2. Respuesta en Estado Estable
Es el comportamiento del sistema después de que ha transcurrido un tiempo largo, cuando la respuesta transitoria ha desaparecido. 

*Características:* La salida se estabiliza cerca del valor deseado. 
**Parámetros importantes:**
- Error en Estado Estacionario ($e_{ss}$): La diferencia final y permanente entre el valor de entrada y la salida. (El objetivo del término Integral del PID es eliminar este error).
- Tiempo de estabilizacion ($t_s$): El tiempo requerido para que la respuesta se mantenga dentro de un pequeño porcentaje (usualmente 2% o 5%) del valor final.

Comprender el modelado (la $G(s)$) y la respuesta temporal es lo que permite entender cómo los términos P, I, y D del controlador afectan directamente la velocidad, la estabilidad y el error del sistema. para esto debemos enteder como es la respeusta temporal de un sistema.

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

## Sistema realimentrado
