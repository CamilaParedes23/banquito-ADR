# ADR-0010 — Limitar el Message Broker al Switch y mantener el Core financiero síncrono

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Mateo
Lifecycle: Microservicios
ASR: ASR-01, ASR-05

## Decision

RabbitMQ se utiliza en el Switch V2 para fragmentar y procesar eventos de líneas, enrutamiento, liquidación, facturación y reportería. El Core no adopta un broker para confirmar afectaciones de saldo y asientos; conserva gRPC/REST síncrono y usa Outbox para efectos secundarios confiables.

## Context

El documento Switch V2 exige un broker, pero no exige introducirlo dentro del Core. Se decidió aplicar asincronía solo donde aporta desacoplamiento y volumen, mientras que las operaciones financieras del Core requieren confirmación inmediata y compensación controlada. La evidencia muestra listeners/publicadores AMQP en los servicios del Switch y ausencia de AMQP en el Core.

## Options considered

- **Selected — broker en Switch; Core síncrono con Outbox para integraciones auxiliares.**
- **Broker en todo el ecosistema:** descartado por consistencia eventual innecesaria en saldos/asientos.
- **Sin broker en Switch:** descartado porque incumple el diseño V2 y limita escalabilidad.

## Consequences

**Positive**
- El Switch escala consumidores por flujo sin bloquear la carga del archivo.
- El Core conserva semántica financiera inmediata.

**Negative / trade-offs**
- El ecosistema opera con dos modelos de comunicación y requiere competencias operativas distintas.
- RabbitMQ añade colas, DLQ, reintentos y monitoreo.

## Evidence

- `banquito-switch/banquito-sw-lotes/.../RabbitPaymentLineEventPublisher.java`
- `banquito-switch/banquito-sw-enrutamiento/.../PaymentLineRequestedListener.java`
- `banquito-switch/banquito-sw-pagos-internos/.../PaymentLineRoutedOnUsListener.java`
- `banquito-core-google-oauth-minimal (sin dependencias AMQP)`
