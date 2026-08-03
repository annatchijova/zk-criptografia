# Pruebas recursivas (Recursive Proofs)

> Nota canónica de la carpeta `recursion/`.

## ¿Qué problema resuelve?
¿Cómo verificar *muchas* pruebas sin leerlas todas? La recursión responde: escribí el **verificador de un SNARK como circuito** y producí una nueva prueba de que "la prueba anterior verifica". Resultado: una única prueba pequeña que avala una cadena larga de cómputo. Es la técnica detrás de la agregación de pruebas, los rollups recursivos y la [[incrementally-verifiable-computation|IVC]].

## Idea intuitiva
Un notario que certifica la firma de otro notario. La prueba $\pi_{i+1}$ afirma dos cosas: (1) el paso $i+1$ se ejecutó bien y (2) existe $\pi_i$ válida que avala todo lo anterior. Al final, un solo chequeo de $\pi_n$ valida toda la historia. Como el verificador es una computación, se aritmetiza como [[arithmetic-circuits|circuito]] igual que cualquier programa.

## Definición formal
Sea $V$ el algoritmo de verificación de un SNARK $\Pi$ y $C_V$ su circuito. Un esquema recursivo produce pruebas $\pi_i$ para la relación
$$R_i = \{(z_i, (z_{i-1}, \pi_{i-1}, \omega_i)) : F(z_{i-1}, \omega_i) = z_i \;\land\; V(vk, z_{i-1}, \pi_{i-1}) = \text{acepta}\},$$
donde $F$ es la función de transición y $\omega_i$ el testigo del paso. Para que el circuito del verificador sea barato se usan **ciclos de curvas elípticas** (Pasta: Pallas/Vesta): el campo base de la curva interna es el campo escalar de la externa, así la verificación "encaja" nativamente.

## Complejidad / garantías / limitaciones
- Verificación final $O(1)$ u $O(\log n)$; el prover paga el circuito del verificador en cada paso ($10^4$–$10^6$ restricciones según el SNARK interno).
- Limitación: los pairing checks en circuito son caros; se mitiga con [[folding-schemes|folding]] (Nova) o acumulación diferida.
- Garantía: soundness se compone inductivamente; ZK se preserva si cada capa es ZK.

## Aplicaciones concretas
- Mina Protocol: blockchain de tamaño constante (~22 kB) vía SNARKs recursivos.
- Zcash Halo 2, StarkNet (SHARP agrega pruebas recursivamente), rollups recursivos.

## Relación con Midnight / Compact
Midnight usa Halo2/Plonk sobre BLS12-381, un entorno donde la recursión es estándar; una app Compact puede verificar pruebas externas como parte de su lógica privada.

## Relación con mi proyecto
Cadena de custodia pericial: cada nueva evidencia extiende la prueba anterior; el juez verifica una sola prueba final que avala toda la cadena sin revelar contenidos.

## Referencias
- Bowe, Grigg, Hopwood, *Recursive Proof Composition without a Trusted Setup* (Halo, 2019).
- Ben-Sasson et al., *Scalable, transparent, and post-quantum secure computational integrity* (2018).
- Chiesa, Tromer, *Proof-Carrying Data and Hearsay Arguments from Signature Cards* (2010).
