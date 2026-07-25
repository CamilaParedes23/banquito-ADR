# ADR-0022 — Incorporar un módulo Interbancario en el frontend del Core

Status: Proposed
Date: 2026-07-18
Author: Kevin, Julio, Lenin

## Decision

El Módulo Satélite de Ventanilla y el Portal de Banca Web (definidos en Core V2, RF-06 y RF-07) incorporan una sección Interbancaria donde el operador o el cliente pueden distinguir visualmente si una transferencia es On-Us (mismo banco) u Off-Us (otro banco), y consultar el estado de operaciones pendientes de conciliación (ADR-0021).

## Context

El Core V2 (RF-07) ya exige que el portal de Banca Web Personas valide y muestre el nombre del titular de la cuenta destino antes de confirmar una transferencia P2P; ese flujo hoy asume que el destino siempre pertenece a BanQuito. El documento Switch V2 introduce el concepto de Enrutamiento Dinámico (On-Us/Off-Us) determinado por el Código de Institución Financiera del beneficiario.

Problema/ASR: sin distinción visual, el usuario (cliente o cajero) no puede anticipar que una operación Off-Us tiene tiempos de liquidación distintos (siguiente día hábil vía Cámara de Compensación) frente a una On-Us (tiempo real), lo que genera expectativas incorrectas y posible carga de soporte.

Restricciones: depende de que el backend (contexto Interbancario, ADR-0015 a ADR-0021) exponga el estado de la operación para que el frontend lo consulte; no introduce lógica de negocio nueva en el frontend.

Alternativas consideradas: no diferenciar visualmente y mostrar todas las transferencias igual (descartada, oculta información relevante sobre tiempos de liquidación); construir un frontend Interbancario completamente separado del Portal de Banca Web y la Ventanilla ya existentes (descartada, duplicaría navegación y autenticación ya resueltas en los módulos satélite de Core V2).

Criterio de selección: extender los módulos satélite ya definidos en Core V2 (RF-06, RF-07) es más simple que construir un frontend nuevo, y mantiene una sola experiencia de usuario para el cliente y el cajero.

Impacto: los componentes de UI de transferencia deben leer el resultado de la clasificación On-Us/Off-Us del backend; el diseño visual específico y el alcance exacto (¿aplica también a Ventanilla, o solo a Banca Web?) quedan `[PENDIENTE DE CONFIRMAR: Kevin/Julio/Lenin]`.

## Opciones consideradas

- **Sección Interbancaria dentro del Módulo Satélite de Ventanilla y el Portal de Banca Web ya existentes (adoptada)**: extiende los módulos satélite ya definidos en Core V2 (RF-06, RF-07). Ventaja: más simple que construir un frontend nuevo, y mantiene una sola experiencia de usuario para el cliente y el cajero.
- **No diferenciar visualmente y mostrar todas las transferencias igual**: descartada por ocultar información relevante sobre tiempos de liquidación distintos entre On-Us y Off-Us.
- **Construir un frontend Interbancario completamente separado del Portal de Banca Web y la Ventanilla existentes**: descartada por duplicar navegación y autenticación ya resueltas en los módulos satélite de Core V2.

## Consecuencias

Positivas:
- El cliente y el cajero pueden anticipar que una operación Off-Us tiene tiempos de liquidación distintos (siguiente día hábil vía Cámara de Compensación) frente a una On-Us (tiempo real), reduciendo expectativas incorrectas y carga de soporte.
- Reutiliza navegación y autenticación ya resueltas en los módulos satélite de Core V2, sin duplicar esfuerzo de frontend.

Negativas:
- Depende de que el backend (contexto Interbancario, ADR-0015 a ADR-0021) exponga el estado de la operación; el frontend no introduce lógica de negocio propia y queda bloqueado si el backend no está disponible.
- El diseño visual específico y el alcance exacto (¿aplica también a Ventanilla, o solo a Banca Web?) quedan `[PENDIENTE DE CONFIRMAR: Kevin/Julio/Lenin]`.
