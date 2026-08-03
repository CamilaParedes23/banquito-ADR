# ADR-0015 — Crear un bounded context interbancario separado

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-07

## Decision

Implementar el bounded context Interbancario como un módulo explícito dentro de `core-account-service` durante R9-K. No se crea un microservicio adicional en esta entrega. El módulo mantiene entidades, servicios, repositorios, endpoints y estados propios; las posiciones y asientos permanecen bajo `core-accounting-service`, y el Switch conserva la orquestación y el broker.

## Context

R9-K introduce Nostro/Vostro, liquidación, confirmación, rechazo, reverso y conciliación. Estas responsabilidades no deben mezclarse con las cuentas de clientes ni trasladarse al Switch. Extraer ahora un microservicio independiente agregaría repositorio, base, pipeline y fallos distribuidos sin aportar valor al alcance académico inmediato.

## Options considered

- **Selected — módulo delimitado dentro de Account.** Mantiene separación lógica y reutiliza transacciones, reservas, auditoría, outbox y contratos gRPC existentes.
- **Microservicio Interbancario independiente:** diferido; añade complejidad operativa prematura.
- **Extender sin límites el servicio Account:** descartado; diluye el lenguaje y responsabilidades.
- **Cargar Nostro/Vostro en el Switch:** descartado; el Switch es orquestador, no custodio financiero.

## Consequences

**Positive**
- Preserva el flujo On-Us y evita un despliegue adicional.
- Mantiene una ruta clara de extracción futura si el contexto crece.
- Reutiliza idempotencia, correlación, auditoría y compensaciones del Core.

**Negative / trade-offs**
- Account incorpora un módulo adicional y debe mantener límites de paquete y contrato.
- La consistencia con Accounting sigue siendo distribuida y exige compensación.

## Implementation evidence

- `core-account-service/.../TransferenciaInterbancaria.java`
- `core-account-service/.../InterbankTransferService.java`
- `core-account-service/.../InterbankTransferController.java`
- `V10__interbank_transfer_lifecycle.sql`
