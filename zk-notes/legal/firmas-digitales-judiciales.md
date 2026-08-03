# Firmas digitales y su validez en procesos judiciales

## ¿Qué problema resuelve?
Una firma cumple funciones jurídicas: identificar al firmante (autenticidad), expresar consentimiento (integración de voluntad) y fijar el contenido firmado (integridad). La pregunta judicial es si una firma electrónica/digital cumple esas funciones con la misma fuerza probatoria que la manuscrita.

## Idea intuitiva
La firma criptográfica (p. ej., ECDSA) vincula matemáticamente un documento con una clave privada que solo el firmante controla. Los marcos legales modernos adoptan **neutralidad tecnológica**: no exigen una tecnología específica, sino funciones equivalentes a la firma manuscrita.

## Definición formal
Marcos principales:
- **EE. UU.**: ESIGN Act (15 U.S.C. § 7001, 2000) y UETA estatal: una firma electrónica no puede negarse efecto legal solo por ser electrónica; validez depende de intención, consentimiento y asociación firma-registro.
- **UE**: Reglamento eIDAS (UE 910/2014) distingue firma electrónica simple, **avanzada** (art. 26: vinculada unívocamente al firmante, creada con datos bajo su control exclusivo, detecta cambios posteriores) y **cualificada** (art. 32: dispositivo cualificado + certificado cualificado), con presunción de equivalencia a la manuscrita.
- **Latinoamérica**: leyes modelo UNCITRAL (Ley Modelo de Firma Electrónica 2001); Argentina (Ley 25.506), México (Código de Comercio art. 89), Colombia (Ley 527/1999), Chile (Ley 19.799).
Judicialmente, la firma digital autentica bajo esquemas tipo Rule 901 y los sellos de tiempo ([[timestamping-rfc3161]]) refuerzan la fecha.

## Alcance / garantías / limitaciones
- **Garantías**: autoría (no repudio razonable), integridad del contenido firmado.
- **Limitaciones**: la firma prueba control de la clave, no necesariamente la identidad real (depende del certificado/CA); claves comprometidas rompen la presunción; requisitos de forma de algunos actos excluyen firma simple (testamentos, familia).

## Aplicaciones concretas
Contratos electrónicos, notificaciones judiciales electrónicas, certificaciones periciales firmadas digitalmente, sellado de actuaciones en expedientes electrónicos.

## Relación con Midnight / Compact (si aplica)
Las credenciales y firmas en Midnight permiten que el perito firme/atestigüe compromisos de evidencia on-chain con una clave vinculada a su acreditación, mientras ZK oculta datos innecesarios. Ver [[daubert]], [[hashes-como-evidencia]].

## Relación con mi proyecto (si aplica)
Mi circuito exige una firma válida del perito acreditado como entrada privada: la prueba pública muestra "un perito habilitado firmó este commitment" sin revelar cuál.

## Referencias
- ESIGN Act, 15 U.S.C. § 7001.
- Reglamento (UE) 910/2014 (eIDAS), arts. 3(10), 25–32.
- UNCITRAL, Model Law on Electronic Signatures (2001).
