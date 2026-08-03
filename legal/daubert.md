# Criterios Daubert: estándar de admisibilidad de peritajes científicos

## ¿Qué problema resuelve?
Los jueces deben decidir qué testimonios de expertos científicos pueden presentarse al jurado, filtrando la "ciencia basura". Daubert da los criterios para ese filtro.

## Idea intuitiva
En *Daubert v. Merrell Dow Pharmaceuticals, Inc.*, 509 U.S. 579 (1993), la Corte Suprema de EE. UU. dijo que el juez actúa como "gatekeeper" (guardián): antes de admitir un peritaje, debe comprobar que el método es científicamente válido, no solo que el perito suena convincente.

## Definición formal
La Corte enumeró factores no exhaustivos:
1. **Testabilidad**: la teoría o técnica puede ser (y ha sido) sometida a prueba empírica (falsabilidad popperiana).
2. **Revisión por pares y publicación**: sometimiento al escrutinio de la comunidad científica.
3. **Tasa de error conocida o potencial** y existencia de estándares que controlen la operación de la técnica.
4. **Aceptación general** en la comunidad científica pertinente (residuo de [[frye]]).
Complementado por *General Electric Co. v. Joiner*, 522 U.S. 136 (1997) (estándar de revisión: abuso de discreción) y *Kumho Tire Co. v. Carmichael*, 526 U.S. 137 (1999) (extensión a peritajes técnicos no científicos), y codificado en la [[federal-rules-evidence|Rule 702]] (enmendada en 2000 y 2023).

## Alcance / garantías / limitaciones
- Garantiza relevancia y fiabilidad metodológica, no corrección del resultado.
- Es flexible: no aplica los cuatro factores mecánicamente.
- Rige en tribunales federales de EE. UU. y en la mayoría de los estados, pero no todos.

## Aplicaciones concretas
Admisibilidad de ADN, toxicología, análisis forense digital, reconstrucción de accidentes.

## Relación con Midnight / Compact (si aplica)
Cada factor se traduce a una garantía técnica verificable: testabilidad → circuitos ZK públicos y auditables en Compact; revisión por pares → código abierto y auditorías; tasa de error → seguridad criptográfica cuantificable (soundness error); estándares → [[timestamping-rfc3161]] y hashes NIST; aceptación general → uso extendido de SHA-256 en forense.

## Relación con mi proyecto (si aplica)
Mi prueba ZK sobre Midnight demuestra identidad pericial y posesión de evidencia sin revelar datos: documentar la matemática de estas traducciones es la clave para pasar el filtro Daubert en un juicio.

## Referencias
- *Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993).
- *Kumho Tire Co. v. Carmichael*, 526 U.S. 137 (1999).
- Fed. R. Evid. 702 (como enmendada 2023).
