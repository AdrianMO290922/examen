> [!NOTE]
 Utilice IA para comparación y redacción porque desconocía los lenguajes, pero fue muy interesante de que me percate de varios de los problemas
> y los acabe comprobando con lo que la IA me redacto, además de otro analisis mas especifico que leí, pero se resumio en esto:
### `cinerex_django` — Deuda Crítica y Proceduralismo Inseguro

* **Diagnóstico y Nivel de Calidad:** **Muy baja (No apto para producción).** Funciona únicamente como un prototipo o demo funcional; presenta vulnerabilidades graves y nula separación de responsabilidades.
* **Fallas Estructurales y de Seguridad:**
* **Inyección SQL directa:** Consultas construidas concatenando strings mediante cursores manuales, incluso en la validación de credenciales del login.
* **Bypass de autenticación:** Control de acceso vulnerable a manipulación de cookies de privilegios (`admin_bypass`).
* **Falta de atomicidad:** La compra de boletos, el cobro simulado, la asignación de asientos y la suma de puntos de socio se ejecutan en scripts sueltos sin transacciones de base de datos (`Unit of Work`).


* **Diagnóstico de Patrones:**
* *Estado actual:* **God View / Script Procedural.** Django se reduce a un simple enrutador de URLs hacia funciones monolíticas.
* *Patrones ausentes:* **Repository**, **Service Layer**, **Data Transfer Objects (DTOs)** y uso del **ORM de Django**.


* **Solución Arquitectónica:**
* Implementar **Django ORM** con consultas tipadas y parametrizadas para erradicar el SQL manual.
* Encapsular la compra en un `VentaBoletosService` con manejo transaccional (`@transaction.atomic`).
* Proteger las vistas con middlewares nativos y decoradores de permisos (`@permission_required`).


* **Veredicto:** **Reemplazar la capa de vistas y persistencia; rescatar rutas y templates HTML.**

---

###  `hotelluna_spring` — Prototipo Estructurado pero con Dominio Anémico

* **Diagnóstico y Nivel de Calidad:** **Intermedia / Prototipo aceptable.** Es el proyecto visual y estructuralmente más ordenado del grupo, pero carece de aislamiento entre la capa web y las reglas del negocio.
* **Fallas Estructurales y de Seguridad:**
* **Ausencia de capa de servicio formal:** Los controladores Spring MVC asumen responsabilidades de coordinación y lógica de reservaciones.
* **Configuración acoplada al entorno:** Manejo plano en `application.properties` sin separación por perfiles de despliegue (*dev*, *test*, *prod*).
* **Falta de validación declarativa:** Inexistencia de validaciones de reglas de negocio (como traslape de fechas o capacidad de habitaciones) antes de persistir.


* **Diagnóstico de Patrones:**
* *Estado actual:* **MVC clásico con Modelo de Dominio Anémico.**
* *Patrones ausentes:* **Service Layer (IoC)**, **State Pattern** (para el ciclo de vida de la habitación) y validación mediante **DTOs / Bean Validation**.


* **Solución Arquitectónica:**
* Inyectar capas `@Service` desacopladas mediante interfaces para aislar la lógica de estancia, check-in y facturación.
* Modelar los estados de las habitaciones (`Disponible`, `Ocupada`, `Mantenimiento`) con el patrón **State**.


* **Veredicto:** **Rescatar y refactorizar.** Su estructura base en Spring Boot es sólida; solo requiere introducir la capa de servicios y enriquecer el dominio.

---

### `pasofit_flutter` — Interfaz Fluida sin Abstracción de Datos

* **Diagnóstico y Nivel de Calidad:** **Aceptable para prototipo / Baja escalabilidad.** La navegación y la jerarquía visual funcionan correctamente, pero carece de arquitectura de software para mantenimiento a largo plazo.
* **Fallas Estructurales y de Seguridad:**
* **Acoplamiento UI-Lógica:** Los widgets y pantallas manejan directamente estados de variables, temporizadores y llamadas asíncronas dentro de los árboles de renderizado.
* **Rutas mágicas en texto:** Navegación propensa a fallos por el uso de strings hardcodeados (`/pantalla_rutina`).
* **Sin abstracción de almacenamiento:** La app no desacopla si las rutinas y métricas de peso provienen de una base de datos local (SQLite/Hive) o de una API remota.


* **Diagnóstico de Patrones:**
* *Estado actual:* **Screen-Driven UI con indicios débiles de estado reactivo.**
* *Patrones ausentes:* **BLoC / Cubit (State Management formal)**, **Repository Pattern** y **Typed Routing**.


* **Solución Arquitectónica:**
* Aislar la lógica del cronómetro y el cálculo de logros en **Cubits/Blocs** independientes de la UI.
* Implementar un `RutinaRepository` que abstraiga el origen y sincronización de datos.


* **Veredicto:** **Rescatar la UI y widgets; reconstruir el flujo de datos y la gestión de estado.**

---

### `sabores_laravel` — Acoplamiento al Framework y Peticiones Circulares

* **Diagnóstico y Nivel de Calidad:** **Baja (Código frágil / Ejercicio escolar).** Presenta vicios de acoplamiento severo donde el framework dicta la lógica en lugar de modelar el negocio de delivery.
* **Fallas Estructurales y de Seguridad:**
* **Llamadas circulares internas:** Uso de `file_get_contents` hacia endpoints locales para obtener menús o datos internos en lugar de consultar clases en memoria.
* **Fat Controllers con SQL concatenado:** Métodos de controlador sobrecargados que ejecutan SQL manual y generan fragmentos HTML en línea dentro de PHP.
* **Seguridad débil:** Autenticación manual dependiente de cookies sin políticas estrictas de middleware.


* **Diagnóstico de Patrones:**
* *Estado actual:* **MVC monolítico mal implementado (Smart Controllers).**
* *Patrones ausentes:* **Repository**, **Form Requests**, **Domain Events** y uso idiomático de **Eloquent ORM**.


* **Solución Arquitectónica:**
* Eliminar las llamadas HTTP locales sustituyéndolas por llamadas internas a servicios.
* Migrar a **Eloquent ORM** con consultas preparadas y validar entradas mediante `FormRequest`.
* Emitir eventos (`PedidoCreadoEvent`) para notificar a repartidores y cocina de manera asíncrona.


* **Veredicto:** **Reemplazar controladores y lógica de consulta; rescatar vistas Blade y rutas.**

---

### `tallerpro_express` — El Espejismo del Patrón sin Arquitectura

* **Diagnóstico y Nivel de Calidad:** **Media-baja (Cargo Cult Programming).** Es el ejemplo clásico donde se implementa un patrón de diseño superficial para aparentar estructura, pero el sistema base sigue roto por dentro.
* **Fallas Estructurales y de Seguridad:**
* **Patrón aislado en un God File:** La cadena de responsabilidad está escrita dentro de un único archivo principal junto con el resto del sistema.
* **SQL injection y render manual:** Consultas a MySQL sin parametrizar y generación artesanal de HTML dentro de los handlers de Express.
* **Bloqueo de hilos:** Envíos de correo mediante Nodemailer ejecutados de forma síncrona dentro del flujo principal de creación de órdenes.


* **Diagnóstico de Patrones:**
* *Estado actual:* **Chain of Responsibility cosmético sobre un script procedural.**
* *Patrones ausentes:* **Middleware Pipeline formal de Express**, **Repository / Query Builder**, **Observer / Event Bus** y **Strategy** (para métodos de cobro).


* **Solución Arquitectónica:**
* Convertir los pasos de la cadena en middlewares nativos reutilizables de Express (Auth, Logging, Validación con Zod).
* Desacoplar el envío de correos y la actualización de stock mediante eventos asíncronos (`EventEmitter`).
* Migrar a una API REST desacoplada eliminando el renderizado de HTML manual.


* **Veredicto:** **Reemplazar persistencia y render; rescatar y refactorizar el flujo lógico de procesamiento hacia una API limpia.**
