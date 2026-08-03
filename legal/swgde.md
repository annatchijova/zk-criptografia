# SWGDE — Lineamientos para evidencia digital

## ¿Qué problema resuelve?
La tecnología cambia más rápido que las normas ISO. SWGDE (Scientific Working Group on Digital Evidence) publica lineamientos actualizados y específicos que llenan ese vacío: desde adquisición de dispositivos móviles hasta evidencia de vehículos, drones y video digital.

## Idea intuitiva
SWGDE es un grupo de trabajo científico estadounidense (bajo la órbita del FBI) que redacta documentos de "mejores prácticas" sometidos a comentario público. Para un tribunal, decir "seguí el documento SWGDE X versión Y" es un atajo retórico para demostrar método aceptado por la comunidad científica —uno de los factores [[reproducibilidad-cientifica|Daubert]].

## Definición formal
Documentos destacados (serie en swgde.org):
- **SWGDE Best Practices for Computer Forensic Examination**: proceso general de examen, documentación y reporte.
- **SWGDE Best Practices for Mobile Phone Evidence Recovery and Analysis**: adquisición lógica/física, manejo de SIM, modo avión, aislamiento de red (Faraday).
- **SWGDE Establishing Confidence in Digital and Multimedia Evidence Forensic Results by Error Mitigation Analysis**: aborda explícitamente la gestión de errores, conectando con [[tasa-de-error]].
- **SWGDE/SWGIT Digital & Multimedia Evidence Glossary**: terminología estandarizada para informes.
- Requisitos transversales: control de versiones del documento, competencia del examinador, y trazabilidad de la [[cadena-de-custodia]].

## Alcance / garantías / limitaciones
Son lineamientos sin fuerza normativa propia; su valor probatorio depende de que el tribunal los acepte como práctica estándar. Están escritos para el contexto estadounidense, aunque se citan internacionalmente junto a [[enfsi]] y [[iso-27037]].

## Aplicaciones concretas
- Adquisición de celulares de víctimas o victimarios siguiendo el manual SWGDE de móviles.
- Sección de "análisis de mitigación de errores" en el informe pericial para resistir objeciones Daubert.
- Uso del glosario para evitar ambigüedades terminológicas en el dictamen.

## Relación con Midnight / Compact (si aplica)
SWGDE exige documentar cada paso del proceso; un compromiso criptográfico on-chain en [[midnight]] puede funcionar como el registro inmutable de esa documentación, verificable sin exponer contenido sensible.

## Relación con mi proyecto (si aplica)
El protocolo de mi peritaje adopta los documentos SWGDE de móviles y mitigación de errores; la prueba ZK demuestra el cumplimiento del procedimiento sin revelar los datos de las víctimas.

## Referencias
- SWGDE, documentos vigentes: https://www.swgde.org/documents
- SWGDE Best Practices for Mobile Phone Evidence Recovery and Analysis
- SWGDE Error Mitigation Analysis

Ver también: [[nist-digital-evidence]], [[tasa-de-error]]
