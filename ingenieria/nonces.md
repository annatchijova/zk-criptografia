# Nonces

## ¿Qué problema resuelve?
Muchos protocolos criptográficos necesitan valores que no se repitan (number used once) para garantizar frescura, unicidad o aleatoriedad. Sin nonces, esquemas que son seguros "en teoría" colapsan: firmas repetidas, mensajes reencriptados idénticos, replays de pruebas.

## Idea intuitiva
Un nonce es como el número de serie único de un formulario sellado: aunque dos personas llenen el mismo formulario con idénticos datos, cada ejemplar es distinguible e irrepetible.

## Definición formal
Un nonce es un valor que se usa a lo sumo una vez dentro de un contexto. Roles típicos:
- **Frescura anti-replay**: en protocolos de autenticación, el verificador emite un nonce que el prover debe incorporar, probando que la respuesta es reciente.
- **IV en cifrado**: en AEAD (AES-GCM, ChaCha20-Poly1305), reusar (clave, nonce) es catastrófico: rompe confidencialidad y, en GCM, la autenticidad.
- **Aleatoriedad en commitments**: `Com(m, r) = H(m || r)`; el randomizer r (nonce) aporta la propiedad *hiding* y evita ataques de diccionario sobre mensajes predecibles.
- **Blinding en pruebas ZK**: los SNARKs incluyen aleatoriedad del prover para la propiedad zero-knowledge; si esa aleatoriedad se repite o es predecible, se filtra el witness.
- **Firmas**: ECDSA/EdDSA derivan un nonce k por firma; su reuso revela la clave privada (caso Sony PS3).

## Complejidad / garantías / limitaciones
Generar nonces es barato (contador o 128+ bits aleatorios; por cumpleaños, la colisión requiere ~2⁶⁴ valores con 128 bits). La limitación no es el costo sino la disciplina: mezclar dominios, RNG débiles o contadores reiniciados tras un fallo destruyen las garantías.

## Aplicaciones concretas
- Nullifiers: `H(clave_secreta, contexto)` como nonce determinista que evita el doble uso de una credencial.
- Anti-replay: el tribunal emite un challenge (nonce) ligado al statement público.
- Randomizers únicos por commitment de cada pieza de evidencia.

## Relación con Midnight / Compact (si aplica)
En Midnight los nullifiers y commitments con randomizer son primitivas del modelo de datos privados; el contrato Compact gestiona el set de nullifiers gastados on-chain para impedir reuso.

## Relación con mi proyecto (si aplica)
La posesión de evidencia se compromete como `H(evidence || r)` con r aleatorio guardado por el perito; la prueba incluye un challenge judicial como nonce anti-replay, de modo que una prueba vieja no pueda presentarse en otro caso. Ver [[trade-offs]] para el esquema que soporta estas primitivas.

## Referencias
- Rogaway, "Nonce-Based Symmetric Encryption", FSE 2004.
- NIST SP 800-90A (generadores de números aleatorios).
- Breitner & Heninger, "Biased Nonce Sense" (análisis de fallos de nonces en ECDSA), IEEE S&P 2019.
