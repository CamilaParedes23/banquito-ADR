# ADR-0004 — Adoptar Google API Gateway como punto de entrada administrado para Core y Switch

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Author: Mathius / Luis
Lifecycle: Microservicios-cloud
ASR: ASR-03, ASR-04, ASR-05

## Decision

El tráfico externo hacia Core y Switch se publica mediante Google API Gateway. Se mantienen APIs y API Keys independientes por aplicación y una cuenta de servicio para el gateway. Los microservicios no se exponen directamente a internet.

## Context

El documento final exige un API Manager cloud y API Keys por aplicación. La propuesta original mencionaba Apigee, pero la implementación real usa Google API Gateway. Ya están habilitadas las APIs, dos llaves y la cuenta de servicio; falta desplegar backends, publicar OpenAPI, restringir llaves y probar el flujo end-to-end.

## Options considered

- **Selected — Google API Gateway:** menor complejidad para el alcance académico y servicios ya configurados.
- **Apigee:** descartado en esta etapa por mayor alcance/costo operativo frente a las necesidades actuales.
- **Gateway autogestionado:** descartado por operación adicional.
- **Exposición directa:** descartada por seguridad y gobierno.

## Consequences

**Positive**
- Centraliza borde, cuotas, API Keys y métricas de acceso.
- Separa políticas de entrada de la lógica de negocio.

**Negative / trade-offs**
- Introduce dependencia de Google Cloud y configuración OpenAPI.
- La publicación depende de URLs reales y restricciones correctas de las llaves.

## Evidence

- `Declaración del equipo: API Gateway/API Core/API Switch creadas, 2 API Keys y service account configuradas (2026-07-25)`
- `banquito-core-google-oauth-minimal.zip`
