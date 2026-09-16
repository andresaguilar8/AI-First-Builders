# PRD-001: Gestor de gastos — control mensual de gastos personales

## Contexto y Problema
Actualmente la gestión de gastos personales se realiza mediante una planilla en la que, mes a mes, se agregan, eliminan y modifican filas para registrar los distintos gastos y sus importes.
Esta operatoria resulta tediosa y propensa a errores, especialmente porque muchos gastos son recurrentes pero pueden cambiar de importe entre un mes y otro. Por ejemplo, ante el aumento permanente de una cuota, actualmente es necesario modificar manualmente el valor correspondiente y recordar trasladar ese cambio a los meses siguientes.
Además, la planilla se utiliza como mecanismo de seguimiento de pagos, marcando visualmente aquellos gastos que ya fueron abonados. Esto obliga a revisar periódicamente la información para detectar qué gastos continúan pendientes y puede provocar que algunos pagos se realicen después de su fecha de vencimiento.
La información tampoco permite obtener de forma inmediata una visión consolidada de cuánto se está gastando, cuáles son las categorías con mayor impacto o cuánto dinero permanece pendiente de pago.

Persona:
  - Andrés: administra sus gastos personales mes a mes. Necesita registrar sus obligaciones de forma rápida, identificar fácilmente qué cosas están pagas y cuales no, conocer próximos vencimientos y consultar en qué categorías concentra sus gastos.

## Objetivos
Gestionar de manera práctica los gastos personales de forma mensual para realizar los pagos en tiempo sin olvidarse de nada. También poder visualizar de manera intuitiva un resumen de los gastos para saber en donde se está gastando mas.

## Requerimientos Funcionales

- RF-01: El sistema debe permitir crear un gasto indicando como mínimo nombre y monto. Opcionalmente se podrá indicar descripción, categoría y fecha de vencimiento.
- RF-02: Al crear un gasto, el usuario debe poder indicar si se trata de un gasto recurrente o puntual.
- RF-03: Un gasto recurrente debe poder generarse automáticamente para los meses siguientes mientras permanezca activo.
- RF-04: Un gasto puntual debe pertenecer únicamente al mes para el cual fue registrado y no debe generarse automáticamente en períodos posteriores.
- RF-05: El sistema debe permitir editar el nombre, descripción, monto, categoría y fecha de vencimiento de un gasto.
- RF-06: Al modificar un gasto recurrente, el usuario debe poder distinguir entre modificar solamente el gasto del mes seleccionado o aplicar el cambio al mes seleccionado y a los meses futuros. Las modificaciones nunca deben alterar meses anteriores.
- RF-07: El sistema debe permitir dar de baja un gasto recurrente. La baja debe impedir que el gasto se genere en meses futuros, pero debe conservar su historial de meses anteriores.
- RF-08: El sistema debe permitir crear categorías de gastos.
- RF-09: El usuario debe poder asociar opcionalmente un gasto a una categoría.
- RF-10: El sistema debe permitir visualizar, para un mes determinado, un resumen de los gastos agrupados por categoría.
- RF-11: El sistema debe permitir marcar un gasto como pagado y registrar la fecha en la que fue abonado.
- RF-12: El sistema debe notificar al usuario cuando exista un gasto pendiente de pago cuya fecha de vencimiento coincida con la fecha actual.
- RF-13: Mientras un gasto permanezca pendiente, el sistema debe distinguir visualmente su situación respecto de la fecha de vencimiento:
    - Pendiente: faltan más de 3 días para su vencimiento o no posee fecha de vencimiento.
    - Próximo a vencer: faltan entre 1 y 3 días.
    - Vence hoy: la fecha de vencimiento coincide con la fecha actual.
    - Vencido: la fecha de vencimiento ya pasó.
- RF-14: El sistema debe mostrar inicialmente los gastos correspondientes al mes actual.
- RF-15: El usuario debe poder navegar entre distintos meses y años para consultar información histórica o períodos futuros.
- RF-16: Cada mes debe conservar su propia información de gastos, montos, fechas de vencimiento y estado de pago.
- RF-17: Los cambios realizados sobre un gasto recurrente en un determinado mes no deben modificar retroactivamente los registros correspondientes a meses anteriores.

## Requerimientos No Funcionales
- RNF-01: Las principales operaciones de consulta y actualización deben responder en menos de 2 segundos en el percentil 95 (p95) bajo la carga esperada para el sistema.
- RNF-02: La interfaz debe ser responsive y usable correctamente desde pantallas de al menos 360 px de ancho.
- RNF-03: La aplicación debe poder instalarse y ejecutarse como una Progressive Web App (PWA) en navegadores compatibles.

## Criterios de Aceptación
- AC-01 (RF-01, RF-02): Dado que el usuario completa un gasto con un nombre y un monto y selecciona que es recurrente, cuando confirma el alta, entonces el gasto debe quedar registrado y visible en el mes correspondiente.
- AC-02 (RF-03): Dado un gasto recurrente activo, cuando el usuario consulta el mes siguiente, entonces el sistema debe disponer de una instancia de ese gasto para dicho período.
- AC-03 (RF-11): Dado un gasto pendiente, cuando el usuario lo marca como pagado, entonces debe visualizarse inmediatamente como pagado y quedar registrada su fecha de pago.
- AC-04 (RF-13): Dado un gasto pendiente cuya fecha de vencimiento es dentro de 2 días, cuando se visualiza el mes, entonces debe identificarse como "Próximo a vencer".
- AC-05 (RF-16, RF-17): Dado un gasto de $25.000 registrado en agosto y posteriormente actualizado a $30.000 para septiembre y meses siguientes, cuando el usuario consulta agosto, entonces debe continuar visualizando $25.000.

## Fuera de Alcance
- Gestión de múltiples usuarios.
- Roles y permisos.
- Multi-tenant.
- Cuentas bancarias y sincronización bancaria.
- Gestión de ingresos.

## Riesgos y Dependencias
- Riesgo: ambigüedad al editar un gasto recurrente. El usuario podría querer modificar únicamente el mes actual o cambiar definitivamente el gasto → mitigación: la interfaz debe solicitar explícitamente el alcance de la modificación: "Solo este mes" o "Este mes y los siguientes".
- Riesgo: pérdida de historial al eliminar un gasto habitual → mitigación: utilizar una baja lógica para los gastos recurrentes, manteniendo sus registros mensuales históricos.
- Dependencia: PostgreSQL para persistencia de los datos.
- Dependencia: backend desarrollado con ASP.NET Core, Entity Framework Core y .NET.
- Dependencia: frontend desarrollado con React y TypeScript.
- Dependencia: navegador compatible con las funcionalidades PWA para permitir la instalación de la aplicación.