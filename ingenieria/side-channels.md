# Canales laterales (Side Channels)

## ¿Qué problema resuelve?
Una prueba ZK puede ser matemáticamente perfecta y aun así filtrar el secreto por vías físicas u operativas: tiempo de ejecución, consumo de energía, caché, tráfico de red, metadatos. El *zero-knowledge* protege el contenido de la prueba, no el entorno que la genera.

## Idea intuitiva
Una caja fuerte inviolable de nada sirve si el ladrón escucha los clics de la cerradura.

## Definición formal
Fuga de información sobre datos secretos (clave del perito, *witnesses* con datos de víctimas) a través de observables no previstos en el modelo de seguridad:
- **Tiempo** (véase [[timing-attacks]]): ramas dependientes del secreto.
- **Caché/memoria**: accesos dependientes del secreto dejan huella observable (Spectre, Flush+Reload).
- **Energía/EM**: análisis de potencia (DPA) en hardware.
- **Tráfico y metadatos**: tamaño y frecuencia de transacciones on-chain pueden correlacionar al perito con un caso aunque el contenido sea privado.
- **Errores**: mensajes de fallo que distinguen entradas (*padding oracle* y análogos).

## Complejidad / garantías / limitaciones
La mitigación (código de tiempo constante, *blinding*, mezcla de tráfico) tiene costo de rendimiento. En contexto hackathon, el riesgo dominante y realista es más modesto: metadatos on-chain, logs de depuración con witnesses, y reutilización de *nonces* — más que DPA sobre hardware.

## Aplicaciones concretas
- No loggear witnesses ni claves en el pipeline de generación de pruebas.
- Uniformar tamaños de transacciones; considerar *padding*.
- Aleatoriedad segura (CSPRNG) para blinding y nullifiers — véase [[nullifiers-nonces]].

## Relación con Midnight / Compact
La privacidad de Midnight cubre el ledger; el prover corre localmente del lado del perito y ahí los canales laterales son responsabilidad nuestra.

## Relación con mi proyecto
Mencionar los canales laterales y su mitigación documentada refuerza la fiabilidad del método ante un escrutinio Daubert.

## Referencias
- Kocher, "Timing Attacks on Implementations of Diffie-Hellman, RSA, DSS", CRYPTO 1996.
- Kocher, Jaffe, Jun, "Differential Power Analysis", CRYPTO 1999.
- OWASP Side-Channel guidance: https://owasp.org
