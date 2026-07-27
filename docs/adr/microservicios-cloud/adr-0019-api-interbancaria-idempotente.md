# ADR-0019 — Exponer una API interbancaria idempotente

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-02, ASR-07
Superseded by: ADR-0020
Disposition: Merged into ADR-0020; API-level rules moved to standards/integration/idempotencia-interbancaria.md

## Decision

No se mantiene como ADR separado. La obligación de idempotencia de la API se integra en la estrategia end-to-end de ADR-0020.

## Context

La idempotencia es un ASR y una propiedad del contrato. La decisión arquitectónica relevante no es “hacer la API idempotente”, sino dónde se genera la clave y cómo se propaga entre sistemas.

## Options considered

- **Selected — fusionar con ADR-0020.**

## Consequences

**Positive**
- Evita duplicidad conceptual.

**Negative / trade-offs**
- El contrato debe referenciar el estándar y ADR-0020.

## Evidence

- `docs/standards/integration/idempotencia-interbancaria.md`
