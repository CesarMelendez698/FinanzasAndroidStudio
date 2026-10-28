# 📘 Documentación Técnica y Funcional Completa: Finanzas UDB (DSM)

**Aplicación Móvil para la Gestión Financiera y Académica de Estudiantes de la Universidad Don Bosco**  
*Materia:* Desarrollo de Software para Móviles (Ciclo 04)  
*Tecnologías:* Kotlin Nativo • Android SDK 34 • Gradle 9.0 • Android Gradle Plugin 8.3.2 • Firebase (Auth & Firestore) • Material Design 3

---

## 📑 Tabla de Contenidos
1. [Visión General del Proyecto](#1-visión-general-del-proyecto)
2. [Arquitectura del Sistema y Tecnologías](#2-arquitectura-del-sistema-y-tecnologías)
3. [Estructura del Proyecto y Paquetes](#3-estructura-del-proyecto-y-paquetes)
4. [Modelo de Datos e Integridad en Firebase Firestore](#4-modelo-de-datos-e-integridad-en-firebase-firestore)
5. [Módulo de Autenticación y Seguridad](#5-módulo-de-autenticación-y-seguridad)
6. [Módulo de Perfil Estudiantil y Propagación Global](#6-módulo-de-perfil-estudiantil-y-propagación-global)
7. [Módulo de Tema Global (Modo Claro / Modo Oscuro)](#7-módulo-de-tema-global-modo-claro--modo-oscuro)
8. [Explicación Detallada de Cada Pantalla (Vistas)](#8-explicación-detallada-de-cada-pantalla-vistas)
   * 8.1 SplashActivity (Pantalla de Bienvenida)
   * 8.2 LoginActivity (Inicio de Sesión y Recuperación)
   * 8.3 RegisterActivity (Registro Estudiantil)
   * 8.4 DashboardActivity (Resumen Financiero y Próxima Clase)
   * 8.5 TransactionsActivity (Movimientos y Filtros Avanzados)
   * 8.6 BudgetsActivity (Presupuestos Mensuales y Cuotas UDB)
   * 8.7 ScheduleActivity (Horario Semanal, Calendario de Actividades y Materias)
   * 8.8 NewsActivity (Noticias, Avisos y Consejos Financieros UDB)
   * 8.9 ProfileActivity (Mi Perfil Estudiantil)
9. [Sistema de Alertas y Notificaciones Locales](#9-sistema-de-alertas-y-notificaciones-locales)
10. [Lógica Financiera y Algoritmos de Cálculo](#10-lógica-financiera-y-algoritmos-de-cálculo)
11. [Guía de Compilación y Ejecución](#11-guía-de-compilación-y-ejecución)
12. [Estructura de Trabajo Colaborativo en GitHub (Equipo)](#12-estructura-de-trabajo-colaborativo-en-github-equipo)

---

## 1. Visión General del Proyecto

**Finanzas UDB** es una aplicación móvil nativa para Android diseñada específicamente para satisfacer las necesidades cotidianas del estudiante de la Universidad Don Bosco (UDB). Integra en una sola plataforma:

* **Control Financiero Personal:** Registro detallado de ingresos y gastos, balances desglosados en Efectivo, Tarjetas y Cuentas Bancarias, modo de privacidad para ocultar montos en público y generación de comprobantes digitales de pago.
* **Control Presupuestario y Cuotas Universitarias:** Límites de gasto por categorías con alertas tempranas al superar el 80% y 100%, junto al seguimiento del pago de cuotas mensuales de la universidad.
* **Gestión Académica Integral:** Horario semanal de clases (teoría y laboratorio), cálculo dinámico de la próxima clase con cuenta regresiva, y un calendario interactivo para tareas, exámenes parciales y entregas de proyectos.
* **Canal Institucional:** Tablón de noticias universitarias, avisos de becas, calendario de ciclo y consejos de ahorro.

---

## 2. Arquitectura del Sistema y Tecnologías

La aplicación implementa el patrón arquitectónico desacoplado entre **Capa de Presentación (UI)**, **Capa de Control/Utilidades (Helpers & Adapters)** y **Capa de Datos/Nube (Repository)**:

```
[Activities & Layouts XML]
         │
         ▼
[Helpers: UserProfileHelper, ThemeHelper, NavigationHelper]
         │
         ├──► [SharedPreferences: user_academic_prefs, finanzas_udb_prefs]
         │
         ▼
[FirebaseRepository: Auth & Firestore Offline + Live Snapshots]
```

### Stack Técnico
* **Lenguaje:** Kotlin 1.9.22
* **Plataforma Objetivo:** Android 8.0 (API 26) hasta Android 14+ (API 34)
* **Build System:** Gradle 9.0 con Android Gradle Plugin 8.3.2
* **Backend:** Google Firebase
  * *Firebase Auth:* Registro, login, actualización de display name y reseteo de contraseñas por correo.
  * *Cloud Firestore:* Base de datos documental NoSQL con soporte de sincronización offline automática y snapshot listeners en tiempo real.
* **Componentes UI:** Material Design 3 (`MaterialCardView`, `ChipGroup`, `AppBarLayout`, `ConstraintLayout`, `BottomNavigationView`).

---

## 3. Estructura del Proyecto y Paquetes

```
app/src/main/java/sv/edu/udb/finanzasudb/
├── FinanzasApp.kt                      # Application class (inicio de canales, tema y Firebase)
├── adapter/                            # Adaptadores para listas dinámicas (RecyclerView)
│   ├── BudgetsAdapter.kt               # Tarjetas de presupuesto con barra de progreso
│   ├── CalendarActivityAdapter.kt      # Tarjetas de tareas, parciales y laboratorios
│   ├── GroupedSubjectAdapter.kt        # Vista unificada de materias (teoría + lab)
│   ├── NewsAdapter.kt                  # Tarjetas de noticias universitarias
│   ├── ScheduleAdapter.kt              # Clases del horario con opciones de editar y clonar
│   ├── TransactionsAdapter.kt          # Lista de transacciones con iconos por categoría
│   └── UniversityFeesAdapter.kt        # Cuotas de ciclo UDB (pagado / pendiente)
├── firebase/
│   └── FirebaseRepository.kt           # Repositorio central para Firestore y Auth
├── model/                              # Modelos de datos (POJOs en Kotlin)
│   ├── AccountCatalog.kt               # Cuentas bancarias o de efectivo del usuario
│   ├── Budget.kt                       # Límite presupuestario por categoría
│   ├── CalendarActivity.kt             # Actividades académicas (parcial, tarea, etc.)
│   ├── CategoryCatalog.kt              # Categorías financieras personalizadas
│   ├── NewsItem.kt                     # Modelo de noticias universitarias
│   ├── ScheduleItem.kt                 # Sesión de clase en el horario
│   ├── Transaction.kt                  # Registro financiero (ingreso / gasto)
│   ├── UniversityFee.kt                # Cuota universitaria del ciclo
│   └── UserProfile.kt                  # Perfil completo del estudiante UDB
├── ui/                                 # Pantallas de la aplicación (AppCompatActivities)
│   ├── BudgetsActivity.kt              # Presupuestos y control de cuotas UDB
│   ├── DashboardActivity.kt            # Resumen de cuentas, saldos y próxima clase
│   ├── LoginActivity.kt                # Inicio de sesión
│   ├── NewsActivity.kt                 # Portal de noticias y avisos
│   ├── ProfileActivity.kt              # Datos del estudiante y cuenta
│   ├── RegisterActivity.kt             # Registro de nuevo usuario
│   ├── ScheduleActivity.kt             # Horarios, calendario y materias
│   ├── SplashActivity.kt               # Splash inicial animado
│   └── TransactionsActivity.kt         # Historial y gestión de transacciones
└── utils/                              # Clases utilitarias y coordinadores
    ├── AppNotificationHelper.kt        # Notificaciones locales de clases y presupuestos
    ├── NavigationHelper.kt             # Barra inferior de navegación nativa
    ├── ThemeHelper.kt                  # Gestión limpia de modo oscuro/claro
    └── UserProfileHelper.kt            # Coordinador global del perfil estudiantil
```

---

## 4. Modelo de Datos e Integridad en Firebase Firestore

Todos los documentos en Firestore se organizan bajo colecciones globales particionadas por el correo del estudiante (`userEmail`) o por IDs únicos, garantizando total aislamiento de los datos entre diferentes estudiantes:

### 1. Perfil Estudiantil (`users/{email}`)
* `userId`: Correo del estudiante (Clave primaria).
* `name`: Nombre y apellido del estudiante (ej. "Cesar Lara").
* `email`: Correo institucional UDB.
* `career`: Carrera inscrita (ej. "Ingeniería en Ciencias de la Computación").
* `cycle`: Ciclo lectivo (ej. "Ciclo 01").
* `campus`: Campus asignado ("Campus Soyapango", "Campus Antiguo Cuscatlán", "Campus Virtual").
* `updatedAt`: Timestamp de la última modificación.

### 2. Transacciones (`transactions/{id}`)
* `id`: Hash único del registro.
* `title`: Descripción o concepto (ej. "Almuerzo cafetería UDB", "Beca mensual").
* `amount`: Monto monetario (Double).
* `type`: Tipo de operación: `"ingreso"` o `"gasto"`.
* `category`: Categoría asignada (ej. "Alimentación", "Transporte", "Universidad").
* `accountName`: Cuenta asociada (ej. "Efectivo", "Tarjeta Débito", "Banco Agrícola").
* `date`: Fecha en formato `dd-MM-yyyy`.
* `time`: Hora en formato `HH:mm`.
* `notes`: Comentarios o notas adicionales.
* `studentEmail`: Correo del estudiante propietario.

### 3. Presupuestos (`budgets/{id}`)
* `id`: Identificador del presupuesto.
* `category`: Categoría monitoreada (ej. "Alimentación").
* `limitAmount`: Monto límite mensual permitido.
* `monthYear`: Período en formato `MM-yyyy`.
* `studentEmail`: Propietario del presupuesto.

### 4. Cuotas Universitarias (`universityFees/{id}`)
* `id`: Identificador de la cuota.
* `feeName`: Nombre de la cuota (ej. "Cuota 1 - Ciclo 01").
* `amount`: Monto a cancelar (ej. $90.00).
* `dueDate`: Fecha límite de pago.
* `isPaid`: Booleano (true = Pagado, false = Pendiente).
* `cycle`: Ciclo correspondiente.
* `studentEmail`: Estudiante asignado.

### 5. Horario Semanal (`schedule/{id}`)
* `id`: Identificador de la sesión de clase.
* `subjectName`: Nombre de la asignatura (ej. "Desarrollo de Software para Móviles").
* `dayOfWeek`: Día ("Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo").
* `startTime`: Hora de inicio (formato 24h: `HH:mm`).
* `endTime`: Hora de fin (`HH:mm`).
* `classroom`: Aula o laboratorio (ej. "Edificio 3 - Aula 204", "Lab C-12").
* `professor`: Nombre del catedrático.
* `type`: `"Teoría"` o `"Laboratorio"`.
* `studentEmail`: Correo del estudiante.

### 6. Actividades de Calendario (`calendarActivities/{id}`)
* `id`: Identificador de la entrega o examen.
* `title`: Título de la actividad (ej. "Parcial 2 DSM", "Entrega Guía 4").
* `subjectName`: Asignatura asociada.
* `date`: Fecha `dd-MM-yyyy`.
* `time`: Hora límite `HH:mm`.
* `type`: `"Examen"`, `"Tarea"`, `"Laboratorio"`, `"Proyecto"`.
* `description`: Instrucciones o rúbrica de la actividad.
* `isCompleted`: Estado (true = entregada/hecha, false = pendiente).
* `studentEmail`: Propietario.

---

## 5. Módulo de Autenticación y Seguridad

* **Validación de Correo y Contraseña:** Cumple con formato estándar y seguridad mínima de 6 caracteres.
* **Soporte Offline y Persistencia de Sesión:** Si el usuario no cierra sesión explícitamente, `SplashActivity` detecta al `FirebaseRepository.currentUser` y lo redirige directamente al `DashboardActivity` sin requerir login continuo.
* **Recuperación de Contraseña:** Diálogo interactivo accesible tanto en `LoginActivity` como en `ProfileActivity` que despacha el correo oficial de restablecimiento de contraseña mediante `FirebaseAuth.sendPasswordResetEmail()`.
* **Cierre de Sesión Seguro (`signOut`):** Al cerrar sesión, se limpian las credenciales activas y se ejecuta `finishAffinity()` abriendo `LoginActivity` con banderas `FLAG_ACTIVITY_NEW_TASK or FLAG_ACTIVITY_CLEAR_TASK` para impedir regresar a datos privados mediante el botón Atrás.
* **Auto-Purga si el Usuario es Eliminado de Firebase:** Si un administrador elimina el usuario desde la consola de Firebase, la app detecta la invalidez en tiempo de ejecución (`FirebaseAuthInvalidUserException`), limpia la caché local y cierra la sesión de forma limpia.

---

## 6. Módulo de Perfil Estudiantil y Propagación Global

Para evitar inconsistencias entre pantallas o que los datos académicos se mostraran solo en una vista, el sistema implementa **`UserProfileHelper`**:

1. **Guardado en Doble Vía (Caché + Firestore):**
   Cualquier edición en el perfil (Nombre, Carrera, Ciclo o Campus) se escribe de inmediato en `SharedPreferences("user_academic_prefs")` bajo claves con el prefijo del correo del usuario (`${email}_name`, `${email}_career`, etc.) y se envía a Firestore mediante `SetOptions.merge()`.
2. **Edición Directa de Nombre de Estudiante:**
   En `ProfileActivity`, tocar el nombre del estudiante o el icono de edición despliega un diálogo para actualizar el nombre completo. Esto actualiza `UserProfileHelper`, el `displayName` de Firebase Auth y el documento en Firestore.
3. **Reflejo Inmediato en Toda la Aplicación:**
   * **En Resumen (Dashboard):** Se actualizan las iniciales del avatar, el nombre del estudiante y el nuevo subtítulo académico (`tvStudentCareer`).
   * **En Horarios:** Se actualiza el título de la carrera y el subtítulo `"Ciclo • Campus"`. Además, los diálogos de cambio de carrera en esta pantalla preservan el nombre del estudiante sin borrarlo.
   * **En Presupuestos:** La insignia de cuotas del ciclo (`tvCycleBadge`) se actualiza automáticamente.
   * **En Perfil:** Todas las tarjetas de información académica quedan sincronizadas.

---

## 7. Módulo de Tema Global (Modo Claro / Modo Oscuro)

El control del tema fue optimizado para que sea **completamente global, fluido y sin parpadeos**:

* **Control Maestro Único en Resumen (`?`):**
  * El cambio de tema se retiró de la vista de Perfil y ahora se controla desde el icono del **signo de interrogación (`?`)** ubicado en el encabezado de Resumen (`btnHeaderHelp`).
  * **Un toque:** Alterna instantáneamente entre Modo Claro y Modo Oscuro en toda la app y despliega un Toast informativo.
  * **Mantener presionado (Long Click):** Despliega el menú de selección explícito (`☀️ Modo Claro` / `🌙 Modo Oscuro`).
* **Eliminación del Parpadeo al Entrar a Cada Vista:**
  * Anteriormente, las pantallas en segundo plano retenidas en memoria sufrían una recarga visible (`recreate()`) al abrirse.
  * Ahora, `ThemeHelper.setThemeMode()` establece `AppCompatDelegate.setDefaultNightMode()` y reinicia la pila de tareas sin animaciones de ventana (`overridePendingTransition(0, 0)`).
  * Como consecuencia, cualquier pantalla a la que se navega posteriormente se infla directamente desde su `onCreate()` ya en el tema correcto en su primer fotograma, eliminando destellos blancos o recargas visuales.

---

## 8. Explicación Detallada de Cada Pantalla (Vistas)

### 8.1 SplashActivity (Pantalla de Bienvenida)
* **Función:** Punto de entrada de la aplicación (`android.intent.action.MAIN`).
* **Comportamiento:**
  1. Aplica el tema guardado mediante `ThemeHelper.applyThemeToActivity(this)`.
  2. Muestra el logotipo corporativo y el título "Finanzas UDB".
  3. Verifica si existe un usuario autenticado activo en Firebase:
     * Si existe: Navega de inmediato a `DashboardActivity`.
     * Si no existe: Navega a `LoginActivity`.
  4. Finaliza la actividad para no permanecer en el historial de navegación.

### 8.2 LoginActivity (Inicio de Sesión)
* **Función:** Acceso para estudiantes registrados.
* **Elementos y Flujo:**
  * Campos de texto para Correo Institucional (`etLoginEmail`) y Contraseña (`etLoginPassword`).
  * Casilla "Recordar sesión" con persistencia en `SharedPreferences`.
  * Botón "¿Olvidaste tu contraseña?": Despliega un diálogo (`dialog_recovery_password.xml`) para solicitar un enlace de restablecimiento al correo ingresado.
  * Botón "Iniciar Sesión": Realiza la validación en Firebase Auth y bloquea la interfaz con estado de carga para evitar pulsaciones dobles.
  * Enlace "¿No tienes cuenta? Regístrate aquí": Abre `RegisterActivity`.

### 8.3 RegisterActivity (Registro Estudiantil)
* **Función:** Creación de nueva cuenta de estudiante UDB.
* **Elementos y Flujo:**
  * Campos para: Nombre Completo, Correo, Contraseña, Confirmación de Contraseña.
  * Selectores de Carrera Universitaria (ICC, Mecatrónica, Industrial, etc.), Campus UDB (Soyapango, Antiguo Cuscatlán, Virtual) y Ciclo Académico inicial.
  * Al presionar "Crear Cuenta":
    1. Registra el usuario en `FirebaseAuth.createUserWithEmailAndPassword()`.
    2. Actualiza el perfil de Firebase con el nombre ingresado.
    3. Crea el documento inicial del estudiante en Firestore (`users/{email}`).
    4. Carga las cuentas predeterminadas ("Efectivo", "Tarjeta Débito", "Cuenta de Ahorros") y categorías base ("Alimentación", "Transporte", "Universidad", "Materiales").
    5. Inicia sesión automáticamente y navega al `DashboardActivity`.

### 8.4 DashboardActivity (Resumen Financiero y Académico)
* **Función:** Panel de control ejecutivo y pantalla principal de la aplicación.
* **Componentes Principales:**
  1. **Encabezado de Usuario:** Iniciales circulares, nombre del estudiante, carrera y ciclo actual (`tvStudentCareer`).
  2. **Acciones del Header:**
     * Icono `?` (`btnHeaderHelp`): Cambia el tema de toda la app (toque corto: alternar / toque largo: diálogo).
     * Icono Cerrar Sesión (`btnLogout`): Confirmación y salida segura.
  3. **Tarjeta de Balance General:**
     * Saldo Neto Total (Ingresos acumulados menos Gastos acumulados).
     * Botón de Privacidad (Ojo 👁️): Oculta o revela las cifras monetarias con un solo toque (reemplaza las cantidades con `$ • • • • •`).
     * Indicadores mensuales de Ingresos (`+$0.00`) y Gastos (`-$0.00`).
     * Desglose por tipos de cuenta: **Efectivo**, **Tarjeta** y **Cuentas Bancarias**.
  4. **Tarjeta de Próxima Clase Universitaria (`cardNextClass`):**
     * Algoritmo en tiempo real que calcula la clase más próxima según el día de la semana y la hora actual.
     * Muestra la asignatura, aula o laboratorio asignado y una cuenta regresiva dinámica (ej. *"EN 35 MIN"*, *"EN CURSO"* o *"DÍA LIBRE"* si no hay más clases hoy).
     * Al tocar la tarjeta, redirige directamente al horario de clases.
  5. **Accesos Rápidos a Servicios:**
     * Botones para ir directamente a Horarios, Noticias, Presupuestos y Perfil.

### 8.5 TransactionsActivity (Movimientos y Filtros)
* **Función:** Historial financiero completo, categorización, comprobantes y catálogo de cuentas.
* **Componentes Principales:**
  1. **Tarjeta Resumen del Filtro:** Balance resultante de los movimientos filtrados, desglose de ingresos/gastos y total de registros coincidentes.
  2. **Buscador Dinámico (`etTxSearch`):** Filtra en vivo por descripción, categoría, monto o notas.
  3. **Filtro Segmentado por Tipo de Movimiento (Fila 1):**
     * 3 columnas iguales: **Todos**, **Gastos**, **Ingresos**. Nunca se cortan ni superponen.
  4. **Filtro por Período y Fecha (Fila 2):**
     * Desplazamiento horizontal con opciones: **Todo**, **Hoy**, **Este Mes**, y **Por Fecha 📅** (abre DatePickerDialog para seleccionar un día específico).
  5. **Filtro por Cuentas y Categorías (Fila 3):**
     * Píldoras deslizables que cargan dinámicamente las cuentas (Efectivo, Tarjeta, Bancos) y categorías creadas por el usuario.
  6. **Botón Flotante "+ Nuevo":** Abre el diálogo modal completo para ingresar transacciones (monto, tipo, categoría, cuenta, fecha, hora y notas).
  7. **Comprobante Digital (Ticket):** Al presionar cualquier transacción de la lista, se abre un comprobante digital estilizado que detalla la transacción, fecha, hora, estado, cuenta y botón para compartir como texto.

### 8.6 BudgetsActivity (Presupuestos Mensuales y Cuotas UDB)
* **Función:** Control de límites de gasto mensual y seguimiento de pagos de colegiatura.
* **Pestaña 1: Presupuestos por Categoría:**
  * Selector de categoría y monto límite mensual.
  * Listado de categorías con barra de progreso porcentual (`ProgressBar`), gasto actual vs. límite establecido, y cambio de color automático:
    * *Verde:* Consumo normal (< 80%).
    * *Ámbar:* Alerta preventiva (>= 80%).
    * *Rojo:* Límite excedido (100%).
* **Pestaña 2: Cuotas Universitarias UDB:**
  * Control del estado de las cuotas del ciclo activo (ej. Cuota 1, 2, 3, 4, 5).
  * Resumen de total pagado y total pendiente de pago.
  * Opciones para marcar cuotas como canceladas o pendientes y añadir cuotas extraordinarias de laboratorio o matrícula.

### 8.7 ScheduleActivity (Horarios, Calendario y Materias)
* **Función:** Gestión académica semanal y calendario de entregas de la UDB.
* **Encabezado Académico:** Muestra la carrera del estudiante, el ciclo y el campus asignado, con botón para cambiar estos datos rápidamente.
* **3 Modos de Visualización:**
  1. **Vista Semanal:** Desglose día por día (Lunes a Domingo) de las clases inscritas. Cada tarjeta muestra horario, aula, catedrático, tipo (Teoría/Lab) y acciones para editar, eliminar o clonar la sesión a otro día.
  2. **Calendario de Actividades:** Selector de fecha interactivo con lista de tareas, exámenes parciales y entregas de proyectos, con opción de marcar como completada.
  3. **Mis Materias:** Agrupa teoría y laboratorio en una sola tarjeta por asignatura, mostrando horas totales semanales.
* **Exportar Horario:** Botón para generar un texto formateado del horario completo para compartirlo por WhatsApp u otras aplicaciones.

### 8.8 NewsActivity (Noticias y Avisos Universitarios)
* **Función:** Centro de información institucional y bienestar financiero.
* **Contenido en Tiempo Real:**
  * Avisos de fechas clave (inscripción de materias, retiro de asignaturas, evaluaciones extraordinarias).
  * Convocatorias de becas y pasantías UDB.
  * Tips y consejos prácticos de finanzas personales para universitarios (presupuesto para transporte, ahorro en fotocopias y cafetería).

### 8.9 ProfileActivity (Mi Perfil Estudiantil)
* **Función:** Gestión de identidad del estudiante, carrera, campus y configuración de cuenta.
* **Secciones:**
  1. **Identidad Estudiantil:**
     * Avatar con iniciales autogeneradas.
     * Nombre del estudiante con botón ✏️ para editarlo en cualquier momento.
     * Correo electrónico del estudiante.
     * Insignia oficial de *"Estudiante Activo UDB"*.
  2. **Información Académica:**
     * Fila Carrera: Diálogo para cambiar de carrera universitaria.
     * Fila Ciclo: Diálogo para editar el ciclo cursado (ej. "Ciclo 02").
     * Fila Campus: Selector de sede (Soyapango, Antiguo Cuscatlán o Virtual).
     * Contador de asignaturas inscritas activas en el horario.
  3. **Resumen de la Cuenta:**
     * Saldo neto consolidado y número total de transacciones registradas.
  4. **Seguridad y Cierre:**
     * Botón para solicitar correo de restablecimiento de contraseña.
     * Botón para cerrar sesión con diálogo de confirmación.

---

## 9. Sistema de Alertas y Notificaciones Locales

El motor `AppNotificationHelper` crea canales de notificación oficiales de Android (`NotificationManager`) compatibles con Android 13+ (requiere `POST_NOTIFICATIONS`):

1. **Canal de Clases Universitarias (`channel_classes`):**
   * Evalúa el horario semanal y notifica al estudiante 15 minutos antes del inicio de su clase indicando la materia y el aula asignada.
2. **Canal de Presupuestos y Gastos (`channel_budgets`):**
   * Dispara una alerta preventiva cuando el gasto de una categoría alcanza el 80% del límite mensual fijado.
   * Dispara una alerta crítica si el estudiante sobrepasa el 100% de su presupuesto mensual.
3. **Canal de Cuotas Universitarias (`channel_fees`):**
   * Avisa con anticipación sobre la proximidad de la fecha límite para el pago de la cuota mensual UDB.

---

## 10. Lógica Financiera y Algoritmos de Cálculo

### 1. Saldo Neto Total
$$\text{Saldo Neto} = \sum \text{Monto}(\text{Ingresos}) - \sum \text{Monto}(\text{Gastos})$$

### 2. Desglose Inteligente por Medio de Pago
El sistema evalúa el nombre de la cuenta vinculada (`accountName`) para clasificar los saldos de forma inteligente:
* **Efectivo:** Cuentas con términos *"efectivo"*, *"cash"*, *"billetera"*.
* **Tarjetas:** Cuentas con términos *"tarjeta"*, *"débito"*, *"debito"*, *"crédito"*, *"credito"*, *"card"*.
* **Bancos:** Cuentas con términos *"banco"*, *"bancaria"*, *"ahorro"*, *"bank"*, *"cuenta"*.

### 3. Algoritmo de Próxima Clase
En `DashboardActivity`:
1. Obtiene el día actual de la semana en español mediante `Calendar.getInstance()`.
2. Filtra las clases registradas en `schedule` correspondientes al día de hoy.
3. Ordena las clases por `startTime` (hora militar `HH:mm`).
4. Compara contra la hora actual del dispositivo:
   * Si la clase ya inició y no ha terminado: La marca como *"EN CURSO"*.
   * Si la hora actual es anterior a `startTime`: Calcula los minutos restantes y muestra *"EN X MIN"*.
   * Si ya concluyeron todas las clases del día: Muestra *"DÍA LIBRE"* e informa que no hay más clases pendientes hoy.

---

## 11. Guía de Compilación y Ejecución

### Requisitos Previos
* Android Studio Iguana / Jellyfish / Koala o superior.
* JDK 17 o superior configurado en `JAVA_HOME`.
* Dispositivo físico Android conectado con depuración USB habilitada o un Emulador Android (API 26 a 34).

### Configuración de la Solución en Windows
Debido a la presencia de caracteres acentuados en la ruta en Windows (`Móviles`), el archivo `gradle.properties` contiene la directiva obligatoria:
```properties
android.overridePathCheck=true
```

### Comandos de Compilación (Terminal PowerShell)
```powershell
# Compilar el APK en modo depuración (Debug)
.\gradlew.bat assembleDebug

# Verificar estado de tareas de Gradle
.\gradlew.bat tasks

# Limpiar y recompilar desde cero
.\gradlew.bat clean assembleDebug
```
*Ubicación del APK resultante:*
`app/build/outputs/apk/debug/app-debug.apk`

---

## 12. Estructura de Trabajo Colaborativo en GitHub (Equipo)

Para presentar el proyecto demostrando trabajo colaborativo y un historial de commits balanceado entre los 3 integrantes, se estructuran las siguientes responsabilidades por módulo:

| Integrante | Módulos y Tareas Asignadas | Archivos Principales del Commit |
| :--- | :--- | :--- |
| **César Lara** | • Coordinación general y arquitectura de datos Firebase<br>• Módulo de Perfil Estudiantil y sincronización global (`UserProfileHelper`)<br>• Sistema de Tema Claro/Oscuro y control con `?` (`ThemeHelper`) | `UserProfileHelper.kt`<br>`ThemeHelper.kt`<br>`ProfileActivity.kt`<br>`activity_profile.xml` |
| **César Meléndez** | • Módulo de Movimientos y Transacciones (`TransactionsActivity`)<br>• Diseño responsivo de filtros (Todos, Gastos, Ingresos, Fecha)<br>• Comprobantes digitales y catálogo de cuentas | `TransactionsActivity.kt`<br>`activity_transactions.xml`<br>`TransactionsAdapter.kt`<br>`Transaction.kt` |
| **Chris** | • Módulo Académico de Horarios y Materias (`ScheduleActivity`)<br>• Módulo de Presupuestos y Cuotas UDB (`BudgetsActivity`)<br>• Tarjeta interactiva de Próxima Clase en Dashboard | `ScheduleActivity.kt`<br>`activity_schedule.xml`<br>`BudgetsActivity.kt`<br>`DashboardActivity.kt` |
