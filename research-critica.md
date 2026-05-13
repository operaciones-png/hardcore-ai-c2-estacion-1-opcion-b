# Autopsia del "Cerebro Financiero Conversacional" — Auditoría de Riesgos de Fracaso
### Green Power E&P | Llanos Orientales | Mayo 2026
> **Metodología:** Este documento no valida el proyecto. Destruye sus supuestos. Cada sección identifica los mecanismos de fracaso más probables, con evidencia externa. Sin veredicto final — solo riesgos.

***

## 1. Riesgos de Fracaso Técnico en RAG Financiero

### 1.1 Tasa de alucinación al devolver cifras numéricas precisas

Los LLMs no son calculadoras. Son predictores de tokens. Cuando devuelven un número, el modelo no "busca" la cifra: genera el token más probable dado el contexto. Esto tiene consecuencias directas en un chat financiero.

El benchmark **FinanceBench** (Patronus AI, Stanford, Contextual AI) es el estándar más relevante: evalúa exactamente el caso de uso de este proyecto — preguntas sobre documentos financieros reales. Los resultados son devastadores para el optimismo:

- Claude-2 con contexto largo: **76% de tasa de éxito**, lo que implica que **1 de cada 4 respuestas con cifras financieras es incorrecta** con RAG convencional.[^1]
- En tareas de razonamiento numérico multi-paso (el tipo más frecuente en reportes de E&P: "¿cuánto gastamos en workover en Kimbo vs. Caliche este trimestre?"), la precisión cae adicionalmente.
- El benchmark FinanceBench evidencia explícitamente "LLM challenges including hallucinations and high refusal rates" incluso con preguntas sobre documentos reales y trazables.[^2]

Desde una perspectiva más amplia, las tasas de alucinación general de los modelos en 2026 varían enormemente: desde 0.7% en modelos top hasta **45.15% en GPT-4o cuando no rechaza la pregunta**. Claude Opus registra 10.1% de tasa de alucinación general. **Pero estas tasas generales subestiman el problema en dominio financiero especializado**, donde la precisión numérica es binaria (el número es correcto o no lo es). Un 5% de error en una respuesta narrativa es tolerable; un 5% de error en el retorno a inversionistas no lo es.[^3][^4]

Adicionalmente, el estudio sobre 5,000 informes anuales encontró que la tasa de error de AI al leer estados financieros fue **9.19% incluso con documentos estructurados en XBRL** — y las facturas de proveedores de petróleo en los Llanos no vienen en XBRL.[^5]

### 1.2 Cómo se rompe el RAG con preguntas ambiguas, multifuente o cross-período

El RAG tiene un defecto arquitectónico fundamental para este proyecto: recupera fragmentos ("chunks") localmente relevantes, pero **no entiende la relación entre documentos de fuentes distintas ni razona sobre agregaciones temporales**. Los modos de fracaso documentados incluyen:[^6][^7]

- **Multi-hop reasoning failure:** Una pregunta como "¿cuánto fue el costo total de operación del bloque Kimbo en el primer trimestre comparado con el mismo período del año anterior?" requiere conectar datos de facturas, nóminas, PILA, caja menor y comunidades across 6+ meses de documentos. Los sistemas RAG fallan silenciosamente en estos casos — responden con autoridad usando solo uno o dos fragmentos recuperados.[^8]
- **Aggregation failure:** RAG no suma. Cuando el contexto de context window no contiene todos los registros relevantes, el modelo agrega parcialmente o inventa totales. IBM documenta explícitamente los "limits on context windows and aggregation operations" como los desafíos más frecuentes del RAG en producción.[^6]
- **Temporal staleness y cross-document contradictions:** Si una factura fue corregida o un valor ajustado (práctica común en contabilidad de E&P), el sistema puede recuperar la versión antigua y la nueva simultáneamente, generando respuestas contradictorias o promediadas.[^7]
- **Ambigüedad de fuente:** "¿Cuánto pagamos de seguridad social?" puede referirse a PILA procesada, provisión de RRHH, o ambas. El RAG no desambigua — elige un fragmento y responde con confianza total.

### 1.3 Problemas específicos de vocabulario técnico en español de oil & gas

Los modelos de embeddings y los LLMs tienen cobertura débil en vocabulario técnico de E&P en español latinoamericano. Términos como **workover, slickline, AFE (Authorization for Expenditure), JOA (Joint Operating Agreement), recuperable, lifting cost, take-or-pay, royalties ANH, regalías variables** tienen representaciones vectoriales pobres en modelos de embeddings genéricos.[^9]

La investigación de SLB sobre LLMs en el sector energético confirma que el RAG estándar sin fine-tuning de embeddings produce "context relevancy" insuficiente para terminología de dominio. Un estudio de EarthDoc demostró que solo con fine-tuning específico para oil & gas se lograba un incremento del 19.46% en context relevancy. Sin ese fine-tuning — que no está en el scope del MVP — el sistema recuperará fragmentos incorrectos cuando el Gerente pregunte sobre conceptos técnicos de operación.[^10][^9]

**El riesgo concreto:** Una pregunta sobre "costo de intervención de pozo" podría recuperar fragmentos de facturas de comunidades mal clasificadas como workover, o ignorar completamente los AFEs aprobados que deberían limitar ese gasto.

### 1.4 El Gerente envía cifras incorrectas a inversionistas

Este es el escenario de destrucción de credibilidad. Los LLMs responden con el mismo tono de autoridad independientemente de si la respuesta es correcta o incorrecta. No hay señal de incertidumbre visible. El Gerente de Operaciones, que no es contador, recibe una cifra con aspecto de precisión contable y la incorpora al reporte.[^11]

El caso Deloitte es ilustrativo: un reporte de $440,000 comisionado por el gobierno australiano fue devuelto por errores significativos incluyendo referencias académicas fabricadas y una cita falsa atribuida a un juez federal — todo generado por AI sin supervisión adecuada. En ese caso, el costo fue económico y reputacional. En el caso de Green Power, el costo es la relación con los inversionistas del proyecto de E&P.[^11]

La investigación de PMC sobre confianza humano-AI en finanzas es directa: **un solo error de advisory resultó en una caída sustancial de confianza (η² = 0.141)**, y los usuarios con mayor expertise financiero muestran caída más abrupta después del error — exactamente el perfil de un inversionista sofisticado de E&P.[^12]

***

## 2. Riesgos de Integridad del Reporte a Inversionistas

### 2.1 Clasificación automática de gastos entre bloques

Este es el riesgo más probable de materialización. Green Power opera dos bloques (Kimbo y Caliche). Muchos costos son compartidos: mano de obra que trabaja en ambos bloques en el mismo período, equipos utilizados en operaciones de los dos activos, facturas de proveedores de servicios generales (logística, alimentación, campamento). La **asignación entre bloques es una decisión contable que requiere criterio humano** — prorrateo por horas, por producción, por acuerdo contractual del JOA.

Un sistema de clasificación automática que asigne mal una factura compartida de $50M COP de servicios de perforación de Caliche al bloque Kimbo genera:
- Utilidad operacional de Kimbo artificialmente alta
- Utilidad operacional de Caliche artificialmente baja
- Retorno a inversionistas incorrecto por bloque
- Si los inversionistas tienen participación diferenciada por bloque (estructura común en JOAs colombianos), el cálculo de distribuciones es erróneo

**No hay forma de que el RAG detecte este error por sí solo**, porque el sistema no tiene el JOA para comparar ni la lógica de prorrateo — a menos que se programe explícitamente, lo cual no está en el alcance del MVP de 4 semanas.

### 2.2 Mitigación del error de "porcentaje de retorno a inversionistas"

La mitigación real requiere: (a) tabla de reglas de asignación por centro de costo codificada explícitamente, no inferida por el LLM; (b) proceso de validación humana antes de consolidar el reporte; (c) cuadre de doble entrada que detecte si los totales por bloque suman al total de la empresa. **Ninguno de estos controles está en el MVP descrito.** Un generador automático de reportes sin cuadre matemático verificable es una fuente de errores con apariencia de precisión.

### 2.3 Facturas compartidas entre bloques

Las facturas de servicios petroleros — perforadores, empresas de transporte, catering en campo — frecuentemente cubren operaciones en múltiples bloques dentro del mismo período. La regla estándar es que la factura se imputa al bloque que solicitó el servicio o se proratea por un criterio técnico (horas-pozo, barriles producidos). Un parser automático de facturas ve el monto total y necesita instrucciones explícitas de prorrateo. Sin esas instrucciones, **la IA asignará el 100% al bloque que aparezca primero en el documento o en el contexto recuperado**.

### 2.4 Casos documentados de pérdida de credibilidad por reportes automatizados

- **Deloitte Australia (2024-2025):** reporte gubernamental de $440,000 con errores de AI incluyendo referencias académicas fabricadas, devuelto por el cliente.[^11]
- **EY Survey (2025):** casi la totalidad de grandes organizaciones que desplegaron AI reportaron pérdidas financieras iniciales; el total combinado estimado fue de **$4.4 billones de dólares** en pérdidas por AI implementations, con errores de compliance e inaccuracy como principales causas.[^13]
- **Bloomberg Law (2024):** La SEC multó dos asesores de inversión en $400,000 por representaciones falsas sobre capacidades de AI en reportes a clientes.[^14]
- La investigación de Xledger documenta que "persistent issues with data quality can erode stakeholder trust; investors and board members may lose confidence" en reportes financieros automatizados.[^15]

***

## 3. Riesgos Regulatorios y Legales en Colombia

### 3.1 Marco regulatorio aplicable a reportes a inversionistas privados

Green Power con dos bloques de E&P en los Llanos Orientales es probablemente una sociedad que, dependiendo de sus activos e ingresos, puede quedar bajo vigilancia de la **Superintendencia de Sociedades**. Las sociedades vigiladas deben presentar estados financieros auditados y están sujetas a control de veracidad informativa. Si los ingresos o activos superan los umbrales definidos (en 2025, activos o ingresos superiores a 30,000 SMMLV), los reportes a inversionistas que contengan información falsa o engañosa pueden generar responsabilidad legal directa para los administradores.[^16][^17]

La **Superintendencia Financiera de Colombia (SFC)** solo regula directamente a entidades del sector financiero formal (bancos, fondos, etc.), pero los reportes a inversionistas privados de E&P están cubiertos por las obligaciones de los administradores bajo la Ley 222 de 1995 y el Código de Comercio. Un reporte con cifras incorrectas enviado a inversionistas es un potencial incumplimiento del deber de lealtad y del deber de informar verazmente.[^18]

### 3.2 Habeas Data (Ley 1581/2012) y datos en cloud externo

Las facturas, nóminas y archivos de caja menor contienen datos personales de empleados (nombres, cédulas, salarios, cargos) y de terceros proveedores. La **Ley 1581 de 2012** y su decreto reglamentario 1377 de 2013 exigen:[^19][^20][^21]

- **Autorización previa, expresa e informada** de los titulares para el tratamiento de sus datos
- **Contrato de encargo de tratamiento** con el proveedor cloud externo (n8n, Anthropic/Claude API, vector DB)
- **Política de tratamiento de datos** publicada y actualizada
- **Medidas de seguridad** acordes con la NTC-ISO/IEC 27017 para servicios cloud[^21]
- Multas por incumplimiento de hasta **2,000 salarios mínimos mensuales** (≈ $2,847 millones COP en 2025)[^20]

La Ley 1581 aplica cuando el Responsable del Tratamiento opera en territorio colombiano, independientemente de dónde esté el servidor. Enviar nóminas de empleados a APIs de Anthropic en servidores estadounidenses sin contratos de encargo de tratamiento y sin autorización de los trabajadores es un incumplimiento directo.[^22]

### 3.3 PILA (seguridad social) — restricciones específicas

Los archivos PILA contienen datos sensibles: salarios, tipo de contrato, EPS, AFP, semanas cotizadas. Son datos de seguridad social que tienen protección especial. **No existe una prohibición legal expresa que impida procesarlos con AI cloud**, pero la Ley 1581 clasifica información laboral como datos personales sujetos a todas las restricciones del régimen general. Adicionalmente, los operadores de PILA (Asopagos, etc.) tienen restricciones contractuales sobre el uso de los archivos descargados. Procesarlos en un pipeline de n8n + Claude sin autorización individual de cada empleado es legalmente frágil.[^23]

### 3.4 Responsabilidad por errores en retención en la fuente e IVA

Este es el riesgo fiscal más concreto. Si el sistema clasifica mal una factura — por ejemplo, trata un gasto no deducible como deducible, o aplica una tarifa de retención incorrecta por no reconocer el tipo de proveedor — se generan:

- **Ineficacia de la retención:** Una retención mal practicada se considera no presentada, obligando a rehacerla con sanción por extemporaneidad e intereses[^24]
- **Requerimientos de la DIAN:** errores en IVA pueden generar requerimientos formales, rechazo de beneficios fiscales y procesos de cobro[^25]
- **Responsabilidad solidaria del contador:** El contador titular, que firma los estados financieros, es legalmente responsable de los errores contables independientemente de si fueron generados por un sistema automatizado. El sistema de AI no tiene NIT ni matrícula profesional[^26]

La DIAN puede revisar períodos hasta 5 años atrás. Un error de clasificación que afecta la base de retención en la fuente hoy puede generar contingencias fiscales en 2029-2031.

***

## 4. Riesgos de Adopción Interna

### 4.1 El Gerente de Operaciones confía en respuestas incorrectas

El Gerente es ingeniero, no contador. Esto crea una brecha de capacidad crítica: **no tiene el entrenamiento para detectar cuando una respuesta financiera del chat es plausible pero incorrecta**. Los errores contables no se ven mal — suman, restan, tienen decimales. Un número fabricado con autoridad de Claude pasa el filtro visual de un ingeniero operativo más fácilmente que el de un contador.

La investigación sobre trust en AI financiero documenta que errores únicos generan caídas sustanciales de confianza (η² = 0.141) — pero solo si el usuario los detecta. El mayor riesgo no es que el Gerente pierda confianza en el sistema: es que **no pierda confianza porque no detecta el error**, y lo propague a inversionistas.[^12]

### 4.2 Reacción del equipo financiero

El equipo financiero (contador, analistas) verá en el chat una amenaza a su rol, no una herramienta. El patrón documentado en empresas que desplegaron AI conversacional financiera interna es consistente: el personal financiero deja de verificar datos que "ya el sistema respondió", o activamente evita usar el sistema por percibir desconfianza hacia su trabajo. La asimetría es peligrosa: si el equipo financiero se desconecta del flujo de información, la organización pierde la única capa de verificación humana con expertise contable.[^27]

Adicionalmente, el fenómeno de **Shadow AI** ya está documentado como crisis en infraestructura financiera: herramientas de AI sin gobernanza formal usadas por personal operativo para tomar decisiones financieras, sin que el CFO o el contador tengan visibilidad. Este proyecto institucionaliza exactamente ese patrón.[^28][^29]

### 4.3 Responsabilidad del contador cuando el reporte OPENLAND tiene errores

El contador titular es el firmante legal de los estados financieros. Si el reporte generado automáticamente sale con errores y se envía a inversionistas:
1. El contador es responsable profesional ante la Junta Central de Contadores
2. La empresa es responsable civil ante los inversionistas
3. El Gerente de Operaciones tiene responsabilidad administrativa como administrador
4. El sistema de AI no tiene responsabilidad legal ninguna

La pregunta "¿quién firma?" es la más incómoda de este proyecto y no tiene respuesta satisfactoria en el diseño actual.

### 4.4 Patrones documentados de fracaso en AI conversacional interna

El patrón más común es el **uso selectivo con sobre-confianza**: el usuario prueba el sistema con preguntas que sabe responder, lo encuentra correcto, y desde ese momento deja de verificar respuestas que no sabe verificar. En empresas con despliegues de AI conversacional interno:[^30]

- 42% de las organizaciones abandonaron la mayoría de sus iniciativas de AI en 2025, frente al 17% del año anterior[^31]
- El 95% de organizaciones que desplegaron AI generativa no vieron retorno medible (MIT Project NANDA, julio 2025)[^32]
- Solo el 48% de proyectos AI llegan a producción, y los que lo logran requieren un promedio de 8 meses de prototipo a producción — no 4 semanas[^30]

***

## 5. Riesgos de Alcance y Timeline

### 5.1 Por qué 4 semanas es insuficiente para producción real

Cuatro semanas no es un cronograma para construir esto. Es un cronograma para construir una demo que parezca funcionar en condiciones controladas. La diferencia entre una demo y un sistema de producción sobre 10 fuentes heterogéneas incluye:

- **Data ingestion pipeline robusto:** parseo de PDFs de facturas, archivos Excel de nóminas, CSV de PILA, datos de ventas de crudo — cada uno con su esquema, sus inconsistencias, sus valores nulos, sus cambios de formato por período. La investigación de producción RAG documenta que "la decisión de chunking es la más impactante en la arquitectura RAG" y requiere estrategia diferente por tipo de documento — facturas ≠ nóminas ≠ reportes de producción.[^33]
- **Normalización de datos:** sin datos en formato unificado, el pipeline genera errores, ineficiencias y problemas de escalabilidad. 10 fuentes heterogéneas = al menos 10 estrategias de normalización distintas.[^34]
- **Curación de datos históricos:** los meses anteriores tienen datos sucios acumulados. Encodings incorrectos, nombres de proveedores inconsistentes, montos en diferentes denominaciones (COP, USD). Cada inconsistencia requiere decisión humana.
- **Testing de calidad de respuestas:** un RAG financiero en producción requiere un set de preguntas con respuestas conocidas contra las que evaluar. Construir ese golden dataset lleva semanas solo.

El RAND Corporation (2024) encontró que **más del 80% de proyectos AI fallan**, al doble de la tasa de proyectos IT no-AI. Gartner proyecta que el 60% de proyectos AI sin datos "AI-ready" serán abandonados — y los datos de Green Power no son AI-ready por definición.[^32][^30]

### 5.2 Tiempo real para curar datos históricos

No existe un estándar publicado para curation de datos financieros de E&P, pero la experiencia de producción en RAG empresarial documenta que **data preparation consume entre el 60% y el 80% del tiempo total del proyecto**. Para 10 fuentes con 12+ meses de historia:[^35][^32]

- Normalización de proveedores: semanas (los nombres de empresas de servicios petroleros en los Llanos tienen decenas de variantes en facturas)
- Resolución de duplicados entre fuentes: semanas
- Validación de totales cross-fuente: semanas
- Construcción de reglas de asignación por bloque: requiere al menos un contador y el JOA completo

### 5.3 Supuestos ocultos en el cronograma

El cronograma de 4 semanas asume implícitamente: que todos los datos históricos están disponibles y digitalizados, que los formatos de fuentes son estables, que el equipo tiene disponibilidad completa, que no hay iteraciones de calibración de prompts, que la infraestructura cloud se provisiona sin fricciones, que el vector DB se configura correctamente desde el inicio. Ninguno de estos supuestos es realista.

### 5.4 Facturas con formato distinto a los ya cargados

Los proveedores de servicios petroleros en Colombia (empresas de perforación, cementación, slickline, wireline) cambian formatos de factura con regularidad: actualizaciones de software contable, cambios de NIT, fusiones. **Un pipeline de ingesta construido para los formatos actuales falla silenciosamente cuando llega una factura con estructura diferente** — no genera error visible, simplemente no extrae los campos correctos. El sistema responderá incorrectamente sobre esa factura sin ninguna señal de alarma. Esto no es un escenario hipotético: es el modo de degradación estándar de pipelines de ingesta en producción.[^36][^37]

***

## 6. Riesgo Competitivo y de Commoditización

### 6.1 Existen alternativas comerciales probadas

El argumento "build vs. buy" es fundamental aquí. Hay plataformas que ya resuelven este problema con años de desarrollo:

| Plataforma | Fortaleza relevante | Riesgo de build propio que evita |
|---|---|---|
| **Hebbia / AlphaSense** | RAG sobre documentos financieros con trazabilidad de fuentes[^38][^39] | Alucinación sin citación de fuente |
| **Microsoft Copilot for Finance** | Reconciliación financiera en Excel, comparación de estructuras de datos, informes de discrepancias — nativamente en Excel 2026[^40] | Stack completo de desarrollo |
| **PowerBI Copilot / Tableau Pulse** | Preguntas en lenguaje natural sobre datos estructurados con actualización en tiempo real | Gestión de vector DB |
| **ChatGPT Team / Claude for Work** | Carga de archivos + preguntas con contexto preservado | Toda la infraestructura RAG |

Microsoft Copilot for Finance, lanzado con capacidades agénticas generalmente disponibles desde abril 2026, ya hace reconciliación de datos financieros en Excel, comparación de estructuras, y troubleshooting de discrepancias — sin construir nada. El archivo OPENLAND podría alimentarse directamente a este sistema.[^41]

### 6.2 Commoditización acelerada

El riesgo de que Microsoft Copilot haga esto nativo ya no es hipotético: **ya lo está haciendo**. Las capacidades de Excel Copilot con agentic workflows (múltiples pasos, análisis de datos financieros, generación de reportes) son generalmente disponibles desde 2026. El sistema personalizado de Green Power compite contra el presupuesto de R&D de Microsoft, SAP, Oracle y Anthropic simultáneamente. La ventana de diferenciación es de meses, no años.[^40][^42][^41]

### 6.3 Build vs. upload-and-ask

La pregunta más incómoda del proyecto: ¿por qué construir infraestructura RAG cuando Claude for Work y ChatGPT Team permiten hoy subir los archivos del mes y hacer exactamente las mismas preguntas? La respuesta honesta tiene que incluir: automatización de ingesta, persistencia de históricos, y generación automática del reporte. Pero esos tres beneficios tienen su propio conjunto de riesgos técnicos documentados arriba. El caso de uso del Gerente de Operaciones haciendo preguntas ad-hoc **puede resolverse hoy con una suscripción de $30/mes sin construir nada**.

***

## 7. Riesgos Políticos y de Gobierno

### 7.1 Conflicto de jurisdicción: Operaciones lidera un sistema Financiero

Un Gerente de Operaciones que construye y opera el sistema que reporta resultados financieros a inversionistas es un conflicto de interés estructural. En la mayoría de organizaciones de E&P, la segregación de funciones entre quien opera y quien reporta los resultados financieros es un control interno básico. La ANH y los acuerdos de JOA típicamente exigen reportes financieros preparados y firmados por el área contable, no por operaciones.

El patrón documentado en empresas donde AI fue liderado desde operaciones en lugar de finanzas termina en dos escenarios: (a) el CFO/contador rechaza el sistema después de encontrar el primer error importante, o (b) el CFO adopta el sistema sin entenderlo y pierde la capacidad de detectar errores.[^27]

### 7.2 Asimetría de incentivos

Si el sistema funciona: el Gerente de Operaciones gana visibilidad estratégica, demuestra capacidad de innovación, y consolida influencia sobre el flujo de información financiero.

Si el sistema falla: el contador pierde credibilidad profesional (firma el reporte), la empresa pierde credibilidad con inversionistas, y el sponsor puede argumentar que "el sistema fue saboteado" o "los datos de entrada estaban mal".

**Esta asimetría de incentivos no está alineada con los incentivos de calidad del reporte.** El sponsor gana si el sistema se lanza; los riesgos los absorben otros.

### 7.3 "Fue la AI" como respuesta a inversionistas

Documentado desde múltiples ángulos: la atribución de errores a sistemas automatizados no mitiga la pérdida de confianza de inversionistas — la agrava. FINRA (2026) documenta que solo el 5% de inversores usa AI para decisiones financieras, y que la confianza en profesionales financieros supera ampliamente la confianza en AI. Un inversionista que recibe un reporte incorrecto y escucha "fue la AI" tiene dos conclusiones disponibles: (a) la empresa no tiene controles adecuados, o (b) la empresa usa "fue la AI" como escudo. Ambas son devastadoras para la relación de inversión.[^43]

***

## 8. Supuestos No Cuestionados que el Equipo Está Dando por Ciertos

A continuación, los supuestos implícitos más frágiles del proyecto, ordenados de mayor a menor gravedad:

**1. "Los datos históricos son suficientemente limpios para alimentar el sistema."**
Las facturas de proveedores de servicios petroleros en campo tienen inconsistencias sistemáticas: nombres de empresa con variantes, conceptos técnicos con vocabulario no estandarizado, montos en divisas mixtas, fechas de servicio vs. fechas de factura desalineadas. No hay un dataset limpio esperando ser indexado. Hay 12+ meses de datos que requieren curación intensiva antes de que el RAG pueda responder correctamente.

**2. "El LLM entenderá el contexto financiero de E&P en Colombia."**
El modelo no ha sido entrenado ni fine-tuneado con regulaciones ANH, terminología JOA colombiana, convenios colectivos de trabajadores del petróleo, o las particularidades contables de bloques de exploración en etapa de desarrollo bajo NIIF. Responderá usando analogías de otros contextos, que pueden ser plausibles pero incorrectas.

**3. "4 semanas es suficiente para llegar a producción."**
Es suficiente para una demo impresionante que funciona con datos seleccionados. No es suficiente para un sistema que responde correctamente a preguntas arbitrarias sobre datos reales con adversarial inputs (preguntas mal formuladas, datos atípicos, meses con actividad inusual).

**4. "El Gerente de Operaciones puede evaluar la calidad de las respuestas financieras."**
No puede. No tiene la formación para distinguir entre una respuesta financieramente plausible y una respuesta financieramente correcta. El equipo financiero es el único con esa capacidad, y no es el usuario diseñado del sistema.

**5. "El área financiera apoya el proyecto."**
"Apoyo del área financiera" es diferente de "el área financiera validará activamente cada respuesta del sistema antes de que llegue a inversionistas". Si ese workflow de validación no está explícitamente diseñado y comprometido, el "apoyo" es político, no operativo.

**6. "n8n + Claude es suficiente para manejar 10 fuentes heterogéneas en producción."**
n8n en producción con RAG muestra response times de 16-18 segundos en configuraciones estándar. Más crítico: los pipelines n8n con AI son inherentemente frágiles — un cambio en el formato de un archivo fuente rompe el flujo sin error visible. "Minor changes cause frequent failures" es documentado explícitamente en la documentación de n8n sobre RAG.[^44][^45][^46]

**7. "El reporte generado automáticamente reemplaza (o puede reemplazar) el trabajo del contador."**
El reporte OPENLAND es un documento financiero que implica decisiones de clasificación, criterios contables bajo NIIF, y responsabilidad profesional. Ningún sistema de AI tiene matrícula de contador público. Ningún sistema de AI puede ser sancionado por la Junta Central de Contadores. La automatización del reporte transfiere el riesgo legal al firmante humano sin transferir el beneficio de la supervisión profesional.

**8. "Los inversionistas no van a cuestionar la metodología de generación del reporte."**
Los inversionistas de E&P en Colombia son sofisticados. Cuando encuentren una discrepancia — y la encontrarán, porque los errores en reportes automatizados tienden a ser sistemáticos, no aleatorios — preguntarán cómo se generó. "Con AI" no es una respuesta que genere confianza en el sector de hidrocarburos, donde el escrutinio financiero de operadores es parte del proceso de due diligence continuo.

***

## Conclusión Forzada

Si el equipo insiste en avanzar, el camino mínimamente defendible no es el MVP descrito. Es:

1. **No conectar el generador de reportes a inversionistas al sistema AI** hasta tener al menos 6 meses de operación del chat interno con verificación humana de cada respuesta.
2. **Reemplazar el RAG genérico por Text-to-SQL** sobre datos estructurados para las preguntas numéricas — los LLMs son materialmente más precisos en SQL que en RAG para agregaciones.[^47]
3. **Eliminar las nóminas y PILA del pipeline** hasta tener contratos de encargo de tratamiento de datos firmados y autorizaciones de empleados.
4. **Asignar al contador titular como co-owner del sistema**, no como receptor pasivo del output.
5. **Evaluar seriamente Microsoft Copilot for Finance** antes de gastar 4 semanas construyendo lo que ya existe.[^40][^41]

El riesgo número uno identificado por el propio equipo — pérdida de credibilidad con inversionistas — es exactamente el riesgo que el diseño actual hace más probable, no menos.

---

## References

1. [FinanceBench: Evaluating LLMs in Finance | PDF - Scribd](https://www.scribd.com/document/921055864/FINANCEBENCH-A-New-Benchmark-for-Financial-Question-Answering) - FINANCEBENCH_A New Benchmark for Financial Question Answering - Free download as PDF File (.pdf), Te...

2. [FinanceBench: Financial QA Benchmark - Emergent Mind](https://www.emergentmind.com/topics/financebench-dataset) - FinanceBench is an open test suite that evaluates LLMs on financial QA tasks using public company fi...

3. [AI Hallucination Rates & Benchmarks in 2026 - Suprmind](https://suprmind.ai/hub/ai-hallucination-rates-and-benchmarks/) - Accuracy: 55.3%. Hallucination rate: 50%, down from Gemini 3 Pro's 88%. This was the biggest single-...

4. [HalluLens: LLM Hallucination Benchmark - arXiv](https://arxiv.org/html/2504.17550v1) - Our analysis shows that the model evaluator LLM achieves 96.67% accuracy for abstention and 95.56% a...

5. [XBRL Cuts AI Errors in Reading Company Filings, Study Finds](https://tax.thomsonreuters.com/news/xbrl-cuts-ai-errors-in-reading-company-filings-study-finds/) - Researchers who reviewed 5,000 annual reports from 2014 through 2023 found AI's overall error rate f...

6. [RAG Problems Persist. Here Are Five Ways to Fix Them | IBM](https://www.ibm.com/think/insights/rag-problems-five-ways-to-fix) - The RAG challenges routinely confronting users include limits on context windows and aggregation ope...

7. [Ten Failure Modes of RAG Nobody Talks About (And How to Detect ...](https://dev.to/kuldeep_paul/ten-failure-modes-of-rag-nobody-talks-about-and-how-to-detect-them-systematically-7i4) - RAG systems fail in subtle ways that aggregate metrics and component-level testing cannot reveal. Th...

8. [I've seen too many RAG pipelines silently fail on cross-references ...](https://www.reddit.com/r/Rag/comments/1s4a88f/ive_seen_too_many_rag_pipelines_silently_fail_on/) - I see a lot of developers building RAG solutions and treating every document like it's a flat wall o...

9. [Enhancing Oil and Gas Knowledge Retrieval: An LLM-Powered ...](https://www.earthdoc.org/content/papers/10.3997/2214-4609.202510629) - This paper addresses the challenges of retrieving domain-specific information in the oil and gas sec...

10. [Tailoring large language models for specific energy domains - SLB](https://www.slb.com/insights/tailoring-large-language-models-for-specific-energy-domains) - This allows LLMs to access relevant context and produce more accurate and domain-specific outputs. R...

11. [When AI Gets It Wrong: Lessons from the Deloitte Report](https://supervision.com.au/when-ai-gets-it-wrong-lessons-from-the-deloitte-report/) - Deloitte agreed to refund part of a $440K government report after AI errors were found. What it warn...

12. [Trust Formation, Error Impact, and Repair in Human–AI Financial ...](https://pmc.ncbi.nlm.nih.gov/articles/PMC12561693/) - Abstract. Understanding how trust in artificial intelligence evolves is crucial for predicting human...

13. [Most companies suffer some risk-related financial loss deploying AI ...](https://www.reuters.com/business/most-companies-suffer-some-risk-related-financial-loss-deploying-ai-ey-survey-2025-10-08/) - Total combined losses were estimated at $4.4 billion, with metrics such as revenue growth, cost savi...

14. [AI Washing Erodes Consumer and Investor Trust, Raises Legal Risk](https://news.bloomberglaw.com/us-law-week/ai-washing-erodes-consumer-and-investor-trust-raises-legal-risk) - AI washing, or the use of false and misleading statements about artificial intelligence, has risen t...

15. [Navigating the Hidden Risks of Automated Financial Reporting](https://www.linkedin.com/pulse/navigating-hidden-risks-automated-financial-reporting-georg-langlotz-arsff) - Persistent issues with data quality can also erode stakeholder trust and credibility; investors and ...

16. [Estas son las sociedades vigiladas por Supersociedades en 2025](https://actualicese.com/sociedades-vigiladas-por-supersociedades-en-2025/) - Las sociedades vigiladas por la Supersociedades por el 2025 son las que cumplan las dos causales que...

17. [Quienes y cuándo se deben presentar estados financieros ante la ...](https://www.hklaw.com/en/insights/publications/2022/01/quienes-y-cuando-se-deben-presentar-estados-financieros) - Con la Circular 100-000016 de 2021 la Superintendencia de Sociedades de Colombia publicó los plazos ...

18. [Normativa General - Superintendencia Financiera](https://www.superfinanciera.gov.co/publicaciones/19167/normativanormativa-general-19167/) - A través de esta información, usted podrá conocer la normativa que rige a la Superintendencia Financ...

19. [HABEAS DATA: ¿QUÉ HACER ANTE EL MAL MANEJO DE SUS ...](https://paezmora.co/2017/05/07/habeas-data-que-hacer-ante-el-mal-manejo-de-sus-datos-personales/) - El habeas data en Colombia es entendido como el derecho de las personas sobre sus datos personales, ...

20. [Colombia | Así es la protección del habeas data y las sanciones por ...](https://dplnews.com/colombia-asi-es-la-proteccion-del-habeas-data-y-las-sanciones-por-mal-uso-de-datos-personales/) - El habeas data es el derecho que tiene cualquier persona de actualizar y rectificar la información q...

21. [Normatividad de Cloud Computing en Colombia: claves y leyes de ...](https://www.nedigital.com/es/blog/normatividad-de-cloud-computing-en-colombia) - Existen leyes y normas colombianas que regulan el manejo y tratamiento de datos personales y financi...

22. [Ley 1581 de 2012 - Gestor Normativo - Función Pública](https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=49981) - Los datos personales, salvo la información pública, no podrán estar disponibles en Internet u otros ...

23. [Protección de datos en Colombia. Leyes, regulaciones y derechos](https://impactotic.co/ciber-seguridad/proteccion-de-datos-en-colombia/) - Conoce las nuevas leyes de protección de datos en Colombia. Asegura tu privacidad con estrategias ac...

24. [Ineffective retention: a silent mistake - YouTube](https://www.youtube.com/watch?v=yJje-MpUgkM) - Los errores silenciosos en retenciones en la fuente pueden ... Contador #Impuestos #Renta #sanciones...

25. [Errores en el IVA y sus soluciones - Siempre al Día](https://siemprealdia.co/colombia/impuestos/errores-en-el-iva/) - Los errores en el IVA pueden generar requerimientos por parte de la Dian y rechazo de beneficios fis...

26. [Errores frecuentes en la contabilidad de empresas colombianas y ...](https://www.ochgroup.co/errores-frecuentes-en-la-contabilidad-de-empresas-colombianas-y-como-evitarlos/) - El error: numeraciones vencidas, datos de terceros sin validar, reglas de IVA/retenciones mal config...

27. [AI Adoption Gaps Among CFO Teams - LinkedIn](https://www.linkedin.com/top-content/artificial-intelligence/challenges-of-ai-adoption/ai-adoption-gaps-among-cfo-teams/) - Everyone agrees AI is a priority. But nobody agrees who is accountable when it fails. The companies ...

28. [Shadow AI Has Reached Financial Infrastructure. Compliance Hasn't](https://www.forbes.com/sites/daraabasiita/2026/05/06/shadow-ai-has-reached-financial-infrastructure-compliance-hasnt/) - What does a failure actually look like when both layers go wrong at once? A payments analyst uses an...

29. [Shadow AI Crisis: 80% of Fortune 500 Lost Control Now](https://guptadeepak.com/the-shadow-ai-governance-crisis-why-80-of-fortune-500-companies-have-already-lost-control-of-their-ai-infrastructure/) - This article is about what that gap actually looks like, why traditional governance fails to close i...

30. [Why 90% of Enterprise AI Implementations Fail (2026)](https://talyx.ai/insights/enterprise-ai-implementation-failure) - Gartner predicted 30% of generative AI projects would be abandoned after proof of concept by end of ...

31. [Why most enterprise AI projects fail — and the patterns that ...](https://workos.com/blog/why-most-enterprise-ai-projects-fail-patterns-that-work) - 42% of companies abandoned most AI initiatives in 2025, up from just 17% in 2024. After analyzing do...

32. [Why 95% of AI Projects Fail and How Data Fixes It](https://sranalytics.io/blog/why-95-of-ai-projects-fail/) - Most AI projects fail before they reach production. MIT, Gartner, and RAND explain why and it starts...

33. [Production RAG Systems: Chunking, Retrieval Metrics and Failure ...](https://www.actinode.com/blog/production-rag-chunking-retrieval-failure-modes) - How you split documents into chunks is the single most impactful decision in RAG architecture. Poor ...

34. [Why Your RAG Pipeline Is Failing: 5 Common Pitfalls and How to Fix ...](https://vectorize.io/blog/why-your-rag-pipeline-is-failing-5-common-pitfalls-and-how-to-fix-them) - Without that, you are looking at difficulties for the pipeline to handle data. It may be prone to er...

35. [AI Project Failure Rate: 95% Fail - MIT Study Reveals Why](https://www.gigenet.com/blog/ai-project-failure-rate-mit-study-95-percent/) - Uncover the shocking ai project failure rate: 95% of AI projects do not yield meaningful business re...

36. [Most RAG systems don't understand sophisticated documents](https://venturebeat.com/orchestration/most-rag-systems-dont-understand-documents-they-shred-them) - Standard RAG pipelines treat documents as flat strings of text. They use "fixed-size chunking" (cutt...

37. [Your Chunks Failed Your RAG in Production - Towards Data Science](https://towardsdatascience.com/your-chunks-failed-your-rag-in-production/) - A RAG pipeline does not retrieve documents. It retrieves chunks ... Before choosing hierarchical chu...

38. [AlphaSense vs Hebbia](https://www.alpha-sense.com/compare/alphasense-vs-hebbia/) - The primary difference between Hebbia and AlphaSense lies in both the scope of content and the role ...

39. [Top 12 AI Financial Research Platforms for 2026 - Hebbia](https://www.hebbia.com/resources/financial-research-platforms) - AlphaSense is a market intelligence platform used by corporate and financial teams. While third-part...

40. [Overview of Finance agents in Microsoft 365 2026 release ...](https://learn.microsoft.com/en-us/copilot/release-plan/2026wave1/finance-agents/) - With next-generation AI support from Copilot for Finance, finance professionals can reconcile their ...

41. [Copilot's agentic capabilities in Word, Excel, and ...](https://www.microsoft.com/en-us/microsoft-365/blog/2026/04/22/copilots-agentic-capabilities-in-word-excel-and-powerpoint-are-generally-available/) - Copilot can take multi-step, app-native actions directly in your documents, worksheets, and presenta...

42. [Overview of Finance agents in Microsoft 365 2025 release ...](https://learn.microsoft.com/en-us/copilot/release-plan/2025wave1/copilot-finance/) - With next-generation AI support from Copilot for Finance, finance professionals can reconcile their ...

43. [FINRA report finds investors trust financial advisors significantly ...](https://www.securitieslaw.com/blog/finra-report-finds-investors-trust-financial-advisors-significantly-more-than-ai/) - A new report finds that investors still place more trust in their financial professionals than in ar...

44. [Most “GPT problems” in n8n workflows are actually pipeline failures](https://www.reddit.com/r/n8n/comments/1rm2jne/most_gpt_problems_in_n8n_workflows_are_actually/) - r/n8n - Most “GPT problems” in n8n workflows are actually pipeline failures ... A visual RAG failure...

45. [Stop Gluing Services Together: Build RAG Pipelines with n8n](https://blog.n8n.io/rag-pipeline/) - A small feature can turn into a collection of services, scripts, and configuration files, where mino...

46. [Is the AI Agent node too slow for production RAG? Seeing 16s+ ...](https://community.n8n.io/t/is-the-ai-agent-node-too-slow-for-production-rag-seeing-16s-response-times/287840) - Describe the problem/error/question I'm running a production RAG chatbot using the standard AI Agent...

47. [Comparative Analysis of Single and Multiagent Large Language ...](https://onepetro.org/SJ/article-abstract/29/12/6869/581789/Comparative-Analysis-of-Single-and-Multiagent?redirectedFrom=fulltext) - This paper explores the application of large language models (LLMs) in the oil and gas (O&G) sector,...

