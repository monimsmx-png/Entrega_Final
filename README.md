# 💳 AI Expense Automation

## Registro automatizado de gastos para control de presupuesto

Entrega Final · CoderHouse · Mónica Maldonado

Sistema autónomo construido en n8n que a partir de los correos de notificación que manda el banco con relación a los gastos generados con tarjeta de crédito, registra cada uno de los gastos que son notificados en Airtable (actúa como base de datos), extrae los datos clave del gasto usando IA (GPT-5), verifica que el establecimiento en donde se realizó el gasto no esté registrado, en este caso solicita aprobación (HITL) para registrarlo, y una vez autorizado, o si el establecimiento ya existe, registra el gasto. Notifica cuando hay un error y lo registra en la tabla Fallas. El final del día manda un resumen del total de registros capturados en el día.

La solución incorpora principios de arquitectura empresarial como:

-	Automatización basada en eventos 
-	Human in the Loop (HITL) 
- Validación de duplicados 
- Modelo relacional 
- Manejo de errores 
- Modularidad 
- Escalabilidad



## 📑 Índice

- [Objetivo](#objetivo)
- [Caso de Uso](#caso-de-uso)
- [Buenas Prácticas Implementadas](#buenas-prácticas-implementadas)
- [Resultados Esperados](#resultados-esperados)
- [Arquitectura](#arquitectura)
- [Estructura de Base de Datos](#estructura-de-base-de-datos-airtable)
- [Human-In-The-Loop (HITL)](#human-in-the-loop-hitl)
- [Salida Multicanal](#salida-multicanal)
- [Pruebas](#pruebas)
- [Enlaces de la Entrega](#enlaces-de-la-entrega)


## Objetivo🎯
Eliminar el registro manual de gastos bancarios y mantener un historial estructurado para análisis financiero, conciliación y generación de reportes.

## Caso de Uso🧩
Es importante llevar un control de todos mis gastos, ya que todos los realizo con tarjetas de crédito, y luego así es fácil perder el control…

El sistema:

Registra automáticamente en Airtable todos los gastos notificados por correo.

Extrae Establecimiento, Fecha, Monto usando IA (GPT‑5).

Solicita aprobación humana cuando detecta un establecimiento nuevo.

Evita duplicados mediante EmailID.

Envía un resumen diario de registros.

Clasificación IA Act: Riesgo Mínimo / Limitado
GDPR: Minimización de Datos (Art. 5.1.c) · Limitación de la Finalidad (Art. 5.1.b)

## Buenas Prácticas Implementadas🍀
- Idempotencia
- Linked Records
- Separación de responsabilidades
- Arquitectura modular
- Manejo de errores
- JSON normalizado
- Validaciones previas
- Human‑In‑The‑Loop

## Resultados Esperados📈
- Reducción del tiempo de captura manual
- Eliminación de duplicados
- Centralización de gastos
- Mejora en trazabilidad
- Base preparada para análisis financiero

## Arquitectura🧱

| Uso | Tecnología | Descripción |
| --- | --- | --- |
| Orquestador | n8n (local) | Flujo principal y flujo para reporte diario |
| Base de datos / Memoria | Airtable | Registro y relaciones |
| Procesamiento AI | OpenAI GPT‑5 | Extracción de datos del correo |
| Trigger | Outlook | Correos de notificaciones bancarias |
| Canal de salida | Outlook | Aprobaciones HITL, errores, notificaciones |


📄 Diagrama completo en: ![Diagrama_Arquitectura.pdf](https://github.com/monimsmx-png/Entrega_Final/blob/master/Diagrama%20AI%20Expense%20Automation-Final%20MNX-MVP-AI.pdf)

## Estructura de Base de Datos Airtable

Tabla: Gastos
| Campo | Tipo | Comentarios |
| --- | --- | --- |
| ID | Autonumérico | — |
| Fecha Registro | Fecha de creación | Registro |
| Fecha | Fecha | Fecha y hora del gasto |
| Establecimiento | Enlace | A tabla Establecimientos |
| Establecimiento_txt | Texto | — |
| Importe | Moneda | — |
| Fuente | Texto | NOT (correo) |
| Estado | Selección | Pendiente / Conciliado / Error / Omitir |
| RegistroNotificado | Selección | Pendiente / Notificado |
| EmailID | Texto | Evita duplicados |


Tabla: Establecimientos
| Campo | Tipo | Comentarios |
| --- | --- | --- |
| ID | Autonumérico | — |
| Fecha Creación | Fecha | — |
| Establecimiento | Selección | — |
| Categoría_txt | Selección | — |
| Subcategoria_id | Enlace | A Subcategorías |
| Revisado | Checkbox | — |
| Aprobado | Checkbox | — |


Tabla: Categorías
| Campo | Tipo |
| --- | --- |
| Name | Texto |
| ID_cat | Autonumérico |


Tabla: Subcategorías
| Campo | Tipo |
| --- | --- |
| Name | Texto |
| Categoría | Enlace a Categorías |


Tabla: Fallas
| Campo | Tipo |
| --- | --- |
| ID_falla | Autonumérico |
| Fecha_Hora | Fecha |
| Workflow | Texto |
| Nodo_Origen | Texto |
| Stacktrace | Texto |
| Mensaje_Error | Texto largo |
| Dato_Origen | Texto |
| Severidad | Texto |


### Relaciones🔗
Cadena jerárquica principal:

Categorias → Subcategorias → Establecimientos → Gastos

La tabla Fallas es independiente.


## Human‑In‑The‑Loop HITL
Se requiere aprobación humana para registrar establecimientos nuevos debido a variaciones en los nombres que envían los bancos.

## Salida Multicanal📡
Actualmente solo correo electrónico (Outlook).
No se incluye WhatsApp Business ni Slack.

## Pruebas🧪
Se realizaron pruebas preliminares con registros reales y correos recibidos durante varios días.
El sistema aún no se publica.

## Enlaces de la Entrega🔗
🎥 Video demo (3 min): https://drive.google.com/file/d/1_O5s3GAERuYR9qR05nNIvyGg_ZiqpFgv/view?usp=sharing

📊 Base Airtable (modo lectura):  
https://airtable.com/invite/l?inviteId=invpa2hVzozdZ0jWk&inviteToken=3f3680aa9f0fba1eda4c66d81eb94daa14c63a80966b274497ca3246d62291e3&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts

📐 Diagrama de Arquitectura: https://drive.google.com/file/d/1PHDS0tsKfAqKOwLawarmdfOppzhP95l3/view?usp=sharing

🔁 Lógica de Flujo principal: https://drive.google.com/file/d/192X5rojsZlg-bwTJl_jzNZ2j1X91EOi-/view?usp=sharing

🔁 Lógica de Flujo 2: https://drive.google.com/file/d/1OLXAYwpWxvhURByQpej6rWKAJfS6Gdcm/view?usp=sharing

📁 Evidencias: https://drive.google.com/file/d/1Q5-7uoiaw28yCSfITOMEXUSdp8-suU_y/view?usp=sharing
