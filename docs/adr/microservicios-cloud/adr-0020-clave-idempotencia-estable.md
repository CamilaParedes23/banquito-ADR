# ADR-0020 — Generar una clave de idempotencia estable en el Switch y propagarla extremo a extremo

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin / Mateo
Lifecycle: Microservicios-cloud
ASR: ASR-02, ASR-07
Supersedes: ADR-0019

## Decision

Usar `paymentLineUuid`, generado una sola vez por el Switch, como llave idempotente end-to-end. En la API banco a banco se envía también en el header `Idempotency-Key`; ambos valores deben coincidir. La identidad persistida es `(sourceRoutingCode, paymentLineUuid)`.

## Context

El `reservationUuid` es interno al Switch y Core emisor y no debe exponerse al banco receptor. Generar nuevas claves por salto impide detectar replays y conciliar una línea única.

## Options considered

- **Selected — `paymentLineUuid` estable y origen bancario.**
- **UUID nuevo por componente:** descartado.
- **Solo UUID de transacción local:** descartado; no representa toda la operación distribuida.

## Consequences

**Positive**
- Un replay equivalente devuelve el resultado ya registrado sin mover dinero.
- Un replay con payload distinto retorna conflicto.

**Negative / trade-offs**
- Todos los adaptadores deben preservar la clave y canonicalizar el payload.

## Implementation evidence

- Índice único `(ROUTING_ORIGEN, PAYMENT_LINE_UUID)`.
- Hash SHA-256 del payload financiero.
- Header `Idempotency-Key` validado contra el cuerpo.
- Métrica `banquito.interbank.idempotency.replays`.
