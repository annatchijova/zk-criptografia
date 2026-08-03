# Limitaciones actuales de Midnight y Compact

## ¿Qué problema resuelve?
Esta nota existe para no prometer de más en el pitch. Conocer los límites reales del toolchain evita que un jurado técnico desarme la demo con una pregunta incómoda y permite presentar el roadmap con honestidad.

## Idea intuitiva
Midnight es un auto nuevo con tecnología superior, pero todavía salió de la fábrica hace poco: el manual cambia seguido, algunas piezas se renombran y no conviene cruzar el desierto sin plan B.

## Definición formal
Limitaciones conocidas, agrupadas:

1. **Madurez del toolchain**
   - El lenguaje fue renombrado recientemente (Compact 0.17 → **Minokawa** 0.18, compilador 0.26) al pasar a Linux Foundation Decentralized Trust: conviven nombres y versiones viejos en tutoriales.
   - La documentación acumuló deuda técnica y ambientes cambiantes (preprod/canary/producción); los ejemplos pueden quedar desactualizados entre releases.
   - `pragma language_version` acopla contrato, compilador y runtime: una actualización puede invalidar contratos desplegados en testnet.

2. **Restricciones del lenguaje**
   - Lenguaje **acotado**: sin recursión ni bucles de cota dinámica; todos los tipos de tamaño fijo ([[tipos]]).
   - La lógica probatoria compleja (muchas pruebas Merkle, hashes masivos) agranda el circuito y el tiempo de generación de prueba off-chain.

3. **Costo y tamaño**
   - Tiempo de prueba en el cliente: segundos por circuito según tamaño; UX afectada en dispositivos modestos.
   - Datos públicos on-chain y tamaño de tx (~5 KB por prueba) encarecen en DUST ([[gas]]).

4. **Modelo operativo**
   - El estado privado no se replica: pérdida local = pérdida total; se requieren backups propios ([[off-chain-computation]]).
   - Los witnesses son security-critical y deben auditarse (advertencia de OpenZeppelin).
   - Sin anonimato de capa de red.

## Aplicaciones concretas
- Slide de "riesgos y mitigación" del pitch; criterio para limitar el alcance del MVP (un circuito, una credencial, un commitment).

## Relación con mi proyecto
Declarado al jurado: el MVP prueba identidad del perito + posesión de evidencia con un solo circuito acotado; escalado a lotes de víctimas vía Merkle tree queda como fase 2, sujeto a límites de tamaño de circuito. Esta honestidad refuerza, paradójicamente, la credibilidad tipo Daubert del propio proyecto.

## Referencias
- https://forum.midnight.network/t/major-compact-milestone-and-release/501 — release Minokawa 0.18 / compilador 0.26
- https://github.com/OpenZeppelin/compact-contracts — warnings de experimentalidad y witnesses
- https://hackmamba.io/case-study/how-we-helped-midnight-network-stabilize-its-documentation/
