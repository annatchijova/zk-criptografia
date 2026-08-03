# Nova

## ¿Qué problema resuelve?
Hace **incremental verifiable computation (IVC)** práctica: probar una computación de N pasos pagando solo el costo de un paso por iteración, sin SNARKs recursivos costosos. Ideal para máquinas de estado largas: VMs, cadenas de custodia, ejecuciones paso a paso.

## Idea intuitiva
La idea del **folding**: en lugar de verificar cada instancia R1CS (como en la recursión clásica de [[halo]], que mete un verificador completo dentro del circuito), el prover *pliega* dos instancias R1CS en una sola "relajada" (Relaxed R1CS) del mismo tamaño, usando un commitment homomórfico y un desafío aleatorio. Al final, una única instancia plegada resume toda la computación, y se cierra con un SNARK ([[spartan]]) una sola vez.

## Definición formal
Esquema de folding para NP: existe una función que toma dos instancias (U₁, w₁), (U₂, w₂) de Relaxed R1CS — con término de error E y escalar u — y produce (U, w) tal que el par plegado es satisfacible iff ambos originales lo son (completitud y knowledge soundness). El paso de IVC ejecuta la función F (un paso de cómputo) más el folding del paso previo, sobre un ciclo de curvas (Pasta, o BN254/Grumpkin).

## Complejidad / garantías / limitaciones
- Circuito de folding: ~10–20 mil restricciones, **independiente del tamaño del paso** verificado — órdenes de magnitud más barato que recursión SNARK-in-SNARK.
- Prover por paso: una multiexponenciación + el paso F; la prueba final (compresión) se paga una vez.
- Sin trusted setup (con commitments de Pedersen/IPA); requiere ciclo de curvas.
- Limitaciones: no da zero-knowledge "gratis" (se agrega con blinding); la IVC es secuencial por naturaleza (SuperNova, HyperNova y ProtoStar generalizan a pasos no uniformes y paralelización).

## Aplicaciones concretas
Pruebas de VMs completas (zkVMs folding-based), verificación de ejecución de TLs/rollup light clients, cómputo incremental sobre streams largos, y proyectos como Sonobe (arkworks) y Nova-Scotia (folding para circom).

## Relación con Midnight / Compact (si aplica)
Midnight usa recursión estilo Halo2; Nova es la alternativa de vanguardia cuando el cómputo es inherentemente secuencial y largo. Ambos comparten la filosofía de amortización sobre ciclos de curvas sin pairing.

## Relación con mi proyecto (si aplica)
La **cadena de custodia** de la evidencia es una máquina de estados: cada acta de transferencia es un paso F. Con Nova se prueba la integridad de toda la cadena con costo constante por eslabón, sin revelar contenidos — argumento elegante frente al estándar Daubert de "metodología con tasa de error conocida".

## Referencias
- Kothapalli, Setty, Tzialla (2021). *Nova: Recursive Zero-Knowledge Arguments from Folding Schemes*. CRYPTO 2022; IACR ePrint 2021/370.
- Kothapalli & Setty (2023). *HyperNova: Recursive arguments for customizable constraint systems*. CRYPTO 2024.
