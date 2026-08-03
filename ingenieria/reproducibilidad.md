# Reproducibilidad

## ¿Qué problema resuelve?
En un contexto judicial o pericial, una prueba ZK solo es persuasiva si un tercero independiente puede volver a ejecutar el proceso y obtener el mismo resultado. Sin [[reproducibilidad]], la prueba es una afirmación, no evidencia verificable.

## Idea intuitiva
Es como una receta de laboratorio: si otro perito, con los mismos insumos y el mismo protocolo, no obtiene el mismo resultado, el experimento no vale como prueba. En ZK, cualquiera debería poder recompilar el circuito, regenerar la prueba (o verificarla) y comprobar que todo coincide.

## Definición formal
Un pipeline de generación y verificación de pruebas es reproducible si:
1. El código fuente del circuito está versionado (hash de commit identificable).
2. El build es determinista: mismas fuentes → mismo artefacto (mismo hash del circuito compilado).
3. Los parámetros públicos (proving key / verifying key o su equivalente transparente) derivan públicamente del circuito.
4. La verificación es un algoritmo determinista y público: `Verify(vk, statement, proof) ∈ {0,1}`.
5. El statement (inputs públicos) y la prueba quedan archivados junto con los hashes.

## Complejidad / garantías / limitaciones
La verificación de un SNARK suele costar milisegundos y es determinista, ideal para re-ejecución por terceros. Limitaciones: el build determinista no es trivial (versiones de LLVM/Rust); la generación de la prueba depende de datos privados que no se publican, así que el tercero verifica pero no necesariamente re-genera. Un setup trusted complica la narrativa: hay que documentar la ceremonia o preferir esquemas transparentes (véase [[trade-offs]]).

## Aplicaciones concretas
- Archivar en el expediente: commit hash, hash del circuito, verifying key, statement y proof.
- CI público que re-verifica la prueba en cada release.
- Instrucciones de un solo comando (`midnight verify proof.json`) para el juez o contraperito.

## Relación con Midnight / Compact (si aplica)
En Midnight, la verificación ocurre on-chain mediante el contrato Compact: cualquier nodo re-verifica la prueba, lo que da reproducibilidad "incorporada". El contrato desplegado y la transacción quedan en la ledger pública.

## Relación con mi proyecto (si aplica)
Para el peritaje de identidad y posesión de evidencia: la reproducibilidad on-chain es el pilar para cumplir el criterio Daubert de "testabilidad": el tribunal no confía en el perito, confía en que cualquiera puede re-verificar.

## Referencias
- Daubert v. Merrell Dow Pharmaceuticals, 509 U.S. 579 (1993).
- The Turing Way — Guía de investigación reproducible: https://the-turing-way.netlify.app/
- Documentación de Midnight: https://docs.midnight.network/
