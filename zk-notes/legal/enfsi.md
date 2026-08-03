# ENFSI — Estándares de la red europea de institutos forenses

## ¿Qué problema resuelve?
Europa carece de un organismo único como el NIST; cada Estado tiene su propio sistema pericial. ENFSI (European Network of Forensic Science Institutes) armoniza criterios para que la evidencia producida en un país sea aceptable y comparable en otro, lo cual es crítico en delitos transfronterizos contra víctimas.

## Idea intuitiva
ENFSI es una red de laboratorios forenses públicos europeos que publica guías de mejores prácticas por disciplina. Si dos laboratorios de países distintos siguen la misma guía ENFSI, sus resultados son comparables y mutuamente verificables.

## Definición formal
Documentos relevantes:
- **ENFSI Best Practice Manual for the Forensic Examination of Digital Technology (ENFSI-BPM-DT-01)**: define el proceso ABCDE —*Acquire, Validate, Analyse, Interpret, Report*— más validación de métodos y competencia del examinador.
- **ENFSI Guideline for Evaluative Reporting** (2015): marco probabilístico (razón de verosimilitud) para informar la fuerza de la evidencia.
- Requisito de acreditación: los laboratorios miembros deben acreditar ISO/IEC 17025, lo que exige métodos validados, ensayos de aptitud y [[tasa-de-error|tasas de error]] documentadas.
- Marco europeo complementario: el Reglamento e-Evidence (UE) 2023/1543 y la Directiva 2014/41/EU (orden europea de investigación) presuponen estándares forenses comunes.

## Alcance / garantías / limitaciones
Las guías ENFSI no son ley: su fuerza proviene de la acreditación ISO/IEC 17025 y del reconocimiento judicial europeo. Aplican a laboratorios, no necesariamente a peritos privados, aunque sirven como estándar de comparación.

## Aplicaciones concretas
- Peritajes en casos de víctimas con evidencia recolectada en varios Estados miembros.
- Validación formal de herramientas y métodos antes de su uso (requisito del BPM).
- Informes evaluativos con verosimilitud en lugar de afirmaciones categóricas.

## Relación con Midnight / Compact (si aplica)
La exigencia ENFSI de "método validado y versionado" encaja con circuitos [[compact]] deterministas: el circuito es un método cuyo comportamiento puede auditarse y cuya salida es reproducible por cualquier verificador.

## Relación con mi proyecto (si aplica)
El sistema de prueba ZK del perito documenta su método como lo haría un laboratorio ENFSI: versión del circuito, parámetros, y verificabilidad independiente, facilitando aceptación transfronteriza.

## Referencias
- ENFSI Best Practice Manual DT-01, https://enfsi.eu/documents/best-practice-manuals/
- ENFSI Guideline for Evaluative Reporting in Forensic Science (2015)
- Reglamento (UE) 2023/1543, e-Evidence
- ISO/IEC 17025:2017

Ver también: [[iso-27037]], [[swgde]], [[reproducibilidad-cientifica]]
