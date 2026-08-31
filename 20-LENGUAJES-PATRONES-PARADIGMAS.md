**1. Java**

* **Paradigma principal:** Orientado a Objetos (basado en clases), Imperativo, Concurrente.
* **Patrón de diseño característico:** Dependency Injection Container (Inyección de Dependencias con Inversión de Control).
* **¿En qué consiste el patrón?:** Desacopla la creación, configuración y ciclo de vida de los objetos respecto a la lógica de negocio que los consume. Un contenedor centralizado inspecciona metadatos (anotaciones) o configuraciones e inyecta las dependencias necesarias a través de constructores o propiedades.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Sobreingeniería en lenguajes dinámicos:** En entornos como Python o JavaScript, donde las funciones son de primer orden y los módulos actúan como singletons nativos, un contenedor de inyección pesado introduce capas innecesarias de abstracción y sobrecarga.
* **Incompatibilidad sin reflexión en tiempo de ejecución:** En lenguajes de bajo nivel compilados como C++ o Rust, la falta de reflexión dinámica nativa obliga a resolver la inyección mediante metaprogramación en tiempo de compilación o macros, limitando la reconfiguración dinámica.



---

**2. C++**

* **Paradigma principal:** Multiparadigma (Sistemas, Orientado a Objetos, Genérico, Imperativo).
* **Patrón de diseño característico:** RAII (*Resource Acquisition Is Initialization*).
* **¿En qué consiste el patrón?:** Liga la vida de un recurso (memoria dinámica, sockets, descriptores de archivos, mutexes) al ciclo de vida de un objeto en el *stack*. El recurso se adquiere en el constructor y se libera automáticamente en el destructor cuando el objeto sale de su ámbito (*scope*), garantizando liberación determinista incluso si se lanzan excepciones.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Falta de destructores deterministas:** En lenguajes con recolección de basura (*Garbage Collection*) como Java, C# o Go, no existe garantía de cuándo se ejecuta un destructor/finalizador, lo que invalida el patrón y obliga a recurrir a bloques manuales como `try-with-resources`, `using` o `defer`.



---

**3. Python**

* **Paradigma principal:** Multiparadigma (Dinámico, Orientado a Objetos, Funcional ligero, Imperativo).
* **Patrón de diseño característico:** Decorator Pattern (Decoradores sintácticos de funciones y clases).
* **¿En qué consiste el patrón?:** Envuelve y altera el comportamiento de una función, método o clase sin modificar su código fuente original. Aprovecha que las funciones son objetos de primera clase para recibir la función destino como parámetro, encapsularla dentro de una clausura (*closure*) con lógica añadida y retornar la nueva función mediante la sintaxis `@decorador`.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Pérdida de flexibilidad estática:** En lenguajes estrictamente tipados sin macros avanzadas (como C++ o C clásico), envolver funciones preservando firmas de tipos arbitrarias requiere plantillas de plantillas (*template metaprogramming*) complejas y verbosas.
* **Sobrecarga de proxies:** En Java requiere el uso de proxies dinámicos o manipulación de *bytecode* (como AspectJ o CGLIB), alejándose de la ligereza original del patrón.



---

**4. JavaScript / TypeScript**

* **Paradigma principal:** Basado en Prototipos, Concurrente basado en Event-Loop, Funcional, Multiparadigma.
* **Patrón de diseño característico:** Middleware / Pipeline Asíncrono (estilo Express / Koa).
* **¿En qué consiste el patrón?:** Encadena una serie de funciones procesadoras donde cada una recibe el contexto de la petición, ejecuta lógica síncrona o asíncrona, y decide si pasa el control al siguiente eslabón mediante una función de callback `next()` o si interrumpe la cadena enviando una respuesta.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Bloqueo de hilos en modelos concurrentes tradicionales:** En plataformas basadas en hilos por petición (como Java servlets estándar o C++ multihilo), un middleware asíncrono requiere gestionar pools de hilos complejos para no bloquear recursos del sistema operativo, perdiendo la naturaleza no bloqueante y de bajo costo del *event loop* de hilo único.



---

**5. Go (Golang)**

* **Paradigma principal:** Concurrente (basado en CSP), Imperativo, Estructurado.
* **Patrón de diseño característico:** Functional Options Pattern.
* **¿En qué consiste el patrón?:** Permite instanciar estructuras complejas con múltiples parámetros opcionales y valores predeterminados mediante funciones que aceptan y modifican un puntero a la estructura. Evita constructores telescópicos y la proliferación de objetos de configuración intermediarios.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Boilerplate innecesario:** En lenguajes que soportan de forma nativa parámetros con nombre y valores por defecto (como Kotlin, Python, C# o Scala), implementar este patrón resulta redundante y añade líneas de código innecesarias frente a la sintaxis del lenguaje.



---

**6. Haskell**

* **Paradigma principal:** Funcional Puro, Tipado Estático Fuerte, Evaluación Perezosa (*Lazy*).
* **Patrón de diseño característico:** Monad Pattern (Mónadas: Maybe, Either, IO, Reader).
* **¿En qué consiste el patrón?:** Encapsula valores dentro de un contexto computacional (manejo de efectos secundarios, ausencia de valor, estado, fallos) y define operadores de encadenamiento secuencial (`bind` / `>>=`) manteniendo la pureza referencial y evitando la mutación de estado.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Ausencia de Tipos de Orden Superior (*Higher-Kinded Types*):** En lenguajes como C#, Java o TypeScript, emular mónadas genéricas obliga a perder seguridad de tipos o escribir envoltorios altamente complejos, ya que el sistema de tipos no puede abstraer el tipo constructor `M<T>`.
* **Efectos secundarios nativos:** En lenguajes imperativos, envolver I/O o mutación en una mónada es contraproducente, pues el propio entorno permite efectos laterales directos sin restricciones del compilador.



---

**7. Elixir / Erlang**

* **Paradigma principal:** Funcional, Concurrente (Modelo de Actores), Distribuido.
* **Patrón de diseño característico:** Supervisor Tree (Filosofía *"Let It Crash"*).
* **¿En qué consiste el patrón?:** Organiza los procesos de una aplicación en jerarquías donde los nodos supervisores monitorean procesos trabajadores y ejecutan estrategias de reinicio automático (`one_for_one`, `one_for_all`) cuando un proceso falla, garantizando tolerancia a fallos y auto-recuperación sin estado corrupto compartido.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Corrupción de memoria compartida:** En lenguajes basados en hilos del sistema operativo con memoria compartida (C, C++, Java), un fallo imprevisto puede corromper punteros o el estado global de la aplicación, haciendo imposible reiniciar un hilo de forma aislada sin reiniciar el proceso completo del sistema operativo.



---

**8. C#**

* **Paradigma principal:** Orientado a Objetos, Funcional, Concurrente, Basado en Componentes.
* **Patrón de diseño característico:** Task-based Asynchronous Pattern (TAP) con Streams Asíncronos (`IAsyncEnumerable`).
* **¿En qué consiste el patrón?:** Combina el modelo `async/await` con iteradores (`yield return`), permitiendo procesar secuencias de datos continuas bajo demanda sin bloquear el hilo principal y consumiéndolas con sintaxis `await foreach`. El compilador transforma el método en una máquina de estados optimizada.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Complejidad sin soporte a nivel de compilador:** En lenguajes que carecen de soporte sintáctico para máquinas de estado asíncronas, implementar este flujo requiere manejar manualmente buffers circulares, primitivas de sincronización y semáforos, aumentando exponencialmente el riesgo de *deadlocks*.



---

**9. Kotlin**

* **Paradigma principal:** Multiparadigma (OOP Estricto, Funcional Ligero, Estructurado).
* **Patrón de diseño característico:** Delegation Pattern nativo (palabra clave `by`).
* **¿En qué consiste el patrón?:** Permite aplicar el principio de composición sobre herencia sin escribir código puente manual. Al declarar `class Car(engine: Engine) : Engine by engine`, el compilador genera automáticamente todos los métodos delegados hacia la instancia interna.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Mantenimiento y código repetitivo:** En Java o C++, implementar la delegación manual de una interfaz con decenas de métodos requiere escribir y mantener cada método puente explícitamente, lo que incrementa el *boilerplate* y el riesgo de omitir delegaciones al actualizar interfaces.



---

**10. Swift**

* **Paradigma principal:** Orientado a Protocolos (*Protocol-Oriented Programming*), Funcional, OOP.
* **Patrón de diseño característico:** Protocol Extensions con Polimorfismo Estático.
* **¿En qué consiste el patrón?:** Define contratos mediante protocolos y proporciona implementaciones predeterminadas a través de extensiones que pueden aplicarse retroactivamente a cualquier tipo (incluso estructuras de valor `struct` y enumeraciones `enum`), evitando jerarquías rígidas de clases base.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Restricción a tipos por referencia:** En la mayoría de los lenguajes OOP clásicos, el polimorfismo exige herencia de clases o interfaces sobre objetos alojados en el *heap*, impidiendo extender tipos por valor o tipos sellados de terceros de manera retroactiva sin envoltorios adicionales.



---

**11. Ruby**

* **Paradigma principal:** Orientado a Objetos Puro, Dinámico, Reflexivo.
* **Patrón de diseño característico:** Internal DSL via Block-Yield y Metaprogramación (`instance_eval`).
* **¿En qué consiste el patrón?:** Permite diseñar minilenguajes declarativos embebidos ejecutando bloques de código dentro del contexto del objeto receptor, evaluando llamadas a métodos como directivas de configuración sin prefijos ni constructores visibles (sintaxis casi en lenguaje natural).
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Violación de ámbitos lexicales estáticos:** En lenguajes con enlace léxico estricto y tipado fuerte (como Java, Go o C++), cambiar el contexto de resolución de variables en tiempo de ejecución es ilegal para el compilador, obligando a usar constructores fluidos (*fluent interfaces*) mucho más verbosos.



---

**12. Scala**

* **Paradigma principal:** Fusión Funcional y Orientado a Objetos, Tipado Estático Avanzado.
* **Patrón de diseño característico:** Typeclass Pattern (Clases de Tipos).
* **¿En qué consiste el patrón?:** Proporciona polimorfismo *ad-hoc* extendiendo las capacidades de tipos existentes sin modificar su código fuente ni utilizar herencia, resolviendo instancias implícitas (`given` / `using`) de forma determinista en tiempo de compilación.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Dependencia del patrón Adapter:** En lenguajes OOP sin resolución implícita (como C# o Java estándar), extender tipos de terceros requiere envolver instancias en adaptadores (*wrappers*), lo que genera sobrecarga de asignación de memoria y rompe la identidad original del objeto.



---

**13. C**

* **Paradigma principal:** Procedural, Imperativo, Estructurado de Bajo Nivel.
* **Patrón de diseño característico:** Opaque Pointer (Puntero Opaco / Handle Pattern).
* **¿En qué consiste el patrón?:** Oculta la definición interna de una estructura (`struct Window;`) dentro del archivo de implementación `.c`, exponiendo al exterior únicamente un puntero incompleto (`Window*`) en el encabezado `.h`. Garantiza encapsulamiento estricto y estabilidad de la ABI (*Application Binary Interface*).
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Innecesario en lenguajes con encapsulamiento nativo:** En Java, C# o Python, los modificadores de acceso (`private`, `internal`) o módulos gestionan la visibilidad de forma nativa; forzar punteros opacos introduce gestión manual de memoria y destruye la introspección de los IDEs.



---

**14. Clojure**

* **Paradigma principal:** Funcional, Lisp, Concurrente (Estructuras Inmutables Persistentes).
* **Patrón de diseño característico:** Software Transactional Memory (STM con referencias `ref` y bloques `dosync`).
* **¿En qué consiste el patrón?:** Controla la mutación de estado compartido mediante transacciones atómicas similares a las de una base de datos (con control de concurrencia multiversión - MVCC). Si ocurre un conflicto de concurrencia entre dos hilos, la transacción se reintenta automáticamente sin usar bloqueos (*locks*) manuales.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Incompatibilidad con mutabilidad por defecto:** Si se intenta implementar STM en lenguajes como C++ o Python, donde las estructuras son mutables, un reintento transaccional que haya mutado un objeto interno dejará el sistema en un estado corrupto e inconsistente.



---

**15. PHP**

* **Paradigma principal:** Multiparadigma (Imperativo, OOP, Scripting Web).
* **Patrón de diseño característico:** Front Controller (Modelo *Share-Nothing*).
* **¿En qué consiste el patrón?:** Centraliza todas las peticiones HTTP a través de un único punto de entrada (`index.php`) que arranca el entorno, resuelve el enrutamiento, ejecuta la lógica y finaliza destruyendo todo el contexto de memoria al emitir la respuesta.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Ineficiencia en servidores persistentes:** En plataformas de larga duración (*long-running processes* como Node.js, Go o Rust), inicializar y destruir todo el contexto de la aplicación por cada petición genera un cuello de botella crítico; estos entornos exigen mantener el estado y las conexiones en memoria persistente.



---

**16. SWI-Prolog**

* **Paradigma principal:** Lógico, Declarativo.
* **Patrón de diseño característico:** Generate and Test (con Backtracking automático).
* **¿En qué consiste el patrón?:** Resuelve problemas combinatorios complejos separando la generación de posibles soluciones de la comprobación de restricciones lógicas. El motor de inferencia realiza retroceso (*backtracking*) de forma nativa si una rama de validación falla.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Explosión de complejidad manual:** En lenguajes imperativos o funcionales, emular el *backtracking* automático requiere implementar motores de búsqueda basados en pilas, árboles de decisión o recursión profunda propensa a desbordamientos de pila (*stack overflow*).



---

**17. Lua**

* **Paradigma principal:** Scripting Ligero, Basado en Prototipos, Procedural.
* **Patrón de diseño característico:** Metatable-based Prototypal Inheritance & Sandboxing.
* **¿En qué consiste el patrón?:** Modifica el comportamiento de tablas asociativas mediante metamétodos (`__index`, `__newindex`) para delegar llamadas a métodos y propiedades, permitiendo herencia prototípica dinámica o restringir accesos a funciones globales modificando la tabla `_ENV`.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Rendimiento y seguridad de tipos:** En lenguajes estáticamente tipados y compilados (C++, Rust), interceptar y redirigir el acceso a miembros de una estructura en tiempo de ejecución anula las optimizaciones del compilador y elimina el chequeo estático de tipos.



---

**18. F#**

* **Paradigma principal:** Funcional, Tipado Estático, Orientado a Expresiones.
* **Patrón de diseño característico:** Railway Oriented Programming (ROP).
* **¿En qué consiste el patrón?:** Modela los flujos de negocio como dos vías continuas (éxito y fallo). Encadena funciones que reciben y retornan tipos de unión discriminada (`Result<Success, Error>`), de modo que si una operación falla, las etapas posteriores se ignoran automáticamente y el error fluye hasta el final sin disparar excepciones.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Falta de tipos de unión discriminada:** En lenguajes que dependen de excepciones no verificadas o carecen de tipos suma nativos (como Java pre-records o Python), emular este patrón produce código verboso con constantes comprobaciones manuales de nulos o clases envoltorio pesadas.



---

**19. Julia**

* **Paradigma principal:** Técnico/Científico, Multiparadigma, Dinámico de Alto Rendimiento.
* **Patrón de diseño característico:** Holy Traits Pattern.
* **¿En qué consiste el patrón?:** Permite asociar comportamientos y optimizaciones a tipos arbitrarios sin recurrir a herencia de clases, utilizando el despacho múltiple (*multiple dispatch*) para seleccionar la implementación óptima de una función en tiempo de compilación según los tipos de rasgos asignados.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Falta de Multiple Dispatch nativo:** En lenguajes con despacho simple (*Single Dispatch* tradicional de OOP como Java o C#), emular este patrón exige implementar el patrón *Visitor* o largas cadenas de `instanceof` en tiempo de ejecución, introduciendo penalizaciones de rendimiento y acoplamiento.



---

**20. SQL**

* **Paradigma principal:** Declarativo, Relacional, Basado en Conjuntos.
* **Patrón de diseño característico:** Recursive Common Table Expression (CTE Recursivo).
* **¿En qué consiste el patrón?:** Procesa estructuras jerárquicas o en grafo (árboles organizacionales, categorías anidadas, redes) mediante una consulta base (*anchor member*) unida a una consulta recursiva que se ejecuta iterativamente sobre los resultados del ciclo anterior hasta agotar el conjunto de datos.
* **Fricción y problemas al trasladarlo a otros lenguajes:**
* **Problema N+1 y costo de I/O:** Si un lenguaje imperativo traslada la lógica de grafos realizando consultas individuales en un bucle hacia la base de datos, colapsa el rendimiento por latencia de red. Replicarlo en memoria requiere mapear los datos relacionales a estructuras de grafos con punteros u objetos en el heap.
