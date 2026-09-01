# Bar Katmandu — TPV

Sistema de punto de venta (TPV) a medida para un bar-restaurante nepalí, pensado para funcionar en una tablet o pantalla táctil tras la barra: gestión de mesas, comandas, cobro, apertura de cajón portamonedas e impresión de tickets — sin caja registradora ni software de terceros.

## El problema que resuelve

Un bar necesita un TPV que lleve mesas, comandas y cobros, pero el software de punto de venta comercial suele ir por suscripción, exige hardware propietario y no está pensado para una carta específica (tapas y platos nepalíes junto a la carta de bar habitual). Esta aplicación es un TPV construido a medida: corre en cualquier tablet o PC con navegador, se adapta exactamente a la carta del local y no depende de ningún servicio externo de pago — el dinero se cobra en persona, la app solo lleva la cuenta.

## Qué hace

- **Gestión de mesas**: barra + mesas numeradas, con la posibilidad de dividir una mesa en submesas (A/B/C) cuando los clientes piden por separado, y de **mover una comanda entera de una mesa a otra** (por ejemplo, si el cliente cambia de sitio).
- **Toma de comandas** por categorías de carta (cócteles, aperitivos, sangría, vino, cerveza, refrescos, tapas, barbacoa nepalí, momos, platos principales, postres), con productos de precio fijo y también líneas de **precio editable** (para artículos que varían, como "Cava" o un chupito).
- **Cobro** con dos métodos — efectivo y tarjeta —, cálculo automático del cambio a partir del importe recibido, y **apertura del cajón portamonedas físico** vía QZ Tray al cobrar en efectivo.
- **Impresión de tickets** de comanda y de cobro en una impresora térmica, usando estilos de impresión dedicados (`@media print`) para que el ticket salga con el formato correcto sin necesidad de una app de impresión aparte.
- **Cierre de caja diario**: resumen de todas las ventas del día separadas por método de pago (efectivo/tarjeta), con total general y opción de imprimir el resumen y cerrar el día.
- **Persistencia local**: todo el estado (comandas abiertas por mesa, ventas del día, número de ticket) se guarda en el propio dispositivo (`localStorage`), así que un refresco de página o un cierre accidental de la pestaña no hace perder la comanda en curso.

## Decisiones técnicas destacables

- **Integración con hardware real de bar**: el cajón portamonedas se abre enviando comandos ESC/POS crudos a la impresora térmica a través de **QZ Tray**, no con un simple `window.print()` — esto es lo que permite que el cajón se abra físicamente al cobrar en efectivo, igual que un TPV comercial.
- **Modelo de mesas con submesas**: cada mesa puede dividirse en A/B/C manteniendo comandas independientes, y "mover comanda" traslada todo el pedido conservando cantidades y líneas — pensado para el caso real de un bar (clientes que se juntan o se separan).
- **Números de ticket correlativos persistentes**: el contador de tickets vive en `localStorage` y sobrevive a recargas, para no repetir ni saltar números en el arqueo del día.
- **Sin backend ni conexión a internet necesaria para operar**: toda la lógica de negocio (comandas, cobro, cierre de caja) es 100% cliente — el bar puede seguir cobrando aunque falle la conexión, algo crítico para un TPV en producción.
- **CSS de impresión dedicado**: las vistas de ticket y de cierre de caja tienen sus propias reglas `@media print`, separadas de la interfaz táctil, para que lo que sale por la impresora térmica tenga el ancho y formato de un ticket real.

## Stack

HTML5 · CSS3 · JavaScript vanilla (ES6+) · `localStorage` para persistencia · QZ Tray (impresión térmica y apertura de cajón) · `window.print()` con hojas de estilo de impresión dedicadas
