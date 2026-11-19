# ADR-010: Salvaguardas del lazo de control

- Estado: Aceptado
- Fecha: 2026-11-19

## Contexto
En la corrida de prueba CIA-E1 del 2026-11-19, después de poner en cuarentena al tenant ruidoso, el detector puso también en cuarentena a dos vecinos inocentes de su celda. Mientras la celda estuvo saturada, la demanda atendida de esos vecinos bajó; la línea base aprendió esa caída como normal, y al recuperarse su subida pareció una anomalía. Los síntomas de la celda, además, tardan una o dos ventanas en desaparecer después de la cuarentena.

## Decisión
1. La línea base de un tenant aprende solo de ventanas en que el tenant no es anómalo **y** su celda está sana.
2. Una celda con un tenant en cuarentena no recibe una segunda cuarentena hasta que el operador libera la primera.
3. Cada corrida cuenta las falsas cuarentenas (tenants aislados que no eran el ruidoso inyectado) como métrica.

## Consecuencias
- (+) El lazo no castiga a las víctimas por los síntomas que les causó el infractor.
- (+) Las falsas cuarentenas quedan medidas y reportadas.
- (−) Si dos tenants de la misma celda se vuelven ruidosos a la vez, el segundo requiere intervención del operador.
