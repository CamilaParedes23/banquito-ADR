# ADR-0009 — Mantener gRPC para comunicaciones internas del Core

Status: Proposed
Date: 2026-07-18
Author: Equipo Core

## Decision

La comunicación síncrona entre microservicios internos del Core (por ejemplo, `core-account-service` hacia `core-accounting-service` para la generación de asientos contables, o hacia `core-admin-service` para la ventana `CORE_CONTABLE`) se realiza vía gRPC. No se usa REST para las llamadas internas entre microservicios del Core.

## Context

Estado actual: `core-account-service` y `core-accounting-service` tienen ambos un paquete `infrastructure/grpc`, con cliente gRPC (`AccountingGrpcClient`) en el primero y servidor gRPC (`AccountingGrpcService`) en el segundo. El ADR histórico R9-F.6 ya documenta que `core-accounting-service` consulta a `core-admin-service` "mediante el contrato gRPC vigente". El documento de requisitos "Core V2" (RF-01) exige explícitamente que, tras afectar el saldo de un cliente, el microservicio de Cuentas "consuma de manera síncrona (vía API REST o gRPC)" al microservicio Contable para generar el asiento.

Problema/ASR: con el patrón *Database per Service* adoptado en Core V2 (Fase 2) y múltiples microservicios internos, se necesita un protocolo de comunicación eficiente, con contrato fuertemente tipado, para llamadas que deben resolverse de forma síncrona antes de responder al cliente original.

Restricciones: uso exclusivamente interno (los clientes externos como la Banca Web o el Switch consumen REST vía el API Gateway, ver ADR-0004); no reemplaza el mecanismo de propagación de correlación (ver ADR-0014).

Alternativas consideradas: REST/JSON para llamadas internas entre microservicios del Core (descartada, mayor overhead de serialización y sin contrato fuertemente tipado entre servicios que cambian con frecuencia); mensajería asíncrona para esta comunicación (descartada, la generación del asiento contable debe confirmarse de forma síncrona antes de responder, según el RF-01 de Core V2 y su regla de compensación ante rechazo o timeout).

Criterio de selección: gRPC ya está implementado y probado entre Cuentas, Contable y Admin, y cumple el requisito explícito de comunicación síncrona fuertemente tipada del documento Core V2.

Impacto: todo microservicio interno nuevo del Core que necesite comunicación síncrona con otro microservicio del Core debe usar gRPC, no REST.
