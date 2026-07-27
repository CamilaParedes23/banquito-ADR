# ADR-0018 — Utilizar posiciones interbancarias prefondeadas para la simulación R9-K

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

La simulación R9-K mantendrá posiciones prefondeadas para pagos Off-Us y controlará disponibilidad antes de enviar instrucciones de compensación.

## Context

El proyecto no se conectará a un Banco Central real. El prefondeo permite representar liquidez, rechazos y conciliación sin solicitar fondos por cada transacción. Debe confirmarse como regla de negocio antes de aceptar.

## Options considered

- **Proposed — prefondeo.**
- **Fondeo en tiempo real por operación:** no viable en la simulación actual.
- **Sin control de posición:** descartado por inconsistencia financiera.

## Consequences

**Positive**
- Simula riesgo de liquidez de forma controlada.

**Negative / trade-offs**
- Requiere seeds, límites y reglas de reposición.

## Evidence

- `Alcance R9-K; pendiente de aprobación de negocio`
