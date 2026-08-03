# Nova: Recursive Zero-Knowledge Arguments from Folding Schemes (Kothapalli, Setty, Tzialla, 2021)

## ¿Qué problema abierto atacaba?
La composición recursiva estilo Halo verificaba (parcialmente) una prueba SNARK dentro de un circuito en cada paso: costoso y complicado. Para cómputos incrementales largos (VMs paso a paso, rollups) se necesitaba algo más barato: **no verificar nada dentro de la recursión**, solo "fusionar" instancias. El problema abierto era lograr IVC (incrementally verifiable computation) sin SNARK interno ni trusted setup.

## Idea central (analogía simple)
En vez de que cada notario revise el trabajo del anterior (caro), Nova propone **mezclar los expedientes**: dos instancias "relajadas" del mismo problema se pliegan en una sola, como sumar dos cheques pendientes en uno. Al final de toda la cadena, un único cheque (SNARK) salda la deuda acumulada.

## Innovación técnica concreta
- **Folding scheme** para R1CS relajado (relaxed R1CS): dada una curva de ciclo (pasta curves), plegar dos instancias cuesta solo una multiexponenciación — órdenes de magnitud más barato que un SNARK recursivo.
- IVC con costo por paso **constante y mínimo**, sin trusted setup (bajo discrete log).
- Finalmente se comprime la instancia plegada con un SNARK externo (p. ej. Spartan) para obtener prueba sucinta.

## Limitaciones (reconocidas o descubiertas después)
- La instancia plegada crece con el tamaño del circuito del paso; la compresión final requiere un SNARK adicional.
- Necesita ciclos de curvas (dos curvas emparejadas) o variantes posteriores.
- R1CS puro, sin compuertas custom en el paper original; extensiones (SuperNova, HyperNova) abordan pasos no uniformes y lookups.

## Impacto histórico (qué sistemas reales lo usan hoy)
Nova inauguró la línea de *folding schemes* (HyperNova, ProtoGalaxy, Nebula), adoptada experimentalmente en zkVMs y rollups. Para el caso pericial en Midnight: una cadena de custodia de evidencia es un cómputo incremental — cada custodio pliega su paso sin revelar contenidos, y un solo SNARK final certifica toda la cadena.

## Referencias
- Kothapalli, Setty, Tzialla. "Nova: Recursive Zero-Knowledge Arguments from Folding Schemes", CRYPTO 2022. ePrint: https://eprint.iacr.org/2021/370 ; DOI: 10.1007/978-3-031-15985-5_13
