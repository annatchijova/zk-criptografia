# Guías NIST para evidencia digital forense

## ¿Qué problema resuelve?
Los tribunales estadounidenses —y por influencia, los de otras jurisdicciones— exigen que las técnicas forenses descansen en metodología reconocida. Sin publicaciones de referencia, cada perito improvisa su método y la oposición puede impugnar la prueba por falta de validación (véase el estándar [[reproducibilidad-cientifica|Daubert]]).

## Idea intuitiva
El NIST (National Institute of Standards and Technology) publica guías prácticas que funcionan como "recetas validadas": si el perito siguió SP 800-86 al recolectar la evidencia y usó herramientas probadas según el programa CFTT, su metodología tiene respaldo institucional.

## Definición formal
Guías clave:
- **NIST SP 800-86** (*Guide to Integrating Forensic Techniques into Incident Response*, 2006): ciclo completo de adquisición, examen, análisis y reporte; principios de preservación y documentación.
- **NIST SP 800-101 Rev. 1** (*Guidelines on Mobile Device Forensics*, 2014): adquisición de teléfonos móviles (métodos manual, lógico, físico, chip-off).
- **NIST SP 800-72** (*Guidelines on PDA Forensics*, 2004, retirada pero históricamente citada).
- **CFTT** (*Computer Forensics Tool Testing Program*): validación de herramientas forenses con especificaciones, planes y resultados de prueba publicados, directamente relevante para la [[tasa-de-error]].
- **NISTIR 8387** y trabajos sobre *black-box/white-box testing* de herramientas forenses (Osborne et al.).

## Alcance / garantías / limitaciones
Las guías son normativamente voluntarias (salvo para agencias federales EE.UU. vía FISMA), pero los tribunales las tratan como referencia de buenas prácticas. No cubren escenarios emergentes (contratos inteligentes, pruebas ZK) de forma específica.

## Aplicaciones concretas
- Protocolos de adquisición de celulares de víctimas siguiendo SP 800-101.
- Selección de herramientas validadas por CFTT para el análisis.
- Fundamentación metodológica del informe pericial ante objeciones Daubert/Frye.

## Relación con Midnight / Compact (si aplica)
El paradigma "documentar el método" de NIST se extiende al circuito [[compact]]: el circuito mismo es una herramienta que puede versionarse, auditarse y someterse a pruebas, análogo al programa CFTT.

## Relación con mi proyecto (si aplica)
La adquisición de evidencia de víctimas sigue SP 800-86/800-101; la verificación ZK se añade como capa de integridad posterior, compatible con el flujo documentado.

## Referencias
- NIST SP 800-86, https://csrc.nist.gov/pubs/sp/800/86/final
- NIST SP 800-101 Rev. 1, https://csrc.nist.gov/pubs/sp/800/101/r1/final
- NIST CFTT, https://www.nist.gov/itl/ssd/software-quality-group/computer-forensics-tool-testing-program-cftt

Ver también: [[iso-27037]], [[swgde]], [[enfsi]]
