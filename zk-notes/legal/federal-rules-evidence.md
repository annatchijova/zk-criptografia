# Federal Rules of Evidence: testimonio experto y evidencia digital

## ¿Qué problema resuelve?
Las FRE codifican cuándo y cómo un tribunal federal de EE. UU. admite evidencia, en especial el testimonio de expertos y la autenticación de registros electrónicos.

## Idea intuitiva
Las reglas relevantes forman una cadena: Rule 702 decide si el experto puede hablar ([[daubert]]); Rules 901–902 deciden si la evidencia es auténtica; Rule 1001–1003 tratan duplicados digitales; Rule 403 permite excluir lo que confunda más de lo que aporte.

## Definición formal
- **Rule 702** (enmendada 2023): el experto puede opinar si (a) su conocimiento ayudará al trier of fact; (b) el testimonio se basa en hechos o datos suficientes; (c) es producto de principios y métodos fiables; y (d) la aplicación del método a los hechos es fiable. La enmienda de 2023 aclaró que el proponente debe demostrar estos requisitos por preponderancia de la evidencia.
- **Rule 702(d)** es la cláusula de "fit" reforzada en *Kumho Tire*.
- **Rule 901(b)(9)**: autenticación de un proceso o sistema mostrando que produce resultados precisos (base para autenticar hashes).
- **Rule 902(13)–(14)**: autenticación automática (self-authentication) de registros generados por proceso electrónico y de datos copiados de dispositivos, certificados por persona calificada.
- **Rule 1001(d)**: para electrónicos, cualquier impresión legible cuenta como original.

## Alcance / garantías / limitaciones
Rigen solo en tribunales federales (los estados tienen códigos propios, a menudo espejo). No garantizan que la técnica sea correcta, solo que el método es fiable y la evidencia es lo que se afirma.

## Aplicaciones concretas
Autenticar un archivo por su hash bajo 902(13); admitir un peritaje forense digital bajo 702; usar duplicados forenses (imágenes bit a bit) bajo 1003.

## Relación con Midnight / Compact (si aplica)
Un registro on-chain en Midnight con timestamp verificable encaja en 901(b)(9) y 902(13): el sistema (blockchain + circuitos Compact) produce resultados cuya precisión es demostrable. Ver [[hashes-como-evidencia]], [[timestamping-rfc3161]], [[chain-of-custody]].

## Relación con mi proyecto (si aplica)
Mi demo debe producir un "paquete probatorio" que un perito pueda certificar bajo 902(13): hash de la evidencia, commitment on-chain y prueba ZK de posesión.

## Referencias
- Fed. R. Evid. 702, 901, 902(13)–(14), 1001–1003 (legalinformationinstitute / eCFR).
- *Kumho Tire Co. v. Carmichael*, 526 U.S. 137 (1999).
