# ADR-0020 — Mantener una clave de idempotencia estable entre bancos

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

La clave de idempotencia de una operación Off-Us se genera una sola vez, en el punto de origen (Switch), y se propaga sin cambios a través de todos los saltos (contexto Interbancario, Cámara de Compensación simulada) hasta la confirmación final. Ningún componente intermedio genera una clave nueva para la misma operación lógica.

## Context

Depende directamente de ADR-0019. El documento Switch V2 describe un flujo con múltiples saltos para una operación Off-Us: Portal Banca Web → API Gateway → Switch (evento en Message Broker) → Enrutamiento Dinámico → Cola de Salida → Cámara de Compensación.

Problema/ASR: si cada salto regenerara su propia clave de idempotencia, un reintento en cualquier punto del flujo podría producir un duplicado, porque el componente que reintenta no reconocería la operación original como "la misma".

Restricciones: la clave debe sobrevivir al cambio de protocolo, del evento interno del Message Broker al mensaje saliente hacia la Cámara de Compensación.

Alternativas consideradas: generar una clave nueva en cada componente del flujo (descartada, rompe la trazabilidad de extremo a extremo y anula el propósito de ADR-0019); usar únicamente el UUID de transacción del Core como clave interbancaria (descartada, ese UUID identifica el movimiento contable local On-Us/Off-Us dentro del Core, no la operación de cara al banco externo, que puede requerir su propio formato).

Criterio de selección: una clave de idempotencia estable de extremo a extremo es el único diseño que garantiza que un reintento en cualquier salto sea detectable como duplicado en todos los sistemas involucrados.

Impacto: el formato exacto de la clave (UUID v4, o compuesto con código de institución) queda `[PENDIENTE DE CONFIRMAR: Mateo]`; debe documentarse en el contrato de la API interbancaria (ADR-0019).

## Opciones consideradas

- **Clave generada una sola vez en el origen (Switch) y propagada sin cambios (adoptada)**: sobrevive a todos los saltos del flujo (Message Broker → Enrutamiento → Cola de Salida → Cámara de Compensación). Ventaja: es el único diseño que garantiza que un reintento en cualquier salto sea detectable como duplicado en todos los sistemas involucrados.
- **Generar una clave nueva en cada componente del flujo**: descartada por romper la trazabilidad de extremo a extremo y anular el propósito de ADR-0019.
- **Usar únicamente el UUID de transacción del Core como clave interbancaria**: descartada porque ese UUID identifica el movimiento contable local On-Us/Off-Us dentro del Core, no la operación de cara al banco externo, que puede requerir su propio formato.

## Consecuencias

Positivas:
- Un reintento en cualquier punto del flujo (Switch, contexto Interbancario, Cámara de Compensación simulada) es detectable como duplicado, sin importar en qué salto ocurra.
- Simplifica la conciliación de operaciones pendientes (ver ADR-0021), ya que la misma clave identifica la operación en todos los sistemas.

Negativas:
- La clave debe sobrevivir al cambio de protocolo, del evento interno del Message Broker al mensaje saliente hacia la Cámara de Compensación, lo que exige disciplina de propagación en cada componente intermedio.
- El formato exacto de la clave (UUID v4, o compuesto con código de institución) queda `[PENDIENTE DE CONFIRMAR: Mateo]`, lo que bloquea la definición final del contrato de la API interbancaria (ADR-0019).
