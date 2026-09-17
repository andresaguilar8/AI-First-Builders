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
- RF-03: Un gasto recurrente debe estar disponible automáticamente en los meses siguientes mientras permanezca activo. Los registros de todos los meses futuros no deben generarse de antemano: el gasto recurrente se proyectará al consultar cada período y se conservará una instancia propia del mes cuando exista información específica de ese período, por ejemplo una modificación o un pago.
- RF-04: Un gasto puntual debe pertenecer únicamente al mes para el cual fue registrado y no debe generarse automáticamente en períodos posteriores.
- RF-05: El sistema debe permitir editar el nombre, descripción, monto, categoría y fecha de vencimiento de un gasto.
- RF-06: Al modificar un gasto recurrente, el usuario debe poder distinguir entre modificar solamente el gasto del mes seleccionado o aplicar el cambio al mes seleccionado y a los meses futuros. Las modificaciones nunca deben alterar meses anteriores.
- RF-07: El sistema debe permitir dar de baja un gasto recurrente. La baja debe impedir que el gasto se genere en meses futuros, pero debe conservar su historial de meses anteriores.
- RF-08: El sistema debe permitir crear categorías de gastos.
- RF-09: El usuario debe poder asociar opcionalmente un gasto a una categoría.
- RF-10: El sistema debe permitir visualizar, para un mes determinado, un resumen de los gastos agrupados por categoría.
- RF-11: El sistema debe permitir marcar un gasto como pagado y registrar la fecha en la que fue abonado.
- RF-12: Al ingresar a la aplicación, el sistema debe mostrar una alerta interna cuando exista al menos un gasto pendiente de pago cuya fecha de vencimiento coincida con la fecha actual. Esta notificación corresponde únicamente a una alerta dentro de la aplicación; no incluye notificaciones push, correo electrónico ni avisos con la aplicación cerrada.
- RF-13: Mientras un gasto permanezca pendiente, el sistema debe distinguir visualmente su situación respecto de la fecha de vencimiento:
    - Pendiente: faltan más de 3 días para su vencimiento o no posee fecha de vencimiento.
    - Próximo a vencer: faltan entre 1 y 3 días.
    - Vence hoy: la fecha de vencimiento coincide con la fecha actual.
    - Vencido: la fecha de vencimiento ya pasó.
- RF-14: El sistema debe mostrar inicialmente los gastos correspondientes al mes actual.
- RF-15: El usuario debe poder navegar entre distintos meses y años para consultar información histórica o períodos futuros.
- RF-16: Cada mes debe conservar su propia información de gastos, montos, fechas de vencimiento y estado de pago.
- RF-17: Los cambios realizados sobre un gasto recurrente en un determinado mes no deben modificar retroactivamente los registros correspondientes a meses anteriores.
- RF-18: El sistema debe requerir autenticación mediante email y contraseña antes de permitir el acceso a la información de gastos. Para el alcance inicial existirá un único usuario y no habrá gestión de múltiples cuentas, roles ni permisos.
- RF-19: El sistema debe permitir exportar a un archivo CSV el historial de gastos, incluyendo como mínimo período, nombre, monto, categoría, vencimiento, estado de pago y fecha de pago.

## Requerimientos No Funcionales
- RNF-01: Las principales operaciones de consulta y actualización deben responder en menos de 2 segundos en el percentil 95 (p95), considerando un único usuario concurrente, hasta 100 gastos por mes y hasta 10 años de información histórica.
- RNF-02: La interfaz debe ser responsive y usable correctamente desde pantallas de al menos 360 px de ancho.
- RNF-03: La aplicación debe poder instalarse y ejecutarse como una Progressive Web App (PWA) en navegadores compatibles. El acceso y modificación de los datos requerirá conexión a Internet; el funcionamiento offline queda fuera del alcance inicial.
- RNF-04: Las contraseñas no deben almacenarse en texto plano y deberán persistirse utilizando un mecanismo seguro de hash.

## Criterios de Aceptación
- AC-01 (RF-01, RF-02): Dado que el usuario completa un gasto con un nombre y un monto y selecciona que es recurrente, cuando confirma el alta, entonces el gasto debe quedar registrado y visible en el mes correspondiente.
- AC-02 (RF-03): Dado un gasto recurrente activo, cuando el usuario consulta el mes siguiente, entonces el gasto debe visualizarse en ese período aunque previamente no se haya creado manualmente un gasto para ese mes.
- AC-03 (RF-04): Dado un gasto puntual creado para un mes de un determinado año, cuando el usuario consulta el més siguiente del mismo año, entonces dicho gasto no debe aparecer en ese período.
- AC-03 (RF-11): Dado un gasto pendiente, cuando el usuario lo marca como pagado, entonces debe visualizarse inmediatamente como pagado y quedar registrada su fecha de pago.
- AC-04 (RF-06): Dado un gasto recurrente de $30.000, cuando en septiembre el usuario cambia su monto a $35.000 seleccionando "Solo este mes", entonces septiembre debe mostrar $35.000 y octubre debe continuar mostrando $30.000.
- AC-05 (RF-06, RF-16, RF-17): Dado un gasto recurrente cuyo monto hasta agosto es de $25.000, cuando en septiembre el usuario lo modifica a $30.000 seleccionando "Este mes y los siguientes", entonces agosto debe continuar mostrando $25.000 y septiembre y los períodos posteriores deben utilizar $30.000.
- AC-06 (RF-07): Dado un gasto recurrente que posee registros entre enero y septiembre, cuando el usuario lo da de baja en septiembre, entonces sus registros históricos deben continuar disponibles y el gasto no debe generarse en los meses posteriores.
- AC-07 (RF-10): Dados gastos por $300.000 asociados a la categoría "Vivienda" y $100.000 asociados a "Servicios" durante septiembre, cuando el usuario consulta el resumen de ese mes, entonces debe visualizar $300.000 en Vivienda y $100.000 en Servicios.
- AC-08 (RF-11): Dado un gasto pendiente, cuando el usuario lo marca como pagado, entonces debe visualizarse inmediatamente como pagado y quedar registrada la fecha de pago.
- AC-09 (RF-12): Dado un gasto pendiente cuya fecha de vencimiento coincide con la fecha actual, cuando el usuario ingresa a la aplicación, entonces debe visualizar una alerta indicando que posee al menos un gasto que vence ese día.
- AC-10 (RF-13): Dado un gasto pendiente cuya fecha de vencimiento es dentro de 2 días, cuando se visualiza el mes, entonces debe identificarse como "Próximo a vencer".
- AC-11 (RF-15): Dado que el usuario está visualizando septiembre de 2026, cuando navega al mes anterior, entonces debe visualizar los gastos correspondientes a agosto de 2026.
- AC-12 (RF-18): Dado un usuario que no se encuentra autenticado, cuando intenta acceder a la información de gastos, entonces el sistema no debe mostrar los datos y debe solicitar autenticación.
- AC-13 (RF-19): Dado que existen gastos registrados en distintos meses, cuando el usuario solicita exportar su historial, entonces el sistema debe generar un archivo CSV que contenga los gastos registrados junto con su período, nombre, monto, categoría, vencimiento, estado y fecha de pago.

## Fuera de Alcance
- Gestión de múltiples usuarios.
- Roles y permisos.
- Multi-tenant.
- Cuentas bancarias y sincronización bancaria.
- Gestión de ingresos.
- Funcionamiento y edición de información sin conexión.
- Sincronización offline.
- Notificaciones push.
- Notificaciones por correo electrónico o WhatsApp.

## Riesgos y Dependencias
- Riesgo: ambigüedad al editar un gasto recurrente. El usuario podría querer modificar únicamente el mes actual o cambiar definitivamente el gasto → mitigación: la interfaz debe solicitar explícitamente el alcance de la modificación: "Solo este mes" o "Este mes y los siguientes".
- Riesgo: pérdida de historial al eliminar un gasto habitual → mitigación: utilizar una baja lógica para los gastos recurrentes, manteniendo sus registros mensuales históricos.
- Dependencia: PostgreSQL para persistencia de los datos.
- Dependencia: backend desarrollado con ASP.NET Core, Entity Framework Core y .NET.
- Dependencia: frontend desarrollado con React y TypeScript.
- Dependencia: navegador compatible con las funcionalidades PWA para permitir la instalación de la aplicación.