# ADR-001: Arquitectura por celdas *share-nothing* con router delgado

- Estado: Aceptado
- Fecha: 2026-11-14

## Contexto
En un SaaS multi-tenant con recursos compartidos (*pool*), un tenant ruidoso o una falla de infraestructura degrada a todos los tenants. El proyecto busca reducir ese radio de impacto y medirlo.

## Opciones
1. **Pool** (todo compartido): más barato y simple; radio de impacto del 100 % de los tenants.
2. **Silo** (una pila por tenant): aislamiento total; costo y operación crecen con el número de tenants.
3. **Celdas**: N copias independientes del servicio, cada una con un subconjunto de tenants, más un router.

## Decisión
Opción 3. Cada celda tiene su propia función y su propia tabla, sin estado compartido. Un router delgado y sin estado decide la celda de cada tenant. El prototipo usa 3 celdas de servicio (c1, c2, c3) con 10 tenants cada una, más una celda de cuarentena (q).

## Consecuencias
- (+) Radio de impacto acotado a 1/N de los tenants; las celdas se agregan por parámetro.
- (−) El router y la tabla de ruteo son compartidos: se mantienen mínimos y su sobrecosto se mide (H3).
- (−) Cada celda tiene menos holgura que un pool del mismo tamaño total; ante picos moderados el pool absorbe mejor (ver la corrección en `docs/CAPACITY.md` del prototipo).
