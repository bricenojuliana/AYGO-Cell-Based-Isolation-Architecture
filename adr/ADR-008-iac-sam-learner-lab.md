# ADR-008: IaC con AWS SAM y despliegue desde CloudShell con LabRole

- Estado: Aceptado
- Fecha: 2026-11-14

## Decisión
AWS SAM, Python 3.13 sobre arm64, `ExecutionRoleArn` parametrizable (vacío crea roles de menor privilegio; en el Learner Lab se pasa `LabRole`), pruebas de reglas sobre los templates, `cfn-lint` y despliegue desde CloudShell. Pilas separadas: plataforma (API y router), una pila por celda y plano de control.

## Restricción
La cuenta tiene un solo rol, `LabRole`, y no permite crear roles ni políticas. Todo recurso que necesite un rol lo recibe por parámetro. Se evitan recursos que crean roles implícitos: la invocación programada del detector usa una regla de EventBridge con permiso por política de recurso, no EventBridge Scheduler.

## Consecuencias
- (+) Pilas por celda: las celdas se despliegan y se borran por separado.
- (+) Una prueba de template verifica que, con `ExecutionRoleArn` definido, ninguna pila crea `AWS::IAM::Role`.
- (−) Con `LabRole` todas las funciones tienen los mismos permisos: el aislamiento entre celdas se garantiza en código y en datos (tablas separadas), no con IAM. Los roles de menor privilegio quedan en el template para cuentas normales.
