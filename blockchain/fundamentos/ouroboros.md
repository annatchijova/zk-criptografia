# Ouroboros: el PoS de Cardano

## ¿Qué problema resuelve?
Ouroboros es la familia de protocolos Proof-of-Stake que asegura [[cardano]]: logra consenso con seguridad **demostrable** (reducción matemática a primitivas criptográficas) sin el gasto energético de PoW. Resuelve el sorteo de líderes de bloque de manera aleatoria, verificable y resistente a adversarios adaptativos. Para el panorama general de consensos, ver [[consenso]].

## Idea intuitiva
En lugar de mineros compitiendo con electricidad, los participantes bloquean stake (ADA). En cada intervalo ("slot"), una lotería criptográfica secreta elige al líder que produce el bloque. Nadie sabe de antemano quién ganó hasta que el ganador lo revela con una prueba verificable (VRF), lo que frustra ataques dirigidos. El tiempo se divide en épocas; la aleatoriedad de cada época se deriva de la cadena anterior.

## Definición formal
Componentes centrales:
- **VRF (Verifiable Random Function)**: cada stake pool evalúa una función aleatoria con su clave secreta sobre el slot y la semilla de época; si la salida cae bajo un umbral proporcional a su stake, es líder y publica la prueba.
- **Épocas y slots**: el tiempo es discreto; la cadena avanza slot a slot con regla de selección de cadena más larga (densidad).
- **Seguridad**: Ouroboros Praos demuestra seguridad contra adversarios con <50% del stake bajo red semi-síncrona, con mensajes retrasados; Ouroboros Genesis añade arranque seguro desde génesis sin checkpoints confiables.

## Complejidad / garantías / limitaciones
- **Garantías**: persistencia y vivacidad con demostraciones formales; resistencia a *grinding* (el adversario no puede "probar suerte" alterando la semilla).
- **Eficiencia**: energía despreciable vs PoW; miles de nodos validadores.
- **Limitaciones**: finalidad probabilística (ver [[finality]]): la irreversibilidad es asintótica (~2160 bloques para garantías estadísticas fuertes en parámetros de Cardano); supone que la mayoría del stake es honesto y está en línea; la aleatoriedad depende de que la cadena previa no sea manipulable.

## Aplicaciones concretas
- Producción de bloques en Cardano desde 2020 (era Shelley).
- Staking delegado: poseedores de ADA delegan en pools sin transferir custodia.
- Base de seguridad para cadenas socias del ecosistema.

## Relación con Midnight / Compact (si aplica)
Midnight nace como cadena socia ("partner chain") del ecosistema Cardano y su seguridad de consenso descansa sobre el modelo Ouroboros: los compromisos de datos privados (pruebas Compact) heredan la inmutabilidad del ledger asegurado por PoS.

## Relación con mi proyecto (si aplica)
El timestamp y la inmutabilidad de los hashes de evidencia del peritaje provienen de este consenso: la "cadena de custodia digital" es tan confiable como las garantías formales de Ouroboros, un argumento técnico citable ante el estándar Daubert (metodología sometida a escrutinio y revisión por pares académicos).

## Referencias
- Kiayias, Russell, David, Oliynykov. *Ouroboros: A Provably Secure Proof-of-Stake Blockchain Protocol* (CRYPTO 2017). https://eprint.iacr.org/2016/889
- David et al. *Ouroboros Praos* (2018). https://eprint.iacr.org/2017/573
- Badertscher et al. *Ouroboros Genesis* (2018). https://eprint.iacr.org/2018/378
