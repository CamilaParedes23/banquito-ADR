# ADR-0003 — Separar configuración local y cloud mediante perfiles

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Mathius
Lifecycle: Microservicios-cloud
ASR: ASR-06
Disposition: Merged into ADR-0001; implementation guidance moved to standards/configuration/perfiles-y-variables-entorno.md

## Decision

No se acepta como decisión arquitectónica independiente. Los perfiles de Spring son el mecanismo de implementación de ADR-0001, no una decisión estructural separada.

## Context

La revisión determinó que el registro carecía de autonomía: si ADR-0001 cambia, este registro cambia necesariamente. El contenido útil se conserva como estándar de configuración.

## Options considered

- **Selected — integrar la decisión en ADR-0001 y conservar el mecanismo como estándar.**
- **Mantener ambos ADR:** descartado por duplicidad.

## Consequences

**Positive**
- El Decision Log evita decisiones artificialmente fragmentadas.

**Negative / trade-offs**
- Las referencias antiguas a ADR-0003 deben apuntar a ADR-0001 o al estándar.

## Evidence

- `docs/standards/configuration/perfiles-y-variables-entorno.md`
