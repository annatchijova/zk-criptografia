# Curvas elípticas: geometría que produce grupos duros

## ¿Qué problema resuelve?
Se necesita un [[grupos|grupo]] cíclico de orden primo grande donde el logaritmo discreto sea lo más difícil posible **por bit de clave**. Las curvas elípticas ofrecen exactamente eso: seguridad de ~128 bits con claves de 256 bits, frente a los ~3072 bits de RSA/DH clásicos, lo que las hace ideales para firmas dentro de circuitos ZK.

## Idea intuitiva
Toma la ecuación $y^2 = x^3 + ax + b$ sobre un [[campos-finitos|campo finito]] $\mathbb{F}_p$. Sus puntos forman un grupo con una regla geométrica: para "sumar" dos puntos, traza la recta que los une, encuentra el tercer corte con la curva y refleja sobre el eje $x$. Repetir la suma $k$ veces es fácil; deducir $k$ mirando el resultado, no.

## Definición formal
Una **curva elíptica** sobre $\mathbb{F}_p$ ($p > 3$) en forma de Weierstrass corta:
$$E: y^2 = x^3 + ax + b, \quad 4a^3 + 27b^2 \neq 0 \text{ (no singular)},$$
más un punto en el infinito $\mathcal{O}$ que actúa como neutro.

**Ley de grupo**: dados $P, Q \in E$, la recta por $P$ y $Q$ (o la tangente si $P = Q$, *duplicación*) corta la curva en un tercer punto $R'$; se define $P + Q = -R'$ (reflejo). Con esta operación $(E(\mathbb{F}_p), +)$ es un grupo abeliano.

- **Teorema de Hasse**: $\big| \#E(\mathbb{F}_p) - (p+1) \big| \le 2\sqrt{p}$.
- **ECDLP**: dados $P$ y $Q = kP$ en un subgrupo de orden primo $\ell$, hallar $k$. Mejor ataque conocido: genérico, $O(\sqrt{\ell})$ (ver [[grupos]]).

## Complejidad / garantías / limitaciones
Multiplicación escalar: $O(\log k)$ sumas de punto con doblar-y-sumar. Garantía empírica: sin avances desde los años 80 para curvas bien elegidas. Limitaciones: curvas con $\#E = p$ (anómalas) o embedding degree bajo son inseguras salvo que se usen deliberadamente para [[emparejamientos-bilineales|pairings]]; la validación de puntos y el cofactor importan (ataques de subgrupo pequeño); vulnerable a Shor cuántico.

## Aplicaciones concretas
ECDSA, EdDSA/Schnorr, ECDH, compromisos de Pedersen, y las curvas "pairing-friendly" (BLS12-381) que sostienen los SNARKs.

## Relación con Midnight / Compact (si aplica)
Midnight hereda el stack ZK estilo Zcash: pruebas sobre curvas pairing-friendly y curvas embebidas para firmas/hash dentro del circuito, verificables desde Compact.

## Relación con mi proyecto (si aplica)
La acreditación del perito puede modelarse como firma Schnorr verificada dentro del circuito: se prueba conocimiento de la firma válida sobre su clave pública registrada sin exponer ni firma ni clave, sustento del requisito Daubert de "método fiable aplicado por perito cualificado".

## Referencias
- Washington, *Elliptic Curves: Number Theory and Cryptography*, 2ª ed., Chapman & Hall/CRC.
- Silverman, *The Arithmetic of Elliptic Curves*, 2ª ed., Springer (GTM 106).
- Koblitz, "Elliptic curve cryptosystems", *Math. Comp.* 48, 1987; Miller, "Use of elliptic curves in cryptography", CRYPTO '85.
- SEC 1 y SEC 2, Standards for Efficient Cryptography (secg.org).
