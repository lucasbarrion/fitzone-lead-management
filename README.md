# fitzone-lead-management
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
| Ingresos estimados totales | ~$44.000.000 |
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

*(Agregar capturas de pantalla en `/screenshots`)*

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

## 🛠️ Metodología de trabajo

Este proyecto siguió un ciclo completo de Data Analytics:

1. Comprender el negocio (contexto, stakeholders, objetivos, KPIs)
2. Definir el proyecto (valor de portafolio, habilidades, herramientas)
3. Diseño del dataset (por qué generarlo, lógica de negocio documentada)
4. Plan del proyecto (alcance, entregables, limitaciones)
5. Limpieza de datos (Power Query — decisiones documentadas por cada tipo de problema)
6. Análisis exploratorio
7. Análisis de negocio (pensando como consultor, no solo describiendo datos)
8. Dashboard (Power BI)
9. Informe ejecutivo con recomendaciones estratégicas
10. Publicación (este repositorio, LinkedIn, entrevistas)

---

## ⚠️ Limitaciones conocidas

- **Delegación de Excel Online:** el conector de Excel en Power Apps tiene un límite de 2.000 filas para operaciones no delegables. La tabla `Historial_Etapas` se acerca a ese límite; en un entorno productivo real se recomienda migrar a SharePoint Lists o Dataverse.
- **Integración con Meta/Instagram Ads:** simulada (el canal "Instagram Ads" existe como dato en el dataset), dado que se trata de un negocio ficticio sin cuenta publicitaria real.
- Los datos son sintéticos; las probabilidades de conversión fueron definidas por el autor con criterio razonable de negocio, no calibradas contra datos históricos reales de la industria.

---

## 🧰 Stack técnico

`Power Apps` `Power Automate` `Power BI` `Power Query (M)` `DAX` `Python (pandas, generación de datos)` `Excel Online / SharePoint`

---

