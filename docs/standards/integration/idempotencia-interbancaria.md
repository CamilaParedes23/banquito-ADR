# Estándar — Contratos idempotentes para operaciones interbancarias

Status: Active
Source: ADR-0019 reclasificado y ADR-0020 aceptado.

- Toda operación Off-Us de escritura exige `paymentLineUuid` estable.
- La API banco a banco exige `Idempotency-Key`; debe coincidir con `paymentLineUuid`.
- La identidad persistida es `(sourceRoutingCode, paymentLineUuid)`.
- La clave se valida antes de producir un nuevo efecto financiero.
- Un replay con payload financiero equivalente retorna el resultado existente y `Idempotency-Replayed: true`.
- La misma clave con payload incompatible produce HTTP 409, código `INTERBANK_IDEMPOTENCY_PAYLOAD_CONFLICT` y auditoría.
- `correlationId` no reemplaza la idempotencia: solo sirve para trazabilidad.
- No se registran UUID, cuentas, identificaciones ni correlation IDs como etiquetas de métricas.
