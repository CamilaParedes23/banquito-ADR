# ADR-0015 — Crear un bounded context interbancario separado

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-07

## Decision

Crear un contexto delimitado Interbancario, separado de Cuentas y del Switch, responsable de Nostro/Vostro, compensación, liquidación y conciliación con otras instituciones.

## Context

El alcance R9-K introduce un lenguaje y responsabilidades que no pertenecen al Core de cuentas ni al enrutamiento del Switch. Aún debe resolverse si se materializa como microservicio independiente o como módulo con límites explícitos; por ello permanece Proposed.

## Options considered

- **Proposed — bounded context separado.**
- **Extender Account:** descartado por mezclar cuentas de clientes y posiciones bancarias.
- **Cargar el modelo en el Switch:** descartado porque el Switch es orquestador.

## Consequences

**Positive**
- Protege cohesión y lenguaje ubicuo.

**Negative / trade-offs**
- Añade un límite desplegable y contratos nuevos.

## Evidence

- `Alcance R9-K definido por el equipo`
- `BancoBanQuito-RequisitosFuncionales-SwitchPagosMasivosV2.pdf`
