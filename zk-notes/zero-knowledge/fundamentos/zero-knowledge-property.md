# La propiedad de conocimiento cero (zero-knowledge)

## ¿Qué problema resuelve?
Formaliza la promesa central: el verificador **no aprende nada** más allá de la validez del enunciado. Ni el witness, ni bits parciales, ni estadísticas: nada que no pudiera haber generado él mismo.

## Idea intuitiva
Todo lo que el verificador ve durante la prueba podría haberlo producido solo, sin interactuar con el prover. Si puede "simularse" la conversación sin conocer el secreto, la conversación real no puede estar filtrando ese secreto. Esta es la técnica del [[simulation|simulador]].

## Definición formal
(P, V) es **conocimiento cero** para una [[np-relations|relación NP]] R si para todo verificador (posiblemente malicioso) V* de tiempo polinomial existe un **simulador** S de tiempo polinomial esperado tal que para todo (x, w) ∈ R:

{ view_{V*}( ⟨P(x,w), V*(x)⟩ ) } ≈ { S^{V*}(x) },

donde view incluye los mensajes recibidos por V* y sus monedas aleatorias, y ≈ denota indistinguibilidad. Según el tipo de ≈:
- **PZK (perfecto)**: distribuciones idénticas.
- **SZK (estadístico)**: distancia estadística despreciable.
- **CZK (computacional)**: indistinguibles por todo distinguidor polinomial.

## Complejidad / garantías / limitaciones
ZK perfecto/estadístico es más fuerte pero más caro y restringido (SZK tiene límites de complejidad: está en AM ∩ coAM). CZK es lo habitual en SNARKs. La definición vía simulador es *simulable*, lo que permite componer protocolos de forma segura (composición secuencial directa; la concurrente requiere cuidado).

## Aplicaciones concretas
- Credenciales anónimas: probar "soy mayor de edad" sin revelar la fecha de nacimiento.
- Transacciones privadas: probar balance suficiente sin revelar montos ni partes.

## Relación con Midnight / Compact (si aplica)
Midnight separa estado público y privado: los datos privados entran como witness del circuito Compact y, por la propiedad ZK, jamás aparecen on-chain ni en la transcripción de verificación.

## Relación con mi proyecto (si aplica)
Es la propiedad que protege a las víctimas: el verificador (corte, contraparte) aprende que el perito es auténtico y que la evidencia existe y está integrada, pero no identidades ni contenido. Ver [[witness-statement]] y [[honest-verifier-zk]].

## Referencias
- Goldwasser, Micali, Rackoff, "The Knowledge Complexity of Interactive Proof Systems", SIAM J. Comput. 18(1), 1989.
- Goldreich, "Foundations of Cryptography, Vol. 1", §4.3, 2001.
- Vadhan, "A Study of Statistical Zero-Knowledge Proofs", Springer, 2003.
