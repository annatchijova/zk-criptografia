# PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge (Gabizon, Williamson, Ciobotaru, 2019)

## ¿Qué problema abierto atacaba?
Los SNARKs universales existentes (Sonic, Marlin) eran poco ergonómicos: los sistemas de restricciones eran rígidos, las pruebas medianas y el costo del probador alto. Se buscaba un esquema con **setup universal y actualizable**, probador eficiente y un "lenguaje de circuitos" flexible que admitiera compuertas personalizadas (crucial para aritmetizar hashes y curvas).

## Idea central (analogía simple)
Un circuito es como una hoja de cálculo donde además de verificar cada celda (compuertas) hay que verificar que los cables conectan las celdas correctas (copy constraints). PLONK comprime ambas verificaciones en un truco: una **permutación** de polinomios. Probar que el cableado es correcto equivale a probar que cierto polinomio es una permutación de otro, y eso se chequea con un "gran producto" verificable con commitments polinomiales.

## Innovación técnica concreta
- Setup **universal y actualizable** (una sola ceremonia para todos los circuitos hasta un tamaño tope).
- Sistema aritmético con **compuertas personalizadas** y *selector polynomials*: suma, multiplicación y operaciones ad-hoc en un mismo marco.
- Copy constraints vía argumento de permutación, estándar de facto hoy.
- Compatible con KZG (pairings) y adaptable a FRI/commitments sin pairing.

## Limitaciones (reconocidas o descubiertas después)
- Con KZG sigue requiriendo un SRS estructurado (aunque universal).
- Los lookups (tablas) no venían incluidos; aparecieron después (plookup, logUp).
- El grado de los polinomios crece con compuertas custom, encareciendo el probador.

## Impacto histórico (qué sistemas reales lo usan hoy)
PLONK es hoy el estándar industrial: **Aztec, Scroll, zkSync (Boojum), Mina (variante Kimchi), Dusk** y la propia familia **Halo2 de Zcash** (PLONKish arithmetization) que inspira a **Midnight/Compact**. Para evidencia pericial: compuertas custom permiten aritmetizar eficientemente la firma de la credencial del perito y el hash de la evidencia.

## Referencias
- Gabizon, Williamson, Ciobotaru. "PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge", 2019. ePrint: https://eprint.iacr.org/2019/953
