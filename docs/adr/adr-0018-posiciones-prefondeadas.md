# ADR-0018 — Utilizar posiciones interbancarias prefondeadas

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

BanQuito mantiene un saldo prefondeado en su Cuenta de Encaje/Nostro para cubrir pagos Off-Us salientes, en lugar de solicitar fondos en tiempo real a la Cámara de Compensación en cada transacción individual.

## Context

El documento Switch V2 describe la Liquidación Neta Multilateral: "la Cámara de Compensación suma todos los envíos y resta todas las recepciones de una entidad durante el día... el banco solo liquida un único monto neto resultante" al final del ciclo, no por transacción.

Problema/ASR: si cada pago Off-Us individual dependiera de una liquidación en tiempo real contra el Banco Central, el Switch no podría cumplir su RF-02 (responder con `HTTP 202 Accepted` de inmediato) ni acreditar de forma predecible, ya que la liquidación real ocurre en un ciclo de corte (por ejemplo, 18:00), no de forma instantánea.

Restricciones: el alcance de la Fase 2 del Switch V2 excluye expresamente la "Conexión Real con Entes Reguladores"; esta decisión aplica al modelo lógico/simulado, no a una integración real con el Banco Central.

Alternativas consideradas: liquidar cada transacción Off-Us contra el Banco Central en tiempo real (descartada, contradice el propio modelo de Liquidación Neta Multilateral descrito en el documento, que es por naturaleza diferida y neteada); no manejar posición alguna y solo registrar promesas de pago sin control de exposición (descartada, no permitiría detectar si BanQuito se está quedando sin capacidad de neteo).

Criterio de selección: el prefondeo refleja fielmente el mecanismo descrito en el documento de requisitos (neteo diferido con cuenta de encaje) y permite al Switch seguir respondiendo de forma inmediata a la empresa emisora.

Impacto: se requiere una regla de negocio que rechace o encole pagos Off-Us cuando la posición prefondeada sea insuficiente; el umbral y su gestión operativa quedan `[PENDIENTE DE CONFIRMAR: Lenin + equipo arquitectura]`.
