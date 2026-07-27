# ADR-MON-0001 — Integrar Switch y Core mediante APIs REST internas y bases independientes

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Equipo Banco BanQuito
Lifecycle: Monolito
ASR: ASR-01, ASR-02, ASR-05

## Decision

El Switch V1 no accede directamente a la base de datos del Core. Core y Switch conservan almacenes relacionales independientes y se integran mediante APIs REST internas. Los intercambios usan UUID, referencias externas y correlation IDs para trazabilidad.

## Context

La documentación V1 exigía integración entre el Switch y el Core, pero no obligaba a compartir una base de datos. El código monolítico confirma un Core con MariaDB, un Switch con PostgreSQL y una integración configurada por `core.base-url`. Compartir tablas habría acoplado ambos ciclos de cambio y habría permitido al Switch alterar saldos sin pasar por las reglas del Core. Esta decisión se documenta retrospectivamente a partir del código y la documentación formal de abril de 2026.

## Options considered

- **Selected — REST interno con bases independientes:** mantiene ownership de datos y contratos explícitos.
- **Acceso directo del Switch a la base del Core:** descartado por acoplamiento y riesgo financiero.
- **Base de datos compartida entre Core y Switch:** descartada por mezclar modelos y ciclos de despliegue.

## Consequences

**Positive**
- El Core permanece como única autoridad sobre saldos y transacciones.
- Los cambios del Switch no requieren acceso ni migraciones sobre la base del Core.

**Negative / trade-offs**
- La integración depende de la red y requiere manejo de timeouts, contratos y reintentos.
- La consistencia entre sistemas debe resolverse mediante idempotencia y trazabilidad, no mediante una transacción local compartida.

## Evidence

- `banquito-monolito/core/README.md`
- `banquito-monolito/core/pom.xml`
- `banquito-monolito/switch/README.md`
- `banquito-monolito/switch/pom.xml`
