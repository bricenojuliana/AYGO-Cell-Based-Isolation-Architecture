# Registros de decisiones de arquitectura

Formato: contexto, opciones, decisión, consecuencias. Las decisiones se tomaron dentro de las restricciones del Learner Lab (un solo rol IAM, `LabRole`).

| ADR | Decisión | Estado |
|---|---|---|
| [001](ADR-001-celdas-share-nothing.md) | Celdas *share-nothing* detrás de un router delgado | Aceptado |
| [002](ADR-002-misma-base-de-codigo.md) | Un solo código; el pool es una celda grande | Aceptado |
| [003](ADR-003-router-lambda.md) | Router en Lambda que invoca las celdas con `lambda:Invoke` | Aceptado |
| [004](ADR-004-capacidad-acotada.md) | Capacidad acotada por celda para saturación reproducible | Aceptado |
| [005](ADR-005-telemetria-emf.md) | Telemetría por tenant con EMF y CloudWatch | Aceptado |
| [006](ADR-006-detector.md) | Z-score EWMA desplegado; Isolation Forest evaluado offline | Aceptado |
| [007](ADR-007-mitigacion-cuarentena.md) | Aislar reenrutando a la celda de cuarentena, sin copiar datos | Aceptado |
| [008](ADR-008-iac-sam-learner-lab.md) | IaC con SAM y despliegue desde CloudShell con `LabRole` | Aceptado |
| [009](ADR-009-carga-y-fallas.md) | Generador de carga en lazo abierto e inyección de fallas por SSM | Aceptado |
| [010](ADR-010-salvaguardas-detector.md) | Salvaguardas del lazo de control: aprender solo de celdas sanas, una cuarentena por celda | Aceptado |
