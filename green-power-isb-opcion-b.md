# Internal Solution Brief — Hardcore AI Cohorte 2

## OPCIÓN B — Célula AI Green Power — Fase 1: Asistente Conversacional Interno + Pre-armado del Reporte OPENLAND con Humano-en-el-Loop

> **Nota del autor (Juanda):** Esta es la **Opción B** de mi entrega de la Estación 1. Aborda un dolor distinto al de la Opción A (causación de facturas): el **armado mensual del reporte financiero a OPENLAND**, único inversionista de Green Power, que hoy consume aproximadamente **80 horas-gerente al mes** entre el Gerente de Operaciones y el Gerente Financiero. La entrega es el día 10 de cada mes — deadline duro contra una audiencia de altísima confianza.
>
> **Re-encuadre tras Deep Research crítico:** El alcance original ("Cerebro Financiero Conversacional" que genera el reporte a inversionistas) tenía dos riesgos letales documentados en la literatura: (1) **alucinación numérica del RAG** (FinanceBench reporta 76% de éxito = 1 de cada 4 cifras incorrecta), (2) **transferencia de responsabilidad legal del contador a un sistema sin matrícula profesional**. El MVP fue rediseñado para mantener la firma humana, separar arquitectura LLM de cifras numéricas (text-to-SQL), y excluir explícitamente fuentes con alto riesgo regulatorio (PILA, nóminas) del primer ciclo.
>
> **Particularidad de la Opción B:** A diferencia de la Opción A, en este caso el sponsor (Gerente de Operaciones) tiene rol legítimo en el proceso financiero: **co-arma el reporte mensual junto al Gerente Financiero**. No es shadow IT — es un usuario directo del proceso. Esta legitimidad funcional reduce el riesgo político principal identificado en la Opción A.

---

## SOLUCIÓN

**Nombre de la solución:** Asistente Conversacional Interno + Pre-armado del Reporte OPENLAND — Green Power Fase 1

**Descripción en una línea:** Sistema interno que centraliza información financiera operativa, permite al Gerente consultarla en lenguaje natural con respuestas trazables a documentos fuente, y pre-arma las dos hojas mensuales del reporte OPENLAND para que el Gerente Financiero las valide y firme antes de enviar al inversionista.

**Empresa / Organización:** Green Power S.A.S. — operadora del bloque que contiene los activos Kimbo y Caliche, exploración y explotación de hidrocarburos, Llanos Orientales (Colombia), ~90 empleados.

**Inversionista único:** OPENLAND — empresa del inversionista a quien Green Power reporta mensualmente el balance financiero el día 10 de cada mes.

---

## 1. PROBLEMA DE NEGOCIO

### Descripción del problema

Cada mes, el Gerente de Operaciones (Juanda) y el Gerente Financiero dedican aproximadamente **una semana de trabajo conjunto** a recopilar, clasificar, calcular y formatear las dos hojas del reporte OPENLAND (Balance ejecutivo + Costos y Gastos detallado por categoría y bloque). El trabajo incluye:

- Recolectar facturas de proveedores, nóminas, caja menor, pagos a comunidades, provisiones de RRHH, seguridad social, facturas de venta de crudo del mes
- Clasificar manualmente cada transacción en una de 19 categorías de gasto
- Distribuir cada gasto entre los bloques Kimbo y Caliche según criterios contables
- Calcular utilidad operacional por bloque, regalías ANH (8%), utilidad bruta, retorno a inversionista (30%)
- Buscar precio Brent y TRM del mes
- Consolidar producción de barriles Kimbo + Caliche
- Generar las dos hojas en formato Excel idéntico al histórico OPENLAND
- Validar internamente y enviar antes del **día 10 del mes siguiente** al cierre

Adicionalmente, durante el mes el Gerente debe responder a consultas ad-hoc de OPENLAND y del dueño que requieren búsqueda manual en facturas, Excel y correos: "¿Cuánto le debemos a tal proveedor?", "¿Cuánto se gastó en químicos en Kimbo el último trimestre?", "¿Cuál fue el costo total de transporte de agua en abril?". Cada consulta puede tomar 15 minutos a 2 horas de búsqueda manual.

### Cuantificación del impacto

| Métrica actual | Valor estimado |
|---|---|
| Tiempo gerente por reporte mensual | ~1 semana de trabajo conjunto = ~80 horas-gerente/mes |
| Costo hora-gerente (estimado conservador Colombia, perfil senior) | USD 50/hora |
| Costo escondido del armado manual del reporte | ~**USD 4.000/mes ≈ COP 16M/mes** (≈ COP 192M/año) |
| Frecuencia | 12 reportes/año, deadline duro día 10 |
| Stakeholder | **1 único inversionista (OPENLAND)** — relación financiera más crítica de la empresa |
| Consultas ad-hoc adicionales del Gerente durante el mes | Estimado: 10–30 consultas/mes, 15–120 min cada una |
| Costo operativo proyectado del MVP (referencia Deep Research validación) | USD 95–155/mes |

**Margen estimado mensual** (tiempo liberado de gerentes − costo del sistema): **≈ USD 3.850/mes** desde el primer mes en producción estable.

### Timeline / urgencia

- **Inmediata:** todos los meses, día 10, deadline duro contra el único inversionista. Cualquier retraso o error daña la relación más importante de la empresa.
- **A 6 meses:** consolidación operativa del sistema, ampliación a más fuentes (PILA + nóminas con DPA firmado).
- **A 12 meses:** roadmap a Fase 2 y Fase 3 (estados financieros completos, tributario integral).

---

## 2. STAKEHOLDERS Y SPONSOR

### Sponsor ejecutivo
- **Dueño / Gerente General de Green Power** — respaldo explícito al proyecto y al presupuesto del MVP. Confirmado.

### Sponsor técnico / líder del proyecto
- **Juanda — Gerente de Operaciones** (ingeniero mecánico). Co-armador actual del reporte OPENLAND junto al Gerente Financiero. Usuario directo del chat conversacional. Esta legitimidad funcional (co-armar el reporte) elimina el riesgo de "shadow IT" identificado en proyectos similares.

### Stakeholders críticos (deben aprobar formalmente antes de producción)
- **Gerente Financiero** — co-armador actual del reporte. Usuario validador del sistema. Co-owner del proyecto. **Apoyo confirmado.**
- **Contador titular** — único responsable legal de la firma con tarjeta profesional (Ley 43/1990 art. 10). Firma cada reporte OPENLAND antes de salir al inversionista. Sin su aprobación documentada del proceso, el sistema no avanza a producción.
- **OPENLAND (inversionista)** — receptor del reporte. **NO recibe nada generado por AI sin validación humana firmada.** Su confianza es el activo crítico a proteger.

### Usuarios finales
- **Juanda y Gerente Financiero** — usuarios directos del chat (consultas internas) y validadores del reporte pre-armado.
- **Contador titular** — validador final + firmante legal del reporte antes de enviar a OPENLAND.

### Bloqueadores potenciales identificados (mitigaciones explícitas)
- **Asimetría de incentivos del sponsor** identificada en Deep Research crítico (sección 7.2): si el proyecto falla, las consecuencias legales recaen sobre el contador y el representante legal, no sobre Operaciones. **Mitigación:** comité semanal con contador titular como co-owner, no como receptor pasivo. Documentación de cada decisión arquitectónica con firma del contador.
- **Resistencia del Gerente Financiero** ante percepción de pérdida de control sobre el reporte: **mitigación** con co-ownership formal del MVP desde día 1.
- **Pérdida de credibilidad con OPENLAND** si llega un reporte con error: **mitigación** con validación humana al 100% en MVP, trazabilidad completa, y nunca usar "fue la AI" como justificación.

---

## 3. ESTADO ACTUAL

### Procesos actuales (armado mensual del reporte OPENLAND)

1. Cierre del mes (día 30/31).
2. Día 1–3 del mes siguiente: recolección manual de facturas, nóminas, PILA, caja menor, pagos a comunidades, provisión RRHH desde correos, carpetas, Excel.
3. Día 3–5: clasificación manual de cada transacción en una de 19 categorías; distribución manual entre Kimbo y Caliche.
4. Día 5–7: cálculo de utilidades, regalías, retorno a inversionista; búsqueda manual de Brent y TRM; integración de barriles producidos.
5. Día 7–9: armado de las dos hojas en Excel formato OPENLAND; revisión cruzada Juanda + Gerente Financiero; firma del contador.
6. **Día 10: envío a OPENLAND.**

### Herramientas existentes

| Herramienta | Uso actual | Limitación |
|---|---|---|
| Excel `Balances OPENLAND 2026` | Sistema único de reportería | Sin trazabilidad transaccional, sin validaciones automáticas |
| Email | Recepción de facturas + comunicaciones con OPENLAND | Búsqueda manual |
| Carpetas en SharePoint/Drive | Repositorio de documentos | Sin estructura de búsqueda |
| WhatsApp | Coordinación de campo, fotos de caja menor | Sin estructura |
| n8n | **Disponible y operativo** en Green Power | Sin uso actual en finanzas |

### Lo que funciona bien (preservar)
- Conocimiento profundo del Gerente Financiero sobre los criterios contables de Green Power
- Validación cruzada Juanda + Gerente Financiero antes de la firma del contador
- Formato OPENLAND estable y aceptado por el inversionista (4 meses ya armados como referencia)

### Oportunidades de mejora
- Reducir ~80 horas-gerente/mes a ~10–15 horas-gerente/mes (revisión y firma)
- Permitir consultas ad-hoc en segundos en lugar de horas de búsqueda manual
- Eliminar errores de digitación y de clasificación silenciosos
- Construir trazabilidad transaccional desde el inicio

---

## 4. ESTADO FUTURO DESEADO

### Proceso ideal post-MVP (fin de las 4 semanas)

#### Durante el mes (consultas ad-hoc)
1. Juanda o el Gerente Financiero abren el **chat conversacional interno**.
2. Escriben preguntas en español natural: "¿Cuánto le debemos a TRANSPORTES MULTICARGAS?", "¿Cuál fue el gasto de químicos en Kimbo en Q1?".
3. El sistema responde con:
   - Una cifra específica o una tabla con datos
   - **Citas obligatorias a los documentos fuente** (factura X, fecha Y)
   - Si no encuentra la respuesta con confianza, dice "no encontré esta información en los documentos disponibles" — **nunca inventa una cifra**.

#### Cierre del mes (armado del reporte OPENLAND)
1. Día 1: Juanda le pide al sistema "Pre-arma el reporte de [mes]".
2. El sistema:
   - Consulta el repositorio centralizado de transacciones del mes (PostgreSQL)
   - Aplica la **tabla maestra de reglas de distribución por bloque** mantenida por el Gerente Financiero (no inferida por LLM)
   - Calcula con fórmulas determinísticas: ingresos, costos, utilidad operacional, regalías 8%, utilidad bruta, retorno 30%
   - Trae automáticamente Brent (EIA API) y TRM (datos.gov.co API)
   - Genera las dos hojas en formato OPENLAND idéntico al histórico
3. Día 1–2: Gerente Financiero revisa cada cifra, corrige excepciones si necesario, valida.
4. Día 2: Contador titular revisa los estados resultantes y firma.
5. Día 2–3: envío a OPENLAND con **7 días de anticipación** sobre el deadline actual.

### Cambios en la experiencia del usuario

| Antes | Después |
|---|---|
| 1 semana armando el reporte | 1–2 días revisando reporte pre-armado |
| Consultas ad-hoc: 15 min – 2 h cada una | Consultas ad-hoc: 30 segundos por respuesta |
| Sin trazabilidad de cifras al documento fuente | Citas obligatorias a documentos para cada número |
| Riesgo de error humano por digitación | Riesgo de error sistemático por clasificación → mitigado con tabla maestra explícita |
| Llegamos justo al deadline día 10 | Reporte listo día 2–3, holgura de 7 días |

---

## 5. CRITERIOS DE ÉXITO

| Métrica | Línea base actual | Target al final del MVP (4 semanas) | Target a 6 meses |
|---|---|---|---|
| Horas-gerente para armar reporte mensual | ~80 h | ≤30 h en piloto | ≤15 h |
| Días desde cierre hasta envío a OPENLAND | 10 días (deadline duro) | ≤7 días en piloto | ≤3 días |
| Tiempo promedio de respuesta a consulta ad-hoc | 15–120 min | ≤30 segundos en chat | ≤10 segundos |
| % de cifras del reporte con cita verificable a documento fuente | ~0% | **100%** desde día 1 | 100% |
| Accuracy de clasificación de gastos en 19 categorías | N/A | ≥90% en piloto controlado de 1 mes | ≥95% |
| Errores materiales detectados por contador antes de firma | N/A | 100% detectados por proceso de validación | 100% |
| Errores materiales que llegan a OPENLAND | 0 (objetivo absoluto) | **0** | **0** |
| Adopción del chat por Gerente Financiero | N/A | Uso semanal documentado | Uso diario |

**Definición de éxito del MVP a 4 semanas:** prototipo funcional con (a) chat conversacional interno operativo sobre los datos del último mes cargados, (b) pre-armado del reporte OPENLAND de un mes piloto con 100% de validación humana de cada cifra, (c) tabla maestra de distribución Kimbo/Caliche poblada por el Gerente Financiero, (d) carta de respaldo del contador titular para avanzar a Fase 2.

---

## 6. RESTRICCIONES

### Técnicas
- **n8n** como orquestador (decisión arquitectónica ya tomada).
- **PostgreSQL self-hosted** para datos numéricos estructurados.
- **Qdrant self-hosted** para vectores de documentos narrativos.
- **Excel actual** como sistema de salida (formato OPENLAND).
- **NO certificación DIAN** — el MVP no emite facturas ni reemplaza un proveedor tecnológico autorizado.
- **NO genera estados financieros NIIF** — solo el reporte OPENLAND interno al inversionista.

### Datos
- **Habeas Data (Ley 1581/2012):** facturas y nóminas contienen datos personales. Multas hasta 2.000 SMMLV (~$2.847M COP).
- **Mitigación:**
  - PILA y nómina excluidas del MVP hasta tener **DPA firmado con Anthropic + autorizaciones individuales de empleados** (Fase 2).
  - Datos de proveedores procesados con redacción de PII sensible (Microsoft Presidio).
  - Anthropic API con acuerdo de no-entrenamiento.
- **Confidencialidad alta del reporte a OPENLAND:** el inversionista nunca recibe nada generado por AI sin validación humana firmada.

### Organizacionales
- Equipo del MVP: 1 sponsor técnico (Juanda) + 1 ingeniero senior (n8n + AI) + Gerente Financiero como co-owner + contador titular como aprobador formal.
- Comité semanal obligatorio: Operaciones + Financiero + Contador + Dueño.
- Sin contratación externa hasta cerrar Fase 1.

### Compliance
- **Ley 43/1990 art. 10** — fe pública contable indelegable a algoritmo. El contador firma cada reporte OPENLAND.
- **Estatuto Tributario** arts. 615, 616-1, 617, 647, 648, 651 — toda transacción debe respaldarse en factura electrónica/equivalente válida.
- **Resolución DIAN 000165/2023** + Anexo Técnico 1.9 UBL 2.1 para parsing de XML.
- **Circular Externa 002/2024 SIC** — DPIA obligatorio antes de productivizar con datos personales.
- **Ley 222/1995 + Código de Comercio** — responsabilidad de los administradores por veracidad informativa a inversionistas.

---

## 7. ENFOQUE TÉCNICO PROPUESTO

### Capacidad AI aplicada
1. **Text-to-SQL sobre PostgreSQL** para todas las preguntas con números (totales, sumas, promedios, comparativos).
2. **RAG con Qdrant** solo para preguntas narrativas/contextuales (descripciones de proveedores, conceptos).
3. **Clasificación automática** de transacciones en 19 categorías mediante embeddings + LLM con feedback loop.
4. **Generación determinística** del reporte OPENLAND (cálculos por fórmulas Excel/SQL, no por LLM).

### Justificación de las decisiones técnicas (atendiendo Deep Research crítico)

- **Text-to-SQL primero, RAG solo para contexto**: el Deep Research de crítica documenta que LLMs alucinan números (FinanceBench: 1 de 4 cifras incorrecta en RAG convencional). Separar arquitectónicamente "el LLM como redactor" del "SQL como calculadora determinística" eleva la accuracy numérica a 99%+.
- **Citación obligatoria con confidence ≥ 0.95**: cada número en una respuesta del chat debe tener cita a documento fuente. Sin cita, el sistema responde "no encontré esta información".
- **Tabla maestra de distribución Kimbo/Caliche explícita**: el Deep Research crítico identifica como riesgo central la asignación incorrecta de facturas compartidas entre bloques. La tabla maestra es **mantenida por el Gerente Financiero**, no inferida por LLM. El sistema solo aplica las reglas humanas.
- **Generación del reporte OPENLAND con cuadre matemático verificable**: totales por bloque deben sumar al total empresa; suma de líneas por categoría debe igualar el total general. Cualquier desbalance bloquea la generación y exige revisión humana.
- **PILA y nómina excluidas del MVP**: el Deep Research crítico documenta que procesar PILA/nómina con cloud externo sin DPA + autorizaciones individuales viola Habeas Data (Ley 1581/2012). Quedan en Fase 2 con marco legal completo.
- **APIs públicas oficiales para Brent (EIA) y TRM (datos.gov.co)**: cero alucinación, cita oficial gubernamental, gratuitas.

### Arquitectura de alto nivel

```
┌─────────────────────────────────────────────────────────────────┐
│  ENTRADAS (Fase 1 MVP)                                           │
│  • Email buzón facturas (XML UBL + PDF)                         │
│  • Carpeta compartida (caja menor de campo en Excel/fotos)      │
│  • Reporte de producción mensual (barriles Kimbo + Caliche)     │
│  • API EIA → precio Brent diario                                │
│  • API datos.gov.co → TRM Banco República diaria                │
│  • Histórico Balances OPENLAND 2026 (4 meses como golden set)   │
│  ❌ NO MVP: PILA, nómina (Fase 2 con DPA + autorizaciones)      │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  CAPA DE INGESTIÓN (n8n + parsers)                               │
│  • Parser XML UBL 2.1 (determinístico)                          │
│  • LlamaParse para PDFs/Excel/imágenes                          │
│  • HTTP Request a APIs (EIA, datos.gov.co)                      │
│  • Redacción PII sensible (Microsoft Presidio)                  │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  CAPA DE ALMACENAMIENTO (dual)                                   │
│  ┌──────────────────────┐    ┌──────────────────────────────┐  │
│  │ PostgreSQL           │    │ Qdrant self-hosted           │  │
│  │ (datos numéricos)    │    │ (vectores narrativos)        │  │
│  │ + tabla maestra      │    │ + metadata por documento     │  │
│  │   distribución K/C   │    │                              │  │
│  └──────────────────────┘    └──────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  CAPA DE AGENTE (Claude Sonnet 4.6 con tool use)                 │
│  • Tool: ejecutar_sql(query) → respuesta numérica determinística│
│  • Tool: buscar_documento(query) → fragmento + cita             │
│  • Tool: solicitar_confirmacion(decision) → pausa pipeline      │
│  • Restricción: cita obligatoria por cada número                │
│  • Si confidence < 0.95 → "no encontré esta información"        │
└─────────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
┌──────────────────────────┐  ┌──────────────────────────────┐
│  USO 1: CHAT INTERNO     │  │  USO 2: PRE-ARMADO REPORTE   │
│  (Juanda + G. Financiero)│  │  (mensual día 1-2)           │
│                          │  │                              │
│  Pregunta natural →      │  │  • Consulta PostgreSQL del   │
│  Tool routing →          │  │    mes completo              │
│  Respuesta + citas       │  │  • Aplica tabla maestra K/C  │
│                          │  │  • Cálculos determinísticos  │
│                          │  │  • Genera 2 hojas Excel      │
│                          │  │  • Cuadre matemático verif.  │
│                          │  │  • Push a bandeja de revisión│
└──────────────────────────┘  └──────────────────────────────┘
                                           │
                                           ▼
                            ┌──────────────────────────────┐
                            │  HUMAN-IN-THE-LOOP           │
                            │  • G. Financiero revisa      │
                            │  • Contador titular firma    │
                            │  • Envío a OPENLAND          │
                            └──────────────────────────────┘
```

### Stack tecnológico

| Capa | Tecnología | Justificación |
|---|---|---|
| Orquestación | **n8n self-hosted** | Ya operativo en Green Power; sin lock-in |
| Vector DB | **Qdrant self-hosted** (VPS ~USD 30/mes) | Mejor ratio precio-performance 2025-26; datos no salen |
| Base de datos relacional | **PostgreSQL self-hosted** | Estándar de la industria; cifrado de columnas para montos |
| Embeddings | **text-embedding-3-small** o **voyage-finance-2** | Optimizado finanzas |
| LLM agente | **Claude Sonnet 4.6** vía nodo nativo n8n | Balance costo/performance; AFIB benchmark 87.82% |
| Parsing | **LlamaParse** (PDFs/Excel) + **Python lxml** (XML UBL) | Estándares 2025-26 |
| Redacción PII | **Microsoft Presidio** | Open source, estándar |

### Costo operativo proyectado

| Concepto | USD/mes | COP/mes (TRM ≈ 4,000) |
|---|---|---|
| Vector DB Qdrant self-hosted (VPS) | 30 | 120.000 |
| Embeddings OpenAI (~5M tokens/mes) | 10 | 40.000 |
| Claude Sonnet 4.6 (con prompt caching) | 30–60 | 120.000–240.000 |
| Almacenamiento documentos S3 | 5 | 20.000 |
| n8n self-hosted o Cloud Starter | 20–50 | 80.000–200.000 |
| **Total operativo** | **95–155** | **380.000–620.000** |

**Comparado con el costo manual actual** (~USD 4.000/mes en horas-gerente): **ahorro proyectado ≈ USD 3.850/mes ≈ COP 15.4M/mes** una vez en producción estable.

**ROI proyectado** (basado en Deep Research validación): **3x–6x a 6 meses, payback en 6–10 meses.**

---

## 8. RIESGOS Y DEPENDENCIAS

### Matriz de riesgos prioritarios (con base en Deep Research crítico)

| # | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| R1 | **Alucinación numérica del LLM** (FinanceBench: 1 de 4 incorrecta en RAG convencional) | Alta sin mitigación | Catastrófico (cifra errónea a OPENLAND) | **Text-to-SQL para todas las cifras**, RAG solo para contexto narrativo. Cita obligatoria con confidence ≥ 0.95. Si no, "no encontré la información". |
| R2 | **Asignación incorrecta de gastos compartidos entre Kimbo y Caliche** | Alta sin mitigación | Alto (utilidad por bloque y retorno a inversionista incorrectos) | **Tabla maestra de distribución mantenida por Gerente Financiero**, no inferida por LLM. Sistema solo aplica reglas humanas. Cuadre matemático verificable obligatorio. |
| R3 | **Pérdida de credibilidad con OPENLAND** por cifra errónea en reporte | Baja en MVP (validación 100%) | Catastrófico | Validación humana al 100% en MVP. Contador firma cada reporte. **Nunca usar "fue la AI" como justificación**. |
| R4 | **Multi-hop reasoning failure** (preguntas cross-período/multi-fuente) | Media | Medio | Arquitectura agéntica con tool use, no RAG plano. Cuando el sistema no puede resolver multi-hop, devuelve respuesta parcial con disclaimer explícito. |
| R5 | **Vocabulario técnico oil & gas en español débil en embeddings genéricos** | Media-Alta | Medio | Fine-tuning few-shot con vocabulario Green Power (workover, AFE, JOA, recuperable, lifting cost) en Sprint 2. Voyage-finance-2 como respaldo. |
| R6 | **Habeas Data violado al procesar PILA/nómina con cloud externo** | Alta si se incluye en MVP | Catastrófico (multa hasta $2.847M COP) | **PILA y nómina excluidas del MVP**. Fase 2 con DPA + autorizaciones individuales. |
| R7 | **Responsabilidad legal del contador** por cifra errónea generada por AI | Alta sin mitigación | Catastrófico | Contador titular es co-owner del sistema desde día 1. Firma cada reporte. Tiene poder de veto sobre cualquier salida del sistema. |
| R8 | **Sub-estimación del alcance** — 4 semanas insuficientes para producción real (RAND: 80% de proyectos AI fallan) | **Alta — reconocida explícitamente** | Medio (gestionado por re-encuadre) | El MVP entrega prototipo con human-in-the-loop al 100% sobre 1 mes piloto, NO producción. Roadmap a 3–5 meses para Fase 2. |
| R9 | **Conflicto político con CFO/Contador** por sponsor de Operaciones | Media | Alto | Gerente Financiero y contador titular son co-owners formales del MVP, no receptores pasivos. Comité semanal documentado en minutas. |
| R10 | **Build vs Buy** — Microsoft Copilot for Finance ya hace partes de esto nativamente | Alta a 12 meses | Medio | Justificación explícita: el MVP construye dominio upstream Green Power (centros de costo por bloque, lógica JOA, integración XML UBL DIAN). Si Copilot for Finance evoluciona, los assets construidos siguen siendo válidos. |
| R11 | **Pipeline n8n se rompe silenciosamente** ante cambios de formato de proveedores | Alta a 6 meses | Medio | Validación schema en cada ingesta. Alertas automáticas ante esquemas no reconocidos. Equipo de mantenimiento dedicado en Fase 2. |
| R12 | **42% de proyectos AI abandonados en 2025; MIT: 95% no ven retorno medible** | Riesgo estructural del segmento | Medio | Gobierno fuerte (comité semanal), métricas de éxito objetivas, criterio de cancelación explícito si MVP no logra tabla 5. |

### Dependencias externas
- API de Anthropic (Claude Sonnet 4.6) operativa
- API EIA (Brent) operativa — fallback yfinance
- API datos.gov.co (TRM) operativa — fallback BanRep WSDL
- DPA firmado con Anthropic para Fase 2 (PILA + nómina)

### Dependencias internas
- n8n self-hosted operativo y mantenido (responsable: Operaciones)
- Tabla maestra de proveedores y reglas de distribución mantenida al día (responsable: Gerente Financiero)
- Comité semanal sin falta durante las 4 semanas del MVP

---

## 9. LÍMITES DE ALCANCE

### IN-SCOPE para el MVP de 4 semanas
- ✅ Repositorio centralizado PostgreSQL + Qdrant self-hosted con datos del **último mes piloto**
- ✅ Ingesta automática de: facturas XML UBL DIAN + PDFs proveedores + caja menor de campo + comunidades + provisión RRHH + facturas venta crudo + producción + Brent + TRM
- ✅ Chat conversacional con text-to-SQL para cifras + RAG para contexto + citación obligatoria
- ✅ Tabla maestra de distribución Kimbo/Caliche poblada por Gerente Financiero
- ✅ Pre-armado de las dos hojas del reporte OPENLAND para **un mes piloto** con human-in-the-loop al 100%
- ✅ Cuadre matemático verificable de totales
- ✅ Trazabilidad por transacción (hash + fuente + timestamp + usuario)
- ✅ Comité semanal documentado en minutas
- ✅ Carta de respaldo del contador titular como criterio de cierre

### OUT-OF-SCOPE explícito en el MVP (Fase 2 o posterior)
- ❌ Procesamiento de PILA y nómina → Fase 2 con DPA Anthropic + autorizaciones individuales empleados
- ❌ Generación de declaraciones de impuestos → Fase 2 con contador especializado tributario
- ❌ Estados financieros completos NIIF → Fase 2
- ❌ Cuentas por cobrar con seguimiento automático → Fase 3
- ❌ Conciliación bancaria → Fase 3
- ❌ Información exógena DIAN (formatos 1001, 1003, etc.) → Fase 3
- ❌ Tributario integral → Fase 4
- ❌ Causación de facturas (objeto de Opción A) → proyecto hermano

### Roadmap multi-fase (referencia, no compromiso del MVP)

| Fase | Alcance | Duración estimada | Criterio para activar |
|---|---|---|---|
| **Fase 1 (MVP)** | Chat interno + pre-armado del reporte OPENLAND con HITL, mes piloto | 4 semanas | EN CURSO |
| **Fase 2 — Producción** | DPA Anthropic + autorizaciones empleados → ingesta PILA y nómina; pre-armado mensual recurrente | 3–5 meses | Cierre exitoso de Fase 1 + carta del contador |
| **Fase 3 — Reportería extendida** | Cuentas por cobrar/pagar, conciliación bancaria, exógena | 4–6 meses | Estabilidad operativa Fase 2 ≥ 3 meses |
| **Fase 4 — Tributario integral** | Estados financieros NIIF, declaraciones tributarias asistidas | Por definir | Por definir |

---

## Checklist de entrega

- [x] Problema cuantificado en horas-gerente/mes y COP/mes
- [x] Sponsor ejecutivo, técnico y stakeholders críticos identificados con sus roles
- [x] Estado actual descrito con herramientas y limitaciones
- [x] Estado futuro descrito con cambios cuantificables en UX
- [x] Criterios de éxito con línea base, target MVP y target 6 meses
- [x] Restricciones técnicas, datos, organizacionales y de compliance documentadas
- [x] Enfoque técnico justificado con arquitectura de alto nivel y costo operativo
- [x] 12 riesgos con probabilidad, impacto y mitigación basados en Deep Research crítico
- [x] Límites de alcance in/out explícitos + roadmap multi-fase
- [x] Deep Research de validación completado y estudiado (`research-validacion.md`)
- [x] Deep Research de crítica completado y estudiado (`research-critica.md`)
- [x] Documentación en Markdown lista para entrega
- [x] Listo para presentar en la Estación 2 (miércoles 13 de mayo de 2026)

---

## Anexos

- **Anexo A:** `research-validacion.md` — Deep Research de validación (6 secciones, 51 fuentes citadas).
- **Anexo B:** `research-critica.md` — Deep Research de crítica (8 secciones, 47 fuentes, 8 supuestos cuestionados).
- **Anexo C:** `Balances-OPENLAND-2026-historico.xlsx` — archivo histórico con 4 meses ya armados (Enero–Abril 2026) que sirve como golden set para validar el MVP.

---

*Hardcore AI by 30X — Cohorte 2 — Estación 1 — Opción B — Mayo 2026*
*Green Power S.A.S. — Llanos Orientales, Colombia*
