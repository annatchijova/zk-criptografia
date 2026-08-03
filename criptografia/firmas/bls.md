# Firma BLS (Boneh–Lynn–Shacham)

## ¿Qué problema resuelve?

BLS permite firmas cortas con **agregación nativa**: N firmas de N firmantes sobre el mismo mensaje (o mensajes distintos) se combinan en una sola firma del tamaño de una, verificable con un número constante de operaciones de *pairing*. [[eddsa|EdDSA]] y [[schnorr|Schnorr]] no permiten esto de forma no interactiva.

## Idea intuitiva

Un *pairing* (emparejamiento bilineal) $e$ es una operación que "multiplica" puntos de dos curvas hacia un grupo destino cumpliendo $e(aP, bQ) = e(P, Q)^{ab}$. Esto convierte la verificación de una ecuación de logaritmo discreto en una igualdad de emparejamientos, y la multiplicatividad hace que las firmas simplemente se sumen.

## Definición formal

Curvas con pairing (p. ej. BLS12-381), grupos $\mathbb{G}_1, \mathbb{G}_2$ de orden primo $q$, hash-a-curva $H: \{0,1\}^* \to \mathbb{G}_1$.

- Clave privada $x \in \mathbb{Z}_q$; pública $pk = x \cdot G_2 \in \mathbb{G}_2$.
- Firma: $\sigma = x \cdot H(M) \in \mathbb{G}_1$ (48 bytes comprimida).
- Verificación: $e(\sigma, G_2) \stackrel{?}{=} e(H(M), pk)$.
- Agregación (mismo mensaje): $\sigma_{agg} = \sum_i \sigma_i$ verifica contra $pk_{agg} = \sum_i pk_i$.

## Complejidad / garantías / limitaciones

- Firma: 1 hash-a-curva + 1 multiplicación escalar. Verificación agregada de N firmas sobre el mismo mensaje: 2 pairings.
- Seguridad bajo el supuesto CDH en grupos con pairing, en el modelo del oráculo aleatorio.
- Limitaciones: pairings ~10× más caros que una exponenciación simple; agregación naíf sobre mensajes distintos sufre *rogue-key attacks* (mitigar con proof-of-possession o el esquema de Boneh–Drijvers–Neven); costo de verificación in-circuit alto.

## Aplicaciones concretas

Ethereum 2.0 (consenso: miles de firmas agregadas por slot), DFINITY, Chia, firmas umbral sin interacción, certificados agregados.

## Relación con Midnight / Compact (si aplica)

Midnight hereda el ecosistema Cardano, donde se investiga BLS12-381 para sidechains y Mithril (certificados de stake agregados). La verificación de pairings in-circuit es costosa; suele verificarse off-chain y attestarse on-chain.

## Relación con mi proyecto (si aplica)

Véase [[agregacion-firmas]]: N peritos avalan una misma evidencia y el juzgado verifica una sola firma agregada de 48 bytes.

## Referencias

- Boneh, Lynn, Shacham, "Short Signatures from the Weil Pairing", ASIACRYPT 2001 / Journal of Cryptology 2004.
- Boneh, Drijvers, Neven, "Compact Multi-Signatures for Smaller Blockchains", ASIACRYPT 2018.
- Boran, "BLS Signatures" (Internet-Draft, IETF CFRG).
