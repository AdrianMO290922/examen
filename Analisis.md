## Actividad 1 — El retrato del dataset
Predigan.

**- Cuando llueve, ¿qué medio creen que domina? ¿Y para el centro de la ciudad? 
¿Cuál será la distancia más larga que recorre una bicicleta en los datos? ¿Habrá envíos con una etiqueta que rompa las reglas del negocio?**


Yo digo que predomina la Camioneta, para el centro talvez una moto, mas que nada por la movilidad, 5km, yo creo que si, talvez un paquete
que tenga mucha prioridad y se pague extra por ello.
### RESULTADO DE PRUEBAS
```shell
medio_recomendado  bicicleta  camioneta   dron   moto
clima                                                
despejado              0.328      0.166  0.135  0.371
lluvia                 0.085      0.167  0.000  0.748
viento_fuerte          0.317      0.184  0.000  0.499
medio_recomendado  bicicleta  camioneta   dron   moto
zona_destino                                         
centro                 0.584      0.135  0.000  0.280
periferia              0.009      0.140  0.126  0.725
residencial            0.262      0.146  0.173  0.419
rural                  0.000      0.408  0.000  0.592
                    min  median    max
medio_recomendado                     
bicicleta          0.37   3.420   7.00
camioneta          0.43   8.990  59.12
dron               2.07   9.535  14.99
moto               0.34  10.630  39.90
valida
True    10000
Name: count, dtype: int64
```

Preguntas.

**- ¿En qué zonas y con qué climas el dron nunca aparece? ¿Qué regla del paso 12 lo explica en cada caso?**

Climas que no sean despejados,y en zonas como centro no se pueden utilizar, estan los if:
```
if medio == "dron" and (paquete["clima"] != "despejado" or noche
                            or paquete["zona_destino"] == "centro"):
```
**- La bicicleta casi no va a la periferia, aunque ninguna regla se lo prohíbe. ¿Qué otra columna lo explica? Demuéstrenlo con un número.**

Es por la probabilidad de los demas medios de transporte los cuales tienen mas probabilidad de ir por esas zonas segun el dataset, entonces las 
predicciones del modelo siempre pondran por encima a otro medio aunque no haya ninguna regla que los especifique. Y esto se da por la distancia ya que
también la bici tiene una distancia limite y es por es que no se han mandado segun el dataset

**- Comparen los máximos de distancia, peso y volumen de cada medio con la tabla LIMITES. ¿Qué límites se pueden adivinar solo mirando los datos? ¿Cuál no y por qué?**

El del dron, bici, y moto ya que son los que se alcanza a ver que llegan hacta cierto limite en cambio el de la camioneta no mucho, tiene mas abertura de
cambio.

**- Hay más de cien camionetas para viajes de menos de 2 km, lo que suena a desperdicio. 
¿Cuántas tienen una razón clara para ir en camioneta? Para las que no, propongan una explicación.**

La razón principal es por el peso, refreigeracion y además el volumen, todas estas son las que hacen que los paqutes hayan sido enviados
por camioneta a pesar de la distancia.

**Para pensar. Si les dieran solo el CSV, sin las reglas, ¿podrían reconstruir las reglas del negocio a partir de los datos? ¿Cuáles sí y cuáles no? ¿Qué riesgo hay en dejar que un modelo «descubra» las reglas en lugar de escribirlas a mano?**

Por ejemplo lo de la distancias y el peso de los medios de transporte pequeños ya que son un poco evidentes, pero por ejempolo con la camioneta
varian mucho y talvez podria llegar a variar mas sin saber exacto su limite, el riesgo es que como las reglas no van de acuerdo al negocio sino a 
un dataset, es que algunos no hayan llegado a sus limites y acortaremos estos, y otras por ejemplo cuando aparezcan casos externos al dataset, que
es lo que va a hacer, es lo que principalmente tendria riesgo.

## Actividad 2 - La hora y dron

**Predigan.** 

¿Cuántos drones creen que salen a las 3:00? ¿Qué porcentaje de los envíos creen que sale de noche (antes de las 6 o desde las 21)?

Unos 4 talvez, 20%

Preguntas.

**¿En qué horas no hay ni un solo dron? ¿Coincide exactamente con la regla?**

Si, a partir de las 21hrs ya no hay drones hasta las 6, y si coincide con la regla de que solo hay drones de las 6 hasta las 20:59

**¿Cuántos envíos de noche hay, comparados con los de día? ¿Qué consecuencia tiene eso para lo que el modelo puede aprender sobre la noche?**

No hay muchos, bajan bastante la cantidad de entregas por la noche, el modelo aprende que no hay mucha demanda por la noche, entonces solo evaluara que tipo de paquetes son los que se envian a esa hora, segun las estadisticas tambien la camioneta es la mas usada estos casos.

**Entre las 20:00 y las 21:00 las bicicletas casi desaparecen y las motos aumentan. Busquen en el README.md qué lo explica.**

es porque las bicicletas y dorenes reciben penalizaciones

**Un árbol solo pregunta «¿la hora es menor o igual que X?». ¿Por qué le cuesta representar «de noche»? ¿Cuántas preguntas sobre la hora necesitaría como mínimo?**

Porque los arboles de decision solo es o una u otra y necesitamos saber condiciones de ambos, o es de dia o de noche, como son dos segmentos para un arbol de decisioon es complicado sin hacer mas flujos de los que parecerian, entonces necesita dos preguntas como minimo

**Para pensar.** Sin mirar los ejercicios de arriba, diseñen una columna nueva que le facilite al árbol entender la noche. Expliquen cómo la calcularían a partir de las columnas que ya existen y cómo comprobarían, con un número, que de verdad ayudó. Si les da tiempo, háganlo.

Conviene hacer una columna booleana preguntando si es de noche por ejemplo es_noche, 0 no, 1 si, con eso simplificamos el flujo y ya con una decision sabemos y arreglamos

## Actividad 3 -Anatomía de los errores

Predigan. ¿Qué confusión creen que será la más frecuente? ¿El modelo confundirá alguna vez una bicicleta con una camioneta?

Yo digo que la mas frecuente puede ser entre moto y bicicleta, no creo que haya bicileta con camioneta porque siento que abarcan caracteristicas muy diferentes

**¿Cuántos errores hay y cuáles son las tres confusiones más frecuentes? ¿Son entre medios parecidos o muy distintos? ¿Qué les dice eso?**

Hay 158 errores en total y las mas frecuentes son moto con bicicleta, camioneta con moto, y moto con dron, aon parecidos, y es que las reglas son parecidas por ser muy cercanos en las reglas, ya sea de tamaño o condiciones

**Revisen los envíos que eran moto y el modelo mandó a dron. ¿Cuántos son de noche? ¿Cuáles corregirían las reglas del negocio y cuáles no? ¿Qué tienen en común los que no?**

Son 29, las reglas de negocio eran 26 y 3 no, los que no tienen en comun la zona, el clima,  el trafico.

**Revisen los que eran moto y el modelo mandó a bicicleta. ¿Están cerca del límite de 7 km? Si no es la distancia, busquen qué columnas los distinguen de las bicicletas que el modelo sí acertó.**

No, la distancia no esta cerca del limite, las columnas que parecen ser son el clima, la prioridad

Para pensar. Si solo pudieran arreglar un tipo de error, ¿cuál elegirían y por qué? ¿Lo arreglarían cambiando el modelo, los datos o las reglas del negocio?

Cambiariamos talvez los datos, dando mas ejemplos de los casos pero siento que podría ser lo de mandar dorn en vez de moto ya que perder un equipo como un dron y pude salir mas caro que equivocarse con otros medios.

## Actividad 4 - Qué columnas usa realmente el modelo?
Predigan. Antes de ver nada, ordenen las catorce columnas de la más importante a la menos importante para elegir el medio. Marquen las que creen que el modelo no usa.

peso, hora de salida, distancia, clima, zona de destino, volumen, largo, acnho, alto, fragil, refirgerado, valor, trafico y prioridad.
Las de refrigerado, fragil, prioridad, valor y trafico.

Preguntas.

**¿Cuáles son las cuatro columnas más importantes? ¿Coinciden con las primeras preguntas del árbol del paso 8? ¿Y con su predicción?**

Distancia, volumen, clima y refrigerado, y con el arbol si coinciden, solo la columna de refrigerado no, y nop, no coinciden con mi predicción solo algunas

**¿Qué columna, al quitarla, baja más la exactitud? ¿Cuáles no la cambian nada?**

La distancia, y las que no son por ejemplo la de ancho, largo, alto, trafico y zona de destino

**La zona de destino tiene importancia cero, pero en la actividad 1 vieron que el dron nunca va al centro. ¿Cómo puede ser que el modelo no use la zona y, aun así, la plataforma nunca mande un dron al centro?**

porque esas son reglas de negocio, no pasan por el modelo para la predicción simplemente eso nunca va a passar por reglas de negocio entonces no hace falta que lo revise el modelo

Para pensar. La app le pide a la persona catorce datos. Si el modelo no usa algunos, ¿deberían dejar de pedirlos? ¿Hay razones para pedir un dato que el modelo no usa?
Son necesarios ya que no lo analiza el modelo pero hay reglas de negocio que ocupan esas metricas que se piensan omitir, lo cual harian que dieran malas predicciones el modelo de cual medio de transporte llevar.

## Actividad 5 - La minoría son de los drones, Hay que balancear el dataset?

Parte A — Pensar antes de tocar los datos
Respondan antes de escribir código:

**¿Qué exactitud tendría un «modelo» que nunca dice dron y siempre acierta todo lo demás? ¿Y uno que siempre dice moto? ¿Qué les dice eso sobre usar la exactitud para juzgar un dataset desbalanceado? ¿Qué medidas del paso 7 usarían en su lugar?**

No sería exacto ya que hay especificaciones donde si conviene mandar un dron y es como si lo igoraras y no estaria correcto para el negocio omitir un medio con el cual cuentan y con el cual se tienen reglas y condiciones para mandarlo sobre otros, y si solo dice moto, peor ya que ni da abertura a agregar unos donde la moto ni si quiera pueda con la carga completa de los paquetes, y nos quiere decir que siempre debe de estar balanceado ya que sino estará dando predicciones mal, y no exactas y el margen de error sera mayor, y usaria del paso 7, las de classifcation_report, el recall de drones, f1, la precision del dron

**¿Por qué hay tan pocos drones? ¿Es un error de los datos o refleja cómo funciona la empresa? ¿Cambia eso la decisión de balancear?**

Puede que sea por el tipico tipo de paquetes que maneja la empresa, y refleja como funciona la empresa en realidad, entonces no se debe de balancear, ya que sino le dirias al modelo que el dron es tan comun como una camioneta y empezará a sugerirlo mas seguido en paquetes que talvez no son los adecuados

Si deciden balancear, ¿se balancea antes o después de separar los datos de entrenamiento y de prueba? ¿Se balancean también los datos de prueba? Justifiquen las dos respuestas antes de experimentar.

Se hace después, los datos de prueba no se balancean, esto es porque si balanceas pueden caer una copia del mismo caso en la misma categoria ya sea de entrenamiento o de prueba y esto hace que la prueba salga inflada, y la prueba no se balancea porque es algo que debe de verse como lo que es en el mundo real donde los drones son pocos y debe de reflejarlo, por eso son de prueba.

Parte B — Probar estrategias
Propongan y prueben al menos tres formas de balancear. No les decimos cuáles; piensen en estas ideas generales y decidan cómo llevarlas a código:

````
 python actividad5.py
                      estrategia  envios_entreno  exactitud  precision_dron  recall_dron  veces_predice_dron  sugerencias_imposibles
                   sin balancear            8000      0.921           0.839        0.905                 205                      31
          quitar (undersampling)            3032      0.898           0.889        0.932                 199                      29
          repetir (oversampling)           14652      0.902           0.766        1.000                 248                      36
                   drones nuevos            9942      0.900           0.722        1.000                 263                      56
           class_weight=balanced            8000      0.903           0.766        1.000                 248                      35
TRAMPA: repetir antes de separar           10880      0.920           0.923        0.989                 975                      59
       TRAMPA: prueba balanceada            3032      0.900           0.967        0.932                 183                       6
drones reales en la prueba: 190

````
Parte C - Interpretar
**¿Qué pasó con el recall del dron y qué pasó con su precision? ¿Por qué una sube cuando la otra baja?**

Al poner el modelo a encontrar más drones, el recall sube a 1000, pero la precision baja de 0.839 a 0.722. Para encontrar todos los drones reales, el modelo etiqueta como dron también los casos dudosos, y varios de ellos son errores. Es un intercambio: una sube porque la otra baja.

**¿Alguna estrategia dejó al modelo con muchos menos datos de entrenamiento? ¿Qué se pierde con eso?**

Solo 3032 envíos de entrenamiento contra los 8000 que eran, se pierde información de las clases grandes y la exactitud total baja a 0.898, aun asi su precision y su recall del dron son buenos, y eso muestra que no siempre hay que tirar datos, pero casi siempre se paga algo.

**Si dos estrategias distintas les dieron números casi iguales para el dron, expliquen por qué son, en el fondo, la misma idea.**

Repetir y class_weight dieron exactamente la misma precision de 0.766, el recall 1000 y casi las mismas veces que predice dron 248. En el fondo las dos hacen que un error en un dron cuente más durante el entrenamiento. Repetir lo hace copiando filas, y class_weight lo hace con un peso. El árbol termina con una pérdida casi equivalente.

**En el paso 15, el motivo de la respuesta dice «confianza 84 %». Después de balancear, ¿ese porcentaje sigue significando lo mismo? (Piensen en qué proporción de drones vio el modelo al entrenar y qué proporción hay en la realidad.)**

Ya no significa lo mismo, un modelo entrenado con drones al 25% estima probabilidades como si el dron fuera mucho más común de lo que es. Un 84% de dron sale inflado frente a la realidad, donde solo el 9.5% de los envíos son de dron. Antes de balancear, el porcentaje reflejaba más o menos la proporción real. Después, ya no es una probabilidad calibrada, y habría que recalibrarla antes de mostrársela a una persona.
