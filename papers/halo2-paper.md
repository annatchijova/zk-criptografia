# Halo2: sistema de pruebas PLONKish sin trusted setup (Electric Coin Company / Zcash, 2020–presente)

## ¿Qué problema abierto atacaba?
Halo (2019) demostró recursión sin trusted setup, pero con una aritmetización rígida tipo R1CS y sin tooling de producción. Zcash necesitaba un sistema para su protocolo Orchard que fuera: sin ceremonia de confianza (riesgo político y de seguridad), eficiente para notas de pago, y expresivo para circuitos criptográficos complejos. No hay "paper único" de Halo2: el sistema vive en su código, su book y sus posts de diseño.

## Idea central (analogía simple)
Halo2 es como un motor de auto: combina el chasis de PLONK (compuertas custom, copy constraints por permutación, lookups) con la transmisión de Halo (accumulation sin setup, curvas pasta). El resultado es un motor que arranca sin que nadie tenga que confiar en el fabricante.

## Innovación técnica concreta
- **Aritmetización PLONKish** sobre el polynomial commitment de Halo (inner product argument): sin trusted setup, con curvas Pallas/Vesta.
- **Lookups** (tablas de búsqueda) integrados en circuito, habilitando rangos y bitwise baratos.
- Gadgets de alto nivel: Poseidon, SHA-256 (via lookups), curvas elípticas, sin; además de recursión/succinctness vía accumulation.
- Book oficial y crates en Rust (`halo2_proofs`, `halo2curves`) como "especificación ejecutable".

## Limitaciones (reconocidas o descubiertas después)
- Verificación más costosa y pruebas más grandes que PLONK-KZG (por el IPA logarítmico).
- Curva de aprendizaje empinada: chips, regions, layouters; errores de "underconstrained circuits" documentados en auditorías (zkSecurity).
- Variantes divergentes (PSE fork, Axiom, Scroll) fragmentan el ecosistema.

## Impacto histórico (qué sistemas reales lo usan hoy)
**Zcash Orchard** (mainnet desde 2022), **Scroll** y Axiom usan forks de Halo2. **Midnight** se apoya en la línea PLONKish/Halo2 (producción de pruebas del lado del cliente, sin setup por circuito), lo que lo hace apto para el caso pericial: probar credencial y posesión de evidencia localmente, revelando solo hashes comprometidos on-chain.

## Referencias
- The Halo2 Book: https://zcash.github.io/halo2/ ; código: https://github.com/zcash/halo2
- Bowe, Grigg, Hopwood. Halo (base teórica): https://eprint.iacr.org/2019/1021
- "Halo 2: recursion without trusted setup", ECC blog: https://electriccoin.co/blog/
