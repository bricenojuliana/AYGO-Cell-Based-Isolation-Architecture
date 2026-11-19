# ADR-002: Un solo código; la topología es configuración

- Estado: Aceptado
- Fecha: 2026-11-14

## Contexto
La comparación pool contra celdas solo es válida si lo único que cambia es la topología.

## Decisión
El pool es una celda con 4 veces la capacidad de una celda (100 contra 25 RCU/WCU). Celdas y pool se despliegan desde el mismo template con distintos parámetros, y las tres configuraciones (P, C, C+IA) pasan por el mismo router. Cambiar de configuración es reescribir la tabla de ruteo y el modo del detector, sin redesplegar.

## Consecuencias
- (+) Comparación justa a igual capacidad total; reusabilidad (QAS10) y modificabilidad (QAS02).
- (−) El pool del prototipo lleva una capa de ruteo que un pool real no tendría; su sobrecosto se mide aparte (H3).
