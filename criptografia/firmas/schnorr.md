# Firma de Schnorr

## ¿Qué problema resuelve?

La firma de Schnorr es el esquema de firma basado en logaritmo discreto más simple y con mejor prueba de seguridad. Es la base conceptual de [[eddsa|EdDSA]], de las firmas umbral (FROST) y de la agregación de claves (MuSig). Su linealidad —que ECDSA no posee— permite combinar firmas y claves algebraicamente.

## Idea intuitiva

Es una prueba de conocimiento del logaritmo discreto convertida en firma mediante la heurística [[fiat-shamir|Fiat-Shamir]]: el firmante demuestra que conoce $x$ tal que $P = xG$, y "ata" la prueba al mensaje incluyéndolo en el desafío hash.

## Definición formal

Sea un grupo cíclico de orden primo $q$, generador $G$, hash $H$. Clave privada $x$, clave pública $P = xG$.

- **Protocolo Sigma interactivo**: el prover envía el compromiso $R = rG$; el verifier responde con un desafío $c$; el prover contesta $s = r + cx$. Verificación: $sG = R + cP$.
- **Firma no interactiva**: $c = H(R \| P \| M)$, $s = r + c \cdot x$. Firma: $(R, s)$. Verificación: $sG \stackrel{?}{=} R + H(R\|P\|M) \cdot P$.

Variante moderna: BIP-340 (Schnorr sobre secp256k1, usado en Bitcoin Taproot).

## Complejidad / garantías / limitaciones

- Firmas de 64 bytes; verificación: una multiexponenciación de dos términos.
- Seguridad EUF-CMA probada bajo el supuesto del logaritmo discreto en el modelo del oráculo aleatorio (prueba más ajustada que la de ECDSA).
- La linealidad que habilita agregación también habilita ataques: la agregación naíf sufre *rogue-key attacks* (mitigado con MuSig/MuSig2). No resistente a computación cuántica.

## Aplicaciones concretas

Bitcoin Taproot (BIP-340), FROST (firmas umbral), base de EdDSA, protocolos de identificación.

## Relación con Midnight / Compact (si aplica)

El patrón "Sigma + Fiat-Shamir" es exactamente la arquitectura de las pruebas ZK que genera Compact: compromiso, desafío derivado por hash, respuesta. Entender Schnorr es entender el esqueleto de todo circuito ZK no interactivo.

## Relación con mi proyecto (si aplica)

La declaración del perito "conozco la clave privada registrada ante el colegio profesional" es literalmente una prueba Schnorr envuelta en un circuito, con [[nullifiers-nonces|nullifier]] para impedir reutilización vinculable.

## Referencias

- C. P. Schnorr, "Efficient Signature Generation by Smart Cards", Journal of Cryptology 4(3), 1991.
- Pointcheval & Stern, "Security Arguments for Digital Signatures and Blind Signatures", Journal of Cryptology, 2000.
- BIP-340: "Schnorr Signatures for secp256k1", Bitcoin, 2020.
