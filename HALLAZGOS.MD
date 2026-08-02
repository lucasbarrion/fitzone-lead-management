# 🔎 Hallazgos y Conclusiones — La historia detrás de los números

Este documento es el corazón analítico del proyecto. Acá no solo dejo los números, sino el razonamiento completo detrás de cada hallazgo —la idea es que sirva como guía para explicar el proyecto en una entrevista, no solo como reporte.

---

## El punto de partida

Con el sistema armado y un año completo de datos cargados (enero a diciembre de 2025), llegó la parte que más me interesaba de todo el proyecto: sentarme a ver qué historia contaban los números. No quería quedarme en "armé un dashboard bonito" —quería poder decir con seguridad qué le está pasando al negocio y por qué.

FitZone recibió **1.050 consultas** en el año. De esas, **327 personas terminaron siendo socias**, lo que da una tasa de conversión del **31%**. A simple vista, es un número razonable —casi 1 de cada 3 personas que consultan termina inscribiéndose. Pero un número así, solo, no dice nada todavía. La pregunta que realmente importa no es "cuántos convertimos", sino **"por qué se perdieron los otros 723 que no"**, y ahí es donde apareció el hallazgo que le cambia el enfoque a todo el análisis.

---

## Una aclaración importante: esto no es un dashboard, es un sistema vivo

Antes de entrar en los hallazgos puntuales, quiero dejar clara una idea que para mí es central en este proyecto: el objetivo nunca fue simplemente "armar un dashboard bonito para mostrar en el portafolio". Los hallazgos que siguen a continuación son el resultado de analizar **un año de datos generado para poder demostrar el análisis**, pero lo que realmente construí es un **sistema de registro de leads en funcionamiento**, no una foto fija de un momento del negocio.

Esto es importante porque cambia por completo el valor real del proyecto para un cliente. Un dashboard tradicional analiza datos que ya existen y ahí termina su función. Este sistema, en cambio, tiene una app (Power Apps) donde el personal carga y actualiza leads todos los días, una automatización (Power Automate) que reacciona en tiempo real a lo que va pasando, y un análisis (Power BI) que **se recalcula solo** cada vez que hay información nueva. Es decir: los hallazgos de este documento son el punto de partida, no el destino final.

En la práctica, esto significa que a medida que FitZone siga usando el sistema y se sigan cargando leads reales, mes a mes, va a ser posible:

- **Medir si las recomendaciones realmente funcionaron** — por ejemplo, confirmar si el tiempo de primera respuesta efectivamente bajó de 12 días a 2-3 días después de aplicar la Prioridad 1 del informe ejecutivo, en vez de asumirlo.
- **Detectar nuevos patrones que hoy no son visibles** con solo un año de datos — por ejemplo, si la estacionalidad se repite año tras año, si un canal nuevo (como TikTok Ads) convierte mejor o peor que Instagram, o si la brecha entre asesores se achica una vez que se comparten las buenas prácticas.
- **Segmentar el análisis con más profundidad** a medida que haya más volumen — por ejemplo, cruzar el motivo de pérdida con la franja horaria en la que se hizo la consulta, o ver si los socios que empiezan en un plan Mensual terminan migrando a planes más largos con el tiempo.
- **Anticipar problemas antes de que se conviertan en pérdidas concretas**, gracias a que el sistema de alertas ya está funcionando desde el primer día, en vez de enterarse del problema recién en el análisis mensual.

Dicho esto, los hallazgos que siguen están basados en el primer año de datos disponible, y ya alcanzan para justificar un cambio de foco importante en cómo FitZone gestiona su proceso comercial.

---

## Las preguntas que me propuse responder, y si lo logré

Antes de tocar Power BI, en la etapa de diseño del proyecto, había definido un set de preguntas de negocio concretas que el sistema tenía que poder contestar. Repasarlas acá, una por una, es la mejor forma de mostrar que el análisis no fue "explorar a ver qué encuentro", sino ir a buscar respuestas específicas:

| Pregunta de negocio | Respuesta encontrada |
|---|---|
| ¿Qué % de los que hacen la clase de prueba se convierten en socios? | El embudo muestra que, una vez que un lead llega a "Propuesta de plan", la conversión a socio es alta —el cuello de botella real está antes, en llegar a agendar y realizar la prueba. |
| ¿En qué etapa se pierden más leads? | La mayor pérdida ocurre antes de agendar la prueba: un lead que nunca es contactado a tiempo ("Sin respuesta") nunca llega siquiera a esa instancia. |
| ¿Cuánto tarda en promedio un interesado en decidirse? | El tiempo de primera respuesta es de 12 días, muy por encima del estándar recomendado de 24-48 horas (ver Hallazgo 1). |
| ¿Qué canal de origen trae mejores conversiones? | Referido (50%) y Pasada por el local (44%) convierten mucho mejor que Instagram Ads (16%), pese a que este último es el que más volumen genera (ver Hallazgo 3). |
| ¿Qué plan se vende más y cuál genera más rechazo? | El plan Mensual es el más elegido (155 socios), lo cual también implica menor compromiso a largo plazo por parte del socio (ver Hallazgo 4). |

Y en cuanto a los objetivos de negocio planteados al principio del proyecto —aumentar la conversión, reducir el tiempo de respuesta, detectar en qué etapa se pierden más leads, y comparar la performance del equipo— los cuatro quedaron cubiertos: el Hallazgo 1 y 2 en conjunto explican dónde y por qué se pierden los leads y cómo se relaciona eso con el desempeño de cada asesor, y el sistema construido (alertas + dashboard) es la herramienta concreta para poder trabajar sobre el objetivo de aumentar la conversión de ahora en adelante.

---

## Hallazgo 1 — El problema no es el precio: es que no contestamos a tiempo

Cuando desglosé el motivo de pérdida de los 442 leads que se cerraron como perdidos, encontré esto:

| Motivo de pérdida | Cantidad | % del total perdido |
|---|---|---|
| Sin respuesta | 230 | **52%** |
| Precio | 84 | 19% |
| No volvió tras la prueba | 50 | 11% |
| Eligió otro gimnasio | 44 | 10% |
| Horarios | 34 | 8% |

Más de la mitad —el 52%— no se perdió por precio, ni porque encontraron algo mejor en otro lado, ni porque los horarios no les cerraban. Se perdieron porque **nadie les contestó a tiempo**. Y cuando fui a ver el tiempo real de respuesta, entendí por qué: el equipo tarda en promedio **12 días** en dar una primera respuesta a una consulta nueva. El estándar recomendado en cualquier proceso comercial —fitness, retail, servicios— es contestar dentro de las primeras 24 a 48 horas, momento en que el interés de la persona está en su punto más alto. Doce días es tiempo de sobra para que cualquiera se olvide de que alguna vez preguntó por el gimnasio, o directamente se haya anotado en otro lado mientras esperaba.

**Por qué esto es importante:** invierte la intuición típica de "para vender más, hay que bajar el precio o mejorar la oferta". Acá el dato muestra que la empresa está perdiendo casi 3 de cada 4 socios potenciales por un problema puramente **operativo y de proceso interno** —no por una decisión racional del cliente comparando opciones.

---

## Hallazgo 2 — Hay una brecha enorme de performance dentro del propio equipo

Cuando miré la conversión por asesor comercial, el patrón fue todavía más claro:

| Asesor comercial | Socios convertidos |
|---|---|
| Martín Rossi | 113 |
| Sofía Medina | 94 |
| Lucas Giménez | 56 |
| Camila Torres | 50 |
| Fede Acosta | 13 |

Martín Rossi cerró 113 socios en el año. Fede Acosta cerró 13. Estamos hablando de una diferencia de casi **9 veces**, gestionando un volumen de leads similar. Esa diferencia no se explica por suerte ni por casualidad —sugiere que dentro del propio equipo ya existe alguien que sabe hacer bien exactamente lo que al resto le está costando, y que ese conocimiento hoy no se está identificando, documentando ni replicando.

**Por qué esto es importante:** conecta directamente con el Hallazgo 1. Es muy probable que Martín y Sofía sean, justamente, quienes más rápido responden y hacen seguimiento —lo cual convierte este segundo hallazgo en evidencia que refuerza al primero, no en un dato aislado.

---

## Hallazgo 3 — No todos los canales de marketing valen lo mismo

| Canal de origen | Tasa de conversión |
|---|---|
| Referido | **50%** |
| Pasada por el local | 44% |
| Web | 31% |
| Instagram Ads | **16%** |

Acá aparece otra paradoja interesante: **Instagram Ads es el canal que más volumen de consultas genera**, pero también el que **peor convierte**. En otras palabras, es el canal más caro por socio efectivamente logrado, aunque en el reporte de marketing "genera muchos leads". Los referidos, en cambio, convierten al 50% —tres veces más que Instagram— sin ningún costo de pauta publicitaria asociado.

**Por qué esto es importante:** un análisis superficial de marketing miraría solo el volumen de leads generados por canal y concluiría que Instagram Ads "funciona bien". Mirando la conversión real, la conclusión es la opuesta: hay que revisar la calidad del lead que trae ese canal (segmentación, mensaje del anuncio), y mientras tanto, invertir más en potenciar el programa de referidos, que hoy es works orgánico y gratuito.

---

## Hallazgo 4 — El plan más elegido es el que menos compromete al socio

| Plan | Socios | Ingresos estimados |
|---|---|---|
| Mensual | 155 | — |
| Trimestral | 98 | — |
| Anual | 73 | — |

**Total: 327 socios · Ingresos estimados totales: $44.000**

El 47% de los socios elige el plan Mensual —el de menor compromiso y el más fácil de dar de baja apenas pasa un mes complicado. Esto no es necesariamente un problema en sí, pero sí representa una oportunidad: cada socio en plan Mensual es una decisión que se vuelve a tomar (y a poner en riesgo) todos los meses, mientras que un socio en plan Trimestral o Anual le da a la empresa mucha más previsibilidad de ingresos y reduce el trabajo de tener que "reconvertirlo" constantemente.

**Por qué esto es importante:** abre una segunda línea de acción, independiente del problema de respuesta lenta: incentivar (con descuentos, beneficios o mejor comunicación de valor) que los socios nuevos elijan planes más largos desde el principio.

---

## La conclusión que le llevaría a la gerencia

Si tuviera que resumir todo esto en una sola idea para gerencia, sería esta:

> **FitZone no tiene un problema de precio ni de marketing —tiene un problema de velocidad de respuesta.** Y es, de todos los problemas posibles que podría tener un negocio, el más barato de resolver: no requiere bajar precios, no requiere gastar más en publicidad. Requiere ordenar un proceso interno que hoy está fallando silenciosamente.

Lo que hace especialmente valioso este hallazgo es que **no hacía falta una intuición brillante para encontrarlo** —estaba escondido en los datos, esperando a que alguien cruzara el motivo de pérdida con el tiempo de respuesta. Eso es, para mí, la esencia de lo que tiene que hacer un analista: no solo mostrar números, sino encontrar la pregunta correcta que esos números pueden responder.

## Cómo se conecta esto con lo que construí

El sistema que armé en este proyecto no es solo un dashboard que "muestra" este problema después de que ya pasó —está diseñado para **atacarlo en tiempo real**:

- La **alerta automática de Power Automate** avisa apenas un lead lleva más de 3 días sin contacto, antes de que se convierta en una pérdida.
- El **indicador visual en Power Apps** le muestra al personal, en la misma pantalla donde trabaja todos los días, cuáles leads están en riesgo.
- El **dashboard de Power BI** permite hacer seguimiento semanal o mensual de si el tiempo de respuesta promedio efectivamente está bajando, convirtiendo la recomendación en algo medible y no en un consejo que se da una vez y se olvida.

En definitiva: la tecnología acá no es el fin del proyecto, es lo que hace posible que una decisión de gestión tan simple como *"contestemos más rápido"* se pueda sostener y medir en el tiempo, en vez de quedar en una buena intención.

Y esa es, en el fondo, la diferencia entre entregar un análisis puntual y entregar un sistema: este informe responde las preguntas que se pueden hacer hoy, con un año de datos. Pero el sistema que lo generó va a seguir funcionando, cargando información y recalculando el análisis mucho después de que este documento se haya escrito —listo para responder las preguntas que todavía no se hicieron.

---

*El desarrollo formal de este análisis, en formato de informe para gerencia con las recomendaciones priorizadas por costo/impacto, está en [`Informe_Ejecutivo_FitZone.docx`](./Informe_Ejecutivo_FitZone.docx).*
