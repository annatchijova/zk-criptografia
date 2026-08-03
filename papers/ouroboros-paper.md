# Ouroboros: A Provably Secure Proof-of-Stake Blockchain Protocol (Kiayias, Russell, David, Oliynykov, 2017)

## ¿Qué problema abierto atacaba?
Bitcoin demostró que el consenso descentralizado funciona, pero el proof-of-work consume energía masiva y su seguridad se analizaba de forma informal. No existía un protocolo proof-of-stake con seguridad *demostrable* comparable a PoW: los diseños PoS anteriores eran ad-hoc y vulnerables a ataques como "nothing at stake" o grinding en la selección de líder.

## Idea central (analogía simple)
En vez de una carrera de mineros quemando electricidad, es una lotería verificable donde tus boletos son tus monedas: en cada época se sortea quién propone cada bloque, el sorteo usa aleatoriedad que nadie puede manipular, y las reglas del juego vienen con una prueba matemática formal de que hacer trampa es tan improbable como romper la criptografía.

## Innovación técnica concreta
Primer PoS con modelo de seguridad riguroso (UC-style, análisis en el framework de "Bitcoin backbone"): selección de líder de slot proporcional al stake mediante VRF/coin tossing con aleatoriedad pública generada por multiparty computation entre stakeholders, épocas con actualización de la cadena, y demostración formal de persistencia y livelineas asumiendo mayoría honesta de stake. Define cómo los nuevos participantes se sincronizan de forma segura. Es la base científica de Cardano.

## Limitaciones (reconocidas o descubiertas después)
El paper original asume sincronía parcial y stakeholders activos; la generación de aleatoriedad por MPC era compleja (simplificada luego con VRFs en Ouroboros Praos). Versiones posteriores (Praos, Genesis, Chronos, Peras) corrigieron ataques de adaptative corruption, disponibilidad dinámica y finalidad. El throughput base es modesto frente a cadenas BFT.

## Impacto histórico (qué sistemas reales lo usan hoy)
Es el protocolo de consenso de **Cardano**, en producción desde 2017/2020 (Shelley), con miles de stake pools. Como Midnight es una *partner chain* de Cardano, Ouroboros es literalmente la capa de consenso que ancla la seguridad del ecosistema donde corre la aplicación de la investigación: la evidencia comprometida con ZK hereda la finalidad e integridad de esta cadena.

## Referencias
- Kiayias, Russell, David, Oliynykov. "Ouroboros: A Provably Secure Proof-of-Stake Blockchain Protocol". CRYPTO 2017. https://eprint.iacr.org/2016/889
- David, Gaži, Kiayias, Russell, "Ouroboros Praos", https://eprint.iacr.org/2017/573
