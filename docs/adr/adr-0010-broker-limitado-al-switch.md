# ADR-0010 — Restringir el broker administrado al Switch

Status: Proposed
Date: 2026-07-18
Author: Mateo

## Decision

El uso de un Message Broker (arquitectura orientada a eventos, por ejemplo RabbitMQ o Kafka) se limita al Switch de Pagos Masivos V2. Los microservicios del Core (Admin, Clientes, Contable, Documentos, Notificaciones, Transaccional) no adoptan mensajería asíncrona y mantienen comunicación síncrona (REST/gRPC, ver ADR-0009).

## Context

Estado actual: ninguno de los seis microservicios del Core revisados declara dependencia de RabbitMQ, Kafka o AMQP en su `pom.xml`; toda su comunicación es síncrona (HTTP/gRPC). El documento "Switch de Pagos Masivos V2" (RF-02, RF-03) exige explícitamente que el Switch fragmente el archivo de pagos y publique cada línea como evento en un Message Broker para consumo asíncrono por un servicio de enrutamiento independiente, mientras que el documento "Core V2" (RF-01) mantiene expresamente comunicación síncrona entre sus microservicios, con reverso compensatorio ante fallo o timeout.

Problema/ASR: evitar introducir complejidad operativa (broker, colas muertas, reintentos asíncronos) en microservicios del Core que no la necesitan, dado que su naturaleza es transaccional y de consistencia inmediata (partida doble, EOD).

Restricciones: el Core debe seguir respondiendo de forma síncrona al Switch cuando este liquida pagos On-Us (RF-04 de Core V2); el broker específico del Switch queda fuera del alcance de este repositorio (Core).

Alternativas consideradas: adoptar el mismo broker en el Core por uniformidad tecnológica (descartada, ningún requisito de Core V2 exige asincronía, y complicaría el patrón de compensación síncrona ya definido en el RF-01); no usar broker ni siquiera en el Switch (descartada, incumple explícitamente el alcance de la Fase 2 del Switch V2).

Criterio de selección: cada sistema adopta el estilo de comunicación que exige su propio documento de requisitos — el Switch necesita desacoplar la ingesta masiva de pagos (HTTP 202 Accepted inmediato) y procesar líneas concurrentemente; el Core necesita consistencia transaccional inmediata para no dejar saldos y asientos contables inconsistentes.

Impacto: la tecnología específica de broker `[PENDIENTE DE CONFIRMAR: RabbitMQ vs Kafka — equipo Switch/Mateo]` es responsabilidad exclusiva del equipo Switch; el Core solo expone endpoints síncronos para que el Switch los consuma.

## Opciones consideradas

- **Broker limitado al Switch (adoptada)**: cada sistema adopta el estilo de comunicación que exige su propio documento de requisitos. Ventaja: el Switch desacopla la ingesta masiva de pagos (HTTP 202 Accepted inmediato) sin imponer complejidad de mensajería asíncrona al Core.
- **Adoptar el mismo broker en el Core por uniformidad tecnológica**: descartada porque ningún requisito de Core V2 exige asincronía, y complicaría el patrón de compensación síncrona ya definido en el RF-01.
- **No usar broker ni siquiera en el Switch**: descartada por incumplir explícitamente el alcance de la Fase 2 del Switch V2.

## Consecuencias

Positivas:
- El Core conserva su modelo de consistencia transaccional inmediata (partida doble, EOD) sin la complejidad operativa de colas muertas y reintentos asíncronos.
- El Switch puede desacoplar la ingesta masiva de pagos y procesar líneas concurrentemente, cumpliendo su RF-02/RF-03.

Negativas:
- La tecnología específica de broker (RabbitMQ vs Kafka) queda `[PENDIENTE DE CONFIRMAR — equipo Switch/Mateo]`, lo que bloquea el diseño detallado del Switch.
- El sistema queda con dos estilos de comunicación distintos (síncrono en Core, asíncrono en Switch), lo que exige que el equipo domine ambos paradigmas.
