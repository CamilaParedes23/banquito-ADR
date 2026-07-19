# ADR R9-F.6 — Restringir el EOD a la ventana operativa con excepciones auditadas

Status: Accepted
Date: 2026-06-19
Author: Equipo Banco BanQuito

## Decision

Accounting consultará la ventana `CORE_CONTABLE` mediante el contrato gRPC vigente de Admin. El EOD ordinario solo podrá ejecutarse sobre la jornada activa y abierta, correspondiente a la fecha física del servidor, dentro del intervalo definido entre la hora de corte y la hora final.

Una fecha contable adelantada o atrasada, o una ejecución fuera de la ventana, requerirá una excepción autorizada por un Administrador Core. La excepción deberá incluir un motivo de 10 a 500 caracteres y quedará registrada como `RUN_EOD_WINDOW_OVERRIDE`. La siguiente jornada continuará calculándose con el calendario administrativo y no podrá seleccionarse manualmente. Los rechazos no modificarán la jornada, los asientos, el balance ni el Outbox. Las pruebas automatizadas podrán declarar una excepción explícita de laboratorio sin alterar el comportamiento estricto predeterminado.

## Context

La fecha contable está desacoplada de la fecha física y el proceso EOD abre automáticamente el siguiente día hábil. Esa separación es necesaria para la operación bancaria, pero una ejecución manual anticipada, atrasada o sobre una fecha futura no debe registrarse como una operación ordinaria.

El catálogo administrativo ya contiene la ventana `CORE_CONTABLE`, con hora de corte, hora final y acción posterior al corte. Se consideraron como alternativas duplicar esa configuración dentro de Accounting o permitir cierres manuales arbitrarios. Ambas opciones fueron descartadas porque crearían fuentes de verdad paralelas o reducirían la trazabilidad operativa.

La decisión mantiene a Admin como fuente de verdad para calendario y ventanas, conserva el desacoplamiento entre fecha física y contable y permite cierres anticipados de laboratorio únicamente como excepciones diferenciadas y auditadas. Un Operador Contable puede ejecutar un EOD normal, pero no autorizar una excepción. No se requieren tablas, migraciones ni parámetros duplicados.
