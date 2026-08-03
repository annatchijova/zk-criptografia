# Ceremonias de Trusted Setup

## ¿Qué problema resuelve?

SNARKs como Groth16 (y la fase universal de Plonk/Marlin) requieren una **cadena de referencia estructurada (SRS)**: parámetros públicos generados con aleatoriedad secreta. Si quien generó los parámetros conserva esa aleatoriedad — el llamado *toxic waste* — puede **falsificar pruebas** (en una blockchain: acuñar fondos invisibles). La ceremonia distribuye la generación entre decenas o cientos de participantes mediante [[mpc|MPC]], de modo que basta que **uno solo** sea honesto y destruya su secreto.

## Idea intuitiva

Cada participante recibe los parámetros acumulados, los "mezcla" con su propia aleatoriedad secreta ($\tau_i$), publica el resultado y una prueba de correcta actualización, y destruye su $\tau_i$. El secreto compuesto es $\tau = \prod \tau_i$: irrecuperable salvo que **todos** los participantes colusionen y conserven sus fragmentos.

## Definición formal

- **Powers of Tau (Zcash, 2017–2018)**: genera $[\tau^i]_1, [\tau^i]_2$ para $i$ hasta $2^{21}$. ~87 contribuyentes; cada uno publica un *attestation* firmado y pruebas de conocimiento del escalar aplicado (esquema de Bowe–Gabizon–Micali, "scalable multi-party computation for zk-SNARK parameters").
- **Sapling MPC**: segunda fase específica por circuito.
- Las contribuciones deben ser **verificables**: cualquiera comprueba que cada paso actualizó correctamente la SRS anterior, aunque no puede comprobar la destrucción del secreto (de ahí el modelo 1-de-N).

## Complejidad / garantías / limitaciones

- Garantía: soundness si ≥1 participante honesto destruyó su fragmento — supuesto extraordinariamente débil, casi "sin confianza".
- Limitaciones: la destrucción del toxic waste no es auditable; contribuciones con RNG débil o malware comprometen la contribución pero **no** la seguridad global si hay otro honesto; coordinación costosa (meses); fases por circuito (Groth16) vs. universales (Powers of Tau, reutilizable).
- Mitigaciones: hardware diverso, fuentes de aleatoriedad públicas (hashes de bloques, loterías), attestations públicos, contribución vía air-gap, y retransmisión en streaming.

## Aplicaciones concretas

Zcash (Powers of Tau, Sapling), Semaphore/Tornado Cash (ceremonias comunitarias), Perpetual Powers of Tau, Hermez, Aztec Ignition (176 participantes), Filecoin, la ceremonia de Ethereum KZG (2023, >140 000 contribuyentes para EIP-4844).

## Relación con Midnight / Compact (si aplica)

Si el circuito del proyecto usa un SNARK con setup (Groth16/Plonk sobre la pila de Midnight), la credibilidad del sistema hereda la de la ceremonia que generó la SRS; conviene preferir SRS universales ya ceremonializadas o sistemas transparentes (STARK/Halo2 sin setup).

## Relación con mi proyecto (si aplica)

Un juez que evalúa fiabilidad técnica (testigo experto, estándar Daubert) puede auditar la transcripción pública de la ceremonia como evidencia de que nadie puede falsificar las pruebas de los peritos.

## Referencias

- Bowe, Gabizon, Micali, "Scalable Multi-party Computation for zk-SNARK Parameters in the Random Beacon Model", ePrint 2017/1050.
- Zcash, "The Design of the Ceremony", blog.z.cash, 2016; parámetros y transcripción en github.com/ZcashFoundation/powersoftau.
- Ethereum KZG Ceremony, ceremony.ethereum.org, 2023.
