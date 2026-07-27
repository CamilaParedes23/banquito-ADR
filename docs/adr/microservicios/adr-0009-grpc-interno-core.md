# ADR-0009 — Usar gRPC para comunicaciones síncronas internas del Core

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios
ASR: ASR-01, ASR-04, ASR-05

## Decision

Las comunicaciones internas críticas del Core —Account con Accounting, Admin y Customer, y Accounting con Admin— usan gRPC y contratos Protobuf versionados. REST se mantiene para canales y APIs de borde.

## Context

Core V2 permitía REST o gRPC. Se seleccionó gRPC para contratos tipados, baja latencia y deadlines explícitos en operaciones financieras. La implementación R9-J contiene clientes, servidores y contratos Protobuf. El costo asumido es una coordinación más estricta de versiones.

## Options considered

- **Selected — gRPC interno y REST en el borde.**
- **REST para todas las interacciones:** descartado por menor tipado y mayor payload en flujos críticos.
- **Mensajería asíncrona para el Core:** descartada para confirmaciones financieras inmediatas.

## Consequences

**Positive**
- Contratos fuertes y comunicación eficiente.
- Permite deadlines y errores técnicos explícitos.

**Negative / trade-offs**
- Los cambios incompatibles en Protobuf requieren coordinación.
- La depuración distribuida exige trazabilidad y herramientas adicionales.

## Evidence

- `banquito-core-google-oauth-minimal/contracts/proto/*.proto`
- `core-account-service/.../AccountingGrpcClient.java`
- `core-accounting-service/.../AccountingGrpcService.java`
