# Solidez (soundness): computacional vs estadística

## ¿Qué problema resuelve?
Es la garantía antifraude: un prover que **no** conoce un witness válido no puede convencer al verificador salvo con probabilidad despreciable. Sin soundness, la prueba no demuestra nada.

## Idea intuitiva
Si el enunciado es falso —el "perito" no tiene credencial o la evidencia no corresponde— cualquier estrategia de trampa es detectada con alta probabilidad. La pregunta clave es *contra quién* vale esa garantía: contra cualquier adversario (incluso con poder infinito) o solo contra adversarios eficientes.

## Definición formal
Para una [[np-relations|relación NP]] R, (P, V) tiene **soundness** con error ε_s si para todo x ∉ L_R y todo prover tramposo P*:

Pr[ ⟨P*, V(x)⟩ = accept ] ≤ ε_s(|x|).

Variantes:
- **Soundness estadística (prueba interactiva, IP)**: vale contra P* de poder computacional **arbitrario**.
- **Soundness computacional (argumento)**: vale solo contra P* de tiempo polinomial, bajo un supuesto de dureza (p. ej. logaritmo discreto, pairings). Los SNARKs son *argumentos*, no pruebas.
- **Proof of knowledge / knowledge soundness**: refuerza la noción: existe un *extractor* E tal que si P* convence con probabilidad no despreciable, E (con acceso de rebobinado a P*) extrae w con (x,w) ∈ R.

## Complejidad / garantías / limitaciones
La soundness estadística requiere interactividad o modelos idealizados; en NIZK con CRS la soundness suele ser computacional. La repetición en paralelo no siempre reduce el error: hay que analizar cada protocolo (para Σ-protocolos, la soundness especial da extracción con dos transcripts).

## Aplicaciones concretas
- En Zcash/Midnight: nadie puede gastar ni acreditar sin poseer los secretos correspondientes.
- En auditoría: un balance probado en ZK no puede falsificarse sin romper el supuesto criptográfico.

## Relación con Midnight / Compact (si aplica)
Midnight usa argumentos (soundness computacional): la integridad on-chain descansa en la dureza de problemas algebraicos, no en una barrera teórica absoluta.

## Relación con mi proyecto (si aplica)
Un impostor no puede presentarse como perito ni fabricar evidencia: necesitaría romper el supuesto criptográfico subyacente o conocer los secretos. Bajo Daubert, esto fija la "tasa de error conocida" del método. Ver [[witness-statement]].

## Referencias
- Goldreich, "Foundations of Cryptography, Vol. 1", Cap. 4, 2001.
- Bellare, Goldreich, "On Defining Proofs of Knowledge", CRYPTO 1992.
- Ben-Sasson, Chiesa, Tromer, Virza, "Succinct Non-Interactive Zero Knowledge for a von Neumann Architecture", USENIX Security 2014.
