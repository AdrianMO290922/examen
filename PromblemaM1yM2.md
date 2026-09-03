## Misión 1
### Control de Acceso y Vistas Monolíticas

**Problema del relato:**

Cada trámite es un archivo independiente con el mismo bloque copiado de sesión, bitácora y encabezado HTML; cambiar la caducidad exige editar 40 archivos.

* **Capa:** Políticas transversales y Presentación.
* **Patrón de diseño:** **Intercepting Filter** (Middleware) + **Composite View**.
* **El escenario real:** Un cambio de 15 a 30 minutos de inactividad obliga a buscar y reemplazar código en 40 archivos PHP sueltos. Olvidar uno deja una brecha de seguridad.
* **La analogía:** Un edificio con 40 oficinas donde se contrata a 40 guardias idénticos en cada puerta en lugar de poner **un único torniquete con credencialización en la entrada principal**.
* **¿Por qué este y no el vecino confuso?**
* *Frente a Decorator:* Decorator envuelve objetos ya instanciados en memoria durante la ejecución; el *Intercepting Filter* intercepta la petición HTTP a nivel de infraestructura web antes de que se ejecute cualquier lógica y puede cancelar la solicitud directamente.


* **¿Cuándo NO aplicaría?**
Cuando se construyen funciones serverless aisladas cuya autenticación y registro ya están delegados al API Gateway perimetral de la nube.

---

### Heterogeneidad Bancaria y Monolitos de Decisión

**Problema del relato:**

El script `pagar.php` es un `switch` de 200 líneas donde cada banco nuevo impone su propio protocolo, códigos numéricos y terminología ajena al dominio.

* **Capa:** Dominio e Integración.
* **Patrón de diseño:** **Strategy** + **Adapter**.
* **El escenario real:** Un banco responde con códigos `00/01` y habla de «créditos», mientras que la universidad maneja `Acreditado` y `Rechazado`. Agregar un nuevo banco arriesga romper los métodos de pago existentes dentro del mismo archivo de 200 líneas.
* **La analogía:** Un **adaptador de viaje universal**. No modificas el cableado interno de tu laptop para viajar; conectas el enchufe estándar al adaptador y este lidia con la clavija extraña de la pared.
* **¿Por qué este y no el vecino confuso?**
* *Frente a Facade:* Facade simplifica una librería compleja agrupando llamadas, pero *no traduce interfaces ni vocabularios*; el Adapter sí traduce entre contratos incompatibles.
* *Frente a State:* State altera el comportamiento según el *ciclo de vida* del objeto (Borrador $\to$ Pendiente $\to$ Pagado); Strategy intercambia el algoritmo de cobro según la elección inicial del usuario.


* **¿Cuándo NO aplicaría?**
En sistemas con un método de pago único y estático que no variará en el tiempo.

---

### Acoplamiento de Consultas y Lógica de Datos

**Problema del relato:**

La plantilla del kardex ejecuta SQL directamente para armar tablas, y los reportes de constancias duplican exactamente esas mismas consultas con otro formato.

* **Capa:** Datos.
* **Patrón de diseño:** **Repository** (con **Data Mapper**).
* **El escenario real:** Si se renombra la columna `calif` a `nota_final` en la base de datos, el kardex, las constancias y los certificados se rompen simultáneamente por tener SQL regado en las vistas.
* **La analogía:** Un **bodeguero en la alacena**. El cocinero pide los ingredientes ya preparados (`obtenerHistorialAcademico`) en lugar de apagar la estufa, salir al campo a sembrar el trigo y matar al cerdo en medio del servicio.
* **¿Por qué este y no el vecino confuso?**
* *Frente a Active Record:* Active Record fusiona la persistencia con la entidad ($1 \text{ tabla} = 1 \text{ clase}$), facilitando que el código SQL se filtre a cualquier script; *Repository* centraliza todas las consultas y aísla por completo el modelo de dominio de la base de datos física.


* **¿Cuándo NO aplicaría?**
En prototipos de un solo uso o scripts CRUD básicos sin reglas de negocio complejas.

---

### Procesamiento Asíncrono y Tolerancia a Duplicados

**Problema del relato:**

Si el envío de correo falla, a veces se cancela el alta de materias; además, hacer doble clic en «pagar» ejecuta dos cobros bancarios reales.

* **Capa:** Aplicación y Dominio.
* **Patrón de diseño:** **Observer** (Domain Events) + **Idempotency Key**.
* **El escenario real:** Un timeout con el servidor SMTP revierte la transacción universitaria a pesar de que el banco ya retiró el dinero del alumno.
* **La analogía:**
* *Observer:* El cajero sella el pago y grita por megáfono: *«¡Pago recibido!»*. El cartero escucha y entrega el recibo a su ritmo; si tropieza en el camino, la inscripción sigue siendo válida.
* *Idempotency Key:* Una ficha de turno con número único (`#4501`). Si el usuario muestra la ficha tres veces en ventanilla por desesperación, el cajero entrega una copia del comprobante previo sin cobrar de nuevo.


* **¿Por qué este y no el vecino confuso?**
* *Frente a Unit of Work:* Unit of Work administra la atomicidad en la base de datos, pero meter servicios de red externos (SMTP/APIs) dentro de una transacción de base de datos satura las conexiones; *Observer* desacopla los efectos secundarios.


* **¿Cuándo NO aplicaría?**
Cuando la notificación forme parte estricta y legal de la validez transaccional inmediata de la operación.

---

### Agregación Multicanal y Optimización de Carga

**Problema del relato:**

La app móvil dispara 12 peticiones HTTP para pintar la pantalla de inicio con datos mínimos, mientras el kiosco físico necesita HTML completo con escudos y tablas.

* **Capa:** Presentación e Integración.
* **Patrón de diseño:** **BFF (Backend for Frontend)**.
* **El escenario real:** Con redes móviles lentas, la app se congela varios segundos haciendo 12 llamadas secuenciales, mientras que el kiosco local requiere páginas web renderizadas en servidor con estilos pesados.
* **La analogía:** Un **mesero personal** en un restaurante con 12 estaciones de comida. El cliente pide una sola vez; el mesero recolecta todo internamente y entrega una sola bandeja optimizada a la mesa.
* **¿Por qué este y no el vecino confuso?**
* *Frente a API Gateway genérico:* Un Gateway estándar impone un único endpoint compartido que devuelve payloads sobrecargados a clientes ligeros o fuerza múltiples viajes de red.


* **¿Cuándo NO aplicaría?**
Cuando todos los clientes comparten el mismo ancho de banda, formato de consumo (solo web) y ciclo de vida de despliegue.

---

### Resiliencia ante Dependencias Externas

**Problema del relato:**

La caída de servicios externos (banco o validador de CURP) congela las consultas locales de kardex que no dependen de esos proveedores.

* **Capa:** Integración.
* **Patrón de diseño:** **Circuit Breaker** (con **Bulkhead**).
* **El escenario real:** El servidor de la CURP tarda 60 segundos en responder. Quinientos alumnos consultando su historial saturan el pool de conexiones del servidor universitario, tirando todo el portal.
* **La analogía:** El **fusible o pastilla termomagnética** de una casa. Ante un cortocircuito en un enchufe, la pastilla se bota para cortar el suministro de esa sección y evitar que se queme toda la instalación eléctrica.
* **¿Por qué este y no el vecino confuso?**
* *Frente a Retry:* Reintentar conexiones sobre un servicio caído satura aún más la red y agota los recursos locales de inmediato; el *Circuit Breaker* corta las llamadas de tajo y activa una respuesta alternativa (*fallback*).


* **¿Cuándo NO aplicaría?**
En llamadas locales dentro del mismo proceso en memoria donde no existe latencia de red ni riesgo de bloqueo de hilos.

---

### Rechazo de Sobreingeniería (Principio YAGNI)

**Problema del relato:**

Un proveedor propone Event Sourcing, CQRS, malla de microservicios y Redux global únicamente para autenticar, consultar un registro y descargar una constancia en PDF.

* **Diagnóstico:** Sobreingeniería severa que viola el principio **YAGNI (You Aren't Gonna Need It)**.
* **Justificación técnica:** Una descarga de constancia es una operación de solo lectura sincrónica. No existe procesamiento asíncrono masivo ni múltiples fuentes de eventos que justifiquen la complejidad operativa de Event Sourcing o CQRS.
* **Solución directa:**
* Autenticación vía **Middleware**.
* Lectura de datos mediante **Repository**.
* Generación del archivo mediante un servicio de plantillas PDF.



---

## Misión 2 – Ciclo de Vida de la Petición «Pagar Inscripción»

Flujo ordenado desde el envío del formulario hasta la confirmación:

```
[Cliente Web / Móvil] 
       │  POST /api/pagos/inscripcion (Header: Idempotency-Key)
       ▼
[1. Enrutador HTTP] ─────────────── Patrón: Front Controller
       │  Despacha la ruta al manejador correspondiente
       ▼
[2. Pipeline de Middlewares] ────── Patrón: Intercepting Filter
       │  Verifica sesión activa y filtra solicitudes duplicadas
       ▼
[3. Controlador Web] ────────────── Patrón: Controller / Mediator
       │  Valida el formato del payload y extrae los datos
       ▼
[4. Servicio de Aplicación] ─────── Patrón: Application Service / Facade
       │  Coordina el caso de uso del negocio
       ├──► [5. Pasarela & Adaptador] ── Patrón: Strategy + Adapter
       │         └── Traduce datos y ejecuta el cobro con el banco
       │
       ├──► [6. Repositorio & UoW] ───── Patrón: Repository + Unit of Work
       │         └── Registra el pago y actualiza la matrícula en BD
       │
       └──► [7. Despachador de Eventos] ─ Patrón: Observer (Domain Events)
                 └── Publica 'PagoAcreditado'
                           ├──► Listener 1: Alta en control escolar
                           ├──► Listener 2: Notificación por correo
                           └──► Listener 3: Registro contable en caja

```
