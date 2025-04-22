# Consultas NoSQL para Sistema Bancario en MongoDB

A continuación se presentan 5 enunciados de consultas basados en las colecciones NoSQL del sistema bancario, junto con las soluciones utilizando operaciones avanzadas de MongoDB.

## 1. Análisis de Saldos por Tipo de Cuenta

**Enunciado:** El departamento financiero necesita un informe que muestre el saldo total, promedio, máximo y mínimo por cada tipo de cuenta (ahorro y corriente) para evaluar la distribución de fondos en el banco.

**Consulta MongoDB:**
```javascript
db.clientes.aggregate([
  {$unwind: "$cuentas"},
  {$group: 
	 {
 	   _id:"$cuentas.tipo_cuenta", 
	   saldoTotal:{$sum: "$cuentas.saldo"}, 
	   saldoMinimo:{$min: "$cuentas.saldo"},
	   saldoMaximo:{$max: "$cuentas.saldo"},
	   promedio:{$avg: "$cuentas.saldo"}				
	 }
	},
  {$project: {_id: 0, tipo_cuenta: "$_id", saldoTotal: 1, saldoMinimo: 1, saldoMaximo: 1, promedio: 1}}
])
```

## 2. Patrones de Transacciones por Cliente

**Enunciado:** El equipo de análisis de comportamiento necesita identificar los patrones de transacciones de cada cliente, mostrando la cantidad y el monto total de transacciones por tipo (depósito, retiro, transferencia) para cada cliente.

**Consulta MongoDB:**
```javascript
db.clientes.aggregate([
{ 
  $lookup: {
    from: "transacciones",
    localField: "_id",
    foreignField: "cliente_ref",
    as: "transacciones"
  }
},
{$unwind: "$transacciones"},
{
  $group: {
    _id: "$transacciones.tipo_transaccion",
    cliente: {$first: "$nombre"},
    cantidad: {$count: {}},
    montoTotal: {$sum: "$transacciones.monto"}
  }
},
{$project: {_id: 0, cliente: 1, tipo_transaccion: "$_id", cantidad:1, montoTotal: 1}}
])
```

## 3. Clientes con Múltiples Tarjetas de Crédito

**Enunciado:** El departamento de riesgo crediticio necesita identificar a los clientes que poseen más de una tarjeta de crédito, mostrando sus datos personales, cantidad de tarjetas y el detalle de cada una.

**Consulta MongoDB:**
```javascript
db.clientes.aggregate([
{$unwind: "$cuentas"},
{$unwind: "$cuentas.tarjetas"},
{$group: 
  {
    _id:"$_id",
    cliente: {$first: {documento: "$cedula", nombre: "$nombre", correo:"$correo", direccion:"$direccion"}},
    cantidadTarjetas:{$count: {}},
    detalleTarjetas: {$push: "$cuentas.tarjetas"}
  }
},
{$project: {_id: 0, cliente: 1, cantidadTarjetas: 1, detalleTarjetas: 1}}
])
```

## 4. Análisis de Medios de Pago más Utilizados

**Enunciado:** El departamento de marketing necesita conocer cuáles son los medios de pago más utilizados para depósitos, agrupados por mes, para orientar sus campañas promocionales.

**Consulta MongoDB:**
```javascript
```

## 5. Detección de Cuentas con Transacciones Sospechosas

**Enunciado:** El departamento de seguridad necesita identificar cuentas con patrones de transacciones sospechosas, definidas como aquellas que tienen más de 3 retiros en un mismo día con un monto total superior a 1,000,000 COP.

**Consulta MongoDB:**
```javascript
```
