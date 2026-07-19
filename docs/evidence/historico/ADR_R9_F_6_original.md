# ADR R9-F.6 — Control operativo de la ventana EOD

- **Estado:** Aceptado
- **Fecha:** 2026-06-19
- **Ámbito:** `core-accounting-service` y Ventanilla/Backoffice

## Contexto

La fecha contable está desacoplada de la fecha física y el EOD abre automáticamente el siguiente día hábil. Esa separación es necesaria para la operación bancaria, pero una ejecución manual anticipada, atrasada o sobre una fecha futura no debe parecer una operación ordinaria.

El catálogo administrativo ya dispone de la ventana `CORE_CONTABLE`, con hora de corte, hora final y acción posterior al corte. No era conveniente duplicar esos valores en Accounting ni permitir cierres arbitrarios sin trazabilidad.

## Decisión

1. Accounting consulta la ventana `CORE_CONTABLE` mediante el contrato gRPC existente de Admin.
2. El EOD normal solo se permite sobre la jornada activa, abierta, correspondiente a la fecha física del servidor y dentro del intervalo comprendido entre la hora de corte y la hora final.
3. Una fecha contable adelantada, atrasada o una ejecución fuera de horario requiere excepción.
4. La excepción solo puede ser autorizada por un Administrador Core, debe incluir un motivo de 10 a 500 caracteres y queda registrada como `RUN_EOD_WINDOW_OVERRIDE`.
5. La siguiente jornada continúa calculándose con el calendario administrativo; no se permite seleccionar manualmente la fecha posterior desde el EOD.
6. Los rechazos no alteran jornada, asientos, balance ni Outbox.
7. La regresión automatizada puede declarar una excepción explícita de laboratorio mediante parámetro; el comportamiento histórico por defecto permanece estricto.

## Consecuencias

- Se conserva el desacoplamiento entre fecha física y contable sin normalizar desfases operativos como si fueran cierres ordinarios.
- Los cierres anticipados de laboratorio siguen siendo posibles, pero quedan diferenciados y auditados.
- Operador Contable puede ejecutar un EOD normal, pero no autorizar una excepción.
- Admin continúa siendo la fuente de verdad de calendario y ventanas.
- No se agregan tablas, migraciones ni parámetros duplicados en base de datos.
