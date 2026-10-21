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
