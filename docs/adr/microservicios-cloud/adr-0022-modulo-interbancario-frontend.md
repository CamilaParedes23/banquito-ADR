# ADR-0022 — Incorporar un módulo interbancario en los frontends

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Kevin / Julio / Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-07
Disposition: Moved to functional/interbancario-frontend.md

## Decision

No se conserva como ADR. La presentación de On-Us/Off-Us, estados y tiempos en Banca Web/Ventanilla es alcance funcional y diseño de experiencia, salvo que una decisión futura cambie el límite de los sistemas frontend.

## Context

La propuesta describe pantallas y comportamiento de usuario, con costo de reversión moderado y sin una decisión estructural independiente. Su contenido se mantiene como especificación funcional para R9-K.

## Options considered

- **Selected — mover a documento funcional.**
- **Mantener como ADR:** descartado por ubicación en el espectro de diseño.

## Consequences

**Positive**
- Separa decisiones arquitectónicas de decisiones UX.

**Negative / trade-offs**
- El equipo debe mantener trazabilidad funcional con R9-K.

## Evidence

- `docs/functional/interbancario-frontend.md`
