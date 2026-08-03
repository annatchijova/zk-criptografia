# Nullifiers y nonces en sistemas ZK

## ¿Qué problema resuelve?

En un sistema anónimo, ¿cómo impedir que alguien use el mismo recurso dos veces (doble gasto, doble voto, reutilización de una credencial) **sin revelar quién es**? El nullifier lo resuelve: una etiqueta única, determinística y pública derivada del secreto del usuario y del recurso, que no es vinculable a la identidad pero cuya repetición delata el doble uso.

## Idea intuitiva

El usuario compromete un secreto en un compromiso ($C = \mathsf{Com}(sk, r)$, p. ej. en un árbol de Merkle). Al gastar/votar/probar, publica $\mathsf{nf} = \mathsf{PRF}_{sk}(\rho)$ junto con una prueba ZK de que $\mathsf{nf}$ fue correctamente derivado de un compromiso válido del árbol. Dos usos del mismo secreto y dominio producen el mismo nullifier → el sistema rechaza el segundo; como $\mathsf{nf}$ parece aleatorio, no revela cuál compromiso se gastó.

## Definición formal

- Compromiso: $C = H(\text{addr} \| sk \| \rho \| r)$ insertado en un árbol de Merkle con raíz $R$.
- Nullifier: $\mathsf{nf} = \mathsf{PRF}_{sk}(\rho)$ (Zcash Sapling) o $\mathsf{nf} = H(sk \| \mathsf{dom} \| \text{acción})$ (Semaphore, para señalización anónima).
- La prueba ZK demuestra: (i) conocer la apertura de algún $C$ en $R$; (ii) conocer $sk$ asociado; (iii) $\mathsf{nf}$ calculado correctamente. Todo con la heurística [[fiat-shamir|Fiat-Shamir]] para hacerlo no interactivo.
- El ledger mantiene el conjunto de nullifiers gastados; la unicidad se chequea on-chain en $O(1)$.

## Complejidad / garantías / limitaciones

- Garantía de no-vinculabilidad si el PRF/hash se comporta como función pseudoaleatoria (o vía [[vrf|VRF]] para salida verificable).
- **Separación de dominios es crítica**: el mismo $sk$ y distinto dominio generan nullifiers independientes (no vinculables entre aplicaciones); mismo dominio, nullifiers iguales (detección de doble uso). Elegir el dominio es una decisión de diseño de privacidad.
- Limitaciones: los nullifiers crecen sin límite en el estado; revocación requiere mecanismos extra; correlación temporal y de red (IP) puede desanonimizar fuera de la capa criptográfica.

## Aplicaciones concretas

Zcash (Sapling/Orchard), Tornado Cash, Semaphore (votación y señalización anónimas), World ID (un nullifier por persona-acción), rate-limiting nulo (RLN) en mensajería.

## Relación con Midnight / Compact (si aplica)

Midnight soporta nativamente este patrón: estados protegidos con compromisos y nullifiers, y en Compact se define la lógica del PRF y el chequeo de unicidad en el contrato.

## Relación con mi proyecto (si aplica)

Cada prueba de perito emite $\mathsf{nf} = H(sk_{\text{perito}} \| \text{ID-caso} \| \text{ID-evidencia})$: el tribunal detecta si el mismo perito reenvía una prueba sobre la misma evidencia (o presenta dos veces), sin poder vincular la prueba con su identidad ni con otros casos.

## Referencias

- Zcash Protocol Specification (Sapling §4), protocol.z.cash.
- Hopwood et al., "Zcash Improvement Proposals 224–226 (Sapling)", 2018.
- Semaphore Protocol, semaphore.pse.dev (nullifiers para señalización anónima).
- RLN: "Rate-Limiting Nullifier", github.com/Rate-Limiting-Nullifier.
