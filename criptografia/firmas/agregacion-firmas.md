# Agregación de firmas

## ¿Qué problema resuelve?

Cuando N partes deben avalar un mismo objeto (un informe, una evidencia, un bloque), presentar N firmas independientes cuesta $O(N)$ en tamaño y verificación. La agregación comprime todo a una sola firma de tamaño constante con verificación (casi) constante — aplicación directa: *"N peritos avalan esta evidencia"* con un solo artefacto verificable.

## Idea intuitiva

Si las firmas son elementos de un grupo con estructura bilineal ([[bls|BLS]]), "sumarlas" produce una firma válida bajo la "suma" de las claves públicas. El verificador solo comprueba una ecuación de pairing en lugar de N ecuaciones.

## Definición formal

Con BLS sobre BLS12-381 y mismo mensaje $M$:

- Cada firmante $i$: $\sigma_i = x_i \cdot H(M)$.
- Firma agregada: $\sigma = \sum_{i=1}^{N} \sigma_i \in \mathbb{G}_1$ (48 bytes).
- Verificación: $e(\sigma, G_2) \stackrel{?}{=} e\big(H(M), \sum_i pk_i\big)$ — 2 pairings independientemente de $N$.
- Mensajes distintos: $e(\sigma, G_2) \stackrel{?}{=} \prod_i e(H(M_i), pk_i)$ — $N+1$ pairings.

Defensa contra *rogue-key attacks*: exigir proof-of-possession (firma sobre la propia clave pública) al registrar cada $pk_i$, o usar el esquema BDN/MuSig2 para [[schnorr|Schnorr]].

## Complejidad / garantías / limitaciones

- Tamaño: constante (48–96 bytes) para agregación sobre el mismo mensaje.
- Verificación: 2 pairings (~1–2 ms) vs. N verificaciones EdDSA.
- La agregación no oculta quién firmó: la lista de claves públicas es entrada de la verificación. Para anonimato hace falta componer con pruebas ZK (probar que los firmantes pertenecen a un conjunto autorizado sin revelarlos) y [[nullifiers-nonces|nullifiers]].
- No soporta revocación trivial: si un firmante se invalida, hay que reagregar.

## Aplicaciones concretas

Consenso de Ethereum (attestations agregadas), Mithril en Cardano, multisig compacto, co-firma de peritajes, "quórum de validadores" en sidechains.

## Relación con Midnight / Compact (si aplica)

En un circuito Compact se puede probar "existe una firma agregada BLS válida de al menos $t$ miembros del registro de peritos sobre el hash de esta evidencia", revelando solo el hecho del quórum y no la identidad de cada perito — alineado con el estándar [[daubert|Daubert]] de fiabilidad metodológica sin exponer datos sensibles.

## Relación con mi proyecto (si aplica)

Caso de uso central: el tribunal verifica una única firma agregada que certifica que $t \geq 3$ peritos habilitados avalaron la cadena de custodia de la evidencia de víctimas.

## Referencias

- Boneh, Drijvers, Neven, "Compact Multi-Signatures for Smaller Blockchains", ASIACRYPT 2018.
- Boneh, Lynn, Shacham, "Short Signatures from the Weil Pairing", JoC 2004.
- "Mithril: Stake-based Threshold Multisignatures", IOG Research, 2022.
