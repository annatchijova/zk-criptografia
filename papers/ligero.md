# Ligero: Lightweight Sublinear Arguments Without a Trusted Setup (Ames, Hazay, Ishai, Venkitasubramaniam, 2017)

## ¿Qué problema abierto atacaba?
Los sistemas de argumentos de conocimiento cero con verificación sublineal solían depender de un *trusted setup* (como los zk-SNARKs basados en emparejamientos) o de supuestos costosos. El reto era lograr un protocolo eficiente, con comunicación sublineal en el tamaño del circuito, sin ceremonia de configuración confiable y con supuestos criptográficos mínimos.

## Idea central (analogía simple)
Es como presentar ante un juez una hoja enorme de cálculos (el "testigo") sellada con una faja de seguridad (un commitment), y que el juez solo revise unas pocas celdas elegidas al azar: si el prover hizo trampa, casi seguro la trampa aparece en alguna celda inspeccionada. La inspección local garantiza la validez global.

## Innovación técnica concreta
Combina el framework "MPC-in-the-head" (IKOS) con códigos correctores de errores (Reed–Solomon) y commitments basados en Merkle trees. El prover simula una ejecución de MPC entre partes virtuales, codifica las vistas con un código lineal y se compromete con ellas; el verificador hace consultas locales aleatorias. Se obtiene comunicación O(√N) para circuitos de tamaño N, seguridad solo desde funciones hash (modelo ROM), sin trusted setup y potencialmente post-cuántica. Es la semilla directa de los "Ligero-style proofs" y de Aurora.

## Limitaciones (reconocidas o descubiertas después)
La comunicación y el tiempo del verificador son O(√N), no puramente polilogarítmicos; el costo del prover crece con la dimensión de la matriz codificada. Las pruebas son interactivas en el modelo original (se hacen no interactivas vía Fiat–Shamir). El tamaño de prueba es mayor que en SNARKs de emparejamiento.

## Impacto histórico (qué sistemas reales lo usan hoy)
Inspiró Aurora, Ligero++, Virgo y la familia de protocolos con hashing y códigos (precursores conceptuales de STARKs y de esquemas usados en sistemas transparentes actuales). Su técnica de verificación local sobre testigos codificados es estándar en IOPs modernos, relevantes para cadenas ZK como Midnight.

## Referencias
- Ames, Hazay, Ishai, Venkitasubramaniam. "Ligero: Lightweight Sublinear Arguments Without a Trusted Setup". CCS 2017. https://eprint.iacr.org/2017/1132
- Ben-Sasson et al., "Aurora" (EUROCRYPT 2019), https://eprint.iacr.org/2018/828
