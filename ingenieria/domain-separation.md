# Separación de dominios (Domain Separation)

## ¿Qué problema resuelve?
Si la misma función hash o firma se usa en varios contextos sin distinguirlos, un valor producido para un propósito puede reinterpretarse maliciosamente en otro (*cross-protocol attacks*, confusión de contexto). La misma evidencia "firmada" para auditoría interna no debería valer como prueba judicial.

## Idea intuitiva
Etiquetar cada mensaje con el "departamento" que lo emitió: un sello de "RECURSOS HUMANOS" no debe ser válido en "LEGAL".

## Definición formal
Técnica de incluir un identificador de dominio (cadena única, estructurada) como prefijo o input dedicado de cada primitiva, de modo que las instancias se comporten como funciones aleatorias independientes:
- `H(tag || datos)` con tags distintos y no ambiguos: `"perito-cred-v1"`, `"evidencia-commit-v1"`, `"nullifier-caso-v1"`.
- En hashes estilo Poseidon, usar el campo/capacidad o un input fijo como tag; en SHA-2, prefijo con longitud codificada para evitar ambigüedad de concatenación (`len(tag)||tag||datos`).
- En Fiat-Shamir, incluir en el transcript **todo** el contexto: statement, dominio, versión del circuito — si no, la prueba es maleable entre contextos.

## Complejidad / garantías / limitaciones
Costo casi nulo; garantía fuerte si los tags son únicos globalmente y nunca se reutilizan entre versiones. Error frecuente: tags ambiguos o reutilizados al actualizar el circuito, reabriendo confusión de contexto.

## Aplicaciones concretas
- Nullifier del perito: `H("midnight-forense/nullifier/v1" || sk || casoID)` — véase [[replay-attacks]] y [[nullifiers-nonces]].
- Commitment de evidencia: dominio distinto del de la credencial, para que un commitment de una cosa jamás se acepte como la otra.

## Relación con Midnight / Compact
En Compact, declarar el tag como constante del contrato y pasarlo al circuito como input fijo; auditar que el transcript de la prueba lo incluya.

## Relación con mi proyecto
La separación de dominios es un argumento técnico directo de fiabilidad metodológica (Daubert): cada objeto criptográfico tiene un significado único y documentado.

## Referencias
- Bellare & Rogaway, "Random Oracles are Practical", CCS 1993.
- "The Fiat-Shamir Transform" y ataques de weak transcript: https://github.com/zksecurity (Frozen Heart, 2022).
- RFC 9380, "Hashing to Elliptic Curves" (DST, domain separation tags).
