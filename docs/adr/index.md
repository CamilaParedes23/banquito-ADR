# Decision Log — Banco BanQuito

Este registro contiene decisiones arquitectónicas históricas y candidatas.
Los ADR históricos conservan sus identificadores originales. Los ADR nuevos utilizan numeración secuencial `ADR-0001`, `ADR-0002`, etc.

| ID | Título | Estado | Responsable técnico | Bloque | Archivo |
|---|---|---|---|---|---|
| ADR R9-E | Liquidar la comisión después del lote con sobregiro controlado | Accepted | Lenin | Histórico | [adr-r9-e-comision-sobregiro-controlado.md](adr-r9-e-comision-sobregiro-controlado.md) |
| ADR R9-F.6 | Restringir el EOD a la ventana operativa con excepciones auditadas | Accepted | Lenin | Histórico | [adr-r9-f6-control-ventana-eod.md](adr-r9-f6-control-ventana-eod.md) |
| ADR-0001 | Mantener una sola base de código para local y cloud | Proposed | Lenin | R9-J.5 | [adr-0001-mantener-una-sola-base-de-codigo.md](adr-0001-mantener-una-sola-base-de-codigo.md) |
| ADR-0002 | Gestionar la documentación arquitectónica como código | Proposed | Kevin | R9-J.5 | [adr-0002-documentacion-como-codigo.md](adr-0002-documentacion-como-codigo.md) |
| ADR-0003 | Separar configuración local y cloud mediante perfiles | Proposed | Mathius | R9-J.5 | [adr-0003-perfiles-local-cloud.md](adr-0003-perfiles-local-cloud.md) |
| ADR-0004 | Usar Apigee como API Manager del despliegue cloud | Proposed | Mathius | R9-J.5 | [adr-0004-apigee-api-manager-cloud.md](adr-0004-apigee-api-manager-cloud.md) |
| ADR-0005 | Validar API Keys en el API Manager y no en la lógica de negocio | Proposed | Mathius | R9-J.5 | [adr-0005-validar-api-keys-en-gateway.md](adr-0005-validar-api-keys-en-gateway.md) |
| ADR-0006 | Sustituir la emisión JWT propia por OAuth2 administrado | Proposed | Mathius | R9-J.5 | [adr-0006-oauth2-administrado.md](adr-0006-oauth2-administrado.md) |
| ADR-0007 | Transformar Identity mediante una transición gradual | Proposed | Mathius | R9-J.5 | [adr-0007-transicion-gradual-identity.md](adr-0007-transicion-gradual-identity.md) |
| ADR-0008 | Externalizar secretos mediante un servicio administrado | Proposed | [PENDIENTE DE CONFIRMAR] | R9-J.5 | [adr-0008-secretos-administrados.md](adr-0008-secretos-administrados.md) |
| ADR-0009 | Mantener gRPC para comunicaciones internas del Core | Proposed | Lenin | R9-J.5 | [adr-0009-grpc-interno-core.md](adr-0009-grpc-interno-core.md) |
| ADR-0010 | Restringir el broker administrado al Switch | Proposed | Mateo | R9-J.5 | [adr-0010-broker-limitado-al-switch.md](adr-0010-broker-limitado-al-switch.md) |
| ADR-0011 | Medir al menos 70 % de cobertura en controllers y services del Core | Proposed | Kevin | R9-J.5 | [adr-0011-cobertura-70-core.md](adr-0011-cobertura-70-core.md) |
| ADR-0012 | Medir al menos 70 % de cobertura en controllers y services del Switch | Proposed | Julio | R9-J.5 | [adr-0012-cobertura-70-switch.md](adr-0012-cobertura-70-switch.md) |
| ADR-0013 | Instrumentar servicios con Actuator y Micrometer | Proposed | Mathius | R9-J.5 | [adr-0013-actuator-micrometer.md](adr-0013-actuator-micrometer.md) |
| ADR-0014 | Propagar correlación en HTTP y gRPC | Proposed | Lenin | R9-J.5 | [adr-0014-correlacion-http-grpc.md](adr-0014-correlacion-http-grpc.md) |
| ADR-0015 | Crear un contexto funcional interbancario separado | Proposed | Lenin | R9-K | [adr-0015-contexto-interbancario-separado.md](adr-0015-contexto-interbancario-separado.md) |
| ADR-0016 | Representar Nostro/Vostro fuera de las cuentas de clientes | Proposed | Lenin | R9-K | [adr-0016-nostro-vostro-fuera-cuentas-cliente.md](adr-0016-nostro-vostro-fuera-cuentas-cliente.md) |
| ADR-0017 | Mantener un único plan contable para operaciones On-Us y Off-Us | Proposed | Lenin | R9-K | [adr-0017-plan-contable-unico-onus-offus.md](adr-0017-plan-contable-unico-onus-offus.md) |
| ADR-0018 | Utilizar posiciones interbancarias prefondeadas | Proposed | Lenin | R9-K | [adr-0018-posiciones-prefondeadas.md](adr-0018-posiciones-prefondeadas.md) |
| ADR-0019 | Exponer una API interbancaria idempotente | Proposed | Lenin | R9-K | [adr-0019-api-interbancaria-idempotente.md](adr-0019-api-interbancaria-idempotente.md) |
| ADR-0020 | Mantener una clave de idempotencia estable entre bancos | Proposed | Lenin | R9-K | [adr-0020-clave-idempotencia-estable.md](adr-0020-clave-idempotencia-estable.md) |
| ADR-0021 | Tratar timeouts remotos como operaciones pendientes de conciliación | Proposed | Lenin + Mateo | R9-K | [adr-0021-timeout-pendiente-conciliacion.md](adr-0021-timeout-pendiente-conciliacion.md) |
| ADR-0022 | Incorporar un módulo Interbancario en el frontend del Core | Proposed | Kevin, Julio, Lenin | R9-K | [adr-0022-modulo-interbancario-frontend.md](adr-0022-modulo-interbancario-frontend.md) |

## Reglas

1. `Proposed` no autoriza por sí solo la implementación definitiva.
2. La decisión se implementa cuando el ADR es aprobado como `Accepted`.
3. Un ADR aceptado no se reescribe.
4. Si la decisión cambia, se crea un ADR nuevo y el anterior pasa a `Superseded`.
5. Kevin mantiene la numeración, el índice y el estado documental; el responsable técnico aporta y valida el contenido.
