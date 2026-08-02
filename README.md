# 🏋️ FitZone Córdoba
### Sistema de Gestión Comercial de Leads

> Proyecto de portafolio — Power Apps · Power Automate · Power BI

---

## 💡 Por qué armé este proyecto

Es un proyecto de portafolio que armé para practicar el ciclo completo de un analista de datos: no solo analizar información que ya existe, sino construir el sistema que la genera, automatizar lo que se pueda automatizar, y después sí, sentarme a sacar conclusiones útiles para el negocio.

FitZone es un gimnasio ficticio de Córdoba. Todo lo que hay acá —los leads, los nombres, los teléfonos— es información sintética que generé yo mismo, pensando en que tuviera lógica de negocio real detrás. La idea era simular un problema que le podría pasar a cualquier PyME de verdad.

## 🎯 El problema que quise resolver

Un gimnasio recibe consultas todo el tiempo: gente que ve un anuncio en Instagram, alguien que pasa caminando y pregunta, un socio que recomienda a un amigo. El tema es que, si ese seguimiento se hace a mano —un cuaderno acá, un WhatsApp allá—, es muy fácil perder de vista a un montón de esas personas sin que nadie se dé cuenta hasta que ya es tarde.

Quise armar un sistema que resolviera eso de punta a punta: una app para cargar y hacer seguimiento de cada consulta, una automatización que avise cuando alguien lleva mucho tiempo sin respuesta, y un dashboard que le muestre a la gerencia qué está pasando realmente con sus números.

## 🛠️ Qué construí

Usé el entorno de Microsoft (Power Apps, Power Automate y Power BI, todo conectado a una misma base en Excel/SharePoint) porque quería mostrar específicamente esa habilidad —hoy se pide mucho en las búsquedas de analista junior, y no es tan común verlo en un portafolio armado de punta a punta, en general la gente muestra solo el dashboard.

**📱 La app (Power Apps)** tiene cuatro pantallas. Una de inicio para navegar, una para cargar un lead nuevo apenas entra la consulta, una para buscar y actualizar el estado de un lead a medida que avanza (o no) en el proceso, y una última para que el encargado administre los precios de los planes y el equipo comercial.

**⚡ La automatización (Power Automate)** corre sola todos los días: revisa si hay algún lead que lleva más de tres días sin que nadie lo contacte, y si encuentra alguno, manda un mail de alerta con los datos de contacto y hasta un link directo de WhatsApp para escribirle en el momento.

**📊 El análisis (Power BI)** se conecta a la misma base de datos que usa la app —no es información aparte— y arma un dashboard de tres páginas: un resumen general, un análisis más fino de conversión por vendedor y por canal, y una vista de cómo evolucionaron los precios de los planes en el año.

📄 Documenté el funcionamiento de cada pantalla y cada herramienta con más detalle en [`FUNCIONAMIENTO_DETALLADO.md`](./FUNCIONAMIENTO_DETALLADO.md), por si a alguien le interesa entender cómo está armado sin tener que abrir la app.

📖 El relato completo de lo que encontré al analizar los datos, y las conclusiones que le llevaría a la gerencia, están en [`HALLAZGOS.md`](./HALLAZGOS.md).


## 🎯 Objetivos y preguntas que guiaron el proyecto

Antes de escribir una sola línea de código, definí qué necesitaba resolver este sistema. Esa etapa de diseño es la que después le da sentido a cada pantalla, cada automatización y cada gráfico del dashboard.

**Objetivos de negocio:**
- Aumentar la conversión de "interesado" a "socio pago".
- Reducir el tiempo de respuesta tras la clase de prueba.
- Detectar en qué etapa se pierden más potenciales socios.
- Comparar la performance del personal de ventas/recepción.

**Preguntas de negocio que el sistema tenía que poder responder:**
- ¿Qué porcentaje de los que hacen la clase de prueba se convierten en socios?
- ¿En qué etapa se pierden más leads —nunca agendan la prueba, la agendan pero no vuelven, o les ofrecen un plan y no compran?
- ¿Cuánto tarda en promedio un interesado en decidirse?
- ¿Qué canal de origen trae mejores conversiones —Instagram, referido, o pasada por el local?
- ¿Qué plan (mensual, trimestral, anual) se vende más, y cuál genera más rechazo?

**KPIs definidos para medir todo esto:**

| KPI | Cómo se calcula |
|---|---|
| Tasa de conversión global | Socios pagos / Total de interesados |
| Tasa de conversión por etapa | Embudo: Consulta → Prueba agendada → Prueba realizada → Propuesta de plan → Socio |
| Tiempo de primera respuesta | Fecha de contacto − Fecha de consulta |
| Leads estancados | Sin actividad reciente, sin cerrar |
| Conversión por vendedor/recepcionista | Socios / Leads asignados |
| Conversión por canal de origen | Socios / Leads, agrupado por canal |

Todas estas preguntas quedaron respondidas en el análisis final —el detalle está en [`HALLAZGOS.md`](./HALLAZGOS.md).

## 🗂️ Sobre los datos

Como no existe ningún dataset público con el historial comercial interno de un gimnasio (es información que ninguna empresa publica), armé el mío propio con Python: 1.050 leads a lo largo de 12 meses, tratando de que cada dato tuviera sentido de negocio real y no fuera pura casualidad.

Por ejemplo, no todos los leads avanzan de etapa con la misma probabilidad —un lead que llegó por referido tiene más chances de convertirse que uno que llegó por un anuncio de Instagram, y eso lo reflejé en cómo generé los datos. También metí de forma intencional algunos errores típicos de carga manual (nombres con mayúsculas mezcladas, teléfonos con formato distinto, algún dato faltante), para poder hacer después un proceso de limpieza real en Power Query, en vez de trabajar con datos ya perfectos de entrada.

Ninguno de los nombres, documentos o teléfonos corresponde a una persona real.

## 🧰 Stack

`Power Apps` `Power Automate` `Power BI` `Power Query` `DAX` `Python (pandas)` `Excel Online / SharePoint`



