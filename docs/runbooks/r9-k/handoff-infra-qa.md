# BanQuito R9-K — Handoff de Infraestructura QA/Staging

## 1. Alcance

Desplegar la versión `r9-k-core-rc1` del Core BanQuito en GKE, con:

- Artifact Registry para imágenes;
- GKE;
- Cloud SQL for MySQL 8.4;
- Secret Manager;
- Workload Identity Federation for GKE;
- Cloud SQL Auth Proxy como sidecar;
- API Gateway;
- API keys restringidas;
- OAuth2/JWT del servicio de Seguridad de BanQuito;
- Cloud Logging;
- Managed Service for Prometheus;
- Cloud Build y, de ser posible, Cloud Deploy.

Este despliegue es para **QA/staging**, no producción.

## 2. Artefactos de release

- Tag de código: `r9-k-core-rc1`
- Seed QA: `BanQuito_R9K_Seed_Completo_E2E_v6.zip`
- Suite de pruebas: `BanQuito_R9K_Gate_E2E_Final_v3.zip`
- Contrato API: `BanQuito_API_Interbancaria_R9K_BanQuill_v1_CORREGIDA.yaml`
- BOM de commits: `r9-k-core-rc1-bom.json`

## 3. Microservicios

1. core-admin-service
2. core-customer-service
3. core-accounting-service
4. identity-access-service
5. document-service
6. notification-service
7. core-account-service

## 4. Versiones Flyway requeridas

| Servicio | Versión |
|---|---:|
| Admin | 7 |
| Clientes | 4 |
| Contable | 5 |
| Seguridad | 9 |
| Notificaciones | 5 |
| Transaccional | 10 |

No usar `flyway clean`. No usar `flyway repair` salvo incidente aprobado y documentado.

## 5. Variables no secretas

```text
BANQUITO_ROUTING_CODE=BQTO001
BANQUITO_ACCOUNT_PREFIX=20202
BANQUILL_ROUTING_CODE=BQLL001
BANQUILL_ACCOUNT_PREFIX=10101
BANQUILL_API_CLIENT_ID=bank-banquill-interbank-client
BANQUILL_API_AUDIENCE=bank-banquill-core
NOSTRO_ACCOUNT_CODE=NOSTRO_BQLL001_USD
VOSTRO_ACCOUNT_CODE=VOSTRO_BQLL001_USD
SPRING_PROFILES_ACTIVE=cloud
ACTUATOR_EXPOSURE=health,info,metrics,prometheus
```

## 6. Secretos mínimos

Crear en Secret Manager, separados por ambiente:

```text
banquito-qa-admin-db-user
banquito-qa-admin-db-password
banquito-qa-customer-db-user
banquito-qa-customer-db-password
banquito-qa-accounting-db-user
banquito-qa-accounting-db-password
banquito-qa-identity-db-user
banquito-qa-identity-db-password
banquito-qa-notification-db-user
banquito-qa-notification-db-password
banquito-qa-account-db-user
banquito-qa-account-db-password
banquito-qa-mongodb-uri
banquito-qa-jwt-signing-secret-or-key
banquito-qa-banquill-client-secret
banquito-qa-switch-client-secret
banquito-qa-smtp-password
```

No reutilizar `password`, secretos locales, `.env.local` ni secretos históricos.

## 7. Identidades e IAM

Crear un GSA y un KSA por microservicio. Conceder solo:

- `roles/cloudsql.client` a los servicios que usan Cloud SQL;
- `roles/secretmanager.secretAccessor` únicamente sobre los secretos que necesita cada servicio;
- permisos de Logging/Monitoring según la configuración del clúster;
- ningún archivo JSON de clave de cuenta de servicio dentro de repositorios, imágenes o Kubernetes Secrets.

Usar Workload Identity Federation for GKE.

## 8. Cloud SQL

Recomendación QA:

- una instancia Cloud SQL MySQL 8.4;
- seis bases lógicas;
- un usuario de base por servicio;
- IP privada;
- backups automáticos y PITR;
- Cloud SQL Auth Proxy sidecar en cada Pod que usa MySQL;
- pool de conexiones limitado por servicio.

Bases:

```text
banquito_core_admin_db
banquito_core_customer_db
banquito_core_accounting_db
banquito_identity_access_db
banquito_notification_db
banquito_core_account_db
```

## 9. MongoDB

No migrar document-service a Firestore dentro de R9-K.

Usar una de estas opciones:

1. MongoDB Atlas desplegado en Google Cloud, recomendado;
2. instancia MongoDB gestionada ya existente;
3. MongoDB en GKE solo si no existe alternativa gestionada.

Guardar la URI completa en Secret Manager y limitar red/orígenes al clúster.

## 10. API Gateway y seguridad

Para QA, exponer únicamente los endpoints interbancarios necesarios.

Cada llamada de Banco BanQuill debe llevar:

```http
x-api-key: <API_KEY_RESTRINGIDA>
Authorization: Bearer <TOKEN_OAUTH2>
X-Correlation-Id: <UUID>
Content-Type: application/json
```

Reglas:

- API key restringida al API de BanQuito;
- API key para identificación, cuota y monitoreo, nunca como único control;
- OAuth2 client credentials emitido por identity-access-service;
- backend valida scopes `core.interbank.receive` y `core.interbank.read`;
- no publicar endpoints administrativos internos;
- TLS obligatorio;
- CORS deshabilitado para integración servidor a servidor;
- cuotas y límites por cliente;
- registrar correlación, routing, resultado y latencia sin registrar secretos ni tokens.

## 11. Imágenes

Etiquetar cada imagen con:

```text
r9-k-core-rc1-<short-sha>
```

Registrar también el digest `sha256`. Desplegar por digest en QA para inmutabilidad.

No usar `latest` como versión de release.

## 12. Kubernetes

Por servicio:

- Deployment;
- ClusterIP Service;
- KSA propia;
- ConfigMap solo para configuración no sensible;
- secretos montados desde Secret Manager;
- Cloud SQL Auth Proxy sidecar cuando aplique;
- readiness `/actuator/health/readiness` o endpoint disponible;
- liveness `/actuator/health/liveness` o endpoint disponible;
- recursos requests/limits;
- `securityContext` no root;
- `readOnlyRootFilesystem` cuando la aplicación lo permita;
- PodDisruptionBudget en ambientes críticos;
- NetworkPolicy;
- HPA después de medir carga real.

No exponer servicios internos como LoadBalancer.

## 13. Observabilidad

- Cloud Logging para stdout/stderr;
- logs JSON de una línea;
- Managed Service for Prometheus;
- scrape de `/actuator/prometheus`;
- dashboard de siete servicios;
- alerta si `banquito_interbank_pending_reconciliation > 0` durante el umbral acordado;
- alertas por 5xx, latencia, reinicios de Pod y fallos de Flyway;
- no registrar payloads sensibles completos.

## 14. Orden de despliegue

1. Crear/validar Cloud SQL y bases.
2. Crear usuarios y secretos.
3. Configurar Workload Identity.
4. Publicar imágenes por digest.
5. Ejecutar migraciones con un Job controlado.
6. Desplegar Admin, Seguridad y Contable.
7. Desplegar Clientes, Documentos y Notificaciones.
8. Desplegar Transaccional.
9. Validar health y Prometheus.
10. Aplicar seed v6 únicamente en QA/staging.
11. Validar `SEED_R9K_VALIDADO`.
12. Desplegar API Gateway.
13. Crear/restringir API key de BanQuill.
14. Registrar cliente OAuth2 y rotar secreto.
15. Ejecutar smoke tests.
16. Ejecutar E2E10 cloud.
17. Ejecutar E2E100 cloud.
18. Revisar logs, métricas, asientos, Nostro/Vostro y pendientes.

## 15. Gates de aceptación cloud

- siete servicios `UP`;
- versiones Flyway exactas;
- ninguna migración fallida;
- Secret Manager accesible sin claves JSON;
- conexión Cloud SQL mediante Auth Proxy;
- API Gateway responde solo con API key válida;
- backend rechaza token ausente/inválido;
- scopes correctos;
- On-Us aprobado;
- Off-Us confirmado, rechazado, pendiente y reversado;
- incoming aprobado;
- idempotencia y conflicto aprobados;
- cero asientos descuadrados;
- clientes = pasivo;
- Nostro/Vostro correctos;
- cero pendientes al cierre;
- métricas visibles en Cloud Monitoring;
- logs correlacionables en Cloud Logging.

## 16. Condición de promoción

Después de aprobar QA cloud y la integración real con BanQuill:

- generar evidencia;
- promover los mismos digests;
- crear el tag final `r9-k`;
- no reconstruir imágenes para producción;
- aplicar cambios mediante Cloud Deploy o proceso equivalente aprobado.
