# Matriz de depuración ADR

Esta matriz documenta la revisión teórica y la disposición aplicada el 25 de julio de 2026.

| ID | Registro | Estado previo | Estado nuevo | Implementación | Etapa | Acción | Disposición |
|---|---|---|---|---|---|---|---|
| ADR-MON-0001 | Integrar Switch y Core mediante APIs REST internas y bases independientes | New retrospective | Accepted | Verified | Monolito | Keep | — |
| ADR-MON-0002 | Aplicar restricciones de base de datos como última línea de defensa financiera | New retrospective | Accepted | Verified | Monolito | Keep | — |
| ADR-MON-0003 | Separar las cuentas institucionales de las cuentas de clientes | New retrospective | Accepted | Verified | Monolito | Keep | — |
| ADR-MON-0004 | Separar credenciales de clientes y usuarios operativos del banco | New retrospective | Accepted | Verified | Monolito | Keep | — |
| ADR R9-E | Liquidar la comisión después del lote con sobregiro controlado | Accepted | Accepted | Verified | Microservicios | Keep | — |
| ADR R9-F.6 | Restringir el EOD a la ventana operativa con excepciones auditadas | Accepted | Accepted | Verified | Microservicios | Keep | — |
| ADR-0009 | Usar gRPC para comunicaciones síncronas internas del Core | Proposed | Accepted | Verified | Microservicios | Accept / reformulate | — |
| ADR-0010 | Limitar el Message Broker al Switch y mantener el Core financiero síncrono | Proposed | Accepted | Verified | Microservicios | Accept / reformulate | — |
| ADR-0001 | Mantener una base de código única parametrizada por entorno | Proposed | Accepted | Verified | Microservicios-cloud | Accept / reformulate | — |
| ADR-0002 | Gestionar la documentación arquitectónica como código | Proposed | Accepted | Verified | Microservicios-cloud | Accept / reformulate | — |
| ADR-0003 | Separar configuración local y cloud mediante perfiles | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Merged into ADR-0001; implementation guidance moved to standards/configuration/perfiles-y-variables-entorno.md |
| ADR-0004 | Adoptar Google API Gateway como punto de entrada administrado para Core y Switch | Proposed | Accepted | In progress | Microservicios-cloud | Accept / reformulate | — |
| ADR-0005 | Validar API Keys en el gateway y no en la lógica de negocio | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Integrated as a consequence of ADR-0004 and moved to standards/security/api-keys-en-gateway.md |
| ADR-0006 | Sustituir completamente los JWT internos por OAuth 2 administrado | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Rejected because the selected implementation is hybrid, not a full replacement |
| ADR-0007 | Adoptar autenticación híbrida con Google OAuth y tokens internos durante la transición | Proposed | Accepted | In progress | Microservicios-cloud | Accept / reformulate | — |
| ADR-0008 | Adoptar Google Secret Manager con IAM y Workload Identity para secretos operativos | Proposed | Proposed | Not started | Microservicios-cloud | Keep proposed | — |
| ADR-0011 | Exigir 70 % de cobertura en controllers y services del Core | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Moved to testing standard; the threshold is an explicit project requirement |
| ADR-0012 | Exigir 70 % de cobertura en controllers y services del Switch | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Merged with ADR-0011 into a single testing standard |
| ADR-0013 | Instrumentar servicios con Actuator y Micrometer | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Moved to observability standard; tool configuration is not retained as a standalone strategic decision |
| ADR-0014 | Propagar correlación en HTTP y gRPC | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Moved to traceability standard; correlation is a mandatory cross-cutting practice |
| ADR-0015 | Crear un bounded context interbancario separado | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0016 | Representar Nostro/Vostro fuera de las cuentas de clientes | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0017 | Mantener un único plan contable para operaciones On-Us y Off-Us | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0018 | Utilizar posiciones interbancarias prefondeadas para la simulación R9-K | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0019 | Exponer una API interbancaria idempotente | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Merged into ADR-0020; API-level rules moved to standards/integration/idempotencia-interbancaria.md |
| ADR-0020 | Generar una clave de idempotencia estable en el Switch y propagarla extremo a extremo | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0021 | Tratar timeouts remotos como pendientes de conciliación | Proposed | Accepted | In progress | Microservicios-cloud | Implement R9-K | Implementado y validado localmente con E2E10/E2E100; pendiente validación QA/staging cloud |
| ADR-0022 | Incorporar un módulo interbancario en los frontends | Proposed | Rejected | Not applicable | Microservicios-cloud | Reclassify / merge | Moved to functional/interbancario-frontend.md |
