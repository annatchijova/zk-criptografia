# RFC 3161: sellado de tiempo confiable y su relación con timestamps on-chain

## ¿Qué problema resuelve?
Probar que un dato existía en un momento determinado (anterioridad). En litigios de evidencia digital, el "cuándo" es tan disputado como el "qué" ([[digital-evidence]], [[chain-of-custody]]).

## Idea intuitiva
Un Time-Stamping Authority (TSA) recibe el **hash** de un documento (nunca el documento), le añade la fecha/hora y lo firma digitalmente. El token resultante prueba que ese hash existía a más tardar en ese instante, porque la TSA no podría haber firmado un hash inexistente ([[hashes-como-evidencia]], [[firmas-digitales-judiciales]]).

## Definición formal
RFC 3161 (X.509 PKIX *Time-Stamp Protocol*, 2001, actualizado por RFC 5816) define:
- **TimeStampReq**: contiene `messageImprint` = hash del dato + identificador del algoritmo.
- **TimeStampResp/TimeStampToken (TST)**: CMS/PKCS#7 firmado por la TSA que incluye el imprint, `genTime` en UTC y número de serie.
- Requisitos: la TSA usa fuente de tiempo confiable (trazable a UTC), clave privada protegida (idealmente HSM), y el token vincula criptográficamente tiempo y hash.
- El cliente verifica: firma de la TSA, certificado válido y que el hash local coincide con el imprint.

## Alcance / garantías / limitaciones
- **Garantía**: anterioridad del hash con precisión declarada por la TSA (`accuracy` field).
- **Limitaciones**: confianza centralizada en la TSA (puede ser comprometida o coludir); el token expira con el certificado si no se re-sella; prueba existencia del hash, no del contenido ni del autor.

## Aplicaciones concretas
Sellado de contratos electrónicos, propiedad intelectual (anterioridad de creación), firma electrónica avanzada/cualificada bajo eIDAS (Reglamento UE 910/2014, art. 41: presunción de exactitud de la fecha en sellos cualificados).

## Relación con Midnight / Compact (si aplica)
Una transacción en blockchain cumple un rol funcional equivalente al TST: el commitment (hash) queda anclado en un bloque con timestamp y orden inmutable, **sin TSA central** y con verificación pública. En Midnight, el anclaje preserva además la privacidad: el timestamp es público pero el contenido y las partes permanecen protegidos vía ZK.

## Relación con mi proyecto (si aplica)
Uso el timestamp on-chain de Midnight como "TSA descentralizada" para fijar cuándo el perito comprometió la evidencia, reforzando la [[chain-of-custody]] sin revelar datos.

## Referencias
- RFC 3161, *Internet X.509 PKI Time-Stamp Protocol* (2001).
- RFC 5816 (actualización ESSCertIDv2).
- Reglamento (UE) 910/2014 (eIDAS), arts. 41–42.
