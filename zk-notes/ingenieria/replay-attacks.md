# Ataques de repetición (Replay Attacks)

## ¿Qué problema resuelve?
Una prueba ZK válida es un objeto digital copiable: cualquiera que la capture puede re-presentarla. Sin protección, un tercero podría "reciclar" la prueba de un perito legítimo para hacerse pasar por él, o reusar la misma evidencia en otro caso o contexto.

## Idea intuitiva
Una entrada de cine válida no debería servir para entrar diez veces: hay que marcarla como usada.

## Definición formal
Ataque en que un adversario retransmite datos previamente válidos para obtener un efecto no autorizado. Defensas canónicas en protocolos ZK:
- **Nullifiers**: valor público determinista derivado del secreto y del contexto (`nullifier = H(sk, dominio)`), que se publica y se marca como gastado. La misma credencial produce el mismo nullifier → doble uso detectable, sin revelar identidad. Mecanismo central en Zcash/Tornado Cash. Véase [[nullifiers-nonces]].
- **Nonces / frescura**: incluir en las señales públicas del circuito un challenge reciente (bloque actual, timestamp, ID de caso) de modo que la prueba solo valide ese contexto.
- **Binding de contexto**: incluir en los inputs públicos el identificador del procedimiento judicial, de modo que la prueba sea inútil fuera de él — se apoya en [[domain-separation]].

## Complejidad / garantías / limitaciones
Los nullifiers previenen doble gasto pero crean *linkability* intencional dentro del dominio (ese es su propósito); fuera del dominio deben ser no correlacionables. Requieren un registro (on-chain) de nullifiers gastados → costo de estado. Un nonce sin verificación de frescura es decorativo.

## Aplicaciones concretas
El perito prueba "estoy habilitado y poseo evidencia X del caso C en el momento T": la prueba incluye nullifier de su credencial y el hash del caso C como señal pública. Reutilizarla en otro caso o duplicar la presentación es detectable.

## Relación con Midnight / Compact
Midnight ofrece soporte nativo de nullifiers en su modelo de estado privado; el contrato Compact mantiene el set de nullifiers gastados.

## Relación con mi proyecto
Sin anti-replay, la "autenticación" del perito es falsable por copia — fatal para la admisibilidad Daubert.

## Referencias
- Zcash Protocol Specification (nullifiers), https://zips.z.cash
- Ben-Sasson et al., "Zerocash", IEEE S&P 2014.
