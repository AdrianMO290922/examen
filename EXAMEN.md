# Instituto Tecnológico de Morelia
## Examen Diagnóstico - Fundamentos de Ingeniería de Software

**Materia:** Tópicos Selectos de Tecnologías Web y Móvil  
**Profesor:** Jesús Eduardo Alcaraz Chávez  

**Nombre del alumno:** Adrián Martínez Ortiz  
**Fecha:** 27-08-2026  

---

**Instrucciones:** Responde de manera clara y concisa a cada una de las siguientes preguntas abiertas. El propósito de esta evaluación es medir tus conocimientos previos en Ingeniería de Software.

---

### 1. Metodologías
¿Cuál es la diferencia principal entre una metodología de desarrollo tradicional (como Cascada) y una metodología ágil (como Scrum) frente a los cambios en los requisitos?

**Respuesta:**
    
  La métodología en cascada es más rigida en cuestión a cambios ya que esta se toman requisitos en un inicio y se diseña y planifica todo, entonces un cambio ya avanzado el proyecto genera una restructuración completa, en cambio en Scrum, esta se maneja por iteraciones, lo cual lo hace más sencillo al momento de recibir cambios 

---

### 2. Requerimientos
Explica la diferencia entre requerimientos funcionales y no funcionales, dando un ejemplo de cada uno aplicable a una plataforma web.

**Respuesta:**

- Los funcionales son los que dan valor a la aplicación, un ejempolo, de una plataforma web es que el login o registro funcione correctamente guardando la información del cliente, ya que es una acción de valor y primordial para el funcionamiento de la plataforma, ya sea para pagos o alguna otra acción que solicite estos datos.
- En cambio los no funcionales son como la paleta de colores de alguno de los formularios, alguno de esos requisitos no es de vital importancia para el funcionamiento de la plataforma, claro que importa pero no es algo que nos impida continuar con el desarrollo 

---

### 3. Arquitectura
Describe el modelo Cliente-Servidor y explica brevemente cómo se comunican el frontend y el backend en una aplicación web moderna.

**Respuesta:**

El modelo cliente servidor es que se sirve la plataforma web o lo que se este utilizando como desarrollo y esta genera peticiones http para solicitar datos y poder traerlos a la computadora cliente, entonces esta pide y el servidor contesta con información si es que esta autorizado, existen codigos para retornar un mensaje a la computadora cliente para saber si es que se encontro lo que se pedia, un 200 para recibido, un 404 not found y asi.
Se comunican como anterior ya se menciono, aunque para estos normalmente se hace un servicio como una API REST la cual se encarga de recibir esas peticiones y procesarlas, y darle la información mas conveniente al forntend, esta esta alojada en el servidor entonces recibe las peticiones y contesta por medio de oos códgios ya mencionados

---

### 4. Bases de Datos
¿En qué escenarios recomendarías utilizar una base de datos relacional (SQL) frente a una no relacional (NoSQL) para el almacenamiento de datos en una aplicación?

**Respuesta:**

Son es casos muy especificos donde no se puede llevar una estructura para todos los campos, donde llega a existir algo de presonalización o cuando empeiza a crecer bastante la base de datos en cuestión de tablas y las querys se estarían haciendo muy pesadas por tener que consultar varias tablas a la vez, entonces se considera un enfoque no relacional, el cual te da mas flexibilidad para no tener que hacer este tipo de consultas y tu mismo poder diseñar un esquema mas flexible dependiendo tus necesidades por ejemplo Facebook

---

### 5. APIs
¿Qué es una API REST y qué papel fundamental juega en la integración entre una aplicación móvil y los servidores (backend)?

**Respuesta:**

Bien, esta se encarga de recibir las peticiones de las apolicaciones y procesarlas, para ahí mismo proporcionar los datos de forma segura y como lo desea el cliente, además de que la capa de seguridad de os datos se puede manejar desde aquí ya que no podemos exponer la base de datos directamente en ningun caso, asi que es fundamental en la seguridad de nuestros sisstemas, al igual que algun error de ejecucuón también estas se encargan para pdoer dar certidumbre de estos posibles errores

---

### 6. Control de Versiones
Explica la importancia de utilizar Git en un equipo de desarrollo de software y describe brevemente qué es un "merge conflict" (conflicto de fusión).

**Respuesta:**

Es muy importante ya que con esta herramienta podemos tener un historial y control de versiones del código, evitando tener que estar creando .zip con codigo y esperar a que alguien termine su parte para poder integrar, entonces en la parte colaborativa git es una herramienta fundamental para cualquier programador y su trabajo en equipo, trabajando desde sus maquinas y al final subiendo commits de oos cambios y unirlos en una misma rama.
Un merge conflict es cuando dos integrantes del equipo modifican el mismo archivo y después quieren unirlos, presenta un conflicto porque se debe de decidir con que version quedarse, y asi pasara si en un mismo merge de toca el mismo archivo por dos personas diferentes.

---

### 7. Pruebas
¿Qué son las pruebas unitarias (unit testing) y por qué son cruciales para asegurar la calidad del software antes de su paso a producción?

**Respuesta:**

Es para poder tener la certeza de que no va a fallar la nueva función o conjuto. de funciones en el código ya que a veces entre tanto código algo puede fallar y poder encontrarlo llega a ser complicado, además que en producción no nos podemos tomar esas libertades ya que podriamos afectar a flujos importantes de trabajo, entonces realizar este tipode test, te da la certeza de que esas funciones van a funcionar en varios tipos de casos y si existe algún error, ayuda a kla deopuración más rapida.

---

### 8. POO
Define los conceptos de encapsulamiento y polimorfismo de la Programación Orientada a Objetos, y menciona cómo ayudan a crear un código más mantenible.

**Respuesta:**

El encapsulamiento es para que no se puedan acceder de una clase a diferentes, sino tiene que importar la clase con sus métodos, esto le da más control a que es lo que podemos modificar y que no.
El polimorfismo ayuda a que una clase pueda comportarse de diferentes maneras, esto es para que clases que tienen atributos simiares unificarlos en una misma y de ahí se puedan desprender algunos hijos, esto con el fin de no repetir lineas de codigo que hacen y sirven para lo mismo.

---

### 9. Patrones de Diseño
¿Qué es el patrón de arquitectura Modelo-Vista-Controlador (MVC) y cómo ayuda a organizar el código en el desarrollo de software?

**Respuesta:**

Bien, este es un modelo por capas, lo cual se le dejan responsabilidades especificacas a cada una, en una se maneja lo que es la logica de negocio y otra solo lo que son las salidas de información, esto con el fin de separar las logicas de cada uno de los procesos además de que también es por seguridad para que se accedan de forma correcta sin riesgo a la exposición de la base de datos. Si existe algun problema en alguno de los servicios, si es de loigica de negocio, al service, si es algo de los campos que no se mapean bien, esta el model, y en caso de que sea la petición te diriges al controlador, entonces se lleva un patron y si esta bien documentado, es mantenible a futuro y facil de enetender.

---

### 10. Seguridad
Explica la diferencia técnica entre "autenticación" y "autorización" en el contexto de seguridad de una aplicación.

**Respuesta:**

- La autenticación tiene que ver con que si la petición que se hace al servidor tiene permiso para poder acceder a lo que se aloje ahí, normalmente se hace con un token que el mismo servidor firma y así decidir si esa plataforma o no tiene accesos a los recursos.
- La autorización, tiene que ver con roles o como sea que se maneje la clasificación de recursos, ya que fue autorizado también deben de haber permisos para los recursos, un ejemplo, un empleado no puede borrar registros de nomina, pero un administrador si, o simplemente verlos, esto hace que aunque ya puedan acceder al servidor exista otra capa para asegurar la integridad de los datos con sus repsectivos permisos