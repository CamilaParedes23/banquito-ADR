# ADR-0018 — Adoptar un modelo bilateral prefondeado para la liquidación interbancaria

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

BanQuito adopta posiciones Nostro/Vostro prefondeadas por contraparte y moneda como modelo bilateral de liquidación interbancaria. Antes de preparar una salida Off-Us se valida la liquidez Nostro; antes de acreditar una entrada se valida la posición Vostro mediante el asiento contable. El seed E2E únicamente inicializa estas posiciones en QA y no constituye la decisión arquitectónica.

## Context

Mientras no exista integración con Banco Central, una cámara de compensación o un mecanismo de liquidación en tiempo real, se requiere un modelo explícito y auditable que controle liquidez, rechazos y posiciones espejo sin crear un fondeo artificial por cada transferencia.

## Options considered

- **Selected — posiciones prefondeadas.**
- **Fondeo en tiempo real por operación:** no viable en el alcance.
- **Sin control de posición:** descartado por permitir saldos interbancarios negativos.

## Consequences

**Positive**
- Simula riesgo de liquidez y rechazo de forma controlada.
- Permite cargas repetibles con un seed único.

**Negative / trade-offs**
- El seed debe mantener posiciones espejo coherentes entre ambos bancos.

## Revisit trigger

Revisar esta decisión cuando exista integración real con Banco Central o una cámara de compensación, liquidación neta, crédito intradía, garantías interbancarias o un esquema distinto de manejo de liquidez.
## Implementation evidence

- `AccountingGrpcClient.validateInterbankPosition`.
- Validación transaccional de saldo en `AccountingService`.
- Cuentas funcionales `NOSTRO_BQLL001_USD` y `VOSTRO_BQLL001_USD`.
