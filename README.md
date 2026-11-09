# DC El Destape - Licores 🍷📦

Aplicación web progresiva (PWA) ejecutiva para la gestión integral de **Inventario**, **Compras**, **Punto de Venta (POS)**, **Apartados**, **Clientes y Fidelidad**, y **Sincronización en la Nube**.

Desarrollada para **Carlos y Daniel (DC)** con una arquitectura **Offline-First**, soporte bimonetario (USD / CRC) y conexión en tiempo real con la aplicación de preventa en ruta.

---

## 🚀 Módulos y Funcionalidades Principales

### 1. 🛒 Punto de Venta (POS / Ventas)
- **Ventas Rápidas y Detalladas:** Búsqueda en vivo de productos por nombre, código de barras o categoría.
- **Soporte Bimonetaria:** Conversión automática en tiempo real entre Dólares ($ USD) y Colones (₡ CRC) según el tipo de cambio oficial del día.
- **Múltiples Formas de Pago:** Efectivo (con cálculo de vuelto), SINPE Móvil, Tarjeta y Pagos Mixtos.
- **Programa de Fidelidad de Clientes:**
  - Acumulación automática de puntos (1 punto por cada ₡100 de compra).
  - Canje de puntos como descuento directo en el carrito.
- **Facturación Integrada desde Preventa:**
  - Carga con un solo clic de pedidos tomados por los vendedores en ruta.
- **Facturación y Liquidación de Apartados:**
  - Precarga directa del apartado en el carrito de Ventas (`cargarApartadoEnPOS`).
  - Asignación automática del cliente y sus puntos.
  - Generación de factura oficial y entrega física.
  - **Control estricto de inventario:** no duplica la deducción de stock al facturar un apartado previamente reservado.
- **Comprobantes y Recibos:**
  - Modal de recibo térmico con desglose completo.
  - Opción de impresión directa o envío del ticket formateado por WhatsApp.

---

### 2. 📑 Módulo de Apartados
- **Creación y Reserva:** Reserva inmediata de stock para que las botellas no se vendan a otros clientes.
- **Control de Abonos:**
  - Registro de múltiples abonos con método de pago (SINPE, Efectivo, Tarjeta).
  - Cálculo instantáneo de monto total, total abonado y saldo pendiente.
  - Comprobante de abono listo para compartir al cliente por WhatsApp.
- **Liquidación Fluida:**
  - Botón **"Facturar en Ventas"** que transfiere los productos, cliente y montos a la caja para su cobro y facturación oficial.

---

### 3. 📦 Gestión de Inventario y Catálogo
- **Catálogo de Licores:** Visualización con filtros por categoría, búsqueda por texto y alertas de stock mínimo.
- **Cálculo de Costo Promedio Ponderado:** Mantiene el costo real según las compras ingresadas.
- **Creación Inteligente de Productos:**
  - **Costo Base Proveedor ($):** Monto cotizado por el distribuidor.
  - **Envío / Flete de Entrega ($):** Cargo fijo precargado en **$2.00** por defecto (editable según el proveedor).
  - **Costo Total Real:** Cálculo de `Costo Base + Flete` en USD y CRC.
  - **Margen de Ganancia (%):** Margen deseado configurable (ej. 70%-80%).
  - **Precio Recomendado Automático:** Fórmula de cálculo automático del precio de venta al público en USD y CRC.
  - Fotografía/URL del producto y nivel de stock mínimo de seguridad.

---

### 4. 🚚 Módulo de Compras (Entradas)
- Registro de compras a distribuidores con fecha, factura de compra y proveedor.
- Cálculo de flete/envío de compra prorrateado o sumado al costo de adquisición.
- Actualización automática del stock y del costo unitario promedio en inventario.

---

### 5. 👥 Clientes y Fidelidad
- Base de datos local de clientes con cédula/identificación, teléfono y dirección.
- Historial de compras y apartados por cliente.
- Balance acumulado de puntos de fidelidad.

---

### 6. 🧾 Facturas y Comisiones
- Historial completo de ventas y facturas emitidas (`VTA-XXXXXX`).
- Posibilidad de consultar el detalle de cada factura y reenviar el comprobante por WhatsApp en cualquier momento.
- Registro del colaborador o vendedor que gestionó o facturó la venta para comisiones.

---

## 🏛️ Arquitectura Técnica

```
┌─────────────────────────────────────────────────────────────┐
│                      DC El Destape                          │
├───────────────────────────────┬─────────────────────────────┤
│   inventory-sheets-app        │      preventa-app           │
│   (App Central / POS)         │   (Vendedores Móviles)      │
└───────────────┬───────────────┴──────────────┬──────────────┘
                │                              │
                │        REST API Sync         │
                ▼                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Backend Serverless                        │
│                 (API REST & Base de Datos)                  │
│   • Productos  • Compras  • Ventas  • Apartados  • Clientes │
└─────────────────────────────────────────────────────────────┘
```

- **Frontend:** Vanilla JavaScript modular y optimizado, HTML5 semántico, Tailwind CSS (CDN) e iconos Lucide.
- **Persistencia Local:** `localStorage` para almacenamiento inmediato ultrarrápido y cache offline.
- **Sincronización:** Cola de eventos offline-first (`encolarAccionSincronizacion`) que reintenta automáticamente cualquier acción cuando se recupera la conexión a internet.
- **Backend:** API REST Serverless expuesta para lectura y sincronización atómica de datos.
- **PWA:** `sw.js` (Service Worker) y `manifest.json` para instalación en computadoras de escritorio, tablets y teléfonos móviles.

---

## 📂 Estructura de Archivos

```
inventory-sheets-app/
├── index.html                   # Interfaz principal de la aplicación POS y administración
├── app.js                       # Lógica central del sistema (POS, inventario, apartados, sync)
├── styles.css                   # Estilos personalizados y ajustes de interfaz
├── sw.js                        # Service Worker para funcionamiento Offline y caché PWA
├── manifest.json                # Manifiesto de la Progressive Web App (PWA)
├── logo.png                     # Logotipo oficial de DC El Destape
├── google_apps_script.gs        # Backend de sincronización y persistencia de datos
├── .gitignore                   # Exclusión de temporales y archivos del sistema
└── README.md                    # Este documento
```

---

## ⚙️ Puesta en Marcha

1. **Abrir Localmente:**
   - Puedes abrir `index.html` directamente en cualquier navegador moderno (Google Chrome, Microsoft Edge, Brave).
   - O ejecutar un servidor HTTP simple (por ejemplo `npx serve` o la extensión Live Server de VS Code).

2. **Configuración del Backend:**
   - Desplegar el script de backend como Aplicación Web (*Web App*).
   - Colocar la URL del endpoint en la configuración de la aplicación (`state.config.scriptUrl`).

---

## 📄 Notas de Versión Recientes

- **Integración de Apartados en Ventas:** Carga directa de apartados al carrito de Ventas sin diálogos invasivos, con asignación de cliente, cálculo de puntos y protección contra doble rebaja de inventario.
- **Flete en Creación de Producto:** Inclusión de cargo fijo de entrega ($2.00) en el cálculo de costo real total y margen de venta recomendado.
- **Módulo de Consulta de Facturas:** Historial completo para Carlos y Daniel con reenvío de tickets por WhatsApp.
