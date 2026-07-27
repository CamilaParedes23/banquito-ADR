# ADR-0007 — Adoptar autenticación híbrida con Google OAuth y tokens internos durante la transición

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Author: Mathius
Lifecycle: Microservicios-cloud
ASR: ASR-03
Supersedes: ADR-0006

## Decision

Google Auth Platform autentica al usuario y entrega un ID Token. Identity valida firma, emisor, audiencia, expiración y `email_verified`; exige que el usuario ya exista y esté activo en BanQuito; posteriormente emite los access/refresh tokens internos existentes. El login tradicional se conserva durante la transición.

## Context

La migración completa de identidad en un solo corte eleva el riesgo y rompe compatibilidad con roles, scopes y flujos ya probados. La variante `banquito-core-google-oauth-minimal` implementa un puente verificable sin cambios de base de datos ni creación automática de usuarios. Aún faltan la integración en frontends, despliegue y prueba end-to-end.

## Options considered

- **Selected — modelo híbrido temporal.**
- **Reemplazo total inmediato:** rechazado por riesgo y alcance.
- **Mantener solo login tradicional:** descartado por incumplir la evolución OAuth cloud.
- **Alta automática de usuarios Google:** descartada por riesgo de gobierno de identidades.

## Consequences

**Positive**
- Reduce riesgo de migración y mantiene compatibilidad.
- Google valida identidad externa y BanQuito conserva autorización de negocio.

**Negative / trade-offs**
- Coexisten dos flujos y Identity sigue siendo una pieza central.
- Debe definirse el criterio y fecha de retiro del login tradicional.

## Evidence

- `banquito-core-google-oauth-minimal/identity-access-service/src/main/java/com/banquito/platform/identity/api/controller/AuthController.java`
- `banquito-core-google-oauth-minimal/identity-access-service/src/main/java/com/banquito/platform/identity/infrastructure/security/GoogleIdTokenVerifierService.java`
- `banquito-core-google-oauth-minimal/identity-access-service/src/main/resources/application.yml`
