# AYGO · Aislamiento de tenants con arquitectura por celdas: artefactos de arquitectura

Artefactos de arquitectura del proyecto final de AYGO (Arquitectura de Software, Arquitectura Empresarial y Gobierno de TI). El prototipo y los experimentos están en [AYGO-Cell-Based-Isolation](https://github.com/bricenojuliana/AYGO-Cell-Based-Isolation).

**Problema.** En un SaaS multi-tenant con recursos compartidos, un tenant que se sale de lo normal (o una falla de infraestructura) degrada a todos los demás.

**Propuesta.** Partir el servicio en celdas independientes (*share-nothing*) detrás de un router delgado, y agregar un plano de control que detecta al tenant anómalo y lo reenruta a una celda de cuarentena sin intervención humana.

**Pregunta.** ¿En qué medida las celdas, con aislamiento automático, reducen el radio de impacto y el tiempo de recuperación frente a un pool compartido, y con qué costo y sobrecarga de latencia?

## Contenido

| Carpeta | Qué hay |
|---|---|
| [views/](views/) | Vistas C4 (contexto, contenedores, componentes del router y del plano de control), vista por capas tipo ArchiMate y secuencia de la cuarentena, en Mermaid |
| [adr/](adr/) | Registros de decisiones de arquitectura (ADR-001 a ADR-010) |
| [quality/](quality/) | Escenarios de atributos de calidad (QAS) con su medida y su resultado, y análisis de *trade-offs* |
| [governance/](governance/) | Política de aislamiento automático y su mapeo a COBIT 2019 |

## Restricciones del entorno

AWS Academy Learner Lab: región us-east-1, un solo rol IAM (`LabRole`), sin creación de roles ni políticas, sesiones de 4 horas. Todas las decisiones se tomaron dentro de esas restricciones (ver [ADR-008](adr/ADR-008-iac-sam-learner-lab.md)).
