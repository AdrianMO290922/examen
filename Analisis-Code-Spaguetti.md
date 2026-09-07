## Arquitectura
Los archivos api.php, login.php y index.php rompen el patron de Remote Code Execution RCE, este abre el servidor a un secuestri tatal inmediato,
debe de usar el patron de intercepting Filter/Guard, para validar sesiones y privilegios , y validar parametros contra una lista blanca.
Esta sección se debe de eliminar por completo.
## Pasarela de pagos con switch de 30 casos
En los archivos pagar.php y pagar1.php para empezar el nombre de los archivos es mala práctica, no se denota cual es su responsabilidad y que es de lo que se 
encarga tal archivo, en cuestion a los datos pasa los datos privados como CVV en texto plano lo cual es incorrecto, el switch gigantescoi que si 
algo falla o se quiere agregar puede ocacionar grandes problemas, se puede mejorar con patrones como Strategy+Adapter+Factory, donde se defina una interfaz 
con implementaciones independientes.
Esta sección remplazar por completo también.
## Inyección SQL
Esta en texto plano las querys además de que las credenciales estan expuestas, esto podrián tratar de solucionarlo con lo que es el patrón de diseño repository
para centralizar el acceso a la base de datos, y usar el 12-factory App, donde consiste en extraer las credenciales en variables de entorno, sin comprometer al servidor.
Esta también se debe de remplazar, ya que son malas practicas todo lo hecho.
## Vistas monoliticas 
Un solo archivo consulta la base de datos, valida las sesiones, procesa la lógica de negocio, es repositorio y además que renderiza la vista, una 
mala práctica de God Files, la cual no hace posible el mantenimiento de este servicio, debe de dividirse el flujo en capas, podría usarse el patrón de arquitectura como
MVC, para dividir estas lógicas, y el composite View, que va de reutilización de vistas y no forjarlas a mano cada una que se ocupe.
Se remplaza toda estas sección tambien
## Duplicación masiva de funciones
Mucho código basura como lo que son los que contienen CSS, en la cual repiten muchas funciones y muchos estilos, además de repetir el contenido en algunos
de los archivos, pueden usar el patrón DRY, para no repetir logica y el Unity Model, para las variantes de una unica función de utilitari y solo reciba
parametros. 
Se remplazaría toda esta logica de la misma manera.
## Peticiones HTTTP redundantes
Hace llamadas muy seguidas y estas son peticiones HTTP que pueden saturar al navegador y agota rapidamente las conexiones al servidor, lo mejor sería
usar llamadas en proceso que es llamar metodos o servicios nativos, además de hacer asincronía con promesas.
Se remplazaría por completo.
