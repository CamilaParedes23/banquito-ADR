# ADR-0017 — Mantener un único plan contable para operaciones On-Us y Off-Us

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Las operaciones interbancarias (Off-Us) se registran contablemente en el mismo Plan Único de Cuentas y el mismo Microservicio Contable ya definidos en Core V2, agregando cuentas institucionales nuevas bajo la jerarquía existente (por ejemplo, bajo `1.1.0.01 Banco Central / Cámara de Compensación`). No se crea un plan contable ni un libro mayor paralelo para operaciones Off-Us.

## Context

El anexo de modelo de base de datos del Core V2 ya define `ACCOUNTING_ACCOUNT` con relación recursiva (`FK_ACCOUNTING_PARENT`) para modelar el árbol contable, y reserva explícitamente la cuenta `1.1.0.01` para "el dinero neto que se envía o recibe de otros bancos (Esencial para la V2 del Switch)".

Problema/ASR: llevar dos libros mayores —uno para On-Us, otro para Off-Us— rompería la garantía de suma cero a nivel de todo el banco y duplicaría el proceso EOD y el Balance de Comprobación exigidos en el RF-09 de Core V2.

Restricciones: toda operación, sea On-Us u Off-Us, debe seguir generando un asiento contable (`JOURNAL_ENTRY`) cuya suma de débitos sea igual a la suma de créditos.

Alternativas consideradas: un libro mayor separado para el contexto Interbancario (descartada, complicaría el cierre EOD único que exige el RF-09 y duplicaría la lógica de validación de cuadre); no distinguir contablemente On-Us de Off-Us (descartada, el banco necesita poder auditar por separado su exposición con cada institución externa).

Criterio de selección: reutilizar el Plan Único de Cuentas y el Microservicio Contable ya construidos evita duplicar la lógica de partida doble, EOD y particionamiento ya implementada.

Impacto: el contexto Interbancario (ADR-0015) es cliente del Microservicio Contable existente, vía gRPC (ver ADR-0009), no un motor contable independiente.

## Opciones consideradas

- **Un único Plan de Cuentas y Microservicio Contable para On-Us y Off-Us (adoptada)**: agrega cuentas institucionales nuevas bajo la jerarquía existente. Ventaja: reutiliza el Plan Único de Cuentas y el Microservicio Contable ya construidos, evitando duplicar la lógica de partida doble, EOD y particionamiento ya implementada.
- **Un libro mayor separado para el contexto Interbancario**: descartada por complicar el cierre EOD único que exige el RF-09 y duplicar la lógica de validación de cuadre.
- **No distinguir contablemente On-Us de Off-Us**: descartada porque el banco necesita poder auditar por separado su exposición con cada institución externa.

## Consecuencias

Positivas:
- Preserva la garantía de suma cero a nivel de todo el banco, sin necesidad de conciliar dos libros mayores distintos.
- Evita duplicar el proceso EOD y el Balance de Comprobación exigidos en el RF-09 de Core V2.

Negativas:
- El contexto Interbancario queda funcionalmente dependiente del Microservicio Contable existente (vía gRPC, ADR-0009); no puede evolucionar su modelo contable de forma independiente.
- Toda operación Off-Us nueva debe encajar en la jerarquía recursiva de `ACCOUNTING_ACCOUNT` ya definida, lo que limita la flexibilidad de modelado específico para el dominio interbancario.
