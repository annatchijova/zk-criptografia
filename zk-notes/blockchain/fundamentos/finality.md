# Finalidad: probabilística vs determinística — NOTA CANÓNICA

## ¿Qué problema resuelve?
¿Cuándo puede considerarse *irreversible* una transacción o un compromiso anclado en la cadena? La finalidad (finality) es el momento a partir del cual revertir un bloque es imposible o astronómicamente improbable. Es la noción que conecta la teoría del consenso con el valor probatorio de un registro: un hash de evidencia solo sirve judicialmente si su posición en la cadena es estable. Nota canónica enlazada desde `legal/` y desde [[consenso]].

## Idea intuitiva
- **Probabilística**: cada bloque nuevo entierra al anterior bajo más "peso" (trabajo o stake). Revertir exige rehacer todo ese peso: posible en teoría, exponencialmente improbable en la práctica. Nunca hay certeza absoluta, solo confianza creciente.
- **Determinística**: una votación explícita de validadores (≥2/3) *finaliza* el bloque; una vez finalizado, revertirlo es imposible salvo que 1/3+ de validadores se autoincriminen (slashing).

## Definición formal
- **Probabilística (Nakamoto: PoW, [[ouroboros]])**: para adversario con fracción $q < 1/2$ del recurso, la probabilidad de revertir un bloque a profundidad $k$ decae como $O(e^{-ck})$ (análisis de doble gasto de Nakamoto; en Cardano, el parámetro de seguridad $k = 2160$ bloques).
- **Determinística (BFT: Tendermint, HotStuff; checkpoints: Casper FFG)**: un bloque finalizado bajo $f < n/3$ bizantinos es irreversible por construcción; cualquier reversión implica evidencia criptográfica de equivocación.

## Complejidad / garantías / limitaciones
- **Latencia**: BFT finaliza en segundos (una o dos rondas); Nakamoto requiere minutos (Bitcoin: ~60 min para 6 confirmaciones; Cardano: garantías estadísticas fuertes tras horas).
- **Trade-off**: la finalidad determinística exige conjunto de validadores acotado y red parcialmente síncrona; la probabilística escala a miles de nodos pero difiere la certeza.
- **Riesgo legal**: confundir "confirmado" con "finalizado" puede invalidar un argumento probatorio.

## Aplicaciones concretas
- Exchanges que exigen N confirmaciones (finalidad probabilística operacionalizada).
- Bridges entre cadenas que esperan finalidad antes de liberar fondos.
- Sellado de evidencia forense: el peritaje debe citar profundidad/época del compromiso.

## Relación con Midnight / Compact (si aplica)
Las pruebas ZK de Compact se anclan en Midnight, cuya finalidad hereda el modelo [[ouroboros]] (probabilística): el timestamp de los compromisos de evidencia se vuelve prácticamente irreversible tras suficientes bloques.

## Relación con mi proyecto (si aplica)
Bajo Daubert, la "tasa de error conocida" del método de sellado de evidencia incluye la probabilidad de reorganización: con finalidad probabilística es cuantificable ($e^{-ck}$) y declarable en el informe pericial. Ver [[blockchain-intro]].

## Referencias
- Nakamoto (2008), análisis de doble gasto, §11. https://bitcoin.org/bitcoin.pdf
- Kiayias et al. *Ouroboros* (2017). https://eprint.iacr.org/2016/889
- Buterin & Griffith. *Casper the Friendly Finality Gadget* (2017). https://arxiv.org/abs/1710.09437
