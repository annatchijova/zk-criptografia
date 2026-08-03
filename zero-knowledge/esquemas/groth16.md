# Groth16

## ¿Qué problema resuelve?
Es el SNARK con **la prueba más pequeña y la verificación más barata** conocidos en la familia de pairings: 3 elementos de grupo (~128–192 bytes) y una sola ecuación de pairing. Cuando el costo on-chain domina (gas, fees), Groth16 es difícil de superar.

## Idea intuitiva
El circuito se expresa como R1CS (Rank-1 Constraint System): cada restricción es ⟨a,w⟩·⟨b,w⟩ = ⟨c,w⟩. Se codifica en un QAP (Quadratic Arithmetic Program): existe un polinomio h tal que A(x)·B(x) − C(x) = h(x)·Z(x). El prover evalúa los polinomios "en el exponente" en un punto secreto τ generado en el setup, y el verificador chequea la identidad con un pairing e(A,B) = e(C,D).

## Definición formal
SNARK concretamente eficiente para R1CS sobre curvas con pairing (BN254, BLS12-381). Setup genera una CRS que depende del circuito: potencias de τ y cocientes polinómicos "encriptados". Prove produce π = (A, B, C); Verify evalúa una única ecuación de pairing que incluye los inputs públicos vía un término lineal.

## Complejidad / garantías / limitaciones
- Prueba: 3 elementos, tamaño constante — el mínimo demostrado para pruebas de un solo mensaje con verificación por pairing.
- Verificación: O(1) en el tamaño del circuito (lineal solo en inputs públicos); ~2–3 ms.
- Prover: O(n log n) por FFTs + multiexponenciaciones; costoso pero maduro (bellman, gnark, circom/snarkjs).
- **Limitación crítica**: trusted setup **por circuito** — cambiar el circuito obliga a una nueva ceremonia, y el "toxic waste" τ debe destruirse (se mitiga con MPC como Powers of Tau).
- No post-cuántico (pairings).

## Aplicaciones concretas
Zcash Sapling (originalmente), Tornado Cash, rollup Loopring, verificación de pruebas en contratos Ethereum con precompilados BN254.

## Relación con Midnight / Compact (si aplica)
Midnight prefirió esquemas de setup universal/actualizable ([[plonk]], [[halo2]]) precisamente porque los contratos Compact cambian con cada deployment; un setup por circuito sería operacionalmente inviable a escala de ecosistema.

## Relación con mi proyecto (si aplica)
Si el circuito pericial es fijo y auditado una sola vez (membresía en registro de peritos + posesión de evidencia comprometida), Groth16 minimiza el costo de verificación on-chain — ideal si el presupuesto del proyecto se mide en fees.

## Referencias
- Groth (2016). *On the Size of Pairing-based Non-interactive Arguments*. EUROCRYPT 2016.
- Bowe, Gabizon, Miers (2017). *Scalable Multi-party Computation for zk-SNARK Parameters in the Random Beacon Model* (Powers of Tau).
