# Decision Log — Banco BanQuito

El registro depurado contiene **28 decisiones/registros**: **12 Accepted**, **7 Proposed** y **9 Rejected/reclasificados**.

`Status` representa el estado de la decisión. `Implementation Status` representa cuánto se ha materializado; una decisión puede estar Accepted y todavía In progress.

| ID | Título | Status | Implementation | Etapa | Responsable | ASR |
|---|---|---|---|---|---|---|
| ADR-MON-0001 | [Integrar Switch y Core mediante APIs REST internas y bases independientes](monolito/adr-mon-0001-integracion-rest-core-switch.md) | Accepted | Verified | Monolito | Equipo Banco BanQuito | ASR-01, ASR-02, ASR-05 |
| ADR-MON-0002 | [Aplicar restricciones de base de datos como última línea de defensa financiera](monolito/adr-mon-0002-invariantes-financieras-base-datos.md) | Accepted | Verified | Monolito | Equipo Banco BanQuito | ASR-01, ASR-02 |
| ADR-MON-0003 | [Separar las cuentas institucionales de las cuentas de clientes](monolito/adr-mon-0003-cuentas-institucionales-separadas.md) | Accepted | Verified | Monolito | Equipo Banco BanQuito | ASR-01, ASR-03 |
| ADR-MON-0004 | [Separar credenciales de clientes y usuarios operativos del banco](monolito/adr-mon-0004-identidades-clientes-operadores-separadas.md) | Accepted | Verified | Monolito | Equipo Banco BanQuito | ASR-03 |
| ADR R9-E | [Liquidar la comisión después del lote con sobregiro controlado](microservicios/adr-r9-e-comision-sobregiro-controlado.md) | Accepted | Verified | Microservicios | Lenin | ASR-01, ASR-02 |
| ADR R9-F.6 | [Restringir el EOD a la ventana operativa con excepciones auditadas](microservicios/adr-r9-f6-control-ventana-eod.md) | Accepted | Verified | Microservicios | Lenin | ASR-01, ASR-04 |
| ADR-0009 | [Usar gRPC para comunicaciones síncronas internas del Core](microservicios/adr-0009-grpc-interno-core.md) | Accepted | Verified | Microservicios | Lenin | ASR-01, ASR-04, ASR-05 |
| ADR-0010 | [Limitar el Message Broker al Switch y mantener el Core financiero síncrono](microservicios/adr-0010-broker-limitado-al-switch.md) | Accepted | Verified | Microservicios | Mateo | ASR-01, ASR-05 |
| ADR-0001 | [Mantener una base de código única parametrizada por entorno](microservicios-cloud/adr-0001-mantener-una-sola-base-de-codigo.md) | Accepted | Verified | Microservicios-cloud | Lenin | ASR-06 |
| ADR-0002 | [Gestionar la documentación arquitectónica como código](microservicios-cloud/adr-0002-documentacion-como-codigo.md) | Accepted | Verified | Microservicios-cloud | Kevin | ASR-04, ASR-06 |
| ADR-0003 | [Separar configuración local y cloud mediante perfiles](microservicios-cloud/adr-0003-perfiles-local-cloud.md) | Rejected | Not applicable | Microservicios-cloud | Mathius | ASR-06 |
| ADR-0004 | [Adoptar Google API Gateway como punto de entrada administrado para Core y Switch](microservicios-cloud/adr-0004-google-api-gateway-cloud.md) | Accepted | In progress | Microservicios-cloud | Mathius / Luis | ASR-03, ASR-04, ASR-05 |
| ADR-0005 | [Validar API Keys en el gateway y no en la lógica de negocio](microservicios-cloud/adr-0005-validar-api-keys-en-gateway.md) | Rejected | Not applicable | Microservicios-cloud | Mathius | ASR-03 |
| ADR-0006 | [Sustituir completamente los JWT internos por OAuth 2 administrado](microservicios-cloud/adr-0006-reemplazo-total-jwt-por-oauth.md) | Rejected | Not applicable | Microservicios-cloud | Mathius | ASR-03 |
| ADR-0007 | [Adoptar autenticación híbrida con Google OAuth y tokens internos durante la transición](microservicios-cloud/adr-0007-autenticacion-hibrida-google-oauth.md) | Accepted | In progress | Microservicios-cloud | Mathius | ASR-03 |
| ADR-0008 | [Adoptar Google Secret Manager con IAM y Workload Identity para secretos operativos](microservicios-cloud/adr-0008-google-secret-manager.md) | Proposed | Not started | Microservicios-cloud | Infraestructura cloud + responsables de microservicios | ASR-03 |
| ADR-0011 | [Exigir 70 % de cobertura en controllers y services del Core](microservicios-cloud/adr-0011-cobertura-70-core.md) | Rejected | Not applicable | Microservicios-cloud | Kevin | ASR-06 |
| ADR-0012 | [Exigir 70 % de cobertura en controllers y services del Switch](microservicios-cloud/adr-0012-cobertura-70-switch.md) | Rejected | Not applicable | Microservicios-cloud | Julio | ASR-06 |
| ADR-0013 | [Instrumentar servicios con Actuator y Micrometer](microservicios-cloud/adr-0013-actuator-micrometer.md) | Rejected | Not applicable | Microservicios-cloud | Mathius | ASR-04, ASR-05 |
| ADR-0014 | [Propagar correlación en HTTP y gRPC](microservicios-cloud/adr-0014-correlacion-http-grpc.md) | Rejected | Not applicable | Microservicios-cloud | Lenin | ASR-04 |
| ADR-0015 | [Crear un bounded context interbancario separado](microservicios-cloud/adr-0015-contexto-interbancario-separado.md) | Proposed | Not started | Microservicios-cloud | Lenin | ASR-07 |
| ADR-0016 | [Representar Nostro/Vostro fuera de las cuentas de clientes](microservicios-cloud/adr-0016-nostro-vostro-fuera-cuentas-cliente.md) | Proposed | Not started | Microservicios-cloud | Lenin | ASR-01, ASR-07 |
| ADR-0017 | [Mantener un único plan contable para operaciones On-Us y Off-Us](microservicios-cloud/adr-0017-plan-contable-unico-onus-offus.md) | Proposed | Not started | Microservicios-cloud | Lenin | ASR-01, ASR-07 |
| ADR-0018 | [Utilizar posiciones interbancarias prefondeadas para la simulación R9-K](microservicios-cloud/adr-0018-posiciones-prefondeadas.md) | Proposed | Not started | Microservicios-cloud | Lenin | ASR-01, ASR-07 |
| ADR-0019 | [Exponer una API interbancaria idempotente](microservicios-cloud/adr-0019-api-interbancaria-idempotente.md) | Rejected | Not applicable | Microservicios-cloud | Lenin | ASR-02, ASR-07 |
| ADR-0020 | [Generar una clave de idempotencia estable en el Switch y propagarla extremo a extremo](microservicios-cloud/adr-0020-clave-idempotencia-estable.md) | Proposed | Not started | Microservicios-cloud | Lenin / Mateo | ASR-02, ASR-07 |
| ADR-0021 | [Tratar timeouts remotos como pendientes de conciliación](microservicios-cloud/adr-0021-timeout-pendiente-conciliacion.md) | Proposed | Not started | Microservicios-cloud | Lenin / Mateo | ASR-01, ASR-02, ASR-05, ASR-07 |
| ADR-0022 | [Incorporar un módulo interbancario en los frontends](microservicios-cloud/adr-0022-modulo-interbancario-frontend.md) | Rejected | Not applicable | Microservicios-cloud | Kevin / Julio / Lenin | ASR-07 |

## Reglas de gobierno

1. `Proposed` indica que la alternativa aún está bajo revisión; no significa que el documento esté incompleto.
2. `Accepted` aprueba la dirección arquitectónica; la implementación puede permanecer `In progress`.
3. Un ADR Accepted no se reescribe para cambiar la decisión. Se crea uno nuevo y el anterior pasa a `Superseded`.
4. `Rejected` se usa cuando una propuesta fue descartada o cuando, tras revisión, se determina que el registro no era un ADR autónomo. La disposición debe indicar dónde se conserva el contenido.
5. Todo ADR debe relacionarse con al menos un ASR, alternativas reales, trade-offs y evidencia.
6. Los estados se actualizan mediante Pull Request documental y revisión del responsable técnico y del custodio del Decision Log.
