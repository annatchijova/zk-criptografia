# Non-Interactive and Information-Theoretic Secure Verifiable Secret Sharing (Pedersen, 1991)

## ¿Qué problema abierto atacaba?
Los esquemas de *verifiable secret sharing* (VSS) permitían repartir un secreto entre n partes verificando que los shares son consistentes, pero o eran interactivos o no protegían al distribuidor con secreto perfecto (information-theoretic privacy). Se buscaba VSS no interactivo donde el secreto quede oculto incondicionalmente.

## Idea central (analogía simple)
Es un cofre con dos cerraduras independientes: la primera encierra el valor (blinding) y la segunda garantiza que no puedes cambiar el contenido después (binding). Nadie que vea el cofre aprende nada del contenido, pero tú tampoco puedes abrirlo de otra forma: el compromiso te ata.

## Innovación técnica concreta
Introduce el **Pedersen commitment**: C = g^x h^r en un grupo de orden primo, donde r es aleatorio. Es *perfectly hiding* (el commitment no revela nada de x, estadísticamente) y *computationally binding* (abrirlo con otro valor implica resolver el logaritmo discreto). Sobre esta primitiva construye un VSS no interactivo: el distribuidor publica commitments a los coeficientes de un polinomio (Feldman-style pero con doble exponente), y cada parte verifica su share homomórficamente. La aditividad homomórfica (C₁·C₂ compromete x₁+x₂) es un subproducto decisivo.

## Limitaciones (reconocidas o descubiertas después)
El binding es solo computacional (se rompe con computación cuántica o si se conoce log_g(h)); se requiere que nadie conozca la relación entre g y h. No aporta por sí mismo pruebas de rango ni de conocimiento: hay que combinarlo con sigma protocols o SNARKs.

## Impacto histórico (qué sistemas reales lo usan hoy)
Es de los building blocks más usados de la criptografía aplicada: transacciones confidenciales (Blockstream/Elements, Monero), MimbleWimble, Zcash, rangos en Bulletproofs, DKGs y umbrales en wallets. En Midnight (basado en Cardano, ZK de datos) los Pedersen-style commitments son la forma natural de comprometer datos sensibles. En el hackathon: comprometer identidad del perito y hashes de evidencia preservando ocultamiento perfecto y verificabilidad posterior (cadena de custodia estilo Daubert).

## Referencias
- Pedersen, T. P. "Non-Interactive and Information-Theoretic Secure Verifiable Secret Sharing". CRYPTO 1991, LNCS 576. DOI: 10.1007/3-540-46766-1_9
