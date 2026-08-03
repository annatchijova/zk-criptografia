# El paradigma de simulación: "si existe simulador, no hay fuga"

## ¿Qué problema resuelve?
¿Cómo se *demuestra* que un protocolo no filtra información? No basta con argumentar que los mensajes "parecen aleatorios". El paradigma de simulación convierte la intuición de privacidad en una propiedad verificable.

## Idea intuitiva
Si todo lo que el adversario ve en la ejecución real puede ser generado por una máquina (el simulador) que **no conoce el secreto**, entonces la vista real no puede contener información sobre ese secreto: cualquier cosa que el adversario compute tras la interacción, ya la podía haber computado solo.

## Definición formal
En pruebas ZK: existe S (tiempo polinomial, posiblemente esperado) tal que para todo (x,w) ∈ R y todo V*:

view_{V*}(real) ≈ S^{V*}(x).

El simulador suele tener poderes que el prover no tiene: **rebobinar** al verificador, **programar el oráculo aleatorio** o **conocer la trapdoor de la CRS**. Estos poderes no invalidan el argumento: existen solo en el experimento mental que delimita qué información podría fluir. Más allá de ZK, el paradigma real/ideal (framework UC de Canetti) define seguridad general: la ejecución real es indistinguible de una ejecución ideal con una funcionalidad confiable.

## Complejidad / garantías / limitaciones
- Composición: la simulación permite razonar modularmente (lema de composición; UC da composición universal).
- Limitación: simuladores con rebobinado fallan bajo composición concurrente general; se requieren nociones más fuertes (ZK no interactivo, UC).
- "Indistinguible" se gradúa: perfecto, estadístico, computacional (ver [[zero-knowledge-property]]).

## Aplicaciones concretas
Es *la* técnica de demostración detrás de [[zero-knowledge-property]], [[honest-verifier-zk]], firmas digitales (simulación del oráculo de firma) y MPC.

## Relación con Midnight / Compact (si aplica)
La privacidad de Midnight descansa en este argumento: el transcript on-chain de un circuito Compact es simulable sin el witness, luego no revela los datos privados del estado.

## Relación con mi proyecto (si aplica)
Formalmente, la protección de las víctimas se reduce a: "existe un simulador que, sin conocer identidades ni evidencia, produce una distribución indistinguible del transcript publicado". Es el enunciado que un peritaje Daubert puede exigir.

## Referencias
- Goldwasser, Micali, Rackoff, SIAM J. Comput. 18(1), 1989.
- Goldreich, "Foundations of Cryptography, Vol. 1", §4.3, 2001.
- Canetti, "Universally Composable Security", JACM 67(5), 2020 (FOCS 2001).
- Lindell, "How to Simulate It – A Tutorial on the Simulation Proof Technique", Tutorials on the Foundations of Cryptography, Springer 2017.
