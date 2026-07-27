# ADR-0014 — Propagar correlación en HTTP y gRPC

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-04
Disposition: Moved to traceability standard; correlation is a mandatory cross-cutting practice

## Decision

Este registro no continúa como ADR autónomo. Su contenido se conserva como estándar técnico activo y verificable.

## Context

La revisión aplicó los criterios de decisión arquitectónica: alternativas reales, impacto estructural, dificultad de reversión y trade-offs significativos. El contenido describe un requisito prescrito o una práctica transversal que debe cumplirse de forma uniforme, pero no una elección estratégica independiente.

## Options considered

- **Selected — reclasificar como estándar.**
- **Mantener como ADR:** descartado por granularidad o por repetir un requisito explícito.

## Consequences

**Positive**
- Se conserva la obligación sin inflar el Decision Log.

**Negative / trade-offs**
- Las referencias existentes deben apuntar al estándar correspondiente.

## Evidence

- `docs/standards/observability/correlation-id.md`
