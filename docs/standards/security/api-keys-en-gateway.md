# Estándar — Validación de API Keys en el gateway

Status: Active; external verification pending
Source: ADR-0005 reclasificado; consecuencia de ADR-0004.

- Core y Switch usan API Keys independientes.
- La validación ocurre en Google API Gateway, no en controllers o services.
- Las llaves deben restringirse por API, ambiente y consumidor antes de producción.
- Los servicios conservan autorización de negocio mediante tokens, roles y scopes.
- Nunca documentar ni versionar el valor de una API Key.
