Aquí tienes el conjunto completo e integral de los **6 Diagramas de Procesos a Alto Nivel** en formato **`mermaid.js`**, diseñados con enfoque exclusivamente de negocio y experiencia del cliente para cubrir el 100% de las 7 Épicas y 22 Historias de Usuario.

---

### **1. Proceso de Selección y Compra de Productos (`EP-ITC`, `EP-TRX`, `EP-SNT`)**

Describe el recorrido funcional del cliente desde la selección de artículos en la vitrina hasta la confirmación de la compra y la emisión del comprobante digital.

```mermaid.js
flowchart TD
    Inicio([Inicio: Cliente en la Tienda]) --> SeleccionarProducto[Explorar y seleccionar producto con talla y color]
    SeleccionarProducto --> VerificarStock{¿Hay inventario disponible?}

    VerificarStock -- No --> MostrarAgotado[Mostrar mensaje de producto agotado] --> FinAgotado([Fin])

    VerificarStock -- Sí --> AgregarBolsa[Agregar producto a la bolsa de compras]
    AgregarBolsa --> IniciarCheckout[Presionar 'Iniciar Compra']

    IniciarCheckout --> ValidarSesion{¿Tiene la sesión iniciada?}
    ValidarSesion -- No --> ModalAcceso[Ingresar correo y contraseña o registrarse]
    ModalAcceso --> SesionLista[Sesión iniciada correctamente]
    ValidarSesion -- Sí --> PasoDireccion[Ingresar o seleccionar dirección de entrega]
    SesionLista --> PasoDireccion

    PasoDireccion --> RevalidarInventario{¿El producto sigue disponible?}
    RevalidarInventario -- No --> AlertaSinStock[Notificar cambio de disponibilidad] --> FinSinStock([Fin])

    RevalidarInventario -- Sí --> IngresarPago[Ingresar datos de pago]
    IngresarPago --> ValidarPago{¿Pago autorizado?}

    ValidarPago -- No --> ErrorPago[Mostrar mensaje de error en el pago] --> IngresarPago

    ValidarPago -- Sí --> ConfirmarOrden[Generar comprobante con número de pedido]
    ConfirmarOrden --> VaciarBolsa[Vaciar la bolsa de compras del cliente]
    VaciarBolsa --> EnviarCorreo[Enviar correo de confirmación al cliente]
    EnviarCorreo --> FinCompra([Fin del Proceso de Compra])
```

---

### **2. Proceso de Registro y Acceso a la Cuenta (`EP-GAC`, `EP-ITC`)**

Muestra los caminos de decisión para el registro de nuevos usuarios, el inicio de sesión y el flujo seguro de recuperación de credenciales.

```mermaid.js
flowchart TD
    InicioAcceso([Inicio: Cliente requiere acceder a su cuenta]) --> ElegirOpcion{¿Qué acción desea realizar?}

    %% Registro
    ElegirOpcion -- Crear Cuenta --> FormRegistro[Completar datos personales]
    FormRegistro --> ValidarCorreo{¿El correo ya está registrado?}
    ValidarCorreo -- Sí --> AlertaCorreoExiste[Mostrar alerta de correo en uso] --> FormRegistro
    ValidarCorreo -- No --> CrearCuenta[Crear cuenta de usuario]
    CrearCuenta --> ConfirmacionRegistro[Mostrar confirmación e invitar a iniciar sesión] --> FinRegistro([Fin])

    %% Iniciar Sesión
    ElegirOpcion -- Iniciar Sesión --> FormLogin[Ingresar correo y clave]
    FormLogin --> ValidarClave{¿Datos correctos?}
    ValidarClave -- No --> ErrorLogin[Mostrar mensaje de credenciales incorrectas] --> FormLogin
    ValidarClave -- Sí --> AbrirPerfil[Conceder acceso a la cuenta]
    AbrirPerfil --> UnificarBolsa[Consolidar productos guardados antes de ingresar] --> FinLogin([Fin])

    %% Recuperar Clave
    ElegirOpcion -- Olvidó su Clave --> FormRecuperar[Ingresar correo registrado]
    FormRecuperar --> MensajeNeutro[Mostrar mensaje de instrucciones enviadas]
    MensajeNeutro --> AbrirEnlace[Cliente abre enlace recibido por correo]
    AbrirEnlace --> ValidarEnlace{¿El enlace es válido?}
    ValidarEnlace -- No --> AlertaEnlaceVencido[Mostrar mensaje de enlace expirado] --> FinEnlaceExpirado([Fin])
    ValidarEnlace -- Sí --> NuevaClave[Ingresar nueva contraseña]
    NuevaClave --> ConfirmarCambio[Confirmar actualización exitosa] --> FinRecuperacion([Fin])
```

---

### **3. Proceso de Exploración y Búsqueda en el Catálogo (`EP-VEC`, `EP-DDP`)**

Representa la interacción del usuario con la vitrina comercial mediante la barra de búsqueda por palabras clave, combinaciones de filtros laterales y la ficha de producto.

```mermaid.js
flowchart TD
    InicioNavegacion([Inicio: Cliente ingresa al Marketplace]) --> VerPortada[Visualizar categorías y productos destacados]
    VerPortada --> ElegirCamino{¿Cómo desea buscar?}

    %% Búsqueda por Texto
    ElegirCamino -- Usar Buscador --> EscribirTexto[Escribir palabra clave]
    EscribirTexto --> ValidarLongitud{¿Tiene al menos 2 caracteres?}
    ValidarLongitud -- No --> IndicarMinimo[Solicitar escribir más letras] --> EscribirTexto
    ValidarLongitud -- Sí --> BuscarProductos[Buscar productos en la tienda]
    BuscarProductos --> HayResultados{¿Se encontraron coincidencias?}
    HayResultados -- No --> MostrarVacio[Mostrar mensaje sin resultados y sugerencias] --> FinBusqueda([Fin])
    HayResultados -- Sí --> MostrarListado[Mostrar lista de productos encontrados]

    %% Filtros
    ElegirCamino -- Filtrar por Categoría / Marca --> ElegirFiltros[Seleccionar opciones en el panel lateral]
    ElegirFiltros --> CoincidenFiltros{¿Hay productos con esos criterios?}
    CoincidenFiltros -- No --> MostrarMensajeFiltro[Mostrar aviso sin coincidencias y opción 'Limpiar Filtros']
    MostrarMensajeFiltro --> LimpiarFiltros[Restablecer catálogo al estado inicial] --> VerPortada
    CoincidenFiltros -- Sí --> MostrarListadoFiltrado[Mostrar catálogo reducido según los filtros]

    %% Ficha de Producto
    MostrarListado --> AbrirProducto[Seleccionar un producto]
    MostrarListadoFiltrado --> AbrirProducto
    AbrirProducto --> VerFicha[Ver Galería de fotos, descripción, precio, ofertas y variantes] --> FinFicha([Ficha del Producto Lista])
```

---

### **4. Proceso de Historial de Compras y Seguimiento de Envío (`EP-SHP`)**

Contempla la lectura de pedidos pasados, el filtrado dinámico por estado, la opción de volver a comprar (_reorder_) y el seguimiento de la trayectoria del paquete.

```mermaid.js
flowchart TD
    InicioHistorial([Inicio: Cliente ingresa a 'Mis Pedidos']) --> ConsultarCompras[Cargar historial de compras del cliente]
    ConsultarCompras --> TieneCompras{¿Tiene compras anteriores?}

    TieneCompras -- No --> MensajeSinCompras[Mostrar 'Aún no has realizado compras' y botón a la tienda] --> FinSinCompras([Fin])

    TieneCompras -- Sí --> DesplegarPedidos[Mostrar lista de pedidos ordenados por fecha]
    DesplegarPedidos --> ElegirAccion{¿Qué desea hacer?}

    %% Ver Tracking
    ElegirAccion -- Ver Seguimiento del Envío --> ConsultarDespacho[Cargar información del transporte]
    ConsultarDespacho --> HayIncidencia{¿Hay alguna reprogramación o alerta?}
    HayIncidencia -- Sí --> MostrarAlertaEnvio[Mostrar aviso explicativo] --> MostrarLineaTiempo[Mostrar barra de progreso: Registrado ➔ En preparación ➔ En ruta ➔ Entregado]
    HayIncidencia -- No --> MostrarLineaTiempo
    MostrarLineaTiempo --> FinTracking([Fin de Consulta])

    %% Volver a Comprar
    ElegirAccion -- Volver a Comprar --> Reordenar[Seleccionar 'Volver a Comprar']
    Reordenar --> ValidarStockReorden{¿Hay inventario disponible?}
    ValidarStockReorden -- Sí --> CargarABolsa[Añadir productos disponibles a la bolsa] --> IrABolsa([Ir a la Bolsa de Compras])
    ValidarStockReorden -- No --> AvisarSinStock[Notificar qué productos no tienen stock] --> FinReorden([Fin])
```

---

### **5. Proceso de Gestión de Lista de Deseos / Favoritos (`EP-ITC`)**

Detalla cómo el cliente registrado guarda artículos de interés en su espacio personal y los traslada a la bolsa de compras tras validar su disponibilidad.

```mermaid.js
flowchart TD
    InicioFav([Inicio: Cliente explorando productos]) --> MarcarFavorito[Hacer clic en 'Guardar en Favoritos']
    MarcarFavorito --> ValidarAcceso{¿Tiene sesión iniciada?}

    ValidarAcceso -- No --> SolicitarLogin[Pedir iniciar sesión para guardar] --> FinLogin([Ir a Iniciar Sesión])

    ValidarAcceso -- Sí --> GuardarLista[Guardar producto en la lista personal de favoritos]
    GuardarLista --> VerLista[Cliente ingresa a la sección 'Mis Favoritos']
    VerLista --> ElegirAccionFav{¿Qué desea hacer con el producto?}

    ElegirAccionFav -- Quitar de Favoritos --> EliminarFav[Remover artículo de la lista] --> ListaActualizada([Lista Actualizada])

    ElegirAccionFav -- Mover a la Bolsa de Compras --> MoverABolsa[Presionar 'Mover a la Bolsa']
    MoverABolsa --> ConsultarInventario{¿Hay inventario disponible?}
    ConsultarInventario -- Sí --> TransferirBolsa[Añadir producto a la bolsa de compras] --> FinTransferencia([Ir a la Bolsa de Compras])
    ConsultarInventario -- No --> NotificarAgotado[Avisar que el producto está agotado actualmente] --> VerLista
```

---

### **6. Proceso de Notificaciones y Comunicaciones al Cliente (`EP-SNT`)**

Describe la emisión automática de comunicaciones por correo electrónico para confirmaciones de compra, catálogo de productos recomendados y avances en la entrega.

```mermaid.js
flowchart TD
    InicioNotif([Inicio: Ocurre un evento en la tienda]) --> EvaluarEvento{¿Qué tipo de evento ocurrió?}

    %% Confirmación de Compra
    EvaluarEvento -- Compra Realizada --> PrepararCorreoCompra[Generar comprobante digital con detalle del pedido]
    PrepararCorreoCompra --> IncluirRecomendados[Agregar sección con productos recomendados del catálogo]
    IncluirRecomendados --> DespacharCorreoCompra[Enviar correo de confirmación al buzón del cliente] --> FinNotif1([Notificación Enviada])

    %% Actualización de Envío
    EvaluarEvento -- Cambio de Estado en Envío --> PrepararCorreoEnvio[Generar aviso de estado: En Ruta o Entregado]
    PrepararCorreoEnvio --> DespacharCorreoEnvio[Enviar correo de actualización de despacho] --> FinNotif2([Notificación Enviada])
```

---

### **Resumen de Cobertura de Requerimientos**

|N° Diagrama|Nombre del Proceso|Épicas Asociadas|Cobertura Funcional del Proyecto|
|:--|:--|:--|:--|
|**1**|Selección y Compra|`EP-ITC`, `EP-TRX`, `EP-SNT`|Carrito, validaciones de stock, checkout multipaso, simulación de pago y confirmación.|
|**2**|Registro y Acceso|`EP-GAC`, `EP-ITC`|Formulario de registro, login con sesión activa, recuperación de clave y unificación de bolsa.|
|**3**|Exploración y Búsqueda|`EP-VEC`, `EP-DDP`|Portada comercial, buscador por palabra clave, filtros por categoría/marca y ficha técnica.|
|**4**|Historial y Seguimiento|`EP-SHP`|Visor "Mis Pedidos", filtrado por estado, barra de progreso de envío y opción _reorder_.|
|**5**|Lista de Deseos|`EP-ITC`|Persistencia de favoritos, eliminación y traslado a bolsa con revalidación de stock.|
|**6**|Notificaciones|`EP-SNT`|Correos de confirmación de pedido, catálogo de recomendación y avisos de despacho.|

---

🎨 Con estos 6 diagramas de procesos listos a alto nivel, ¿procedemos a redactar la especificación del **Mapa de Navegación de Pantallas y Wireframes** para maquetar los prototipos del **Hito 2** en **Stitch AI / Figma**?