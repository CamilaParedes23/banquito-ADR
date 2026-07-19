# ADR-0006 — Sustituir la emisión JWT propia por OAuth2 administrado

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

La emisión y validación de tokens de acceso migra de JWT generado internamente por cada microservicio (librería `jjwt`) a un proveedor OAuth2 administrado en el entorno cloud. Los microservicios dejan de firmar y emitir tokens propios.

## Context

Estado actual: `banquito-core-admin` (y, por el mismo patrón de seguridad, presumiblemente los demás microservicios del Core) usa `io.jsonwebtoken:jjwt-api/impl/jackson` para emitir y validar JWT propios, con roles verificados vía `@PreAuthorize` en los controllers.

Problema/ASR: la emisión propia de JWT implica gestionar rotación de llaves de firma, expiración y revocación manualmente en cada servicio, lo que no escala ni es auditable en un despliegue cloud con múltiples microservicios y un API Gateway centralizado (ADR-0004).

Restricciones: `[PENDIENTE DE CONFIRMAR: proveedor OAuth2 específico a adoptar — Mathius/Luis]`; la migración debe ser gradual para no romper los microservicios ya probados (ver ADR-0007).

Alternativas consideradas: mantener JWT propio con rotación manual de llaves (descartada, riesgo operativo y sin soporte de introspección centralizada); usar sesiones con estado en servidor (descartada, incompatible con la arquitectura de microservicios sin estado ya adoptada).

Criterio de selección: un proveedor OAuth2 administrado centraliza emisión, expiración, revocación y auditoría de tokens, y se integra de forma natural con Apigee (ADR-0004) como punto de validación de borde.

Impacto: los microservicios deben migrar de validar firmas JWT propias a validar tokens emitidos por el proveedor OAuth2 (introspección o JWKS); afecta a todos los servicios que actualmente dependen de `jjwt`.
