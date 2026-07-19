# ADR-0011 — Medir al menos 70 % de cobertura en controllers y services del Core

Status: Proposed
Date: 2026-07-18
Author: Kevin

## Decision

Cada microservicio del Core aplica una regla de cobertura mínima del 70 % (por línea), restringida a los paquetes `api.controller` y `application.service`, verificada automáticamente por JaCoCo en la fase `verify` de Maven. El build de CI falla si no se cumple.

## Context

Estado actual: confirmado en los seis `pom.xml` del Core (`admin`, `clientes`, `contable`, `documentos`, `notificaciones`, `transaccional`), que declaran el plugin `jacoco-maven-plugin` con una regla `PACKAGE` sobre `api.controller` y `application.service`, `COVEREDRATIO` mínimo `0.70`, ejecutada dentro de `mvn clean verify` en el pipeline de CI (`ci.yml`). Se ejecutaron las pruebas de `banquito-core-admin` y `banquito-core-documentos` en este proyecto y el build pasó sin fallos de cobertura (38 pruebas, 0 fallos, en `banquito-core-documentos`).

Problema/ASR: cumplir el requisito institucional que exige "implementar pruebas unitarias para todos los microservicios... con un cubrimiento de al menos el 70 %... solo se aplicará pruebas unitarias para Controladores y Servicios".

Restricciones: solo `controller` y `service` quedan dentro del alcance de medición; `domain`, `infrastructure` y `shared` quedan explícitamente fuera.

Alternativas consideradas: medir cobertura global de todo el proyecto sin restringir paquetes (descartada, penalizaría clases de infraestructura, DTO o configuración que no son objeto de prueba unitaria según el alcance institucional definido); usar un umbral distinto al 70 % (descartada, es un requisito fijo del proyecto, no negociable por el equipo).

Criterio de selección: cumplimiento directo y automatizado del requisito institucional, verificado en el pipeline de CI sin depender de revisión manual.

Impacto: cualquier Pull Request que reduzca la cobertura de `controller`/`service` por debajo del 70 % rompe el pipeline de CI y no puede fusionarse sin corregirlo.
