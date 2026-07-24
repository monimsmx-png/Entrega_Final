📘 AI Expense Automation
Registro automatizado de gastos para control de presupuesto
Entrega Final | CoderHouse | Mónica Maldonado

Sistema autónomo construido en n8n que, a partir de los correos de notificación enviados por el banco sobre gastos realizados con tarjeta de crédito, registra automáticamente cada gasto en Airtable (base de datos).
La IA (GPT‑5) extrae los datos clave del correo, verifica si el establecimiento existe, solicita aprobación humana cuando es nuevo (HITL), registra el gasto y notifica errores.
Al final del día envía un resumen con los registros capturados.

📑 Índice
Objetivo
Caso de Uso
Buenas Prácticas Implementadas
Resultados Esperados
Arquitectura
Estructura de Base de Datos
Human‑In‑The‑Loop (HITL)
Salida Multicanal
Pruebas / Evidencias
Enlaces de la Entrega

🎯 Objetivo
Eliminar el registro manual de gastos bancarios y mantener un historial estructurado para análisis financiero, conciliación y generación de reportes.

🧩 Caso de Uso
“Para mí es importante llevar un control de todos mis gastos, ya que todos los realizo con tarjetas de crédito, y luego así es fácil perder el control de lo que se gasta…”
(Contenido tomado del documento adjunto)

El sistema:

Registra automáticamente en Airtable todos los gastos notificados por correo electrónico.

Extrae Establecimiento, Fecha y Monto usando IA.

Solicita aprobación humana cuando detecta un establecimiento nuevo.

Evita duplicados mediante verificación del EmailID.

Envía un resumen diario de registros.

Clasificación IA Act: Riesgo Mínimo / Limitado
Criterio GDPR: Minimización de Datos (Art. 5.1.c) y Limitación de la Finalidad (Art. 5.1.b)

🛠️ Buenas Prácticas Implementadas
Idempotencia

Linked Records

Separación de responsabilidades

Arquitectura modular

Manejo de errores

JSON normalizado

Validaciones previas

Human‑In‑The‑Loop (HITL)

📈 Resultados Esperados
Reducción del tiempo de captura manual

Eliminación de duplicados

Centralización de gastos

Mejora en trazabilidad

Base preparada para análisis financiero

🏗️ Arquitectura
Uso	Tecnología	Descripción
Orquestador	n8n (local)	Flujo principal y flujo para reporte diario
Base de datos / Memoria	Airtable	Registro y relaciones
Procesamiento AI	OpenAI GPT‑5	Extracción de datos del correo
Trigger	Outlook	Correos de notificaciones bancarias
Canal de salida	Outlook	Aprobaciones HITL, errores, notificaciones


📄 Diagrama completo en: Diagrama_Arquitectura.pdf

🗄️ Estructura de Base de Datos (Airtable)
Tabla: Gastos
Campo	Tipo	Comentarios
ID	Autonumérico	—
Fecha Registro	Fecha de creación	Registro
Fecha	Fecha	Fecha y hora del gasto
Establecimiento	Enlace	A tabla Establecimientos
Establecimiento_txt	Texto	—
Importe	Moneda	—
Fuente	Texto	NOT (correo)
Estado	Selección	Pendiente / Conciliado / Error / Omitir
RegistroNotificado	Selección	Pendiente / Notificado
EmailID	Texto	Evita duplicados


Tabla: Establecimientos
Campo	Tipo	Comentarios
ID	Autonumérico	—
Fecha Creación	Fecha	—
Establecimiento	Selección	—
Categoría_txt	Selección	—
Subcategoria_id	Enlace	A Subcategorías
Revisado	Checkbox	—
Aprobado	Checkbox	—


Tabla: Categorías
Campo	Tipo
Name	Texto
ID_cat	Autonumérico


Tabla: Subcategorías
Campo	Tipo
Name	Texto
Categoría	Enlace a Categorías


Tabla: Fallas
Campo	Tipo
ID_falla	Autonumérico
Fecha_Hora	Fecha
Workflow	Texto
Nodo_Origen	Texto
Stacktrace	Texto
Mensaje_Error	Texto largo
Dato_Origen	Texto
Severidad	Texto


🔗 Relaciones
Cadena jerárquica principal:

Código
Categorias → Subcategorias → Establecimientos → Gastos
La tabla Fallas es independiente.

👤 Human‑In‑The‑Loop (HITL)
Se requiere aprobación humana para registrar establecimientos nuevos debido a variaciones en los nombres que envían los bancos.

📡 Salida Multicanal
Actualmente solo correo electrónico (Outlook).
No se incluye WhatsApp Business ni Slack.

🧪 Pruebas
Se realizaron pruebas preliminares con registros reales y correos recibidos durante varios días.
El sistema aún no se publica.

🔗 Enlaces de la Entrega
🎥 Video demo (3 min)

📊 Base Airtable (modo lectura):  
https://airtable.com/invite/l?inviteId=invpa2hVzozdZ0jWk&inviteToken=3f3680aa9f0fba1eda4c66d81eb94daa14c63a80966b274497ca3246d62291e3&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts

📐 Diagrama de Arquitectura

🔁 Lógica de Flujo

📁 Evidencias
