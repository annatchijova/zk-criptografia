# Supuestos de seguridad (Security Assumptions)

## ¿Qué problema resuelve?
Toda prueba ZK descansa sobre supuestos matemáticos. Si no se enuncian explícitamente, no se puede razonar sobre qué ocurre cuando uno falla — ni explicárselo a un tribunal.

## Idea intuitiva
Un edificio es seguro si sus cimientos aguantan. Cada componente criptográfico declara qué "cimientos" necesita: que cierto problema sea difícil de computar.

## Definición formal
- **Curva elíptica (BLS12-381 / curvas de Midnight)**: seguridad basada en la dificultad del logaritmo discreto (ECDLP) y decisional Diffie-Hellman. Si ECDLP se rompe (ordenador cuántico con Shor), las firmas y *commitments* colapsan.
- **Hash (Poseidon / SHA-256)**: resistencia a colisiones y a preimagen. Poseidon es nuevo y algebraicamente estructurado: menor historial de criptoanálisis público que SHA-2.
- **Setup**: los SNARKs tipo Groth16 requieren una ceremonia de parámetros confiables (*trusted setup*); si el *toxic waste* se conservó, el atacante puede falsificar pruebas. Sistemas con setup transparente o universal (PLONK, Halo2) reducen este supuesto.
- **Aleatoriedad**: generadores débiles rompen *nonces* y *blinding factors*.

## Complejidad / garantías / limitaciones
Seguridad condicional: la garantía es "seguro **si** el supuesto se mantiene". Fallar en un supuesto suele ser catastrófico y silencioso (falsificación indetectable).

## Aplicaciones concretas
Documentar en el KB qué supuesto cubre cada pieza: credencial del perito (firma sobre curva), *commitment* de evidencia (hash), prueba (SNARK con o sin setup).

## Relación con Midnight / Compact
Midnight usa pruebas ZK sobre Compact; conviene identificar qué sistema de pruebas subyace y si su setup es transparente. Véase [[cryptographic-agility]] para el plan si un supuesto cae.

## Relación con mi proyecto
Bajo Daubert, listar supuestos y su estado de aceptación comunitaria es evidencia de metodología testeada y revisada por pares.

## Referencias
- Shor, "Algorithms for Quantum Computation", FOCS 1994.
- Grassi et al., "Poseidon: A New Hash Function for ZK", USENIX Security 2021.
- Ben-Sasson et al., "SNARKs…", EUROCRYPT 2014 (trusted setup).
