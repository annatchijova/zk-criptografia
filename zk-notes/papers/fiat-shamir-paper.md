# How To Prove Yourself: Practical Solutions to Identification and Signature Problems (Fiat, Shamir, 1986)

## ¿Qué problema abierto atacaba?
En 1986, identificarse criptográficamente exigía revelar un secreto (contraseña) o usar firmas RSA con claves pesadas. No existía una forma práctica de *probar conocimiento* de un secreto sin revelarlo, ni de convertir protocolos interactivos en firmas no interactivas eficientes.

## Idea central (analogía simple)
Probar que conoces el camino secreto de una cueva con dos salidas sin mostrarlo: el verificador grita "¡sal por la izquierda!" al azar, y tú sales por donde pidió. Tras varias rondas, solo alguien que conoce el pasaje puede cumplir siempre. La heurística Fiat–Shamir reemplaza al verificador que grita por un hash determinista: el desafío lo genera la propia matemática, sin interacción.

## Innovación técnica concreta
Dos contribuciones fundacionales: (1) el **protocolo de identificación Fiat–Shamir**, prueba de conocimiento cero basada en raíces cuadradas módulo n compuesto, mucho más ligera que RSA por mensaje; (2) la **transformación Fiat–Shamir**: tomar cualquier protocolo sigma interactivo (commitment → challenge → response) y reemplazar el challenge del verificador por H(commitment, mensaje), obteniendo firmas digitales y pruebas no interactivas (NIZK en el Random Oracle Model). Es el puente entre ZK interactivo y aplicaciones reales.

## Limitaciones (reconocidas o descubiertas después)
La seguridad de la transformación se prueba en el modelo del oráculo aleatorio; existen construcciones teóricas de hashes para las que falla (Canetti–Goldreich–Halevi). Requiere incluir en el hash todo el contexto (transcript completo) para evitar ataques de "weak Fiat–Shamir" que aparecieron en implementaciones reales (vulnerabilidades en 2022 en librerías ZK). El esquema original de factorización hoy es solo histórico.

## Impacto histórico (qué sistemas reales lo usan hoy)
La transformación está en *todas* las pruebas ZK prácticas: Schnorr signatures (Bitcoin Taproot), SNARKs, STARKs, Bulletproofs, y por tanto en Midnight/Compact. Todo protocolo no interactivo moderno hereda este truco de 1986. Para el hackathon: permite que el perito genere su prueba de identidad offline y el juez la verifique sin interacción.

## Referencias
- Fiat, A., Shamir, A. "How To Prove Yourself: Practical Solutions to Identification and Signature Problems". CRYPTO 1986, LNCS 263. DOI: 10.1007/3-540-47721-7_12
- Bernhard, Pereira, Warinschi, "How not to Prove Yourself: Pitfalls of the Fiat-Shamir Heuristic", https://eprint.iacr.org/2016/771
