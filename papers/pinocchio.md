# Pinocchio: Nearly Practical Verifiable Computation (Parno, Howell, Gentry, Raykova, 2013)

## ¿Qué problema abierto atacaba?
La computación verificable permitía (en teoría) que un cliente débil delegara un cálculo a un servidor y comprobara el resultado sin rehacerlo, pero las construcciones previas eran puramente teóricas o de propósito específico. No existía un sistema general donde compilar un programa común (C) a una prueba verificable resultara remotamente práctico.

## Idea central (analogía simple)
Es como entregar una tarea de matemática: el alumno (servidor) no solo da el resultado, sino que firma cada paso con una tinta especial que el profesor (cliente) puede verificar en segundos sin repetir los cálculos. Pinocchio convirtió esa metáfora en un pipeline real: programa en C → circuito → prueba compacta.

## Innovación técnica concreta
- Primer sistema **general y casi práctico** de verifiable computation: front-end que compila C a circuitos aritméticos (QAP, refinando GGPR13).
- Pruebas no interactivas de tamaño constante (~288 bytes) y verificación en milisegundos.
- Separación setup/prove/verify con clave de evaluación y clave de verificación públicas.
- Base del formato R1CS que domina los SNARKs hasta hoy.

## Limitaciones (reconocidas o descubiertas después)
- Trusted setup por circuito (heredado por Groth16).
- El tiempo de prueba, aunque histórico, seguía siendo minutos para circuitos medianos.
- La traducción de C a circuitos perdía eficiencia (memoria y bucles caros de aritmetizar).

## Impacto histórico (qué sistemas reales lo usan hoy)
Pinocchio inauguró la era "práctica" de los zkSNARKs: libsnark y toda la línea Zcash descienden de él, y su pipeline compilador→R1CS→SNARK es la plantilla de frameworks modernos como circom y las librerías que alimentan a **Midnight (Compact)**. Para una investigación de evidencia pericial, es el punto de partida conceptual: "compilá tu afirmación legal (soy perito, poseo la evidencia) a un circuito verificable".

## Referencias
- Parno, Howell, Gentry, Raykova. "Pinocchio: Nearly Practical Verifiable Computation", IEEE S&P (Oakland) 2013. DOI: 10.1109/SP.2013.47 ; ePrint relacionado (GGPR13): https://eprint.iacr.org/2012/215
