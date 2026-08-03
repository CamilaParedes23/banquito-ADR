# Estándar — Actuator, Micrometer y métricas R9-K

Status: Active
Source: ADR-0013 reclasificado; lineamiento explícito de observabilidad del Proyecto Final.

## Endpoints técnicos

Todos los microservicios backend deben incluir Actuator y el registro Prometheus de Micrometer y exponer, como mínimo:

- `/actuator/health`
- `/actuator/health/liveness`
- `/actuator/health/readiness`
- `/actuator/info`
- `/actuator/metrics`
- `/actuator/prometheus`

`health`, `info` y `prometheus` pueden ser consultados por la plataforma; el resto de endpoints permanece protegido o no expuesto. No se publican secretos, payloads financieros ni datos personales.

## Métricas técnicas

Se conservan métricas estándar de HTTP, JVM, CPU, memoria, disco, pools y base de datos. `http.server.requests` habilita histograma para análisis de percentiles en la plataforma cloud.

Variables:

- `METRICS_ENVIRONMENT`
- `PROMETHEUS_METRICS_ENABLED`
- `HTTP_METRICS_HISTOGRAM_ENABLED`
- `ACTUATOR_EXPOSURE` o `ACTUATOR_ENDPOINTS`, según el servicio existente

## Métricas interbancarias

`core-account-service` publica:

- `banquito.interbank.transfers`: contador por dirección, estado y contraparte.
- `banquito.interbank.amount`: distribución de monto por dirección, estado, contraparte y moneda.
- `banquito.interbank.processing`: duración del procesamiento.
- `banquito.interbank.pending.reconciliation`: gauge de operaciones pendientes.
- `banquito.interbank.idempotency.replays`: replays atendidos sin duplicar movimientos.
- `banquito.interbank.errors`: errores funcionales controlados.

Solo se utilizan etiquetas de cardinalidad acotada: dirección, estado, routing code configurado, moneda y código de error controlado. Nunca se etiquetan UUID, número de cuenta, identificación, correo o correlation ID.

## Correlación

`X-Correlation-Id` debe propagarse entre HTTP, gRPC, auditoría, transacciones, asientos, documentos y notificaciones. Los logs de infraestructura deberán incluirlo al configurar la plataforma cloud.

## Evidencia mínima

Antes de declarar R9-K verificado se debe adjuntar:

1. respuesta de health/readiness;
2. muestra de `/actuator/prometheus`;
3. métricas después de una operación entrante, saliente, replay, rechazo y pendiente;
4. logs correlacionados sin datos sensibles;
5. tablero/alerta cloud configurado por infraestructura.
