# Product Backlog — AgroValle Connect

**Proyecto:** AgroValle Connect  
**Stack Tecnológico:** Java 17 / Spring Boot, PostgreSQL
**Metodología de Estimación:** Planning Poker con Escala Fibonacci (1, 2, 3, 5, 8, 13)
**Técnica de Priorización:** MoSCoW (Must Have, Should Have, Could Have, Won't Have)

---

## Matriz Resumen del Product Backlog

| ID | Historia de Usuario | Módulo | Priorización (MoSCoW) | Estimación (Story Points) |
| :---: | :--- | :--- | :---: | :---: |
| **HU-01** | Registro de Agricultores | Autenticación | **Must Have (M)** | **5** |
| **HU-02** | Publicación de Productos | Productores y Ofertas | **Must Have (M)** | **5** |
| **HU-03** | Visualización de Precios Regionales | Catálogo y Búsqueda | **Should Have (S)** | **5** |
| **HU-04** | Filtro de Categorías y Municipios | Catálogo y Búsqueda | **Must Have (M)** | **3** |
| **HU-05** | Contacto Directo / Mensajería | Comunicación | **Should Have (S)** | **3** |
| **HU-06** | Asignación de Ruta de Envío | Logística y Trazabilidad | **Should Have (S)** | **5** |
| **HU-07** | Pago Exitoso con Producto Disponible | Pedidos y Pasarela | **Must Have (M)** | **8** |
| **HU-08** | Validación de Inventario / Sobre-demanda | Pedidos y Stock | **Must Have (M)** | **3** |
| **HU-09** | Calificación de Productos y Agricultores | Reputación | **Could Have (C)** | **3** |
| **HU-10** | Calificación de Servicio de Transporte | Reputación | **Could Have (C)** | **3** |
| **HU-11** | Descarga de Recibo en PDF | Facturación y Historial | **Should Have (S)** | **5** |
| **HU-12** | Confirmación y Emisión de Pedidos | Pedidos y Stock | **Must Have (M)** | **5** |
| **HU-13** | Consulta de Ruta y Trazabilidad en Tiempo Real | Logística y Trazabilidad | **Should Have (S)** | **3** |
| **HU-14** | Registro de Fincas por Municipio | Productores y Ofertas | **Must Have (M)** | **5** |
| **HU-15** | Confirmación de Alistamiento de Lote | Logística y Trazabilidad | **Should Have (S)** | **3** |

---

## Especificación Detallada de Historias de Usuario (BDD)

### Tabla 1. HU-01: Registro de Agricultores
- **Historia:** Como Agricultor, quiero registrarme en la plataforma para ofrecer mis productos.
- **Priorización:** **M** (Must Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** que el usuario no autenticado ingresa a `/api/v1/auth/register`.
  - **When** envía un JSON con `nombre`, `ubicacion_valle` y `cedula` válida.
  - **Then** el sistema responde con status `201 Created` y el registro persiste en la base de datos PostgreSQL.

---

### Tabla 2. HU-02: Publicación de Productos
- **Historia:** Como Agricultor, quiero publicar mis cosechas para que sean visibles en la plataforma.
- **Priorización:** **M** (Must Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** un agricultor autenticado con token JWT.
  - **When** publica un producto enviando `tipo`, `cantidad` y `fecha_cosecha`.
  - **Then** el sistema valida que la fecha no sea anterior a hoy, asigna un ID de producto único y responde status `201 Created`.

---

### Tabla 3. HU-03: Visualización de Precios Regionales
- **Historia:** Como Usuario, quiero ver los precios promedio del Valle del Cauca para negociar mejor.
- **Priorización:** **S** (Should Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** que existen mínimo 50 transacciones registradas de "Café" en las últimas 24 horas.
  - **When** solicito el precio promedio mediante petición GET a `/api/v1/precios/promedio?producto=Cafe`.
  - **Then** el sistema calcula la media aritmética de las transacciones y despliega el valor exacto en pesos colombianos (COP) con status `200 OK`.

---

### Tabla 4. HU-04: Filtro de Categorías y Municipios
- **Historia:** Como Comprador, quiero filtrar entre categorías y municipios del Valle (Dagua, Palmira, Buga) para localizar rápidamente los productos deseados.
- **Priorización:** **M** (Must Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que el sistema cuenta con productos registrados en PostgreSQL clasificados por municipios y categorías.
  - **When** el usuario realiza una petición GET a `/api/v1/productos?municipio=Dagua&categoria=Frutas`.
  - **Then** el sistema responde status `200 OK` y actualiza la vista mostrando únicamente los productos que coinciden con los filtros aplicados.

---

### Tabla 5. HU-05: Contacto Directo / Intención de Compra
- **Historia:** Como Usuario interesado en un producto, quiero contactar directamente al agricultor para obtener información adicional sobre la cosecha.
- **Priorización:** **S** (Should Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** un comprador autenticado con token JWT asociado a un pedido u oferta activa.
  - **When** el comprador selecciona la opción de contactar agricultor enviando un mensaje `POST` a `/api/v1/contacto/mensaje`.
  - **Then** el sistema registra la interacción en la base de datos con fecha, hora, producto asociado, remitente y destinatario, retornando status `200 OK`.

---

### Tabla 6. HU-06: Asignación de Ruta de Envío
- **Historia:** Como Productor agrícola, quiero programar la ruta de envío de mis lotes alistados para asegurar la trazabilidad y evitar pérdidas postcosecha.
- **Priorización:** **S** (Should Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** que el productor tiene un lote de cosecha con estado "Alistado" y una orden de compra confirmada.
  - **When** el productor selecciona la fecha/hora de salida y confirma la asignación de transporte en el Módulo de Logística.
  - **Then** el sistema actualiza el estado del pedido a "En camino", genera un código de seguimiento en tiempo real y notifica inmediatamente al comprador.

---

### Tabla 7. HU-07: Pago Exitoso con Producto Disponible
- **Historia:** Como Comprador, quiero realizar el pago de mi pedido mediante una pasarela segura para confirmar mi compra y notificar al agricultor.
- **Priorización:** **M** (Must Have) | **Estimación:** **8 Story Points**
- **Escenario BDD:**
  - **Given** un usuario autenticado con productos en inventario suficiente.
  - **When** selecciona el método de pago y confirma la transacción exitosamente en la pasarela.
  - **Then** el sistema registra la orden con estado "Pago aprobado", genera la referencia de pago, descuenta las unidades del inventario en PostgreSQL y notifica al agricultor.

---

### Tabla 8. HU-08: Cantidad Solicitada Superior al Inventario
- **Historia:** Como Comprador, quiero que el sistema me impida pagar cantidades que superan el stock para evitar inconsistencias en las compras.
- **Priorización:** **M** (Must Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que un usuario autenticado intenta comprar 16 unidades de un producto y la plataforma solo cuenta con 10 disponibles.
  - **When** procesa la orden hacia la pasarela de pagos.
  - **Then** el sistema rechaza la solicitud, detiene el proceso de pago, no crea la orden y despliega un mensaje notificando la falta de stock.

---

### Tabla 9. HU-09: Calificación de Productos y Agricultores
- **Historia:** Como Comprador, quiero calificar y dejar una reseña al agricultor después de recibir el pedido para informar a otros usuarios sobre la calidad.
- **Priorización:** **C** (Could Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que el comprador tiene un pedido completado y recibido en la plataforma.
  - **When** asigna una puntuación de 1 a 5 estrellas y escribe un comentario en `/api/v1/valoraciones/producto`.
  - **Then** el sistema guarda la valoración, actualiza el promedio de calificación visible en el perfil del agricultor y responde status `201 Created`.

---

### Tabla 10. HU-10: Calificación del Servicio de Transporte
- **Historia:** Como Comprador, quiero calificar el servicio de transporte recibido para evaluar el cumplimiento de tiempos y conservación de la cosecha.
- **Priorización:** **C** (Could Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que el pedido ha sido marcado como "Entregado" en la plataforma.
  - **When** el comprador califica el servicio de transporte con puntuación de 1 a 5 estrellas y comentario.
  - **Then** el sistema guarda la evaluación, recalculando el promedio de desempeño de la ruta de transporte con status `201 Created`.

---

### Tabla 11. HU-11: Descarga de Recibo de Compra en PDF
- **Historia:** Como Comerciante o Restaurante, quiero descargar el recibo de compra en PDF para llevar el registro contable de mis adquisiciones.
- **Priorización:** **S** (Should Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** que el comerciante tiene un pedido registrado con estado "Pago aprobado".
  - **When** hace clic en "Descargar recibo" en el historial de pedidos (`GET /api/v1/pedidos/{id}/pdf`).
  - **Then** el sistema genera y descarga dinámicamente un documento PDF con los detalles del pedido, datos de las partes, desglose de productos y total pagado.

---

### Tabla 12. HU-12: Confirmación y Emisión de Pedidos
- **Historia:** Como Comprador, quiero consolidar los productos de mi carrito en una orden de compra directa para adquirir cosechas de los productores.
- **Priorización:** **M** (Must Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** que el comprador tiene ítems con stock disponible en su carrito de compras.
  - **When** presiona el botón "Confirmar pedido" invocando `POST /api/v1/pedidos`.
  - **Then** el sistema genera la orden de compra en estado "Pendiente de pago", reserva el stock temporalmente y retorna el resumen de la orden.

---

### Tabla 13. HU-13: Consulta de Ruta y Trazabilidad en Tiempo Real
- **Historia:** Como Usuario, quiero consultar la ruta de mi envío mediante un código de seguimiento para coordinar la recepción de la mercancía.
- **Priorización:** **S** (Should Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que el comprador cuenta con una orden en estado "En camino" y un código de seguimiento válido.
  - **When** consulta el código en el Módulo de Logística (`GET /api/v1/logistica/trazabilidad/{codigo}`).
  - **Then** el sistema retorna status `200 OK` con el historial de ruta, punto de origen, destino, ETA (fecha/hora estimada de entrega) y última actualización.

---

### Tabla 14. HU-14: Registro de Fincas por Municipio
- **Historia:** Como Agricultor, quiero registrar los datos de mi finca vinculada a un municipio del Valle (Dagua, Palmira, Buga) para asociar mis cosechas geolocalizadas.
- **Priorización:** **M** (Must Have) | **Estimación:** **5 Story Points**
- **Escenario BDD:**
  - **Given** un agricultor autenticado en la plataforma mediante un token JWT válido.
  - **When** envía una petición `POST` a `/api/v1/fincas` con el JSON de la finca (`nombre`, `municipio_valle`, `hectareas`, `vereda`).
  - **Then** el sistema valida que el municipio pertenezca al departamento del Valle del Cauca, guarda el registro en PostgreSQL y responde status `201 Created`.

---

### Tabla 15. HU-15: Confirmación de Alistamiento de Lote
- **Historia:** Como Productor agrícola, quiero marcar un lote como "Alistado" para indicar que la mercancía está preparada para la recogida logística.
- **Priorización:** **S** (Should Have) | **Estimación:** **3 Story Points**
- **Escenario BDD:**
  - **Given** que el productor tiene una orden de compra confirmada o pagada por un comerciante urbano.
  - **When** selecciona el pedido y envía una petición `PATCH` a `/api/v1/pedidos/{id}/alistamiento`.
  - **Then** el sistema actualiza el estado del pedido a "Alistado" en PostgreSQL, activa la notificación para el módulo de transporte y responde status `200 OK`.

