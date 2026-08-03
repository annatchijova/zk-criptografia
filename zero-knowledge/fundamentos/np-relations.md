# Relaciones NP: el punto de partida de todo circuito ZK

## ¿Qué problema resuelve?
Antes de escribir una sola línea de circuito hay que responder: ¿qué relación matemática estoy probando? Toda prueba ZK es, en el fondo, una prueba de membresía en un lenguaje NP.

## Idea intuitiva
NP es la clase de problemas cuya solución, una vez exhibida, se *verifica* rápido. Eso es exactamente lo que hace un verificador ZK: no busca la solución, solo chequea que el witness satisface la relación. Diseñar un sistema ZK = expresar el problema real como "∃w: R(x, w) = 1".

## Definición formal
Una **relación NP** es R ⊆ {0,1}* × {0,1}* tal que:
1. existe un polinomio p con |w| ≤ p(|x|) para todo (x,w) ∈ R (witness corto), y
2. R es decidible en tiempo polinomial: hay una máquina determinista M con M(x, w) = 1 ⟺ (x, w) ∈ R.

El lenguaje asociado es L_R = { x : ∃w, (x,w) ∈ R }. Ejemplos canónicos:
- **Satisfactibilidad**: R_SAT = { (φ, a) : a satisface φ }.
- **Logaritmo discreto**: R_DL = { (g, h, x) : g^x = h }.
- **Circuitos aritméticos (R1CS)**: R = { (x, w) : (A·z) ∘ (B·z) = C·z, z = (1, x, w) } — la forma estándar que consumen SNARKs tipo Groth16/PLONK (ver [[historia-zk]]).

## Complejidad / garantías / limitaciones
Por Cook–Levin, todo lenguaje NP se reduce a SAT, y por tanto a circuitos aritméticos: en principio *cualquier* afirmación verificable es demostrable en ZK (resultado de GMW86). El costo práctico depende del tamaño del circuito resultante: la reducción puede inflar el problema. La calidad del diseño está en elegir una representación R compacta.

## Aplicaciones concretas
Hash preimagen (R = {(h, s): H(s) = h}), membresía Merkle, rangos, firmas: cada "feature" de privacidad es una relación NP.

## Relación con Midnight / Compact (si aplica)
Un contrato Compact se compila exactamente a una de estas relaciones (sistema de restricciones); el prover de Midnight busca el witness y el nodo verifica la membresía del statement. Ver [[witness-statement]].

## Relación con mi proyecto (si aplica)
Mi relación: R((root_registro, com_evidencia), (perito, credencial, path_Merkle, evidencia)) = 1 ⟺ la credencial firma la identidad del perito ∧ la raíz Merkle de {perito, credencial} abre en root_registro ∧ H(evidencia) está comprometida en com_evidencia. Eso traduce Daubert a NP.

## Referencias
- Cook, "The Complexity of Theorem-Proving Procedures", STOC 1971.
- Arora, Barak, "Computational Complexity: A Modern Approach", Cap. 2, Cambridge UP, 2009.
- Goldreich, "Foundations of Cryptography, Vol. 1", Cap. 2, 2001.
