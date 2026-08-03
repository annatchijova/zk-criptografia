# Heurística Fiat-Shamir

## ¿Qué problema resuelve?

Muchas pruebas de conocimiento (protocolos Sigma) son interactivas: el verifier envía un desafío aleatorio y el prover responde. La interacción es impracticable en blockchain: el "verifier" es un contrato o miles de nodos futuros. Fiat-Shamir convierte cualquier protocolo Sigma público en **no interactivo** reemplazando el desafío del verifier por un hash.

## Idea intuitiva

Si el desafío debe ser impredecible para el prover y generado *después* del compromiso, una función hash aplicada al compromiso (y al mensaje/contexto) cumple ambos requisitos: el prover no puede "prever" la salida del hash, así que es como si un verifier honesto la hubiera elegido.

## Definición formal

Sea un protocolo Sigma $(P_1, V, P_2)$ sobre una relación $\mathcal{R}$:

1. Prover: compromiso $a = P_1(x, w)$.
2. Verifier: desafío $c \leftarrow \mathcal{C}$ uniforme.
3. Prover: respuesta $z = P_2(x, w, a, c)$. Verifier acepta si $V(x, a, c, z) = 1$.

Transformada Fiat-Shamir: $c = H(\mathsf{dom} \| x \| a \| m)$, donde $\mathsf{dom}$ es una etiqueta de dominio y $m$ el mensaje/contexto. La prueba no interactiva es $\pi = (a, z)$; la verificación recomputa $c$ y chequea $V$.

Ejemplo canónico: la firma [[schnorr|Schnorr]] es Fiat-Shamir aplicado a la prueba de conocimiento de logaritmo discreto.

## Complejidad / garantías / limitaciones

- Sin rondas extra; tamaño de prueba idéntico al de una ronda.
- **Soundness** probada en el modelo del oráculo aleatorio (Forking Lemma de Pointcheval–Stern); en el modelo estándar existen contraejemplos teóricos para ciertos protocolos.
- Riesgos prácticos: omisión del mensaje o de la clave pública en el hash (ataques de replay y *rogue key*), reutilización de dominio entre protocolos, y hashes débiles. Por eso el *domain separation* es obligatorio.
- Es la base de la no interactividad de prácticamente todos los SNARKs modernos (con el upgrade "hash esponja" en STARKs).

## Aplicaciones concretas

Firmas Schnorr/[[eddsa|EdDSA]], Bulletproofs, Plonk/Halo2, [[vrf|VRFs]], credenciales anónimas.

## Relación con Midnight / Compact (si aplica)

Los circuitos de Midnight (ZK-SNARKs sobre Halo2/Plonk) usan Fiat-Shamir internamente con funciones hash aptas para circuitos (Poseidon/Rescue) para derivar desafíos del transcript.

## Relación con mi proyecto (si aplica)

Toda prueba de "el perito es quien dice ser y posee la evidencia" generada off-chain y verificada on-chain en Midnight existe gracias a esta transformada.

## Referencias

- Fiat & Shamir, "How to Prove Yourself: Practical Solutions to Identification and Signature Problems", CRYPTO 1986.
- Pointcheval & Stern, "Security Arguments for Digital Signatures and Blind Signatures", JoC 2000.
- Bernhard, Pereira, Warinschi, "How Not to Prove Yourself: Pitfalls of the Fiat-Shamir Heuristic", ASIACRYPT 2012.
