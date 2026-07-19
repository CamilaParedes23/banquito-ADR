# ADR-0015 — Crear un contexto funcional interbancario separado

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Se crea un contexto delimitado (bounded context) funcional independiente, "Interbancario", separado del contexto Core de Cuentas y del contexto Switch de Pagos, responsable de modelar Nostro/Vostro, compensación y liquidación con otras instituciones financieras.

## Context

El documento "Switch de Pagos Masivos V2" introduce conceptos regulados propios de un dominio distinto al de cuentas de clientes: Compensación Interbancaria (Clearing), Liquidación Interbancaria (Settlement), Cámara de Compensación, Liquidación Neta Multilateral y Cuenta de Encaje. Ninguno de estos conceptos existe en el dominio actual del Core de Cuentas, que solo modela `ACCOUNT` (cuentas de clientes) e `INSTITUTIONAL_ACCOUNT` (cuentas internas del banco), según el documento Core V2.

Problema/ASR: mezclar la lógica de compensación con otros bancos dentro del Core de Cuentas o del Switch existente violaría el principio de responsabilidad única de cada microservicio y complicaría su modelo de datos con conceptos regulatorios que no aplican a operaciones locales.

Restricciones: el alcance de la Fase 2 del Switch V2 deja explícitamente fuera de alcance la "Conexión Real con Entes Reguladores" (la liquidación se simula lógicamente), por lo que este contexto se construye en modo simulado, no contra APIs reales del Banco Central.

Alternativas consideradas: extender el Core de Cuentas actual para incluir compensación interbancaria (descartada, mezclaría el dominio de "cuentas de clientes de BanQuito" con "relaciones con otros bancos"); extender el Switch V2 sin un contexto propio (descartada, el Switch es orquestador de enrutamiento y no debe cargar con el modelo financiero de Nostro/Vostro).

Criterio de selección: DDD recomienda un bounded context por dominio con lenguaje ubicuo propio; "Interbancario" tiene vocabulario propio (Clearing, Settlement, Clearinghouse, Cuenta de Encaje) que no se solapa con "Cuentas de clientes" ni con "Enrutamiento de pagos".

Impacto: implica un nuevo microservicio o módulo con su propio modelo de datos. Su forma exacta (microservicio nuevo vs. extensión de Contable) queda `[PENDIENTE DE CONFIRMAR: Lenin]`.
