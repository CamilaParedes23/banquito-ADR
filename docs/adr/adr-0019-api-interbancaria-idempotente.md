# ADR-0019 — Exponer una API interbancaria idempotente

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Toda API que el contexto Interbancario exponga para recibir o consultar operaciones Off-Us exige una clave de idempotencia en la petición. Una misma clave nunca genera dos movimientos contables ni dos liquidaciones.

## Context

El Core V1 ya define este patrón para el dominio de cuentas: el RF-06 ("Trazabilidad e Idempotencia — Soporte al Switch") exige que el Core registre un UUID de transacción y rechace por "Duplicidad" si se repite el mismo día. El ADR histórico R9-E también reutiliza "las reglas de idempotencia existentes" para reintentos de cobro de comisión.

Problema/ASR: las comunicaciones con la Cámara de Compensación (sistema externo) son más propensas a timeouts y reintentos que las internas, y un reintento sin idempotencia podría duplicar un pago hacia otro banco.

Restricciones: debe ser compatible con el mecanismo de idempotencia ya implementado en el Core (registro de idempotencia en `core-account-service`), no un mecanismo nuevo y distinto.

Alternativas consideradas: no exigir idempotencia y confiar en que el llamador (Switch) nunca reintente (descartada, contradice el RF-01/RF-04 de Core V2, que ya contemplan timeouts y reversos como parte normal del flujo); deduplicar solo por contenido del payload sin clave explícita (descartada, dos pagos legítimos con el mismo monto y beneficiario en el mismo día serían indistinguibles).

Criterio de selección: extender el patrón de idempotencia ya validado en el Core (UUID más rechazo por duplicidad) en vez de diseñar uno nuevo para el contexto Interbancario.

Impacto: toda operación Off-Us debe originarse con una clave de idempotencia generada por quien la origina (ver ADR-0020) y propagarse sin modificación hasta la Cámara de Compensación.

## Opciones consideradas

- **Clave de idempotencia obligatoria en toda API interbancaria (adoptada)**: extiende el patrón de idempotencia ya validado en el Core (UUID más rechazo por duplicidad, RF-06) en vez de diseñar uno nuevo. Ventaja: compatible con el mecanismo ya implementado en `core-account-service`.
- **No exigir idempotencia y confiar en que el llamador (Switch) nunca reintente**: descartada por contradecir el RF-01/RF-04 de Core V2, que ya contemplan timeouts y reversos como parte normal del flujo.
- **Deduplicar solo por contenido del payload sin clave explícita**: descartada porque dos pagos legítimos con el mismo monto y beneficiario en el mismo día serían indistinguibles.

## Consecuencias

Positivas:
- Reduce el riesgo de duplicar un pago hacia otro banco ante timeouts o reintentos en la comunicación con la Cámara de Compensación.
- Reutiliza un patrón ya validado en el Core, sin necesidad de diseñar ni auditar un mecanismo de idempotencia nuevo.

Negativas:
- Depende de que la clave se genere una sola vez y se propague sin cambios en todos los saltos del flujo (ver ADR-0020); un error de propagación anula la protección.
- Todo componente que exponga una API interbancaria queda obligado a validar la clave de idempotencia, lo que añade una responsabilidad adicional a cada integración nueva.
