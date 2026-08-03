# Circuito aritmético

## ¿Qué problema resuelve?
Para probar en ZK que una función se evaluó correctamente, hay que representarla de forma que un sistema de pruebas pueda "consumirla". El circuito aritmético es esa representación: un grafo acíclico de sumas y multiplicaciones sobre un campo finito $\mathbb{F}_p$. Es el modelo de computación estándar de los SNARKs modernos.

## Idea intuitiva
Descomponé cualquier cálculo en operaciones elementales: sumar y multiplicar. Cada operación es una "puerta" y cada valor intermedio un "cable". Evaluar el circuito es llenar los cables; probarlo es convencer a alguien de que conocés valores de cables consistentes desde las entradas hasta la salida, sin mostrar los cables privados.

## Definición formal
Un circuito aritmético sobre $\mathbb{F}$ es un DAG $C$ con puertas de entrada (variables y constantes) y puertas internas etiquetadas $+$ o $\times$, de fan-in 2. Evalúa un polinomio multivariado $f: \mathbb{F}^n \to \mathbb{F}^m$. La relación de interés es $R_C = \{(x, w) : C(x, w) = y\}$: la salida pública $y$ es correcta dadas entradas públicas $x$ y testigo $w$. Este modelo se aritmetiza a [[r1cs]] y luego a [[qap]] (para Groth16) o a sistemas de Plonk.

## Complejidad / garantías / limitaciones
- El **tamaño** (número de puertas) determina el costo del prover; en R1CS solo las multiplicaciones cuestan, las sumas son "gratis" (se absorben en combinaciones lineales).
- Limitaciones: divisiones, comparaciones y operaciones bitwise requieren gadgets costosos; el campo es primo y grande, no hay aritmética entera nativa (hay que probar rangos).
- Garantía: todo cálculo en $\mathbb{F}$ de tamaño $s$ admite un circuito de tamaño $O(s)$.

## Aplicaciones concretas
- Verificación de Merkle paths (gadget de hash Poseidon).
- Transferencias privadas (Zcash Sapling/Orchard).
- Compiladores: circom, Noir, Leo, Compact.

## Relación con Midnight / Compact
Un contrato **Compact** con lógica privada compila a un circuito aritmético sobre el campo escalar de BLS12-381; el circuito se materializa como restricciones para el backend de pruebas de Midnight.

## Relación con mi proyecto
El circuito del perito incluye: gadget de hash de la evidencia, chequeo de la clave pública del perito y pertenencia a un registro acreditado (Merkle), todo como puertas de suma y multiplicación.

## Referencias
- Ben-Sasson et al., *SNARKs for C*, CACM 2014.
- Documentación de Compact: https://docs.midnight.network/
- Zcash Protocol Specification, secciones de circuitos Sapling/Orchard.
