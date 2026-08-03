# Estándar Frye: aceptación general en la comunidad científica

## ¿Qué problema resuelve?
Antes de Daubert, los tribunales necesitaban un criterio simple para admitir peritajes de técnicas científicas novedosas. Frye lo resolvió apoyándose en el consenso científico.

## Idea intuitiva
En *Frye v. United States*, 293 F. 1013 (D.C. Cir. 1923), la corte rechazó un precursor del polígrafo. Razonó que una técnica solo es admisible cuando ha pasado de la etapa experimental a la demostrable, ganando "aceptación general" (*general acceptance*) en el campo científico al que pertenece.

## Definición formal
Test de Frye: la evidencia basada en una técnica científica es admisible si la técnica es "generalmente aceptada" como fiable por la comunidad científica pertinente. A diferencia de [[daubert]], no exige al juez evaluar testabilidad, tasa de error ni revisión por pares: delega la validación en la comunidad experta.

## Alcance / garantías / limitaciones
- **Garantía**: filtra pseudociencia sin exigir al juez competencia científica.
- **Limitaciones**: puede rechazar ciencia válida muy nueva (aún sin consenso) y aceptar prácticas establecidas pero erróneas (p. ej., algunas técnicas forenses comparativas).
- **Jurisdicciones**: Daubert rige en tribunales federales y la mayoría de los estados; Frye (o variantes) sobrevive en California, Nueva York, Illinois, Pensilvania, Maryland, Nueva Jersey y Washington, entre otros. Florida alternó entre ambos (finalmente adoptó Daubert en 2019, *In re Amendments to Florida Evidence Code*).

## Aplicaciones concretas
En jurisdicciones Frye, la admisibilidad de hashing forense o de una prueba ZK depende de mostrar su uso aceptado: SHA-256 como estándar forense NIST, [[hashes-como-evidencia]], marcas de tiempo conforme a [[timestamping-rfc3161]].

## Relación con Midnight / Compact (si aplica)
Un circuito Compact que prueba posesión de evidencia puede argumentar aceptación general apoyándose en primitivas estándar (hash, firmas, commitments) ya validadas por el forense digital; la parte novedosa (ZK) reduce el riesgo porque no exige revelar datos.

## Relación con mi proyecto (si aplica)
Si mi hackathon apunta a jurisdicciones Frye, debo enfatizar que la técnica criptográfica subyacente ya goza de consenso técnico, en lugar de pedir al juez que evalúe la matemática ZK.

## Referencias
- *Frye v. United States*, 293 F. 1013 (D.C. Cir. 1923).
- *Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993).
- *In re Amendments to Florida Evidence Code*, 278 So. 3d 551 (Fla. 2019).
