# Estándar — Contratos idempotentes para operaciones interbancarias

Status: Draft; depends on ADR-0020
Source: ADR-0019 reclasificado.

- Toda operación Off-Us de escritura exige una idempotency key.
- La clave se valida antes de producir un nuevo efecto financiero.
- Un replay con payload equivalente retorna el resultado existente.
- La misma clave con payload incompatible produce conflicto y auditoría.
- El formato y punto de generación se decidirán en ADR-0020.
