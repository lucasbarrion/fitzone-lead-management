# 📱 Funcionamiento Detallado del Sistema

Este documento explica en profundidad qué hace cada pantalla y cada herramienta del proyecto, pensado para acompañar con capturas de pantalla propias.

---

## 1. Power Apps — La aplicación operativa

Power Apps es el punto de partida de todo el sistema: es la interfaz que usa el personal de FitZone día a día para cargar y actualizar información. Se diseñó como una app de 4 pantallas, pensada para que cualquier persona sin conocimientos técnicos pueda operarla.

### 1.1 Pantalla de Inicio

Es la primera pantalla que ve el usuario al abrir la app. Su única función es la **navegación**: presenta 3 botones grandes y claros que llevan a cada una de las secciones funcionales del sistema (Registro de Leads, Buscar/Actualizar Lead, Gestión de Planes y Personal). Está configurada como pantalla de inicio (`StartScreen`) de la aplicación, por lo que es lo primero que carga siempre.

*[Insertar foto: Pantalla de Inicio]*

### 1.2 Pantalla — Registro de Leads

Esta pantalla resuelve el primer paso del proceso comercial: cuando una persona consulta por primera vez sobre el gimnasio (ya sea por Instagram, referido, la web o pasando por el local), el personal de recepción carga sus datos acá mismo, en el momento.

**Qué contiene:**
- Un formulario con los datos básicos del interesado: documento, nombre completo, teléfono, fecha de la consulta.
- Un selector desplegable para elegir el **canal de origen** (Instagram Ads, Referido, Pasada por el local, Web), con opciones fijas para evitar que se cargue el mismo canal con variaciones de escritura distintas.
- Un selector desplegable para elegir el **asesor comercial** que atendió la consulta, que se actualiza automáticamente si se agrega un empleado nuevo (lee en tiempo real la tabla de Personal).
- Un botón de "Guardar", que crea un registro nuevo en la base de datos con la etapa inicial "Consulta".

**Por qué está diseñada así:** en el momento de la primera consulta, todavía no se sabe si esa persona va a convertirse en socia, qué plan va a elegir, ni por qué motivo podría no seguir adelante. Por eso el formulario de esta pantalla es deliberadamente simple: solo pide la información que existe en ese momento. El resto de los datos (etapa, plan, motivo de pérdida) se completan más adelante, a medida que el proceso avanza, en la siguiente pantalla.

*[Insertar foto: Pantalla de Registro de Leads con el formulario cargado]*

### 1.3 Pantalla — Buscar y Actualizar Lead

Esta es la pantalla más usada del sistema en el día a día, porque es donde el personal hace el seguimiento comercial real: a medida que un lead avanza (se le agenda una clase de prueba, la realiza, se le ofrece un plan, se convierte en socio o se pierde), acá es donde se actualiza su estado.

**Qué contiene:**
- Un buscador de texto que filtra la lista de leads a medida que se escribe (por nombre).
- Una lista (galería) con los resultados de la búsqueda, mostrando de un vistazo el nombre, documento y etapa actual de cada lead — con un indicador visual de color según la etapa (por ejemplo, se puede distinguir rápidamente un lead "Cerrado - Socio" de uno "Cerrado - Perdido").
- Al tocar un lead de la lista, se despliega un formulario debajo con su información editable: la etapa actual (con un selector de las 6 etapas posibles del embudo comercial), el plan ofrecido, y el motivo de pérdida (si corresponde).
- Un indicador de alerta visual (⚠️) que aparece automáticamente al lado de los leads que llevan más de 3 días en etapa "Consulta" sin que nadie los haya contactado — para que el personal identifique de un vistazo, sin necesidad de revisar uno por uno, qué interesados están en riesgo de perderse por demora.
- Un botón "Guardar cambios", que actualiza el lead y, además, registra automáticamente en una tabla de historial la fecha exacta en la que se produjo el cambio de etapa — esto es lo que permite después calcular, en el análisis de Power BI, cuánto tiempo pasa un lead en cada etapa del proceso.

**Por qué está diseñada así:** el objetivo de esta pantalla es que el personal pueda hacer todo el seguimiento comercial (buscar, revisar el estado, actualizar) sin salir de una sola vista, y que el sistema deje registro automático de cada movimiento sin que el usuario tenga que hacer nada extra ni recordar completar una fecha a mano.

*[Insertar foto: Pantalla de Buscar Lead — galería de resultados]*
*[Insertar foto: Pantalla de Buscar Lead — formulario de edición de un lead]*

### 1.4 Pantalla — Gestión de Planes y Personal

Esta pantalla es de uso administrativo, pensada para el encargado de compras o dirección, no para el personal operativo del día a día.

**Sección de Planes:**
- Un selector desplegable para elegir cuál de los 3 planes (Mensual, Trimestral, Anual) se quiere modificar.
- Al elegir un plan, su precio actual se muestra automáticamente.
- Un campo para escribir el nuevo precio, y un botón "Guardar", que actualiza el precio vigente **y**, en la misma acción, deja registrado en una tabla de historial cuál era el precio anterior y desde cuándo rige el nuevo — sin este registro, se perdería la posibilidad de analizar después cómo evolucionaron los precios en el año.

**Sección de Personal:**
- Un campo de texto simple para escribir el nombre de un empleado nuevo, y un botón "Agregar", que lo suma a la lista de personal disponible.
- Apenas se agrega un nombre nuevo acá, automáticamente pasa a estar disponible como opción en el selector de "asesor comercial" de la Pantalla de Registro de Leads — sin necesidad de tocar nada más en el sistema.

*[Insertar foto: Pantalla de Gestión de Planes]*
*[Insertar foto: Sección de Alta de Personal]*

---

## 2. Power Automate — Lo que ocurre sin intervención humana

Mientras Power Apps depende de que una persona cargue o actualice algo, Power Automate resuelve la otra mitad del sistema: las tareas que deben ejecutarse solas, todos los días, sin que nadie tenga que acordarse de hacerlas.

### 2.1 Flujo de Alerta de Leads sin Contactar

Este es el flujo más importante del sistema desde el punto de vista de negocio, porque ataca directamente el problema central que reveló el análisis: los leads se pierden principalmente por falta de seguimiento, no por precio.

**Cómo funciona, paso a paso:**
1. Todos los días, automáticamente, el flujo se activa por sí solo (no depende de que alguien lo ejecute).
2. Revisa la base de datos completa de leads y filtra únicamente los que están en la etapa "Consulta" (es decir, los que todavía no fueron contactados ni avanzaron).
3. Para cada uno de esos leads, calcula cuántos días pasaron desde la última actualización.
4. Si ese número supera los 3 días, el sistema arma automáticamente un correo electrónico dirigido al equipo comercial, con el nombre del lead, su número de documento, su teléfono, y un **link directo de WhatsApp** (armado automáticamente a partir del número de teléfono) para poder contactarlo con un solo clic.
5. Si el lead fue contactado a tiempo (menos de 3 días), no se genera ninguna alerta — el flujo simplemente sigue de largo sin hacer nada.

**Por qué esto es valioso:** convierte una tarea que antes dependía de que alguien "se acuerde" de revisar manualmente la lista de leads pendientes, en un proceso que corre solo, todos los días, sin margen de olvido humano.

*[Insertar foto: Diagrama del flujo en Power Automate]*
*[Insertar foto: Ejemplo de email de alerta recibido]*

---

## 3. Power BI — El análisis que se actualiza solo

Power BI no carga ni modifica ningún dato — cumple un rol distinto y complementario: **lee** la misma base de datos que alimentan Power Apps y Power Automate, y la convierte en indicadores, gráficos y hallazgos de negocio.

### 3.1 Página — Resumen Ejecutivo

Pensada para que, de un vistazo de 10 segundos, cualquier persona (incluso sin conocimiento del detalle operativo) entienda cómo viene el negocio: cuántos leads entraron, cuántos se convirtieron en socios, cuál es la tasa de conversión general, y cómo se distribuyen los leads a lo largo de las distintas etapas del proceso comercial (con un gráfico de embudo). También muestra la evolución mes a mes de las consultas recibidas, lo que permite detectar estacionalidad (por ejemplo, más consultas en los meses de verano).

*[Insertar foto: Página de Resumen Ejecutivo]*

### 3.2 Página — Análisis Comercial

Esta página responde la pregunta "¿qué está funcionando y qué no, dentro del equipo y los canales de marketing?". Muestra la conversión de cada asesor comercial (permitiendo detectar quién convierte mejor y por qué), la conversión de cada canal de origen (para saber si vale la pena invertir más en publicidad o en el programa de referidos), el tiempo promedio que tarda el equipo en dar una primera respuesta, y un desglose de los motivos por los que se pierden los leads que no se convierten.

*[Insertar foto: Página de Análisis Comercial]*

### 3.3 Página — Planes y Precios

Esta página se enfoca en la parte comercial/financiera del negocio: cómo evolucionaron los precios de cada plan a lo largo del año, cuál es el plan que más eligen los socios, y una estimación de los ingresos generados en base a los planes vendidos.

*[Insertar foto: Página de Planes y Precios]*

### 3.4 Cómo se mantiene actualizado

Power BI Desktop no se actualiza en tiempo real por sí solo: cada vez que se quiere reflejar la información más reciente (por ejemplo, 
después de que el personal cargó leads nuevos desde Power Apps durante la semana), simplemente se abre el archivo y se presiona el botón "Actualizar".
En ese momento, Power BI vuelve a leer la base de datos completa, aplica automáticamente todos los pasos de limpieza y transformación que ya están programados,
y recalcula todos los gráficos y medidas — sin que haya que tocar ni rehacer nada manualmente.

---

## 4. Cómo se conectan las tres herramientas entre sí

```
El personal carga o actualiza un lead
              ↓
      (Power Apps escribe en la base de datos - Excel/SharePoint)
              ↓
Power Automate revisa la base todos los días y dispara alertas si corresponde
              ↓
Power BI lee la misma base y, al actualizar, recalcula todo el análisis
```

La idea central de esta arquitectura es que **el dato se carga una sola vez, en un solo lugar** (la app), y a partir de ahí se propaga solo hacia la automatización y hacia el análisis — sin necesidad de cargar la misma información dos veces en sistemas distintos.
