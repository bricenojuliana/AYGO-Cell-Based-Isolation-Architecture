# ADR-007: Aislar al tenant reenrutándolo a la celda de cuarentena, sin copiar datos

- Estado: Aceptado
- Fecha: 2026-11-18

## Contexto
Cuando el detector identifica a un tenant infractor, sus vecinos de celda deben recuperarse lo antes posible (MTTR) sin dejar de atender al infractor, que puede tener un pico legítimo.

## Opciones
1. **Migrar** al tenant con copia de sus datos a la celda de cuarentena.
2. **Limitar su tasa** en el router.
3. **Reenrutar sin copiar datos:** las escrituras y lecturas nuevas van a la celda de cuarentena; la lectura de un pedido que no está ahí se resuelve en la celda de origen (lectura con respaldo).

## Decisión
Opción 3.

## Justificación
- **MTTR mínimo:** el aislamiento es un cambio de ruta (segundos); no hay ventana de copia ni de indisponibilidad para el infractor, que sí tendría la opción 1.
- **Sin pérdida de datos visibles:** el historial sigue disponible en la celda de origen mediante la lectura con respaldo.
- **El aislamiento es temporal:** la cuarentena dura mientras el tenant es anómalo; copiar todo su historial para devolverlo después es trabajo innecesario (YAGNI).
- **Por qué no la opción 2:** un límite de tasa en memoria dentro de Lambda no es global, y uno global pone un almacén compartido en el camino crítico; además castiga al infractor.

## Consecuencias
- (+) Recuperación rápida de las víctimas, sin penalizar al infractor, con poco código.
- (−) Las lecturas de pedidos antiguos del tenant aislado pasan por dos celdas (latencia extra solo para ese tenant y solo en esas lecturas).
- (−) La liberación es manual (operador), como exige la política de gobierno.
