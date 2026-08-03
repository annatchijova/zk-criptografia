# Circuito booleano

## ¿Qué problema resuelve?
No toda computación se expresa naturalmente como polinomios: hashes clásicos (SHA-256), comparaciones y lógica bit a bit son binarias. El circuito booleano (puertas AND, OR, NOT) es el modelo clásico de computación y la base de los primeros sistemas ZK generales (GMW, garbled circuits). Saber cuándo usarlo frente al [[arithmetic-circuits|circuito aritmético]] evita explosiones de costo.

## Idea intuitiva
Un circuito booleano es un plano de compuertas lógicas como el de un procesador: cada puerta toma dos bits y produce uno. Se puede probar conocimiento de una entrada que hace que el circuito dé 1 (por ejemplo, una preimagen de hash) sin revelar la entrada. Los primeros protocolos ZK prueban la satisfacibilidad puerta por puerta con compromisos de bits.

## Definición formal
Un circuito booleano es un DAG con puertas de un conjunto universal (p. ej. $\{\text{AND}, \text{XOR}, \text{NOT}\}$) sobre $\{0,1\}$. La relación asociada es $R = \{(x, w) : C(x, w) = 1\}$, la clase NP. Todo circuito booleano se puede simular en un campo $\mathbb{F}_p$ codificando cada bit $b$ con la restricción de booleanidad $b(b-1) = 0$ en [[r1cs]], pero un AND de bits cuesta una multiplicación completa: un solo SHA-256 cuesta del orden de $10^4$–$10^5$ restricciones.

## Complejidad / garantías / limitaciones
- Tamaño $O(s)$ para cualquier cálculo binario de $s$ pasos; universal y simple.
- Limitación decisiva: embebido en campos grandes es ineficiente; conviene usar hashes "ZK-friendly" (Poseidon, Rescue) aritméticos en vez de SHA-256.
- Ventaja: base natural de MPC (garbled circuits) y de pruebas estilo GMW.

## Aplicaciones concretas
- Protocolo de Goldreich–Micali–Wigderson (1986) para 3-coloración de grafos.
- Garbled circuits (Yao) para cómputo seguro de dos partes.
- Gadgets de comparación y rangos dentro de circuitos aritméticos.

## Relación con Midnight / Compact
En Compact se evita la lógica booleana pesada: se usan tipos y hashes nativos del campo; cuando hace falta lógica de bits, el compilador genera los gadgets de descomposición binaria.

## Relación con mi proyecto
Para la evidencia de víctimas: si el hash de la evidencia se calcula on-chain conviene Poseidon (aritmético); si llega como SHA-256 externo, hay que pagar el gadget booleano de verificación de preimagen.

## Referencias
- Goldreich, Micali, Wigderson, *Proofs that Yield Nothing but Their Validity* (1986).
- Yao, *How to Generate and Exchange Secrets* (1986).
- Grassi et al., *Poseidon: A New Hash Function for Zero-Knowledge Proof Systems* (2019).
