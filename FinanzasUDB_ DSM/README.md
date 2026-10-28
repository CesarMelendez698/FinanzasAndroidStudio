# Finanzas UDB - Aplicación Android (Kotlin)

Aplicación móvil desarrollada en **Kotlin** para la gestión financiera y académica de la comunidad estudiantil de la Universidad Don Bosco (UDB).

---

## 🏛️ Pantallas y Módulos del Sistema

### 1. Autenticación y Perfil
* **Inicio de Sesión (`LoginActivity.kt`):** Soporte de credenciales con opción "Recordar usuario", conmutador para ver/ocultar contraseña y validación de correo institucional `@udb.edu.sv`.
* **Registro (`RegisterActivity.kt`):** Validación de datos de nuevo usuario (nombre, apellido, correo institucional y contraseña segura).
* **Recuperación de Contraseña:** Diálogo modal flotante para solicitud y restablecimiento de contraseña vía Firebase Authentication.
* **Perfil de Usuario (`ProfileActivity.kt`):** Selección de carrera institucional, cambio de sede/campus, alternancia de tema Claro/Oscuro y cierre de sesión.

### 2. Panel Principal (`DashboardActivity.kt`)
* **Balance General:** Tarjeta de saldo neto con conmutador de privacidad (`$1,250.00` vs `$ • • • • • •`) e indicadores de flujo de ingresos y gastos.
* **Widget de Horario en Tiempo Real:** Identifica automáticamente la asignatura en curso o la próxima clase según la hora y día actual.
* **Accesos Rápidos Campus UDB:** Navegación directa hacia Mi Horario, Noticias UDB y Mi Perfil.
* **Saldos por Cuenta:** Desglose de saldos en Efectivo, Tarjeta y Cuenta bancaria.

### 3. Historial de Transacciones (`TransactionsActivity.kt`)
* **Búsqueda en Tiempo Real:** Filtrado por concepto, categoría o importe.
* **Filtros por Tipo y Período:** Selector para `Todos`, `Ingresos`, `Gastos` y filtrado por meses.
* **Comprobante Digital:** Al presionar cualquier transacción se despliega un comprobante con detalles completos y opción de eliminar o compartir.
* **Registro de Transacciones:** Modal para registrar nuevos ingresos o gastos asociando cuenta y categoría.

### 4. Presupuestos y Cuotas Universitarias (`BudgetsActivity.kt`)
* **Presupuestos por Categoría:** Barras de progreso de avance porcentual, gasto acumulado y balance disponible.
* **Control de Aranceles UDB:** Registro y seguimiento de cuotas del ciclo académico con estados `PAGADO` / `PENDIENTE`, métricas de total pagado vs pendiente y selector de ciclo.

### 5. Horario Académico (`ScheduleActivity.kt`)
* **Organizador Semanal:** Pestañas para cada día (Lunes a Sábado) con asignaturas, horarios, aulas y docentes.
* **Compartir Horario:** Generación de resumen estructurado para compartir mediante el selector nativo de Android.

### 6. Noticias UDB (`NewsActivity.kt`)
* **Consumo de API Institucional:** Conexión con el servicio oficial de noticias de la UDB mediante Retrofit.
* **Búsqueda y Filtro Anual:** Filtro por texto y año de publicación.
* **Detalle de Noticia:** Visualización completa con imagen optimizada vía Glide, descripción y enlaces para abrir o compartir.

---

## 🛠️ Tecnologías y Librerías

* **Lenguaje:** Kotlin
* **SDK Mínimo:** Android 7.0 (API 24)
* **SDK Compilación / Objetivo:** Android 14 (API 34)
* **Arquitectura:** Componentes de Android Jetpack (ViewBinding, ViewModel, Lifecycle)
* **Servicios e Integraciones:**
  * **Firebase:** Authentication y Cloud Firestore
  * **Retrofit 2 & OkHttp:** Consumo de APIs REST institucional
  * **Glide:** Carga asíncrona de imágenes
  * **Kotlin Coroutines:** Concurrencia y tareas en segundo plano
  * **Material Components:** Diseño con Material Design 3

---

## 🚀 Compilación y Ejecución

1. Abrir **Android Studio** (Koala, Jellyfish, Iguana o versiones recientes).
2. Seleccionar la opción **Open** y abrir la carpeta raíz de este proyecto.
3. Esperar que Gradle sincronice las dependencias del proyecto.
4. Conectar un dispositivo físico Android (con depuración USB activada) o iniciar un emulador de Android (API 24+).
5. Presionar **Run 'app'** (`Shift + F10`).
