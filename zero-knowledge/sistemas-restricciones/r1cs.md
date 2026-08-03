# R1CS (Rank-1 Constraint System)

## ¿Qué problema resuelve?
Un [[arithmetic-circuits|circuito aritmético]] es una descripción estructural; los SNARKs necesitan una forma algebraica plana y uniforme. R1CS es esa forma canónica: una lista de ecuaciones cuadráticas simples cuya satisfacibilidad equivale a la correcta evaluación del circuito. Es el formato de entrada estándar de Groth16 y de circom/snarkjs.

## Idea intuitiva
Aplaná el circuito: cada puerta de multiplicación se convierte en una fila "vector A · cables × vector B · cables = vector C · cables". Las sumas no generan filas nuevas: se acumulan dentro de las combinaciones lineales de A, B y C. Al final, la corrección de todo el circuito es una sola matriz de ecuaciones del mismo molde.

## Definición formal
Una instancia R1CS sobre $\mathbb{F}$ consiste en matrices $A, B, C \in \mathbb{F}^{m \times n}$ y un vector de variables $z = (1, x, w)$ (constante 1, entradas públicas $x$, testigo $w$). La asignación $z$ satisface el sistema si
$$(Az) \circ (Bz) = (Cz),$$
donde $\circ$ es el producto coordenada a coordenada (Hadamard). Cada fila $i$ codifica la restricción $\langle a_i, z\rangle \cdot \langle b_i, z\rangle = \langle c_i, z\rangle$. Derivación desde un circuito: un cable por variable, una restricción por multiplicación; las sumas se fusionan en las filas.

## Complejidad / garantías / limitaciones
- El tamaño es $O(\text{#multiplicaciones})$: óptimo en restricciones, pero el prover de Groth16 paga $O(n \log n)$ por FFTs/exponentiaciones.
- Limitaciones: requiere trusted setup por circuito (en Groth16); no soporta restricciones custom ni lookups (Plonkish los agrega); cada puerta suma "gratis" pero las no nativas (rangos, XOR) inflan el sistema.
- Garantía: R1CS-satisfacibilidad es NP-completa.

## Aplicaciones concretas
- Input de Groth16 en Zcash Sapling, Tornado Cash, circom.
- Gadgets estándar: booleanidad $b(1-b)=0$, igualdad condicional, Merkle path.

## Relación con Midnight / Compact
El compilador de Compact produce un sistema de restricciones estilo Plonkish (generalización de R1CS con restricciones de grado arbitrario y copy constraints) sobre el campo escalar de BLS12-381, que alimenta el proving system de Midnight.

## Relación con mi proyecto
La prueba de identidad del perito se codifica como: filas de hash (preimagen de la clave pública registrada), filas de booleanidad para flags de acreditación y filas de igualdad con el hash público de la evidencia — todo sin exponer $w$.

## Referencias
- Gennaro, Gentry, Parno, Raykova, *Quadratic Span Programs and Succinct NIZKs without PCPs* (2013).
- Groth, *On the Size of Pairing-Based Non-interactive Arguments* (2016).
- circom/snarkjs docs: https://docs.circom.io/
