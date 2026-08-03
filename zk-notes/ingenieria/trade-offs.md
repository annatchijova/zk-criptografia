# Trade-offs: SNARK vs STARK vs Bulletproofs vs Nova

## ¿Qué problema resuelve?
Elegir el esquema de prueba es la decisión arquitectónica central del proyecto: determina setup, tamaño de prueba, costo de verificación on-chain y supuestos de seguridad. Una mala elección puede hacer el peritaje impracticable o insostenible ante una contrapericia.

## Idea intuitiva
No hay "el mejor esquema": hay un tetraedro de compromisos entre confianza en el setup, tamaño de prueba, velocidad del prover y resistencia post-cuántica. La decisión se toma ponderando los requisitos concretos.

## Definición formal

| Propiedad | Groth16 (SNARK) | PLONK/Halo2 | STARK | Bulletproofs | Nova (folding) |
|---|---|---|---|---|---|
| Setup | trusted, por circuito | trusted universal / transparente (KZG vs IPA) | transparente | transparente | transparente |
| Tamaño prueba | ~192 B, constante | ~0.5–1 kB | ~50–200 kB | ~1–2 kB (log) | compacto tras compresión |
| Verificación | O(1), ~ms | O(1) | O(log² n) | O(n) | recursiva, incremental |
| Prover | rápido | rápido | más lento, sin curvas | lento en n grande | óptimo para pasos repetidos |
| Post-cuántico | no | no (KZG) | sí (hash-based) | no | no |
| Supuestos | pairings, q-PKE | pairings o IPA | solo hashes | log discreto | log discreto |

## Complejidad / garantías / limitaciones
Groth16 minimiza verificación pero exige ceremonia por circuito: difícil de justificar ante un tribunal. STARKs eliminan el setup y dan transparencia total, pero con pruebas grandes y fees altos. Bulletproofs sirven para statements pequeños (rangos). Nova/IVC brilla en cómputo incremental repetido (cadenas de custodia paso a paso).

## Aplicaciones concretas
- Cadena de custodia de evidencia como cómputo incremental → Nova.
- Verificación on-chain barata de identidad pericial → SNARK tipo PLONK con setup universal.
- Máxima auditabilidad sin confianza en terceros → STARK.

## Relación con Midnight / Compact (si aplica)
Midnight adopta un backend SNARK (línea PLONK/Halo2, con recursión) integrado en Compact: la decisión queda acotada por la plataforma, con setup estándar de la red, verificación nativa on-chain y costos de [[complejidad]] y [[benchmarking]] medibles en testnet.

## Relación con mi proyecto (si aplica)
Decisión: usar el SNARK nativo de Midnight. Razones: (1) verificación on-chain económica y [[reproducibilidad|reproducible]] por cualquier nodo; (2) sin ceremonia propia que defender ante el tribunal; (3) tooling maduro. Se acepta la no post-cuánticidad como riesgo declarado y mitigable por rotación de esquema.

## Referencias
- Groth, EUROCRYPT 2016 (Groth16).
- Gabizon, Williamson, Ciobotaru, "PLONK", 2019 (eprint 2019/953).
- Ben-Sasson et al., "STARKs", 2018 (eprint 2018/046).
- Bünz et al., "Bulletproofs", IEEE S&P 2018.
- Kothapalli, Setty, Tzialla, "Nova", 2021 (eprint 2021/370).
