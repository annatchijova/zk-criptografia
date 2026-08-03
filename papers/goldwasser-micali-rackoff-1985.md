# The Knowledge Complexity of Interactive Proof Systems (Goldwasser, Micali, Rackoff, 1985)

## ¿Qué problema abierto atacaba?
Antes de 1985, "demostrar" en criptografía se confundía con "revelar": para convencer a alguien de que conocés un secreto (una factorización, una identidad), lo normal era mostrarlo. No existía una definición formal que capturara la intuición de que una prueba interactiva puede convencer al verificador sin transferirle ningún conocimiento adicional más allá de la verdad del enunciado. El paper definía formalmente qué significa que una prueba "no revele nada".

## Idea central (analogía simple)
Imaginá una cueva con dos pasillos que se unen en una puerta cerrada con contraseña. El probador entra por un pasillo al azar; el verificador grita por cuál salir. Si el probador conoce la contraseña, siempre sale por el pedido; si no, acierta solo la mitad de las veces. Tras muchas rondas, el verificador queda convencido, pero jamás aprendió la contraseña. Eso es conocimiento cero: convencer sin enseñar.

## Innovación técnica concreta
- Definición formal de *zero-knowledge* mediante el paradigma del simulador: todo lo que el verificador ve podría haberlo generado él solo, luego no aprendió nada.
- Definición de la clase IP (pruebas interactivas) y medición de su "complejidad de conocimiento".
- Protocolo ZK para el residuo cuadrático módulo N, mostrando que el concepto era alcanzable.

## Limitaciones (reconocidas o descubiertas después)
Los primeros protocolos requerían muchas rondas de interacción, costosas en la práctica, y la eficiencia era lejana de lo utilizable: la teoría precedió por décadas a las implementaciones reales.

## Impacto histórico (qué sistemas reales lo usan hoy)
Es la piedra fundacional de todo ZK. Sus ideas derivaron en ZKPs no interactivos (Blum-Feldman-Micali), SNARKs y, en la cadena de aplicación, en Zcash, Polygon zkEVM y **Midnight**, donde un perito podría probar su identidad y posesión de evidencia sin exponer datos sensibles de víctimas.

## Referencias
- Goldwasser, Micali, Rackoff. "The Knowledge Complexity of Interactive Proof Systems", STOC 1985; versión extendida en SIAM Journal on Computing 18(1), 1989. DOI: 10.1137/0218012
