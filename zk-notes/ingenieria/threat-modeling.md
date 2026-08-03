# Modelado de amenazas (Threat Modeling)

## ¿Qué problema resuelve?
Un sistema ZK que maneja identidad de peritos y evidencia de víctimas tiene superficie de ataque amplia: sin un análisis explícito, es fácil proteger la criptografía y dejar abierto el pipeline de datos (ingesta de evidencia, metadatos, claves del perito).

## Idea intuitiva
Antes de escribir código, dibujar quién ataca, qué quiere y por dónde puede entrar. Como una inspección forense preventiva del propio sistema.

## Definición formal
Proceso estructurado (STRIDE, LINDDUN, attack trees) que, dado un modelo del sistema (activos, actores, flujos, fronteras de confianza), enumera amenazas: suplantación (*spoofing*), manipulación, repudio, revelación de información, denegación de servicio y escalada de privilegios. Para privacidad, LINDDUN añade *linkability* e *identifiability* — directamente relevantes cuando el demandado intentaría re-identificar víctimas a partir de pruebas públicas en la blockchain.

## Complejidad / garantías / limitaciones
No garantiza seguridad; garantiza cobertura sistemática. Su calidad depende de la honestidad del modelo de atacante (¿atacante con acceso a la mempool? ¿perito malicioso? ¿juez que colude?). Debe ser un artefacto vivo: se revisa en cada iteración.

## Aplicaciones concretas
- Activo: hash de evidencia de víctimas. Amenaza: diccionario de hashes si el espacio es pequeño → mitigar con *blinding* (véase [[domain-separation]]).
- Activo: credencial del perito. Amenaza: robo de clave → mitigar con revocación y [[nullifiers-nonces]].

## Relación con Midnight / Compact
Midnight separa estado público y privado; el modelo de amenazas debe decidir explícitamente qué campos van a cada lado del *ledger*.

## Relación con mi proyecto
Fundamento para argumentar Daubert: un threat model documentado demuestra "tasas de error conocidas" y diseño defendible.

## Referencias
- Shostack, *Threat Modeling: Designing for Security*, Wiley, 2014.
- LINDDUN framework: https://linddun.org
- OWASP Threat Modeling Cheat Sheet: https://cheatsheetseries.owasp.org
