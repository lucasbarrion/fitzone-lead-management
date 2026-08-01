# 🏋️ FitZone Córdoba — Sistema de Gestión Comercial de Leads

**Power Apps · Power Automate · Power BI**

Sistema end-to-end de captura, automatización y análisis de leads comerciales para un gimnasio ficticio, diseñado para demostrar el ciclo completo de un proyecto de Data Analytics: desde el diseño del proceso de negocio hasta las recomendaciones estratégicas basadas en datos.

---

## 📌 El problema de negocio

FitZone Córdoba recibe consultas de potenciales socios por múltiples canales (Instagram Ads, referidos, web, pasada por el local), pero el seguimiento comercial se gestionaba de forma manual y desordenada. Como consecuencia, la dirección no tenía visibilidad de:

- Cuántos interesados se perdían por falta de seguimiento
- Cuánto tardaba el equipo en responder a una consulta nueva
- Qué asesor y qué canal convertían mejor

Este proyecto digitaliza ese proceso completo y lo convierte en un sistema analizable.

---

## 🧩 Cómo funciona cada herramienta (explicación conceptual)

### Power Apps — la puerta de entrada de los datos

Lo primero que se construyó fue el **diseño de la interfaz**: una aplicación con distintas pantallas pensada para que el personal de FitZone la use en el día a día, sin necesitar conocimientos técnicos. Esta app permite:

- **Insertar nuevos leads:** cuando alguien consulta por primera vez, el personal carga sus datos (nombre, teléfono, canal de contacto) directo desde la app.
- **Agregar personal nuevo:** si se contrata un asesor comercial, se da de alta desde la misma app, sin tocar el Excel a mano.
- **Modificar leads existentes:** a medida que un lead avanza (agenda una clase de prueba, se le ofrece un plan, se convierte en socio o se pierde), el personal busca ese lead y actualiza su estado desde la app.

Todo lo que se carga o modifica en Power Apps se guarda automáticamente en la base de datos central (Excel en SharePoint) — es la única puerta de entrada de información al sistema.

### Power Automate — lo que pasa "solo", sin que nadie lo dispare

Una vez que los datos existen, hay reglas de negocio que deben ejecutarse automáticamente, sin depender de que una persona se acuerde de hacerlas. Por eso se armó un flujo que **corre todos los días por su cuenta** y revisa: ¿hay algún lead que lleva más de 3 días sin que nadie lo contacte? Si es así, manda una alerta por email al instante, sin que nadie tenga que pedirlo.

### Power BI — el mismo proyecto, pero mirado desde arriba

Power BI no carga ni modifica ningún dato — **lee** la misma base de datos que alimentan Power Apps y Power Automate, y la transforma en indicadores y gráficos. Esto es lo que hace que el proyecto sea un sistema **automatizable de punta a punta**: cada vez que el personal carga un lead nuevo o actualiza uno existente desde la app, ese cambio queda reflejado en la base de datos, y basta con apretar "Actualizar" en Power BI para que el dashboard completo (conversión, tiempos de respuesta, performance del equipo) se recalcule solo, con los datos más recientes — sin que haya que tocar ni una fórmula ni un gráfico de nuevo.

En otras palabras: **cargar un dato una sola vez, en un solo lugar (la app), alimenta automáticamente todo el resto del sistema** — la automatización de alertas y el análisis de negocio.

---

## 🧱 Arquitectura de la solución

```
Power Apps (captura)  →  Excel / SharePoint (fuente de datos)  →  Power Automate (automatización)
                                        ↓
                              Power BI (análisis y dashboard)
```

| Capa | Herramienta | Función |
|---|---|---|
| **Captura de datos** | Power Apps | 3 pantallas: alta de leads, gestión de planes/personal, búsqueda y actualización de etapa |
| **Automatización** | Power Automate | Alerta diaria por email (+ link de WhatsApp) para leads sin contactar en más de 3 días; registro de historial de precios |
| **Análisis** | Power BI | Limpieza de datos (Power Query), modelo relacional, 9 medidas DAX, dashboard de 3 páginas |
| **Fuente de datos** | Excel Online / SharePoint | 5 tablas: Leads, Historial_Etapas, Personal, Planes, Historial_Precios_Planes |

---

## 📊 Dataset

Dataset sintético de **1.050 leads** a lo largo de 12 meses (2025), generado con lógica de negocio realista (no aleatoria):

- Embudo de conversión de 6 etapas: Consulta → Prueba agendada → Prueba realizada → Propuesta de plan → Cerrado (Socio / Perdido)
- Probabilidades de avance por etapa, ponderadas por canal de origen y performance de cada asesor
- Estacionalidad (picos en enero-marzo)
- Errores de carga simulados intencionalmente (duplicados, formatos de teléfono inconsistentes, nulos reales vs. lógicos) para poder documentar un proceso de limpieza genuino

> **Nota:** todos los datos (nombres, documentos, teléfonos) son 100% ficticios, generados con Python. Ningún dato corresponde a personas reales.

---

## 🔍 Hallazgos clave del análisis

| Métrica | Resultado |
|---|---|
| Total de leads | 1.050 |
| Tasa de conversión global | 31% (327 socios) |
| Tiempo promedio de primera respuesta | **12 días** |
| Motivo de pérdida #1 | Falta de respuesta (**52%** de las pérdidas) |
| Mejor canal de conversión | Referido (50%) |
| Canal de mayor volumen pero menor conversión | Instagram Ads (16%) |
| Brecha entre mejor y peor asesor | 113 vs. 13 socios convertidos |
| Ingresos estimados totales | ~$44.000 |
| Plan más elegido | Mensual (155 socios), seguido de Trimestral (98) y Anual (73) |

**El hallazgo central:** más de la mitad de las oportunidades perdidas no se debe a precio ni a la competencia, sino a que el equipo tarda en promedio 12 días en responder — muy por encima del estándar recomendado de 24-48 horas.

📄 Ver el análisis completo con recomendaciones estratégicas en [`Informe_Ejecutivo_FitZone.docx`](./Informe_Ejecutivo_FitZone.docx).

---

## 🖥️ Power Apps — Capturas

| Pantalla | Función |
|---|---|
| Registro de Leads | Alta de un interesado nuevo, con dropdowns dinámicos de canal y personal |
| Gestión de Planes y Personal | Actualización de precios (con historial automático) y alta de nuevos empleados |
| Buscar y Actualizar Lead | Búsqueda por nombre/documento, cambio de etapa con indicador visual de leads sin contacto reciente |


---

## ⚙️ Power Automate

- **Flujo de alerta diaria:** revisa automáticamente los leads en etapa "Consulta" sin actividad reciente y envía un email con los datos de contacto y un link directo de WhatsApp.
- **Flujo de historial de precios:** registra cada cambio de precio de un plan, preservando el valor anterior para poder analizar la evolución en el tiempo.

---

## 📈 Power BI — Dashboard

**Página 1 — Resumen Ejecutivo:** KPIs generales, embudo de conversión, evolución mensual de consultas.
**Página 2 — Análisis Comercial:** conversión por asesor y por canal, motivos de pérdida, tiempo de primera respuesta.
**Página 3 — Planes y Precios:** evolución de precios por plan, plan más elegido, ingresos estimados.

*(Agregar capturas del dashboard en `/screenshots`)*

---



## ⚠️ Limitaciones conocidas

- **Delegación de Excel Online:** el conector de Excel en Power Apps tiene un límite de 2.000 filas para operaciones no delegables. La tabla `Historial_Etapas` se acerca a ese límite; en un entorno productivo real se recomienda migrar a SharePoint Lists o Dataverse.
- **Integración con Meta/Instagram Ads:** simulada (el canal "Instagram Ads" existe como dato en el dataset), dado que se trata de un negocio ficticio sin cuenta publicitaria real.
- Los datos son sintéticos; las probabilidades de conversión fueron definidas por el autor con criterio razonable de negocio, no calibradas contra datos históricos reales de la industria.

---

## 🧰 Stack técnico

`Power Apps` `Power Automate` `Power BI` `Power Query (M)` `DAX` `Python (pandas, generación de datos)` `Excel Online / SharePoint`

---

