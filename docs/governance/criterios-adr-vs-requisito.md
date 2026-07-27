# Criterios — ADR, requisito, estándar o diseño

## Es un ADR cuando

- existen alternativas reales y autoridad para elegir;
- responde a un ASR;
- afecta límites, datos, comunicaciones, despliegue o varios componentes;
- es costoso de cambiar;
- implica trade-offs significativos.

## No es un ADR cuando

- el documento base ya prescribe exactamente qué hacer y no existe elección;
- es un porcentaje o política obligatoria;
- es una práctica transversal rutinaria;
- describe una pantalla, endpoint aislado o configuración táctica;
- solo repite una tecnología sin justificar alternativas y consecuencias.

## Disposición

- Requisito/ASR → `docs/asr/` o trazabilidad de requisitos.
- Práctica repetible → `docs/standards/`.
- Experiencia o comportamiento de pantalla → `docs/functional/`.
- Decisión arquitectónica → `docs/adr/`.
