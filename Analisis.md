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
