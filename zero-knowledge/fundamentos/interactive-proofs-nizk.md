# Pruebas interactivas vs NIZK: qué gana y qué pierde Midnight

## ¿Qué problema resuelve?
El modelo original de conocimiento cero ([[historia-zk|GMR85]]) era un diálogo: prover y verificador intercambian mensajes. En una blockchain no hay diálogo posible: la prueba debe viajar en una transacción y ser verificable por cualquiera, para siempre.

## Idea intuitiva
Interactivo = interrogatorio en vivo, donde el verificador elige desafíos frescos. No interactivo = affidavit autosuficiente: el prover publica una cadena única que cualquiera verifica después. La magia es que el desafío del verificador se sustituya por una función de hash (Fiat–Shamir) o por una cadena de referencia común (CRS).

## Definición formal
- **Prueba interactiva (IP)**: tupla (P, V) con rondas de mensajes; V acepta o rechaza al final. Garantías: [[completeness]] y [[soundness]] (posiblemente estadística, contra provers de poder ilimitado).
- **NIZK**: en el modelo CRS, hay un algoritmo Setup → crs, un prover P(crs, x, w) → π y un verificador V(crs, x, π) → {0,1}. Se exigen completitud, soundness (computacional) y ZK (simulador con trapdoor de la crs, ver [[simulation]]).
- **Fiat–Shamir**: transforma un Σ-protocolo HVZK público-moneda en NIZK reemplazando el desafío e por H(transcript) en el modelo de oráculo aleatorio (ver [[honest-verifier-zk]]).

## Complejidad / garantías / limitaciones
**Lo que se gana con NIZK**: verificación offline y pública, reusabilidad (una prueba sirve para todos), composición sencilla en sistemas sin interacción. **Lo que se pierde**: la soundness deja de ser estadística —pasa a ser computacional ([[soundness]])—; se requiere CRS (confianza en el setup, mitigable con setups universales/actualizables tipo PLONK) o el modelo de oráculo aleatorio (heurística no instanciable en general: contraejemplos de Canetti–Goldreich–Halevi).

## Aplicaciones concretas
Firmas digitales (NIZK de identidad), Zcash/Monero, rollups, votación electrónica verificable.

## Relación con Midnight / Compact (si aplica)
Midnight es inherentemente NIZK: las transacciones Compact portan pruebas sucintas verificables por los nodos sin interactuar con el usuario. Gana privacidad pública auditable; asume supuestos computacionales y la robustez del setup/oráculo aleatorio del sistema subyacente.

## Relación con mi proyecto (si aplica)
Para un contexto judicial (Daubert), la no interactividad es esencial: la prueba del perito queda fijada on-chain y cualquier parte —juez, contraparte— la verifica años después sin contactar al prover ni a las víctimas. Ver [[witness-statement]].

## Referencias
- Blum, Feldman, Micali, "Non-Interactive Zero-Knowledge and Its Applications", STOC 1988.
- Fiat, Shamir, "How to Prove Yourself", CRYPTO 1986.
- Canetti, Goldreich, Halevi, "The Random Oracle Methodology, Revisited", JACM 51(4), 2004.
- Goldwasser, Micali, Rackoff, SIAM J. Comput. 18(1), 1989.
