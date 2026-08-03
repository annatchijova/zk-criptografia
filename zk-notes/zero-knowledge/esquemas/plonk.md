# PLONK

## ¿Qué problema resuelve?
Elimina la necesidad de un trusted setup **por circuito**: un único setup universal y actualizable sirve para cualquier circuito hasta un tamaño máximo. Además introduce una aritmetización más flexible que R1CS, con custom gates y lookup arguments, reduciendo drásticamente el costo del prover en programas reales.

## Idea intuitiva
El circuito se describe con restricciones de "puertas" (q_L·a + q_R·b + q_O·c + q_M·a·b + q_C = 0) y restricciones de "cableado" (qué salida alimenta qué entrada, chequeadas con un argumento de permutación sobre polinomios). Todo se reduce a probar que ciertos polinomios son cero sobre un subgrupo multiplicativo, lo que se verifica con un **polynomial commitment** (KZG) evaluado en un punto aleatorio.

## Definición formal
*Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge*. Componentes:
- Aritmetización: gates de grado 2 (extensibles a custom gates de mayor grado) + permutación de copia.
- IOP polinomial compilado con commitments KZG (Kate–Zaverucha–Goldberg), que exigen un CRS estructurado de potencias de τ.
- Setup universal: la SRS sirve para todo circuito de tamaño ≤ n; es actualizable (cualquiera puede contribuir aleatoriedad; basta un participante honesto).

## Complejidad / garantías / limitaciones
- Prueba: ~400–600 bytes (unos 7–9 elementos de grupo); verificación: 2 pairings, O(1).
- Prover: O(n log n), típicamente 2–3× más costoso que [[groth16]] por circuito equivalente, pero la flexibilidad de custom gates suele dar circuitos más chicos.
- Limitaciones: sigue requiriendo trusted setup (aunque universal); no post-cuántico con KZG; la permutación añade polinomios y rondas.

## Aplicaciones concretas
Aztec Network, zkSync Era, Scroll, Mina (variante kimchi), Dusk Network. Es el caballo de batalla de los rollups.

## Relación con Midnight / Compact (si aplica)
Midnight adoptó la línea Plonk/[[halo2]] (con commitments tipo KZG/IPA) precisamente por el setup universal: cada contrato Compact compila a su propio circuito sin nueva ceremonia. Los lookups y custom gates son clave para las operaciones criptográficas internas.

## Relación con mi proyecto (si aplica)
El circuito del perito (verificación de firma sobre compromiso de evidencia + membresía en árbol de Merkle del registro) combina hash, curva y rangos: los custom gates y [[lookup-tables-custom-gates]] de Plonk hacen ese circuito viable donde R1CS puro explotaría en restricciones.

## Referencias
- Gabizon, Williamson, Ciobotaru (2019). *PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge*. IACR ePrint 2019/953.
- Kate, Zaverucha, Goldberg (2010). *Constant-Size Commitments to Polynomials and Their Applications*. ASIACRYPT.
