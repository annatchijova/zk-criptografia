# Reproducibilidad científica y el estándar Daubert

## ¿Qué problema resuelve?
La evidencia pericial solo es admisible si descansa en método científico, no en la autoridad del perito. El problema central: ¿cómo distingue un juez —que no es científico— entre ciencia legítima y "junk science"?

## Idea intuitiva
Reproducibilidad significa que otro examinador, con los mismos datos y método, llega al mismo resultado. Si una técnica solo funciona en manos de su inventor, no es ciencia: es artesanía. El derecho estadounidense convirtió esta exigencia en regla de admisibilidad.

## Definición formal
El caso *Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993), fijó al juez federal como "gatekeeper" y enumeró factores no exhaustivos:
1. **Comprobabilidad (testability)**: la teoría puede ser falsada (Popper).
2. **Revisión por pares y publicación**.
3. **[[tasa-de-error|Tasa de error]] conocida o potencial**.
4. **Existencia de estándares que controlen la operación** de la técnica.
5. **Aceptación general** en la comunidad científica (resabio de *Frye v. United States*, 293 F. 1013, D.C. Cir. 1923).

Estos factores se codificaron en la **Regla 702 de las Reglas Federales de Evidencia** (enmendada en 2000 y aclarada en 2023: la proponente debe demostrar fiabilidad "más probable que no"). *Kumho Tire Co. v. Carmichael*, 526 U.S. 137 (1999) extendió Daubert a peritajes técnicos, no solo científicos.

## Alcance / garantías / limitaciones
Daubert es un marco flexible, no una lista rígida; no todos los factores aplican a toda disciplina. Rige en tribunales federales de EE.UU. y en muchos estatales, pero no en otras jurisdicciones (Argentina, p. ej., usa el estándar de sana crítica del art. 389 del Código Procesal Civil y Comercial de la Nación, donde la reproducibilidad opera como criterio de valoración).

## Aplicaciones concretas
- Impugnación de software forense propietario sin código auditado (casos *United States v. Chubbs*, 2012, sobre herramientas de software).
- Exigencia de que el peritaje digital cite método, versión de herramienta y validación (véase [[nist-digital-evidence|CFTT]]).

## Relación con Midnight / Compact (si aplica)
Un circuito ZK [[compact]] es intrínsecamente reproducible: cualquier verificador ejecuta la misma verificación y obtiene el mismo bit. Esto satisface el factor 1 (falsabilidad: la prueba verifica o falla) y el 4 (el estándar es el propio protocolo criptográfico).

## Relación con mi proyecto (si aplica)
Probar on-chain que el perito es quien dice ser y posee la evidencia comprometida es verificable por cualquiera: la reproducibilidad deja de ser una promesa del perito y pasa a ser una propiedad del sistema.

## Referencias
- *Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993)
- *Kumho Tire Co. v. Carmichael*, 526 U.S. 137 (1999)
- Federal Rules of Evidence, Rule 702 (as amended 2023)
- *Frye v. United States*, 293 F. 1013 (D.C. Cir. 1923)

Ver también: [[tasa-de-error]], [[swgde]], [[enfsi]]
