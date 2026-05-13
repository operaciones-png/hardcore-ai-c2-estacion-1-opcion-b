# Presentación Estación 1 — Green Power Célula AI Fase 1 — OPCIÓN B

**Expositor:** Juanda — Gerente de Operaciones, Green Power S.A.S.
**Duración estimada:** 8–10 minutos
**Formato:** guion de exposición con notas de soporte

---

## 1. Contexto en una frase (30 seg)

> "Soy Juanda, ingeniero mecánico, Gerente de Operaciones de Green Power. Operamos un bloque en los Llanos Orientales con dos activos: Kimbo y Caliche. Tenemos un único inversionista: OPENLAND. Cada mes, el día 10, le entregamos un reporte financiero. Esa es la relación más crítica de nuestra empresa."

---

## 2. El problema en números (1 min)

> "Hoy ese reporte mensual lo armamos a mano entre el Gerente Financiero y yo. Nos toma una semana de trabajo conjunto. Eso son aproximadamente **80 horas-gerente al mes**, o sea unos **USD 4.000 al mes (COP 16 millones)** en costo escondido de tiempo de gerencia."

> "Y eso es solo el reporte. Durante el mes recibo entre 10 y 30 consultas ad-hoc del dueño y de OPENLAND: '¿Cuánto le debemos a tal proveedor?', '¿Cuánto se gastó en químicos en Kimbo el último trimestre?'. Cada una me toma entre 15 minutos y 2 horas de búsqueda manual en Excel, correos y carpetas."

> "Y todo esto contra un deadline duro: día 10 del mes siguiente, sin margen de error contra el único inversionista de la empresa."

---

## 3. Por qué Opción B y diferenciación con Opción A (30 seg)

> "Esta es mi Opción B. La Opción A ataca la causación de facturas — entrada de datos. La Opción B ataca otro dolor distinto: el armado del reporte a inversionistas — consolidación + análisis. Son dos proyectos hermanos que comparten infraestructura pero atacan dolores distintos. La Opción B además es más cercana a mi rol: yo personalmente co-armo el reporte hoy, así que el sponsor (yo) tiene legitimidad funcional, no es shadow IT."

---

## 4. La trampa que esquivé — el Deep Research crítico (2 min)

> "Lo más valioso del ejercicio no fue el research de validación — que dice que es viable. Fue el research crítico, que casi mata el proyecto. Cinco hallazgos cambiaron el diseño:"

**1. "FinanceBench: 1 de cada 4 cifras es incorrecta en RAG convencional"** — el benchmark más relevante para este caso de uso reporta solo 76% de éxito. Si el chat le entrega cifras erróneas al Gerente y el Gerente las pasa al reporte a OPENLAND, perdemos la relación con el único inversionista. **Corrección:** text-to-SQL para todas las cifras (el LLM no calcula, llama herramientas determinísticas), RAG solo para preguntas narrativas. Cita obligatoria a documento fuente con cada número.

**2. "La asignación de gastos compartidos entre Kimbo y Caliche es decisión humana, no inferible por LLM"** — un servicio de perforación que cubre los dos bloques se asigna por reglas contables del JOA, no por un LLM mirando una factura. Si el sistema asigna mal, la utilidad por bloque sale incorrecta y el retorno a OPENLAND mal calculado. **Corrección:** tabla maestra de distribución mantenida por el Gerente Financiero. El sistema solo aplica reglas humanas.

**3. "Procesar PILA y nómina con cloud externo viola Ley 1581/2012 — multa hasta COP 2.847 millones"** — los archivos PILA contienen datos sensibles de seguridad social. Sin DPA firmado con Anthropic ni autorizaciones individuales de cada empleado, es violación directa de Habeas Data. **Corrección:** PILA y nómina **excluidas del MVP**. Pasan a Fase 2 con marco legal completo.

**4. "El contador titular es el responsable legal indelegable a un algoritmo"** — Ley 43/1990 art. 10. Si el reporte sale mal, sancionan al contador, no a la AI. **Corrección:** contador titular es **co-owner del sistema desde día 1**, no receptor pasivo. Firma cada reporte. Tiene poder de veto.

**5. "MIT 2025: 95% de proyectos AI no ven retorno medible. RAND: 80% fallan."** — esto es un riesgo estructural del segmento, no específico al proyecto. **Corrección:** gobierno fuerte (comité semanal con Operaciones + Financiero + Contador + Dueño), métricas de éxito objetivas, criterio de cancelación explícito si el MVP no logra los targets.

> "El MVP que voy a presentar es la respuesta a estos cinco hallazgos."

---

## 5. La solución re-encuadrada — qué construyo en 4 semanas (1.5 min)

> "Un **asistente conversacional interno** que pre-arma el reporte OPENLAND, con humano-en-el-loop al 100%. Tres componentes:"

**Componente 1 — Repositorio centralizado (PostgreSQL + Qdrant self-hosted)** que ingiere 8 fuentes de datos del último mes piloto:
- Facturas de proveedores (XML UBL + PDF)
- Caja menor de campo
- Pagos a comunidades
- Provisión RRHH
- Facturas de venta de crudo
- Reporte de producción (barriles Kimbo + Caliche)
- API EIA → precio Brent
- API datos.gov.co → TRM Banco República

**Componente 2 — Chat conversacional con arquitectura híbrida:**
- Para preguntas con números → text-to-SQL (cero alucinación)
- Para preguntas narrativas → RAG con citación obligatoria
- Si confidence < 0.95 → "no encontré la información", nunca inventa

**Componente 3 — Pre-armado del reporte OPENLAND con human-in-the-loop:**
- El sistema pre-llena las dos hojas en formato Excel idéntico al histórico
- Aplica la tabla maestra de distribución Kimbo/Caliche del Gerente Financiero
- Cuadre matemático verificable obligatorio (totales por bloque suman al total empresa)
- Gerente Financiero revisa, corrige excepciones, valida
- Contador titular firma antes de enviar a OPENLAND

**Stack:** n8n (ya operativo) + Claude Sonnet 4.6 + PostgreSQL + Qdrant self-hosted + LlamaParse + APIs públicas.

**Costo operativo: USD 95–155/mes.** Esto sustituye un costo manual de USD 4.000/mes.

---

## 6. Cómo se ve el éxito (1 min)

| Métrica | Hoy | Target MVP (4 sem) | Target 6 meses |
|---|---|---|---|
| Horas-gerente para armar reporte | ~80 h | ≤30 h en piloto | ≤15 h |
| Días desde cierre hasta envío a OPENLAND | 10 días (deadline duro) | ≤7 días en piloto | ≤3 días |
| Tiempo respuesta consulta ad-hoc | 15–120 min | ≤30 segundos | ≤10 segundos |
| % cifras del reporte con cita verificable | ~0% | **100%** | 100% |
| Errores materiales que llegan a OPENLAND | 0 | **0** (objetivo absoluto) | **0** |
| Accuracy clasificación 19 categorías | N/A | ≥90% en piloto | ≥95% |

**Definición de éxito del MVP:** prototipo funcional con chat operativo + pre-armado de UN mes piloto + tabla maestra Kimbo/Caliche + carta de respaldo del contador titular para avanzar a Fase 2.

---

## 7. Riesgos que asumo conscientemente (1 min)

> "Documenté 12 riesgos con probabilidad, impacto y mitigación. Los tres que más me preocupan:"

1. **Alucinación numérica del LLM** — mitigada con text-to-SQL + citación obligatoria.
2. **Asignación incorrecta entre bloques** — mitigada con tabla maestra humana, no LLM.
3. **Pérdida de credibilidad con OPENLAND** — mitigada con HITL al 100%, contador firma todo, **nunca usar 'fue la AI' como justificación**.

---

## 8. Roadmap multi-fase (30 seg)

> "Soy explícito: esto es Fase 1. La visión completa es:"

- **Fase 1 — MVP (4 semanas):** lo que les estoy presentando.
- **Fase 2 — Producción (3–5 meses):** DPA + ingesta de PILA y nómina; pre-armado mensual recurrente.
- **Fase 3 — Reportería extendida (4–6 meses):** cuentas por cobrar/pagar, conciliación bancaria, exógena.
- **Fase 4 — Tributario integral:** estados financieros NIIF, declaraciones asistidas.

---

## 9. Cierre (30 seg)

> "Tres aprendizajes me llevo de esta Estación 1, en línea con la Opción A:"

1. **El Deep Research crítico vale más que el de validación.** Sin él habría llegado con un sistema RAG genérico que falla 1 de cada 4 cifras al inversionista.
2. **El alcance correcto no es el más grande.** Pasé de 'AI que genera el reporte completo en automático' a 'AI que pre-arma con humano firmando cada cifra'. Mucho más defendible y mucho más útil.
3. **AI sobre datos financieros confidenciales NO es un problema técnico, es un problema de gobierno y de arquitectura.** La tecnología existe. Lo que define el éxito es: quién firma, quién revisa, qué se puede automatizar y qué no, y cómo se separa el LLM de los cálculos.

> "Listo para Estación 2. Gracias."

---

## Anexo: preguntas probables del tutor y respuestas

**P: ¿No es esto lo mismo que tu Opción A?**
R: No. Opción A es automatización de causación — entrada de datos. Opción B es centralización + consulta conversacional + pre-armado de reporte a inversionistas. Comparten infraestructura técnica (n8n, Claude) pero atacan dolores distintos: Opción A libera al equipo financiero administrativo, Opción B libera a dos gerentes y reduce el riesgo de error en la relación con el único inversionista.

**P: ¿Por qué no usas Microsoft Copilot for Finance que ya hace partes de esto?**
R: Lo evalué. Copilot for Finance hace conciliación financiera genérica en Excel. No maneja: (a) lógica de distribución por bloque petrolero según JOA, (b) integración con XML UBL DIAN, (c) APIs específicas colombianas (TRM Banco República), (d) clasificación con vocabulario upstream Green Power. El MVP construye esa capa de dominio sobre infraestructura abierta. Si Copilot evoluciona, mi data layer y mi tabla maestra siguen siendo activos válidos — cambio el LLM, conservo el resto.

**P: ¿Qué pasa si el chat le da una cifra incorrecta a Juanda y él la usa para responder a OPENLAND?**
R: Por eso el chat NUNCA es la fuente directa para nada que vaya a OPENLAND. Para el reporte mensual: el sistema pre-arma, el Gerente Financiero revisa cada cifra, el contador firma. Para consultas ad-hoc del dueño: las cifras del chat tienen citación obligatoria al documento fuente, así Juanda puede verificar antes de pasar la respuesta. Si el sistema no encuentra la cifra con confidence ≥ 0.95, responde "no encontré la información", nunca inventa.

**P: ¿Por qué excluiste PILA y nómina del MVP si dijiste que las querías al inicio?**
R: Porque el Deep Research crítico me mostró que procesar esos datos con cloud externo sin DPA firmado y sin autorización individual de cada empleado viola Ley 1581/2012, con multas hasta COP 2.847 millones. Es un riesgo regulatorio que no podemos asumir en un MVP de 4 semanas. Pasan a Fase 2, con el marco legal completo en lugar.

**P: ¿Cuál es tu mayor incertidumbre?**
R: La accuracy real de la clasificación automática en 19 categorías sobre proveedores nuevos que el sistema nunca ha visto. Por eso el piloto del MVP es sobre UN mes con datos reales, midiendo la tasa de aciertos. Si la accuracy cae por debajo del 80% en proveedores nuevos, replantamos: o aumentamos el feedback loop, o limitamos el alcance a proveedores recurrentes.

**P: ¿Y si OPENLAND descubre que estás usando AI para armar el reporte?**
R: No es algo a esconder, es algo a documentar. El reporte tiene la firma del contador titular como siempre. La AI es una herramienta interna de productividad, no el firmante. Si OPENLAND pregunta cómo se generó, la respuesta honesta es "el contador firma como siempre, con un sistema interno que pre-llena los datos de manera trazable a documentos fuente". Esa transparencia, lejos de erosionar confianza, la fortalece — porque demuestra modernización con controles.

**P: ¿Cuál es la métrica que te haría matar el proyecto al final del MVP?**
R: Si en el mes piloto la cantidad de errores materiales detectados por el contador es mayor que en el proceso manual actual. Es decir: si el sistema introduce más errores de los que evita, no avanza a Fase 2. El criterio es objetivo y se mide contra el reporte manual hecho en paralelo durante el mes piloto.

---

*Hardcore AI by 30X — Cohorte 2 — Estación 1 — Opción B — Mayo 2026*
