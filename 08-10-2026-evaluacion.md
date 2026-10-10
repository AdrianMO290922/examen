# Actividad individual — Lo que hay detrás de la implementación

Nombre:Adrián Martínez Ortiz
Matrícula:22121308
Equipo:5

## A1
Bien, lo que dice Ana tiene algo de razón, al usar docker ya tienes un sistema aislado pero no significa que se anule el uso de entornos virtualues
ya que este es mas para un desarrollo local y pruebas y docker ya para empaquetar sistemas que ya funcionan, entonces si podria ahorrar el no usar 
un entorno virtual pero se estaria desaprovechando una herramienta tan util para el desarrollo como os entornos virtuales.
Ahora lo que dice  Luis, esta completamente erroneo, como ya mencione los entornos virtuales son para desarrollo y pruebas locales, no para que se conecten
externos, simeplemente se utiliza para mantener aisladas las versiones de las libererias mediante el desarrollo no afectar el codigo maquina por maquina

## A2
a) No, la carpeta .venv lo unico que aisla son las versiones de las librerías, de hecho por eso se pone en el gitignore, ya que solo son paquetes, el 
código lo sigues teniendo tu, fuera de esta carpeta.
b) Simplemente volver a crear el entorno, y el papel de requirements es lo que importa al momento de estar utilizando entornos virtuales ya que 
es lo que si perdura y o que se necesita para poder iniciar una y que funcione correctamente, este archivo tiene las librerias con las versiones
especificas con las que funciona el código.
c) Realmente no instala python de nuevo, la maquina ya lo debe de tener, solo que cuando se activa este aisla las versiones, se sigue teniendo la misma de
la maquina pero si se decide trabajar con otra, aqui mismo la puedes instalar y queda instalada pero en el entorno, no en la maquina, y se repite en cada terminal
porque son variables diferentes, entornos diferentes, como queda completamente aislado, debes de hacer lo mismo para cada entorno si quieres manejar varios

## A3
a) Bueno app, es donde quedará empaquetada toda tu aplicación, la raíz a donde iran los demas comandos, no es que exista precisamente en tu proyecto, pero es la ruta en la que se empaquetara
al momento de hacer el contenedor, y el copy, lo que hace es copiar el documento de las librerias, dependencias con las que se han trabajado, como
con docker dejamos la maquina anfitrion y se crea una nueva en ese contenedor, nos llevamos lo que necesitamos para que el código se pueda ejecutar sin
probkema alguno, como ya mencione, las librerias con versiones especificas.
b) Bueno, evita que las imagenes de docker se vuelvan pesadas, como estamos alojando un SO completo pero lo mas ligero para ejecutar nuestro código
la base de datos crece, el .venv tambien se modifica cada que hacemos algo, muchos archivos que pueden hacer que falle por lo ligero que se maneja docker

## B1
a) Bueno como se emepzo a programar en web, habrá duplicación de código y logica, ya que si, se resolvio, pero para movil es volver a empezar con esta logica
y con el lenguaje, incopatibilidad al actualizar el codigo, tenerlo en dos lados difernetes hace que cualquier cambio, en ambos sea necesario
b) Se puede cambiar mientras la logica completa este en el back y el fornt solo la pinten, tenerlos desacoplados es la forma correcta de trabjar ya que
si un día se cambia de lenguaje para el front, solo tiene que consultar lo mismo que ya da los servicios de back, y solo pintar la información, toda
la logica de negocio se debe de encargar el back. Lo que usualmente le entrega el back al front es información ya procesada por medios de json, donde ya no tienes
que preocuparte por relacionar tablas o si quiera saber como se relacionan, solo consultas información ya procesaday lista para pintar.

## B2
a) si se cambia ese campo, la aplicación dejará de cargar los campos ya que como mapea el JSON, al momento de cambiar no el valor sino la clave, el front no 
encontrará el campo y este dejará de mostrar información, simpre deberá de ser consistete lo que hay en la documentación con lo que se entrega, y si se
cambia algun valor se necesita notificar y el campo versión indica con la que estamos trabajando, a veces con eso nos damos cuenta si hubo algun cambio, y con
que version trabaja nuestro front, ya que puede haber surgido ese cambio sin notificacion y solo ver el error, al ver la version con la que se trabaja y despues
con la que salio el error, te daras cuenta que no es error del front, sino que algo paso en el nuevo commit a producción de la api, y se ataca el problema, ademas
de que para no dejar sin servicio al cliente, le indicas que despliegue la version del back con la que no hubo problemas mientras se descubre que fallo.
## B3
a) Bien, con respecto al problema yo siento que no se debería de eliminar ya que es obligatorio para decidir el medio de transporte, de hecho importa bastante
y el dejarlo sin validación y permitirlo podría generar un hueco, mal calculo o error dentro de la operacion al momento de decirtdir y mandar el paquete, 
la diferencia entre validacion de back y front es que en front, pues permite llamar a la api y posiblemente producir un error, pero si desde el back
esta validado, no llega a mostrar error de calculo o algo similar, sino manda que fue una mal peticion por no cumplir con los parametros

## C1
a) Bueno, escalar verticalmente es seguir usando la misma arquitectura ysimplemente darle mas rendimiento al servidor donde esta alojado, esto con el fin
de que tenga la potencia para solucionar todos estos problemas de latencia aunque a veces puede salir contraproducente, en cambio el crecimiento horizontal
es repartir la carga en diferentes servidores y servicios, para que no se sature uno solo y puedan operar mas sin necesidad de tantos recursos, esto
ayuda a cuando el sistema crece mucho y se busca alijerar la carga y le damos responsabilidades especificas a cada servicio como lo de manejar dos bases
una para consultas y otra para inserts y updates, asi dividiendo la carga.
b) El servidor puede llegar a tener un limite y además de que también los costos, estos se pueden llegar a incrementar de manera exponencial
c)Actua como balanceador de cargas o proxy inverso

## C2
a) Cada una tiene su propio diccionario de memoria, porque siguen siendo copias pero cada una corre en su propio contenedor
b)Se menciona que trabaja con dos workers, entonces el primero se encarga de lo que es la incercion, y el balanceador manda la peticion de busqueda
por el otro worker, el cual maneja propia RAM y propios procesos asi como su archivo de memoria.
c) No, esta mal, docker no hace eso, el siple hecho de tratar de guardar los archivos de memoria, en local ya esta mal diseñado y destinado a fallar

## C3
Como ya se menciono en lo anterior, al momento de usar el balanceador este manda la primera peticion a un worker y una segunda a otro ya que estamos guardando
todo localmente, esto hace que falle, en lo otro si funciona porque Django lo pone en una cookie que se lee y se almacena en la web, teniendo consistencia en 
lo que se acaba de subir.
Lo que investigue es que por defecto en Django, las sesiones se almacenan en la base de datos centralizada (en la tabla django_session de PostgreSQL) 
o en una memoria caché compartida en red como Redis.
## C4
a) Cada una de las 5 copias ejecutará su propio temporizador/cron interno de forma independiente. A las 8:00, los clientes recibirán 
5 veces el mismo aviso diario
b) Si cada copia ejecutara python manage.py migrate al arrancar en paralelo, se generarían bloqueos en la base de datos, varias instancias 
intentarían alterar las mismas tablas e insertar registros de control al mismo tiempo, corruptos o fallando la inicialización.
c) De acuerdo a lo que investigue Si hay 20 copias y cada una tiene 4 procesos de Gunicorn, la plataforma abrirá un total de 80 
conexiones concurrentes a la base de datos y Escalar horizontalmente el backend web no es infinito. Si se agregan demasiadas copias web, 
se sobrepasará rápidamente el límite de conexiones del motor de base de datos o se saturará su CPU/RAM. SQLite es una base de datos basada en un archivo
local en disco que utiliza bloqueos a nivel de archivo completo.

## D1
a) Según lo investigado porque se está evaluando el modelo con los mismos 10 000 datos con los que se entrenó. Esto no demuestra que el árbol haya aprendido a predecir, 
sino únicamente que memorizó los datos. No mide la capacidad real del modelo para generalizar con información nueva.
b) Sirve para reservar un conjunto de datos no vistos y evaluar el modelo simulando cómo se comportará en un entorno 
real de producción.
c) Según lo investigado evita que la partición de los datos quede desbalanceada. Como los envíos por dron son una minoría dentro del dataset, stratify=y
garantiza que tanto el conjunto de entrenamiento como el de prueba mantengan la misma proporción exacta de entregas por dron que el conjunto original.

## D2
Le estaría dando la idea falsa de que existe una jerarquía o un orden de magnitud matemática entre los climas (relación ordinal), 
haciendo pensar al algoritmo que (viento_fuerte) (2) es "mayor" o el doble que (lluvia) (1). El one-hot encoding lo evita al crear variables 
binarias independientes (0 o 1) para cada estado del clima, tratándolos como categorías puramente nominales sin ningún orden intrínseco.

Según lo investigado se guardan juntos en un Pipeline para empaquetar el preprocesamiento de datos y el modelo en un solo flujo. Esto evita errores de inconsistencia
en producción cuando llega un paquete nuevo con datos crudos (por ejemplo, `clima = "lluvia"`), garantizando que los datos reciban exactamente 
la misma transformación antes de ser procesados por el árbol sin necesidad de código manual extra.

## D3

a) Según lo investigado el fenómeno se llama sobreajuste Ocurre porque el árbol sin límite creció demasiado (346 hojas) hasta memorizar el ruido y
casos atípicos de los datos de entrenamiento, perdiendo la capacidad de generalizar y fallando más al evaluar datos nuevos.

b) Se elige profundidad 8 por el principio de simplicidad y eficiencia del modelo. Ofrece prácticamente el mismo nivel de 
exactitud (92.1% frente a 92.4%), pero con la mitad de complejidad y hojas (47 frente a 92). 
Un modelo más pequeño es más rápido, consume menos memoria y es más robusto ante variaciones en los datos.

## D4
a)Precisión (0.839 /83.9%): De todas las veces que el modelo predijo que un paquete debía enviarse en dron, acertó el 83.9% de las veces. 
Exhaustividad / Recall (0.905 / 90.5%): De todos los paquetes reales que verdaderamente eran aptos para ir en dron, el modelo logró identificar 
correctamente el 90.5% (dejando fuera solo un 9.5% como falsos negativos).   
b)El error más grave es mandar en dron un paquete que NO debía ir en dron (falso positivo), ya que puede causar accidentes, violar
regulaciones de vuelo o superar el límite físico del dron.Este error lo mide la precisión (a mayor precisión, menor cantidad de falsos positivos). 
c)No llega al 100% porque los datos reales tienen ruido, incertidumbre o factores no observados y no es malo porque un 100% en pruebas reales 
indicaría sobreajuste (overfitting) o datos manipulados. Un modelo probabilístico admite un margen de incertidumbre que luego es filtrado y corregido
por la capa de reglas del negocio. 
## D5
El Árbol de Decisión permite auditar y entender visualmente sus reglas (caja blanca), mientras que el Random Forest es un modelo complejo (caja negra)
donde no se puede explicar fácilmente a un cliente o auditor por qué se tomó una decisión además de el consumo de recursos es mayor.
Según lo investigado no cambia nada la vista ya que ambos están empaquetados bajo la misma interfaz estandar un Pipeline de Scikit-Learn, la vitsa
unicamente invoca la funcion .predict() o .predic_proba().

## E1
a) Porque en el dataset de entrenamiento probablemente había pocos o nulos ejemplos de envíos realizados a las 3:00 a.m. 
 o el árbol no tuvo datos suficientes para aprender la restricción del horario nocturno.  
b) Porque los modelos de Machine Learning son probabilísticos y siempre tienen un margen de error ante situaciones raras o no vistas. 
c) Por el principio de separación de responsabilidades. El modelo de Machine Learning solo hace recomendaciones basadas en estadísticas, 
mientras que reglas.py valida la viabilidad real según las políticas de la empresa. Si una política cambia (por ejemplo, prohibir vuelos con lluvia),
solo se actualiza reglas.py al instante sin tener que reentrenar ni re-desplegar todo el modelo de datos.   

## E2
No es una contradicción, un pedido es un dato dinámico con estado stateful que se modifica constantemente, si se guarda en la memoria de una copia,
las demás no lo ven y los datos quedan inconsistentes.
El modelo es un recurso de solo lectura stateless e inmutable. Dado que no cambia durante las peticiones de los usuarios, es seguro que cada copia o
proceso conserve su propia copia cargada en la memoria RAM.
Causaría un problema grave de rendimiento y latencia al leer del almacenamiento y deserializar el archivo .joblib en cada petición HTTP consumiría demasiado 
tiempo de CPU e I/O de disco, volviendo la plataforma extremadamente lenta.

## E3
El archivo requiriments.txt justamente es para poner las versiones ya que el código que se ejecutará en el contenedor trabaja con versiones
exactas, si alguna no se especifica esta corre el riesgo y es lo mas probable que algo no funcione ya que no se especifico la versión con la
que se programo en un inicio
Se evita siempre generando el archivo con los comandos y comprobando que si se aplique la version del modelo
Como ya sabemos el entorno virtual es para que puedas trabajar con el proyecto sin interferir con versiones de tu computadora, vaya
algo aislado, algo como lo que es docker, ya que tiene su propio SO, obviamente no es lo mismo pero es similar porque la imagen debe de 
funcionar igual que como el que creo y testeo el programa, entonces es importante trabajar de esta manera ya qeu se estará corriendo de forma 
independiente y totalmente aislado el contenedor.

## F1
a) Se debe de escribir un Adaptador que implemente el patrón Adapter para traducit la respuesta del proveedor externo, para que sepa la interfaz
ya usada la comience a implementar, el metodo recomendar no se quita ya que seguira funcionando, solo la fuente se cambiará pero lo demas debera
de seguir funcionando igual, las reglas se deben de seguir validando ya que son las restricciones de negocio, la parte de crear_medio, tampoco ya que
lo unico que s automatizo fue la parte de la desicion, no el metodo.
b) La vista de /entregas/recomendar ya que sigue recibiendo la solicitud HTTP con los datos del paquete, la validación de reghlas de negocio que es el
reglas.py, con el patron Factory la desicion de crear_medio, y al final el patron Strategy que sigue la estrategia de entrega instanciada calcula 
la tarifa, el tiempo estimado y la ruta, generando el plan de entrega final.
c) 1: La IA no hace desaparecer los patrones de diseño porque solo esta tomando la decision, la parte probabilistica, de con los datos cual transporte
debería de utilizarse, pero aun asi, sigue habiendo los metodos de crear cdad uno y que conlleva, la logica de la entrega, este es solo un pequeño 
paso que nos ahorramos en vez de construir un IF gigantesco sin saber cual sera mejor.
2: Que tengamos un proveedor externo que nos ayude con la desicion en base a un entrenamiento no quiere decir que sea el unico, podemos conectar varios
para saber que hacer, hasta tomarlo manual y el sistema debe de seguir respondiendo sin caerse por depender de un solo proveedor, para esto estan los patrones.
