# Arquitectura — Banco BanQuito

Documentación de arquitectura del proyecto Banco BanQuito: Core de Cuentas (monolito V1 → microservicios V2: Transaccional + Contable) y Switch de Pagos Masivos (monolito síncrono V1 → distribuido, orientado a eventos V2).

## Contenido

- **[ADR](adr/index.md)** — Registro de decisiones arquitectónicas (Decision Log), plantilla y ADR individuales.
- **Arquitectura** — Diagramas de contexto, componentes y evolución V1/V2 de cada sistema.
- **Seguridad** — API Management, autenticación, autorización y gestión de secretos.
- **Pruebas** — Estrategia y cobertura de pruebas unitarias (Core y Switch).
- **Interbancario** — Documentación del contexto funcional interbancario (bloque R9-K).
- **Evidencia** — Material de respaldo, incluyendo las versiones originales (pre-normalización) de los ADR históricos.

## Formato de ADR

El proyecto usa el **formato institucional compacto**: Título, metaelementos (Status, Date, Author), `Decision` y `Context` únicamente. No se usan secciones de Options, Consequences ni Advice de forma independiente; las alternativas y trade-offs relevantes se resumen dentro de `Context`.
