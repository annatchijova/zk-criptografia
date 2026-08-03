# EdDSA (Edwards-curve Digital Signature Algorithm)

## ¿Qué problema resuelve?

EdDSA resuelve dos problemas históricos de ECDSA: la necesidad de un nonce aleatorio de alta calidad por firma (cuya filtración o repetición revela la clave privada, como ocurrió con la PlayStation 3 y con varios monederos de Bitcoin) y la fragilidad de implementación. EdDSA es determinística: la misma clave y el mismo mensaje siempre producen la misma firma.

## Idea intuitiva

En lugar de tirar un dado para generar el nonce, EdDSA "deriva el dado" del propio mensaje y de una clave secreta extendida mediante una función hash. Como el mensaje ya es impredecible para el atacante, el nonce derivado es pseudoaleatorio sin necesidad de un generador aleatorio externo.

## Definición formal

EdDSA (RFC 8032) es una variante de la firma [[schnorr|Schnorr]] sobre curvas de Edwards retorcidas. En Ed25519 (curva edwards25519, grupo de orden primo $\ell$):

1. Clave privada: semilla $k$ de 32 bytes; se calcula $h = H(k)$, de donde salen el escalar $a$ y un prefijo.
2. Clave pública: $A = a \cdot G$.
3. Firma sobre $M$: nonce $r = H(\text{prefijo} \| M) \bmod \ell$; $R = r \cdot G$; $S = (r + H(R \| A \| M) \cdot a) \bmod \ell$. La firma es $(R, S)$.
4. Verificación: $S \cdot G \stackrel{?}{=} R + H(R \| A \| M) \cdot A$.

## Complejidad / garantías / limitaciones

- Firma y verificación rápidas en software (~decenas de microsegundos); firmas de 64 bytes, claves de 32 bytes.
- Seguridad EUF-CMA bajo el supuesto del logaritmo discreto, con $H$ modelado como oráculo aleatorio (ver [[fiat-shamir]]).
- El determinismo elimina la clase entera de fallos por nonce débil. Limitación: susceptible a ataques de canal lateral y de inyección de fallos si el dispositivo firmante es físicamente accesible.

## Aplicaciones concretas

SSH, TLS 1.3, Signal, OpenPGP, y casi todas las blockchains modernas (Cardano, Solana, Midnight).

## Relación con Midnight / Compact (si aplica)

Midnight usa Ed25519 en su capa de cuentas y firmas; los circuitos Compact pueden verificar firmas EdDSA in-circuit cuando se requiere probar autoría sin revelar la clave pública.

## Relación con mi proyecto (si aplica)

Un perito firma su compromiso de evidencia con Ed25519; el circuito ZK prueba que conoce una firma válida bajo una clave registrada, sin revelar cuál — ver [[nullifiers-nonces]].

## Referencias

- Bernstein et al., "High-speed high-security signatures", CHES 2011 / Journal of Cryptographic Engineering 2012.
- RFC 8032, "Edwards-Curve Digital Signature Algorithm (EdDSA)", IETF, 2017.
- https://ed25519.cr.yp.to/
