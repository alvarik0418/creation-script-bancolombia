# Consultas SQL para Base de Datos Bancaria

## Enunciado 1: Clientes con múltiples cuentas y sus saldos totales

**Necesidad:** El banco necesita identificar a los clientes que tienen más de una cuenta, mostrar cuántas cuentas tiene cada uno y el saldo total acumulado en todas sus cuentas, ordenados por saldo total de mayor a menor.

**Consulta SQL:**
```sql
select c.cedula, c.nombre, count(ct.*) cantidadCuentas, sum(ct.saldo) SaldoTotalAcumulado
from cliente c inner join cuenta ct on ct.id_cliente = c.id_cliente
group by c.cedula, c.nombre
having count(ct.*) > 1
order by c.nombre asc;
```

## Enunciado 2: Comparativa entre depósitos y retiros por cliente

**Necesidad:** El departamento de análisis financiero necesita comparar los montos totales de depósitos y retiros realizados por cada cliente, para identificar patrones de comportamiento financiero.

**Consulta SQL:**
```sql
select c.cedula, c.nombre, tx.tipo_transaccion, sum(tx.monto) MontoTotal
from cliente c inner join cuenta ct on ct.id_cliente = c.id_cliente
			   inner join Transaccion tx on tx.num_cuenta = ct.num_cuenta
where tx.tipo_transaccion in ('deposito','retiro')
group by c.cedula, c.nombre, tx.tipo_transaccion
order by c.nombre asc;
```

## Enunciado 3: Cuentas sin tarjetas asociadas

**Necesidad:** El departamento de tarjetas necesita identificar todas las cuentas que no tienen tarjetas asociadas para ofrecer productos específicos a estos clientes.

**Consulta SQL:**
```sql

```

## Enunciado 4: Análisis de saldos promedio por tipo de cuenta y comportamiento transaccional

**Necesidad:** La gerencia necesita un análisis comparativo del saldo promedio entre cuentas de ahorro y corriente, pero solo considerando aquellas cuentas que han tenido al menos una transacción en los últimos 30 días.

**Consulta SQL:**
```sql

```

## Enunciado 5: Clientes con transferencias pero sin retiros en cajeros

**Necesidad:** El equipo de marketing digital necesita identificar a los clientes que utilizan transferencias pero no realizan retiros por cajeros automáticos, para dirigir campañas de banca digital.

**Consulta SQL:**
```sql

```
