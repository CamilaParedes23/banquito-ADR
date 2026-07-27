# ADR-0005 — Validar API Keys en el gateway y no en la lógica de negocio

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Mathius
Lifecycle: Microservicios-cloud
ASR: ASR-03
Disposition: Integrated as a consequence of ADR-0004 and moved to standards/security/api-keys-en-gateway.md

## Decision

No se mantiene como ADR autónomo. La ubicación de la validación de API Keys es una política de borde derivada de la adopción de Google API Gateway y del requisito explícito del proyecto.

## Context

El proyecto ya exige API Keys por aplicación. El valor del registro es normativo, no una decisión estructural separada. Se conserva como estándar de seguridad para evitar validación duplicada en microservicios.

## Options considered

- **Selected — integrar en ADR-0004 + estándar de seguridad.**
- **Mantener como ADR independiente:** descartado por granularidad excesiva.

## Consequences

**Positive**
- Simplifica el Decision Log y mantiene la regla operativa visible.

**Negative / trade-offs**
- La política debe actualizarse junto con cualquier cambio de gateway.

## Evidence

- `docs/standards/security/api-keys-en-gateway.md`
