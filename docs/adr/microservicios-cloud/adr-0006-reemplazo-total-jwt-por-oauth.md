# ADR-0006 — Sustituir completamente los JWT internos por OAuth 2 administrado

Status: Rejected
Implementation Status: Not applicable
Date: 2026-07-25
Author: Mathius
Lifecycle: Microservicios-cloud
ASR: ASR-03
Superseded by: ADR-0007
Disposition: Rejected because the selected implementation is hybrid, not a full replacement

## Decision

No se adopta la sustitución completa de los tokens internos. BanQuito conserva temporalmente el login tradicional y la emisión de access/refresh tokens internos.

## Context

La implementación disponible valida Google ID Tokens, relaciona el correo con un usuario local existente y luego emite los tokens internos vigentes. La propuesta original de eliminar completamente JWT propio no coincide con la estrategia implementada ni con la necesidad de migración gradual.

## Options considered

- **Rejected — reemplazo total inmediato.**
- **Selected in ADR-0007 — autenticación híbrida y transición gradual.**

## Consequences

**Positive**
- Evita documentar una arquitectura que no corresponde al código.

**Negative / trade-offs**
- Durante la transición continúan coexistiendo dos mecanismos de autenticación.

## Evidence

- `identity-access-service/.../AuthController.java`
- `identity-access-service/.../GoogleIdTokenVerifierService.java`
- `identity-access-service/.../AuthenticationService.java`
