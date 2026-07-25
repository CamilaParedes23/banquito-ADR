# ADR-0014 — Propagar correlación en HTTP y gRPC

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Toda petición HTTP recibida por un microservicio propaga un identificador de correlación mediante un filtro (`CorrelationIdFilter`/`CorrelationIdHolder`). Cuando esa petición deriva en una llamada gRPC interna, el mismo identificador se transmite como campo explícito dentro del mensaje gRPC, no como metadata de transporte.

## Context

Estado actual: los seis microservicios del Core implementan `CorrelationIdFilter` y `CorrelationIdHolder` en su paquete `shared/tracing`. En la llamada gRPC de `core-account-service` hacia `core-accounting-service` (`AccountingGrpcClient`), el `correlationId` se envía mediante `setCorrelationId(...)` como campo dentro del payload del mensaje gRPC; no se encontró un interceptor gRPC (`ServerInterceptor`/`ClientInterceptor`) que propague correlación vía Metadata.

Problema/ASR: sin correlación de extremo a extremo, un fallo que atraviesa HTTP → gRPC (por ejemplo, un asiento contable rechazado por el Microservicio Contable) no se puede rastrear en logs distribuidos entre los microservicios involucrados.

Restricciones: no existe todavía un interceptor gRPC genérico implementado; la propagación depende de que cada cliente gRPC incluya el campo manualmente en el mensaje.

Alternativas consideradas: usar gRPC Metadata junto con un interceptor genérico reutilizable (más estándar y menos propenso a omisiones, pero no es el mecanismo actualmente implementado); no propagar correlación en gRPC y correlacionar solo por `TRANSACTION_UUID`/timestamp (descartada, insuficiente para trazabilidad exacta de una petición HTTP específica a través de múltiples saltos gRPC).

Criterio de selección: documentar y mantener el mecanismo ya implementado y probado (campo explícito en el mensaje), reconociendo como riesgo abierto que un interceptor de Metadata sería más reutilizable a futuro si se agregan más microservicios internos.

Impacto: todo nuevo cliente gRPC interno del Core debe incluir explícitamente el `correlationId` en el mensaje; no puede asumir que gRPC lo propaga automáticamente.

## Opciones consideradas

- **Campo explícito de correlación dentro del mensaje gRPC (adoptada)**: documenta y mantiene el mecanismo ya implementado y probado (`setCorrelationId(...)`). Ventaja: es el patrón que ya funciona hoy en `AccountingGrpcClient`, sin requerir desarrollo adicional.
- **gRPC Metadata junto con un interceptor genérico reutilizable**: más estándar y menos propenso a omisiones, pero no es el mecanismo actualmente implementado; queda como riesgo abierto a futuro si se agregan más microservicios internos.
- **No propagar correlación en gRPC y correlacionar solo por `TRANSACTION_UUID`/timestamp**: descartada por ser insuficiente para trazabilidad exacta de una petición HTTP específica a través de múltiples saltos gRPC.

## Consecuencias

Positivas:
- Permite rastrear en logs distribuidos un fallo que atraviesa HTTP → gRPC (por ejemplo, un asiento contable rechazado por el Microservicio Contable).
- No requiere desarrollo adicional, ya que reutiliza un mecanismo ya implementado y probado.

Negativas:
- Todo nuevo cliente gRPC interno del Core debe incluir explícitamente el `correlationId` en el mensaje; una omisión humana rompe la trazabilidad para esa llamada, ya que gRPC no lo propaga automáticamente.
- Al no existir todavía un interceptor gRPC genérico, la propagación depende de que cada cliente lo implemente manualmente, lo que no escala tan bien como un interceptor de Metadata si se agregan más microservicios internos.
