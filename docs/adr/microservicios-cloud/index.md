# ADR — Microservicios-cloud

Registros asociados a la etapa **Microservicios-cloud**.

| ID | Decisión | Status | Implementation | ASR |
|---|---|---|---|---|
| ADR-0001 | [Mantener una base de código única parametrizada por entorno](adr-0001-mantener-una-sola-base-de-codigo.md) | Accepted | Verified | ASR-06 |
| ADR-0002 | [Gestionar la documentación arquitectónica como código](adr-0002-documentacion-como-codigo.md) | Accepted | Verified | ASR-04, ASR-06 |
| ADR-0003 | [Separar configuración local y cloud mediante perfiles](adr-0003-perfiles-local-cloud.md) | Rejected | Not applicable | ASR-06 |
| ADR-0004 | [Adoptar Google API Gateway como punto de entrada administrado para Core y Switch](adr-0004-google-api-gateway-cloud.md) | Accepted | In progress | ASR-03, ASR-04, ASR-05 |
| ADR-0005 | [Validar API Keys en el gateway y no en la lógica de negocio](adr-0005-validar-api-keys-en-gateway.md) | Rejected | Not applicable | ASR-03 |
| ADR-0006 | [Sustituir completamente los JWT internos por OAuth 2 administrado](adr-0006-reemplazo-total-jwt-por-oauth.md) | Rejected | Not applicable | ASR-03 |
| ADR-0007 | [Adoptar autenticación híbrida con Google OAuth y tokens internos durante la transición](adr-0007-autenticacion-hibrida-google-oauth.md) | Accepted | In progress | ASR-03 |
| ADR-0008 | [Adoptar Google Secret Manager con IAM y Workload Identity para secretos operativos](adr-0008-google-secret-manager.md) | Proposed | Not started | ASR-03 |
| ADR-0011 | [Exigir 70 % de cobertura en controllers y services del Core](adr-0011-cobertura-70-core.md) | Rejected | Not applicable | ASR-06 |
| ADR-0012 | [Exigir 70 % de cobertura en controllers y services del Switch](adr-0012-cobertura-70-switch.md) | Rejected | Not applicable | ASR-06 |
| ADR-0013 | [Instrumentar servicios con Actuator y Micrometer](adr-0013-actuator-micrometer.md) | Rejected | Not applicable | ASR-04, ASR-05 |
| ADR-0014 | [Propagar correlación en HTTP y gRPC](adr-0014-correlacion-http-grpc.md) | Rejected | Not applicable | ASR-04 |
| ADR-0015 | [Crear un bounded context interbancario separado](adr-0015-contexto-interbancario-separado.md) | Accepted | In progress | ASR-07 |
| ADR-0016 | [Representar Nostro/Vostro fuera de las cuentas de clientes](adr-0016-nostro-vostro-fuera-cuentas-cliente.md) | Accepted | In progress | ASR-01, ASR-07 |
| ADR-0017 | [Mantener un único plan contable para operaciones On-Us y Off-Us](adr-0017-plan-contable-unico-onus-offus.md) | Accepted | In progress | ASR-01, ASR-07 |
| ADR-0018 | [Utilizar posiciones interbancarias prefondeadas para la simulación R9-K](adr-0018-posiciones-prefondeadas.md) | Accepted | In progress | ASR-01, ASR-07 |
| ADR-0019 | [Exponer una API interbancaria idempotente](adr-0019-api-interbancaria-idempotente.md) | Rejected | Not applicable | ASR-02, ASR-07 |
| ADR-0020 | [Generar una clave de idempotencia estable en el Switch y propagarla extremo a extremo](adr-0020-clave-idempotencia-estable.md) | Accepted | In progress | ASR-02, ASR-07 |
| ADR-0021 | [Tratar timeouts remotos como pendientes de conciliación](adr-0021-timeout-pendiente-conciliacion.md) | Accepted | In progress | ASR-01, ASR-02, ASR-05, ASR-07 |
| ADR-0022 | [Incorporar un módulo interbancario en los frontends](adr-0022-modulo-interbancario-frontend.md) | Rejected | Not applicable | ASR-07 |
