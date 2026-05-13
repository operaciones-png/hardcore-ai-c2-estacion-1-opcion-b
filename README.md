# Hardcore AI Cohorte 2 — Estación 1 — Opción B

**Autor:** Juanda — Gerente de Operaciones, Green Power S.A.S.
**Empresa:** Green Power S.A.S. (E&P hidrocarburos, Llanos Orientales, Colombia, ~90 empleados, opera bloque con activos Kimbo y Caliche)
**Inversionista único:** OPENLAND
**Plantilla utilizada:** Internal Solution Brief (caso de uso corporativo)
**Fecha de entrega:** 13 de mayo de 2026

---

## Esta es la Opción B de mi entrega — complementaria a la Opción A

| | **Opción A** ([repo separado](https://github.com/operaciones-png/hardcore-ai-c2-estacion-1)) | **Opción B** (este repo) |
|---|---|---|
| **Dolor que ataca** | Causación manual de facturas + 3 reportes financieros mensuales | Armado mensual del reporte a OPENLAND (único inversionista) que consume ~80 horas-gerente/mes |
| **Naturaleza** | Automatización de un proceso (causación) | Centralización + consulta conversacional + pre-armado de reporte |
| **Usuario principal** | Equipo financiero (3 personas) | Yo (Juanda) + Gerente Financiero |
| **AI principal** | Extracción estructurada (visión + esquemas) | Agente conversacional con text-to-SQL + RAG |
| **Output** | Causaciones pre-llenadas en Excel | Respuestas a preguntas + reporte OPENLAND pre-armado |

---

## Contenido de esta entrega

| Archivo | Descripción |
|---|---|
| [`green-power-isb-opcion-b.md`](./green-power-isb-opcion-b.md) | **Internal Solution Brief completo** — 9 secciones del template + anexos. Es el artefacto principal de la entrega. |
| [`presentacion-opcion-b.md`](./presentacion-opcion-b.md) | **Guion de presentación** — 8–10 minutos para exponer ante el tutor con preguntas probables y respuestas. |
| [`research-validacion.md`](./research-validacion.md) | **Deep Research de validación** — casos comparables, arquitectura RAG, clasificación de gastos, integración fuentes Colombia, seguridad de datos, ROI (51 fuentes citadas). |
| [`research-critica.md`](./research-critica.md) | **Deep Research de crítica** — 8 secciones de riesgos: técnicos, integridad reporte, regulatorios, adopción, alcance/timeline, competitivo, políticos, supuestos cuestionados (47 fuentes, conclusión forzada incluida). |
| [`Balances-OPENLAND-2026-historico.xlsx`](./Balances-OPENLAND-2026-historico.xlsx) | Archivo histórico de Green Power con los 4 meses ya armados (Enero–Abril 2026). Sirve como golden set para validar el MVP. |

---

## Resumen ejecutivo del proyecto

**Iniciativa:** Célula AI Green Power — Fase 1 financiera (rama Opción B).

**MVP a 4 semanas:** Asistente Conversacional Interno + Pre-armado del Reporte OPENLAND con Humano-en-el-Loop. Tres componentes:
1. **Repositorio centralizado** (PostgreSQL + Qdrant self-hosted) que ingiere 8 fuentes de datos financieros operativos
2. **Chat conversacional** con text-to-SQL para cifras + RAG para contexto + citación obligatoria
3. **Pre-armado automático** de las dos hojas mensuales del reporte OPENLAND con human-in-the-loop al 100%

**Volumen / dolor cuantificado:**
- ~80 horas-gerente/mes hoy en armar el reporte (Gerente de Operaciones + Gerente Financiero)
- ~USD 4.000/mes (≈ COP 16M/mes) costo escondido del armado manual
- Deadline duro día 10 de cada mes contra único inversionista (OPENLAND)

**Stack técnico:** n8n + Claude Sonnet 4.6 + PostgreSQL + Qdrant + LlamaParse + APIs públicas (EIA, datos.gov.co).

**Costo operativo proyectado:** USD 95–155/mes.

**Ahorro proyectado vs proceso manual actual:** ~USD 3.850/mes una vez en producción estable. **ROI: 3x–6x a 6 meses, payback 6–10 meses.**

---

## Decisión de diseño clave

El MVP fue **re-encuadrado** tras estudiar el Deep Research de crítica para evitar los modos de fallo más caros documentados en la literatura financiera y regulatoria colombiana:

| Crítica recibida | Decisión de diseño |
|---|---|
| "FinanceBench: 1 de cada 4 cifras es incorrecta en RAG convencional" | **Text-to-SQL para todos los números**, RAG solo para contexto narrativo. Cita obligatoria con confidence ≥ 0.95. Si no, "no encontré la información". |
| "Asignación de gastos compartidos entre bloques es decisión humana, no inferible por LLM" | **Tabla maestra de distribución Kimbo/Caliche** mantenida por el Gerente Financiero. El sistema solo aplica reglas humanas. |
| "Procesar PILA/nómina con cloud externo viola Habeas Data Ley 1581/2012" | **PILA y nómina excluidas del MVP**. Fase 2 con DPA Anthropic + autorizaciones individuales. |
| "El contador titular es responsable legal del reporte (Ley 43/1990)" | Contador titular es **co-owner del sistema** desde día 1. Firma cada reporte antes de enviar a OPENLAND. Tiene poder de veto. |
| "Sponsor de Operaciones liderando sistema financiero genera shadow IT" | El sponsor (Juanda) **ya co-arma el reporte** junto al Gerente Financiero. Esto es legitimidad funcional, no shadow IT. Co-ownership formal con Financiero y Contador. |
| "4 semanas insuficientes para producción contable real (RAND: 80% AI proyectos fallan)" | El MVP entrega **prototipo funcional con HITL al 100% sobre 1 mes piloto**, no producción mensual recurrente. Roadmap a 3–5 meses para Fase 2. |
| "Microsoft Copilot for Finance ya hace partes de esto nativamente" | Justificación explícita: el MVP construye dominio upstream Green Power (centros de costo por bloque, lógica JOA, integración XML UBL DIAN, APIs Brent/TRM). Si Copilot evoluciona, los assets construidos siguen siendo válidos. |
| "Inversionista no debe recibir nada generado por AI sin validación humana firmada" | **OPENLAND nunca recibe output directo del sistema**. El reporte se envía solo después de revisión del Gerente Financiero + firma del Contador titular. Nunca usar "fue la AI" como justificación. |

---

## Roadmap multi-fase

| Fase | Alcance | Duración | Estado |
|---|---|---|---|
| **Fase 1 — MVP** | Chat interno + pre-armado reporte OPENLAND con HITL, mes piloto | 4 semanas | EN ENTREGA |
| **Fase 2 — Producción** | DPA + ingesta PILA y nómina; pre-armado mensual recurrente | 3–5 meses | Pendiente cierre Fase 1 |
| **Fase 3 — Reportería extendida** | Cuentas por cobrar/pagar, conciliación bancaria, exógena | 4–6 meses | Pendiente |
| **Fase 4 — Tributario integral** | Estados financieros NIIF, declaraciones tributarias asistidas | Por definir | Pendiente |

---

## Cómo leer esta entrega

1. **Empezar por** [`green-power-isb-opcion-b.md`](./green-power-isb-opcion-b.md) — es el artefacto principal y autocontenido.
2. **Profundizar en** [`research-validacion.md`](./research-validacion.md) para entender la evidencia técnica, costos, casos comparables y APIs colombianas.
3. **Cerrar con** [`research-critica.md`](./research-critica.md) para entender los riesgos identificados (especialmente la "Conclusión Forzada" al final) y por qué el MVP fue re-encuadrado como está.
4. **Revisar** [`presentacion-opcion-b.md`](./presentacion-opcion-b.md) si vas a evaluarme en exposición oral.
5. **Consultar** el Excel `Balances-OPENLAND-2026-historico.xlsx` para ver el formato real del reporte que el MVP automatiza.

---

*Entrega para Hardcore AI by 30X — Cohorte 2 — Estación 1 — Opción B*
