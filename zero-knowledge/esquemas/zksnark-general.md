# zkSNARK general

## ¿Qué problema resuelve?
Permite probar que una computación se ejecutó correctamente (o que el prover conoce un testigo) sin revelar ese testigo, con una prueba corta y verificación rápida. Resuelve el dilema "confiá en mí" vs "revelá todos tus datos": el verificador gana certeza criptográfica sin acceder a la información privada.

## Idea intuitiva
Un SNARK convierte un programa en un circuito aritmético y el testigo en un polinomio. El prover demuestra que ciertas ecuaciones polinómicas se cumplen "en un punto aleatorio oculto". Verificar equivale a chequear pocas igualdades algebraicas en vez de re-ejecutar el programa completo.

## Definición formal
zk-SNARK = *Zero-Knowledge Succinct Non-Interactive Argument of Knowledge*:
- **Zero-knowledge**: la prueba no revela nada del testigo más allá de la verdad del enunciado.
- **Succinct**: tamaño de prueba y tiempo de verificación sublineales (idealmente constantes o logarítmicos) en el tamaño del circuito.
- **Non-interactive**: un solo mensaje del prover al verificador (vía CRS o Fiat–Shamir).
- **Argument of knowledge**: seguridad computacional; existe un extractor que obtiene el testigo de cualquier prover exitoso.

Formalmente es un sistema para relaciones NP con algoritmos Setup, Prove, Verify y propiedades de completitud, knowledge soundness y zero-knowledge (existencia de un simulador).

## Complejidad / garantías / limitaciones
Verificación típicamente O(1) u O(log n); pruebas de cientos de bytes. Costos: el prover es caro (tiempo y memoria superlineales en n), y muchos esquemas requieren *trusted setup* ([[groth16]], [[plonk]]) o sacrifican sucintez ([[bulletproofs]], [[spartan]]). Seguridad basada en supuestos como knowledge-of-exponent o logaritmo discreto en curvas elípticas; la mayoría no es post-cuántica (contrastar con [[zkstark]]).

## Aplicaciones concretas
Pagos privados (Zcash), rollups de validez (zkSync, Polygon zkEVM), identidad auto-soberana, cumplimiento regulatorio sin divulgación.

## Relación con Midnight / Compact (si aplica)
Midnight ejecuta contratos inteligentes con datos privados mediante pruebas ZK: el estado privado nunca sale del dispositivo del usuario y solo se publica la prueba verificable on-chain, compilada desde el lenguaje Compact.

## Relación con mi proyecto (si aplica)
Un perito puede probar "mi credencial está vigente y poseo evidencia válida comprometida" sin revelar la evidencia ni la identidad de las víctimas — exactamente el patrón de membresía + posesión que un SNARK captura.

## Referencias
- Goldwasser, Micali, Rackoff (1985). *The Knowledge Complexity of Interactive Proof Systems*. STOC.
- Groth (2016). *On the Size of Pairing-based Non-interactive Arguments*. EUROCRYPT.
- Bitansky et al. (2012). *From extractable collision resistance to succinct non-interactive arguments of knowledge*. ITCS.
