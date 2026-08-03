# Halo2

## ¿Qué problema resuelve?
Es la ingeniería de producción de la idea de [[halo]]: un sistema de pruebas **sin trusted setup**, con aritmetización tipo [[plonk]] (custom gates, lookups) y recursión práctica. Resuelve el trilema "transparencia + flexibilidad + pruebas cortas" que frenaba la adopción industrial.

## Idea intuitiva
Halo2 combina dos piezas: (1) el *frontend* PLONKish — una tabla de celdas con selectores, gates definidos por el usuario, argumentos de permutación y de lookup — y (2) un *backend* de polynomial commitment por producto interno (IPA) sobre curvas sin pairing, heredado de Halo, que evita el setup tóxico. El resultado: cualquiera puede generar parámetros, y los circuitos se diseñan como "chips" reutilizables con constraints declarativos.

## Definición formal
Implementación (Electric Coin Co.) de un IOP polinomial PLONKish compilado con IPA sobre el ciclo Pasta (Pallas para el circuito, Vesta para la recursión). Soporta: columnas de advice/instance/fixed, custom gates de grado arbitrario, permutation argument, y lookup arguments estilo plookup. Existe también `halo2-kzg` (Variante con KZG sobre BN254/BLS12-381, usada por Axiom y Scroll) que recupera pruebas cortas a costa de setup universal.

## Complejidad / garantías / limitaciones
- Sin trusted setup (versión IPA); pruebas de pocos KB, verificación O(log n) con recursión amortizada.
- Prover: costoso (dominado por multiexponenciaciones y FFTs); la comunidad aporta aceleración GPU/MSM.
- Limitaciones: verificación on-chain más cara que KZG/Groth16; tooling más exigente que circom (Rust, diseño manual de chips y regiones).

## Aplicaciones concretas
**Zcash Orchard** (en mainnet desde 2022, pool blindado sin trusted setup — el motivo histórico del proyecto), Scroll y Axiom (vía halo2-kzg), Taiko, y el stack de pruebas de Midnight.

## Relación con Midnight / Compact (si aplica)
Midnight adoptó la línea Halo2/Plonkish como motor ZK: los contratos Compact compilan a circuitos PLONKish y la ausencia de setup por circuito permite desplegar contratos arbitrarios. Además la transparencia simplifica la narrativa de cumplimiento (nada que auditar en una ceremonia).

## Relación con mi proyecto (si aplica)
La metáfora de "chips" encaja con el caso pericial: un chip de verificación de firma sobre la evidencia, un chip de membresía Merkle del registro de peritos, un chip de lookup para rangos temporales — componibles en un solo circuito probatorio conforme a Daubert (metodología testeable y revisada por pares).

## Referencias
- Electric Coin Co. *halo2* book y repositorio: github.com/zcash/halo2.
- Hopwood et al. (2021). *Zcash Orchard specification* (NU5).
- Gabizon & Williamson (2020). *plookup: A simplified polynomial protocol for lookup tables*. IACR ePrint 2020/315.
