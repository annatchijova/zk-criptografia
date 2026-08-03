# zkSTARK

## ¿Qué problema resuelve?
Ofrece pruebas de integridad computacional escalables **sin trusted setup** y con plausibilidad post-cuántica, eliminando la necesidad de confiar en una ceremonia de generación de parámetros (el punto débil de [[groth16]] y [[plonk]]).

## Idea intuitiva
En lugar de curvas elípticas y pairings, un STARK codifica la traza de ejecución como un polinomio de bajo grado sobre un campo finito y usa **FRI** (Fast Reed–Solomon IOP of Proximity) para probar, por muestreo aleatorio, que ese polinomio efectivamente tiene grado bajo. La aleatoriedad del verificador se deriva de hashes (Fiat–Shamir), logrando no-interactividad y transparencia: toda la seguridad descansa en funciones hash resistentes a colisiones.

## Definición formal
STARK = *Scalable Transparent ARgument of Knowledge*. Es un IOP (Interactive Oracle Proof) compilado con árboles de Merkle: el prover se compromete a oráculos (evaluaciones de polinomios Reed–Solomon) y el verificador consulta pocas posiciones. Las restricciones se expresan como AIR (Algebraic Intermediate Representation): cada transición de estado de la máquina virtual es un polinomio de restricción sobre filas consecutivas de la traza.

## Complejidad / garantías / limitaciones
- Prover: O(n log n), con buenas constantes prácticas y alta paralelización.
- Verificador y tamaño: poli-logarítmicos O(log² n), pero las **pruebas son grandes**: típicamente 50–200 KB, uno o dos órdenes de magnitud más que un SNARK de pairings.
- Garantías: transparencia total, resistencia post-cuántica plausible, soundness estadístico ajustable por número de queries.
- Limitaciones: verificación on-chain cara en gas; consumo de RAM del prover elevado.

## Aplicaciones concretas
StarkNet/StarkEx (rollups de validez sobre Ethereum), Polygon Miden, pruebas de ejecución de VMs completas (Cairo).

## Relación con Midnight / Compact (si aplica)
Midnight prioriza pruebas de tamaño constante para minimizar costos on-chain en Cardano; un STARK sería caro en fees. Es, sin embargo, la referencia natural si el threat model exige post-cuántica.

## Relación con mi proyecto (si aplica)
Si la evidencia pericial debe conservar valor probatorio a 20+ años frente al riesgo "store now, decrypt later", la post-cuántica de los STARK es un argumento de peso, al costo de pruebas más pesadas y verificación más cara.

## Referencias
- Ben-Sasson, Bentov, Horesh, Riabzev (2018). *Scalable, transparent, and post-quantum secure computational integrity*. IACR ePrint 2018/046.
- Ben-Sasson et al. (2018). *Fast Reed–Solomon Interactive Oracle Proofs of Proximity (FRI)*. ICALP 2018.
- Ben-Sasson et al. (2019). *Aurora: Transparent Succinct Arguments for R1CS*. EUROCRYPT.
