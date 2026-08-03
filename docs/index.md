# Arquitectura — Banco BanQuito

Repositorio documental que preserva la evolución **monolito → microservicios → microservicios con servicios cloud**, los ASR, las decisiones, estándares y evidencia del Core Bancario y Switch de Pagos Masivos.

## Accesos principales

- [Decision Log](adr/index.md)
- [ADR del monolito](adr/monolito/index.md)
- [ADR de microservicios](adr/microservicios/index.md)
- [ADR de microservicios-cloud](adr/microservicios-cloud/index.md)
- [Catálogo ASR](asr/index.md)
- [Criterios ADR vs. requisito](governance/criterios-adr-vs-requisito.md)
- [Matriz de depuración](governance/matriz-depuracion.md)
- [Estándares técnicos](standards/index.md)
- [Inventario de evidencia](evidence/source-inventory.md)
- [Contrato API interbancaria Core](contracts/interbank-core-v1.yaml)
- [Contrato interno Switch/Core R9-K](contracts/switch-core-interbank-lifecycle-v1.yaml)
- [Guía funcional R9-K Nostro/Vostro](functional/r9-k-interbank-core.md)

## Principio rector

El código y C4 muestran **cómo** funciona la solución; los ADR preservan **por qué** se eligió esa estructura y qué trade-offs se aceptaron.
