# Estándar — Correlation ID entre HTTP, gRPC y eventos

Status: Active
Source: ADR-0014 reclasificado.

- Aceptar o generar `X-Correlation-Id` en el borde.
- Propagar el mismo identificador por HTTP, gRPC, eventos, Outbox, logs y respuestas de error.
- No generar un identificador nuevo para el mismo flujo de negocio salvo inicio de una operación independiente.
- Automatizar la propagación mediante filtros/interceptores cuando sea posible.
- Incluir el identificador en evidencia de soporte y auditoría.
