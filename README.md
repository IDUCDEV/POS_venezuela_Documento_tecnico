# POS_venezuela_Documento_tecnico

# PLAN DE DESARROLLO COMPLETO - POS VENEZUELA

## 📋 INFORMACIÓN DEL PROYECTO

| Campo | Valor |
|-------|-------|
| **Nombre** | BodegaPOS / TuCaja |
| **Tipo** | Aplicación móvil multiplataforma (Flutter) |
| **Target** | Bodegas y negocios pequeños en Venezuela |
| **Arquitectura** | Offline-First con sincronización |
| **Stack Backend** | Serverpod + Supabase |
| **Stack Frontend** | Flutter + Hive + BLoC |
| **Arquitectura** | Clean Architecture (Domain, Data, Presentation) |
| **Offline-First** | Sí (Hive como fuente primaria) |

---

## 🎯 RESUMEN EJECUTIVO

Aplicación móvil que convierte el teléfono móvil del empleado o dueño de tienda en una caja registradora completa. Permite gestionar inventarios mediante código de barras y realizar ventas de forma offline, sincronizando datos cuando hay conectividad.

### Problema de Negocio
- Negocios pequeños en Venezuela no tienen sistemas de punto de venta accesibles
- Conectividad internet inestable en muchas zonas (aunque algunos negocios sí tienen)
- Necesidad de controlar inventario y ventas desde cualquier dispositivo

### Solución
- App **100% offline-first** que funciona sin internet
- Detección automática de conectividad (online/offline)
- Sincronización bidireccional cuando hay conexión (push + pull)
- Escaneo de códigos de barras (cámara del teléfono)
- SKU interno para productos sin código de barras

---

# 🧩 1. ETAPAS DEL DESARROLLO (ROADMAP)

## Fase 1: Fundación y Arquitectura (Semanas 1-2)

### Semana 1: Setup y Arquitectura Base

**Objetivo Estratégico**: Establecer la base técnica del proyecto

**Qué se construye exactamente**:
- [ ] Inicializar proyecto Flutter con estructura Clean Architecture
- [ ] Configurar Hive para almacenamiento local
- [ ] Configurar Supabase Client
- [ ] Configurar Serverpod (backend)
- [ ] Implementar sistema de autenticación local (PIN)
- [ ] Crear modelos de datos locales (Product, Sale, User, Category)
- [ ] Implementar Repository Pattern para acceso a datos
- [ ] Configurar logger y manejo de errores

**Qué NO se construye aún**:
- UI de ventas
- Escaneo de códigos
- Impresión de recibos
- Sincronización avanzada

**Dependencias Técnicas**:
- Flutter SDK 3.x
- Supabase account configurado
- Servidor para Serverpod (puede ser local inicialmente)

**Riesgos**:
- Configuración inicial de Serverpod puede tomar más tiempo
- Sincronización offline puede tener bordes complejos

**Validaciones Obligatorias**:
- [ ] App compila para Android y iOS
- [ ] Hive inicializa correctamente
- [ ] Modelos se serializan/deserializan correctamente
- [ ] Autenticación con PIN funciona localmente

---

### Semana 2: Módulo de Inventario (CRUD Productos)

**Objetivo Estratégico**: Permitir gestión completa de productos

**Qué se construye exactamente**:
- [ ] Pantalla de listado de productos
- [ ] Pantalla de detalle de producto
- [ ] Pantalla de agregar/editar producto
- [ ] Generador de SKU interno (formato: BOD-XXXXXX)
- [ ] Gestión de categorías
- [ ] Búsqueda y filtrado de productos
- [ ] Control de stock mínimo (alertas)
- [ ] Validación de datos de producto

**Qué NO se construye aún**:
- Escaneo de códigos de barras
- Sincronización con servidor
- Reportes

**Dependencias Técnicas**:
- Fase 1 completada
- Modelos de Product y Category creados

**Riesgos**:
- Ninguno significativo

**Validaciones Obligatorias**:
- [ ] CRUD completo de productos funciona offline
- [ ] SKU se genera automáticamente sin duplicados
- [ ] Stock mínimo muestra alertas correctamente
- [ ] Búsqueda es rápida (<100ms para 1000 productos)

---

## Fase 2: Módulo de Ventas (Semanas 3-4)

### Semana 3: Carrito y Proceso de Venta

**Objetivo Estratégico**: Implementar flujo completo de ventas

**Qué se construye exactamente**:
- [ ] Pantalla de caja/ventas (main POS screen)
- [ ] Agregar productos al carrito (por código, búsqueda, categoría)
- [ ] Modificar cantidad en carrito
- [ ] Eliminar items del carrito
- [ ] Cálculo de subtotal, descuento, total
- [ ] Aplicar descuento (porcentaje o monto fijo)
- [ ] Selección de método de pago (efectivo, transferencia, móvil, mixto)
- [ ] Finalizar venta
- [ ] Guardar venta en Hive

**Qué NO se construye aún**:
- Escaneo de códigos de barras en ventas
- Impresión de recibos
- Sincronización de ventas

**Dependencias Técnicas**:
- Fase 1 completada
- Inventario operativo

**Riesgos**:
- Manejo de decimales en moneda venezolana (Bs)

**Validaciones Obligatorias**:
- [ ] Carrito actualiza correctamente al agregar/eliminar items
- [ ] Cálculos matemáticos son exactos
- [ ] Venta se guarda correctamente en Hive
- [ ] Stock se descuenta correctamente

---

### Semana 4: Escaneo de Códigos de Barras

**Objetivo Estratégico**: Integrar escaneo de códigos para ventas e inventario

**Qué se construye exactamente**:
- [ ] Integrar package mobile_scanner
- [ ] Pantalla de escaneo (overlay con guía)
- [ ] Detección de códigos de barras (EAN, UPC, Code128, QR)
- [ ] Búsqueda de producto por código escaneado
- [ ] Agregar producto escaneado al carrito
- [ ] Flashlight (linterna) para zonas oscuras
- [ ] Cámaras (frontal/trasera)
- [ ] Escaneo desde pantalla de inventario (agregar nuevo)

**Qué NO se construye aún**:
- Impresión de recibos
- Productos sin código (solo búsqueda manual)

**Dependencias Técnicas**:
- mobile_scanner configurado
- Permisos de cámara en Android/iOS

**Riesgos**:
- Compatibilidad con diferentes dispositivos
- Rendimiento del escaneo en tiempo real

**Validaciones Obligatorias**:
- [ ] Escaneo detecta códigos correctamente
- [ ] Producto encontrado se agrega al carrito
- [ ] Producto no encontrado muestra mensaje de error (NO permite crear desde venta)
- [ ] Flash funciona correctamente

---

## Fase 3: Impresión y Hardware (Semanas 5-6)

### Semana 5: Impresión de Recibos

**Objetivo Estratégico**: Integrar impresión térmica de recibos

**Qué se construye exactamente**:
- [ ] Integrar flutter_thermal_printer_pos o esc_pos_bluetooth
- [ ] Pantalla de configuración de impresoras
- [ ] Búsqueda de impresoras Bluetooth
- [ ] Conexión/desconexión de impresoras
- [ ] Template de recibo (diseño ESC/POS)
- [ ] Impresión de recibo tras venta
- [ ] Reimpresión de recibos anteriores
- [ ] Logo en encabezado (imagen a texto)

**Qué NO se construye aún**:
- Caja(ch) (otras integraciones de hardware)

**Dependencias Técnicas**:
- Impresora térmica compatible ESC/POS
- Bluetooth en dispositivo

**Riesgos**:
- Compatibilidad con diferentes modelos de impresoras
- Conexión Bluetooth puede ser inestable

**Validaciones Obligatorias**:
- [ ] Impresora se descubre y conecta
- [ ] Recibo se imprime correctamente
- [ ] Formato es legible y profesional
- [ ] Manejo de errores de conexión

---

### Semana 6: Usuarios y Roles

**Objetivo Estratégico**: Sistema multi-usuario con controles de acceso

**Qué se construye exactamente**:
- [ ] Crear usuario administrador
- [ ] Crear usuarios empleados (con PIN)
- [ ] Login con PIN
- [ ] Restricciones por rol (admin vs employee)
- [ ] Historial de ventas por usuario
- [ ] Cerrar sesión
- [ ] Cambiar PIN

**Qué NO se construye aún**:
- Sincronización de usuarios con servidor

**Dependencias Técnicas**:
- Fase 1 completada

**Riesgos**:
- Almacenamiento seguro de PIN (hash)

**Validaciones Obligatorias**:
- [ ] PIN se guarda hasheado
- [ ] Login valida correctamente
- [ ] Roles se respetan en toda la app

---

## Fase 4: Sincronización y Backend (Semanas 7-8)

### Semana 7: Sincronización con Supabase

**Objetivo Estratégico**: Sincronización de datos cuando hay internet

**Qué se construye exactamente**:
- [ ] Configurar tablas en Supabase
- [ ] Implementar Row Level Security
- [ ] Sincronización de productos (upload/download)
- [ ] Sincronización de ventas
- [ ] Sync Queue (cola de operaciones pendientes)
- [ ] Detección de conectividad (connectivity_plus)
- [ ] Sync automático cuando hay conexión
- [ ] Sync manual (botón forzar sync)

**Qué NO se construye aún**:
- Serverpod (se mantiene solo con Supabase por ahora)

**Dependencias Técnicas**:
- Supabase proyecto creado
- Tablas creadas

**Riesgos**:
- Conflictos de datos (mismo producto editado en dos dispositivos)
- Pérdida de datos si no se sincroniza

**Validaciones Obligatorias**:
- [ ] Productos se suben a Supabase
- [ ] Productos se descargan de Supabase
- [ ] Ventas se sincronizan
- [ ] Cola de sync procesa correctamente

---

### Semana 8: Serverpod como API Layer

**Objetivo Estratégico**: Implementar Serverpod para lógica de negocio

**Qué se construye exactamente**:
- [ ] Setup de Serverpod
- [ ] Endpoints REST/gRPC para productos
- [ ] Endpoints para ventas
- [ ] Endpoints para reportes
- [ ] Integración con Supabase como database
- [ ] Lógica de sincronización
- [ ] Validaciones de negocio

**Qué NO se construye aún**:
- Funcionalidades avanzadas de reportes

**Dependencias Técnicas**:
- Supabase configurado
- Docker para Serverpod (opcional)

**Riesgos**:
- Configuración inicial de Serverpod
- Latencia de red

**Validaciones Obligatorias**:
- [ ] Serverpod responde correctamente
- [ ] CRUD funciona via API
- [ ] Sincronización con Serverpod + Supabase

---

## Fase 5: Reportes y Funcionalidades Avanzadas (Semanas 9-10)

### Semana 9: Reportes

**Objetivo Estratégico**: Dashboard de analytics y reportes

**Qué se construye exactamente**:
- [ ] Reporte de ventas diarias
- [ ] Reporte de ventas por período
- [ ] Productos más vendidos
- [ ] Ganancias (ingresos - costos)
- [ ] Inventario actual (stock)
- [ ] Productos bajo stock mínimo
- [ ] Exportar a PDF
- [ ] Exportar a Excel/CSV

**Qué NO se construye aún**:
- Reportes avanzados (dashboards interactivos)

**Dependencias Técnicas**:
- Fase 4 completada

**Riesgos**:
- Cálculos de ganancias requieren costo del producto

**Validaciones Obligatorias**:
- [ ] Reportes muestran datos correctos
- [ ] Filtros funcionan correctamente
- [ ] Exportación genera archivos válidos

---

### Semana 10: Funcionalidades Extras y polish

**Objetivo Estratégico**: Mejorar UX y agregar features adicionales

**Qué se construye exactamente**:
- [ ] Módulo de clientes (registro, historial)
- [ ] Cuenta corriente / ventas a crédito
- [ ] Notificaciones de stock bajo
- [ ] Backup manual de datos
- [ ] Importar productos desde Excel
- [ ] Tema oscuro (dark mode)
- [ ] Animaciones y transiciones
- [ ] Optimización de rendimiento

**Dependencias Técnicas**:
- Todas las fases anteriores

**Riesgos**:
-scope creep (expandir demasiado el alcance)

**Validaciones Obligatorias**:
- [ ] Todas las features funcionan offline
- [ ] UI es consistente y profesional
- [ ] App pasa pruebas de rendimiento

---

## Fase 6: Lanzamiento (Semanas 11-12)

### Semana 11: Testing y QA

**Qué se construye exactamente**:
- [ ] Pruebas unitarias (unit tests)
- [ ] Pruebas de integración
- [ ] QA manual completo
- [ ] Bug fixing
- [ ] Testing en dispositivos reales

### Semana 12: Despliegue y Lanzamiento

**Qué se construye exactamente**:
- [ ] Build de producción (Android APK/AAB)
- [ ] Build de producción (iOS)
- [ ] Configurar Firebase (Crashlytics, Analytics)
- [ ] Documentación de usuario
- [ ] Play Store listing
- [ ] App Store listing (si aplica)

---

# 🎨 2. UI/UX — DISEÑO CON INTENCIÓN

## 2.1 Design System Completo

### Paleta de Colores

```dart
// Colores principales
const Color primaryColor = Color(0xFF1E88E5);      // Azul profesional
const Color primaryDark = Color(0xFF1565C0);      // Azul oscuro
const Color primaryLight = Color(0xFF42A5F5);     // Azul claro

// Colores funcionales
const Color successColor = Color(0xFF4CAF50);      // Verde éxito
const Color warningColor = Color(0xFFFF9800);      // Naranja advertencia
const Color errorColor = Color(0xFFF44336);        // Rojo error
const Color infoColor = Color(0xFF2196F3);        // Azul info

// Colores neutros
const Color backgroundColor = Color(0xFFF5F5F5);  // Fondo general
const Color surfaceColor = Color(0xFFFFFFFF);       // Fondo tarjetas
const Color textPrimary = Color(0xFF212121);       // Texto principal
const Color textSecondary = Color(0xFF757575);    // Texto secundario
const Color dividerColor = Color(0xFFBDBDBD);      // Divisores

// Colores específicos para Venezuela
const Color bsColor = Color(0xFF9C27B0);          // Color para Bolivares
const Color usdColor = Color(0xFF4CAF50);          // Color para Dólares
```

### Tipografías

```dart
// Familia principal: Roboto (default en Material)
const String fontFamily = 'Roboto';

// Tamaños
const double fontSizeH1 = 32.0;    // Títulos principales
const double fontSizeH2 = 24.0;    // Títulos de sección
const double fontSizeH3 = 20.0;    // Subtítulos
const double fontSizeBody = 16.0;  // Texto normal
const double fontSizeCaption = 14.0; // Texto secundario
const double fontSizeSmall = 12.0;  // Labels, hints

// Pesos
const FontWeight fontWeightBold = FontWeight.w700;
const FontWeight fontWeightSemiBold = FontWeight.w600;
const FontWeight fontWeightMedium = FontWeight.w500;
const FontWeight fontWeightRegular = FontWeight.w400;
```

### Jerarquías Visuales

```
┌─────────────────────────────────────────┐
│ H1 - Título de pantalla                 │ 32sp Bold
│ color: textPrimary                      │
├─────────────────────────────────────────┤
│ H2 - Títulos de sección                │ 24sp SemiBold
│ color: textPrimary                      │
├─────────────────────────────────────────┤
│ H3 - Subtítulos                        │ 20sp Medium
│ color: textPrimary                      │
├─────────────────────────────────────────┤
│ Body - Texto principal                  │ 16sp Regular
│ color: textPrimary                      │
├─────────────────────────────────────────┤
│ Caption - Texto secundario             │ 14sp Regular
│ color: textSecondary                    │
├─────────────────────────────────────────┤
│ Button - Texto en botones              │ 14sp Medium
│ color: white / primaryColor             │
└─────────────────────────────────────────┘
```

### Estados Visuales

```dart
// Botón primario - Estados
enum ButtonState { enabled, pressed, disabled, loading }

// Enabled: primaryColor, opacity 1.0
// Pressed: primaryDark, scale 0.98
// Disabled: grey[400], opacity 0.5
// Loading: CircularProgressIndicator en lugar de texto

// Input fields - Estados
enum InputState { default, focused, error, disabled }

// Default: border grey[300]
// Focused: border primaryColor, 2px
// Error: border errorColor, mensaje de error abajo
// Disabled: background grey[100], texto grey[500]

// Cards - Estados
enum CardState { normal, selected, loading }

// Normal: elevation 2, borderRadius 12
// Selected: elevation 4, border primaryColor
// Loading: Shimmer effect
```

### Micro-animaciones

| Elemento | Qué Animar | Cuándo | Duración | Sensación |
|---------|-------------|--------|----------|-----------|
| Botón | Scale (0.98) | onTap down | 100ms | Táctil/responsivo |
| Card producto | Elevación | onTap | 150ms | Feedback de selección |
| Carrito | Badge bounce | Al agregar item | 300ms | Satisfacción |
| Navegación | Slide | Cambio de pantalla | 250ms | Fluidez |
| Toast/Snack | Fade + Slide up | Mostrar error/éxito | 200ms | No intrusivo |
| Skeleton | Shimmer | Loading datos | 1500ms | Progreso visual |
| Scanner | Línea escaneo | Escaneando | 2000ms loop | Actividad |

---

## 2.2 Pantallas y Vistas

### Pantalla 1: Login / Pantalla de Inicio

**Nombre**: LoginScreen

**Objetivo Funcional**: Autenticar usuario mediante PIN

**Objetivo Psicológico**: Sentirse seguro, acceso rápido

**Componentes UI Clave**:
- Logo de la app (centro superior)
- Campo de PIN (4 dígitos)
- Teclado numérico
- Botón de cambio de usuario (si hay múltiples)
- Indicador de versión

**Estados Posibles**:
- [ ] Default: Teclado listo, cursor en PIN
- [ ] Loading: Verificando PIN
- [ ] Error: PIN incorrecto (vibración + mensaje)
- [ ] Success: Transición a siguiente pantalla

**Acciones del Usuario**:
- Ingresar dígitos del PIN
- Presionar borrar
- Presionar enter (o auto-submit al completar 4 dígitos)

**Navegación**:
- Login exitoso → DashboardScreen

**Validaciones**:
- PIN debe tener exactamente 4 dígitos
- Máximo 3 intentos antes de bloquear

---

### Pantalla 2: Dashboard / Menú Principal

**Nombre**: DashboardScreen

**Objetivo Funcional**: Acceso rápido a todas las funciones principales

**Objetivo Psicológico**: Control, claridad, eficiencia

**Componentes UI Clave**:
- Header con nombre de tienda y usuario
- Botones de acción principales en grid 2x2:
  - 💰 Nueva Venta (Caja)
  - 📦 Inventario
  - 📊 Reportes
  - ⚙️ Configuración
- Indicador de conexión (online/offline)
- Símbolo de sync si hay datos pendientes
- Acceso rápido a últimas ventas

**Estados Posibles**:
- [ ] Default: Menú principal
- [ ] Loading: Cargando datos
- [ ] Offline: Banner indicando modo offline

**Navegación**:
- Nueva Venta → POSScreen
- Inventario → InventoryScreen
- Reportes → ReportsScreen
- Configuración → SettingsScreen

---

### Pantalla 3: POS / Caja (MAIN)

**Nombre**: POSScreen

**Objetivo Funcional**: Realizar ventas rápidamente escaneando productos

**Objetivo Psicológico**: Velocidad, facilidad, satisfacción al vender

**Componentes UI Clave**:
- **Header**: Total actual (prominente), botón scan, botón búsqueda
- **Lista de items**: Carrito con productos agregados
  - Imagen thumbnail (si existe)
  - Nombre producto
  - Cantidad (+/-)
  - Precio unitario
  - Subtotal
  - Botón eliminar (swipe o X)
- **Keyboard numérico**: Para cantidad rápida
- **Footer**: 
  - Subtotal
  - Descuento (botón para aplicar)
  - Total (grande, destacado)
  - Botón "Cobrar" (prominente)
- **FAB**: Botón flotante para escanear

**Estados Posibles**:
- [ ] Empty: "Escanea un producto para comenzar"
- [ ] Con items: Carrito con productos
- [ ] Loading: Procesando
- [ ] Error: Mensaje de error

**Acciones del Usuario**:
- Escanear código de barras (cámara)
- Buscar producto por nombre
- Seleccionar de categorías
- Modificar cantidad
- Aplicar descuento
- Eliminar producto
- Finalizar venta

**Navegación**:
- Scan → ScannerScreen
- Buscar → SearchScreen
- Finalizar venta → PaymentScreen

**Flujo de Venta**:
```
1. Escanear producto
2. Producto se agrega al carrito
3. Repetir 1-2 hasta completar
4. Opcional: aplicar descuento
5. Presionar "Cobrar"
6. Seleccionar método de pago
7. Confirmar venta
8. (Opcional) Imprimir recibo
9. Carrito se limpia
10. Listo para siguiente venta
```

---

### Pantalla 4: Inventario

**Nombre**: InventoryScreen

**Objetivo Funcional**: Gestionar productos, stock y categorías

**Objetivo Psicológico**: Control, organización

**Componentes UI Clave**:
- Search bar (buscar por nombre, SKU, código)
- Filtros: Categoría, Stock bajo, Todos
- Listado de productos en cards:
  - Imagen
  - Nombre
  - SKU / Código
  - Precio
  - Stock actual (con indicador si bajo)
  - Categoría
- FAB para agregar nuevo producto
- Pull to refresh (si online)

**Estados Posibles**:
- [ ] Default: Lista de productos
- [ ] Empty: "No hay productos. Agrega el primero"
- [ ] Loading: Skeleton loading
- [ ] Error: Mensaje de error
- [ ] Stock bajo: Badge de advertencia

**Acciones del Usuario**:
- Buscar producto
- Filtrar por categoría
- Ver detalle de producto
- Agregar nuevo producto
- Editar producto
- Eliminar producto

**Navegación**:
- Agregar/Editar → ProductFormScreen
- Detail → ProductDetailScreen
- Scan código → ScannerScreen (para agregar)

---

### Pantalla 5: Agregar/Editar Producto

**Nombre**: ProductFormScreen

**Objetivo Funcional**: Crear o modificar producto

**Componentes UI Clave**:
- Campo: Nombre (requerido)
- Campo: Precio de venta (requerido)
- Campo: Precio de costo (opcional, para ganancias)
- Campo: Stock actual
- Campo: Stock mínimo (para alertas)
- Campo: Código de barras (opcional, escanear)
- Campo: SKU (auto-generado, editable)
- Selector: Categoría
- Campo: Descripción (opcional)
- Imagen del producto (opcional)
- Switch: Producto activo/inactivo
- Botón: Guardar

**Estados Posibles**:
- [ ] Default: Formulario vacío o con datos
- [ ] Loading: Guardando
- [ ] Success: Redirección
- [ ] Error: Validación mostrada

**Validaciones**:
- Nombre: requerido, max 100 caracteres
- Precio: requerido, mayor a 0
- Stock: numérico, mayor o igual a 0
- SKU: único, auto-generado si está vacío

---

### Pantalla 6: Escáner

**Nombre**: ScannerScreen

**Objetivo Funcional**: Escanear códigos de barras

**Componentes UI Clave**:
- Cámara en tiempo real
- Overlay con guía de escaneo (cuadro central)
- Indicador de linterna (flash)
- Indicador de cámara (front/back)
- Feedback visual al detectar código
- Botón cerrar

**Estados Posibles**:
- [ ] Escaneando: Guía activa
- [ ] Detectado: Sonido + vibrate + highlight
- [ ] No encontrado: Mensaje "Producto no registrado"
- [ ] Error cámara: Mensaje de error

**Flujo desde PANTALLA DE VENTA**:
```
1. Cámara activa
2. Usuario apunta a código
3. Detecta código
4. Busca en base de datos local
5. Si existe: retorna producto → agregar al carrito
6. Si no existe: muestra mensaje "Producto no registrado"
   (NO permite crear producto desde aquí)
```

**Flujo desde PANTALLA DE INVENTARIO (agregar nuevo)**:
```
1. Cámara activa
2. Usuario apunta a código
3. Detecta código
4. Permite crear nuevo producto con ese código prellenado
```

---

### Pantalla 7: Cobro / Pago

**Nombre**: PaymentScreen

**Objetivo Funcional**: Completar la transacción

**Componentes UI Clave**:
- Total a pagar (prominente)
- Método de pago:
  - Efectivo (campo para monto recibido, calcula cambio)
  - Transferencia (registro de referencia)
  - Pago móvil (registro de teléfono + referencia)
  - Mixto (combinación de anteriores)
- Resumen de compra
- Botón Confirmar
- Botón Cancelar

**Estados Posibles**:
- [ ] Default: Métodos de pago
- [ ] Efectivo seleccionado: Input monto recibido
- [ ] Procesando: Loading
- [ ] Success: Venta completada
- [ ] Error: Mensaje

---

### Pantalla 8: Reportes

**Nombre**: ReportsScreen

**Objetivo Funcional**: Visualizar analytics del negocio

**Componentes UI Clave**:
- Selector de período (hoy, semana, mes, custom)
- Cards resumen:
  - Total ventas
  - Cantidad de transacciones
  - Ticket promedio
  - Productos vendidos
- Gráfico de ventas (barras/líneas)
- Top productos vendidos (lista)
- Lista de transacciones recientes
- Botón exportar (PDF/Excel)

**Estados Posibles**:
- [ ] Default: Reporte del día
- [ ] Loading: Datos cargando
- [ ] Empty: Sin datos para el período
- [ ] Error: Error al cargar

---

### Pantalla 9: Configuración

**Nombre**: SettingsScreen

**Objetivo Funcional**: Ajustes de la aplicación

**Componentes UI Clave**:
- Sección: Tienda
  - Nombre de tienda
  - Dirección
  - Teléfono
- Sección: Impresora
  - Conectar/desconectar
  - Configurar/imprimir prueba
- Sección: Usuarios
  - Listado de usuarios
  - Agregar usuario
- Sección: Datos
  - Sync ahora
  - Último sync
  - Exportar datos
  - Importar datos
- Sección: App
  - Tema (claro/oscuro)
  - Moneda predeterminada
  - Acerca de
- Sección: Seguridad
  - Cambiar PIN
  - Configurar PIN

---

## 2.3 Flujos de Navegación

```
App Start
    ↓
LoginScreen (PIN)
    ↓ (success)
DashboardScreen
    ├── POSScreen (Nueva Venta)
    │   ├── ScannerScreen
    │   ├── SearchScreen
    │   └── PaymentScreen
    │       └── (recibo) → Printer
    │
    ├── InventoryScreen
    │   ├── ProductFormScreen (agregar)
    │   ├── ProductDetailScreen
    │   └── ProductFormScreen (editar)
    │
    ├── ReportsScreen
    │   └── ExportScreen
    │
    └── SettingsScreen
        ├── PrinterScreen
        ├── UsersScreen
        │   └── UserFormScreen
        └── DataScreen
```

---

# 👤 3. HISTORIAS DE USUARIO

## MVP (Core)

### Como administrador de bodega
Quiero registrar productos con código de barras
Para tener mi inventario digitalizado y poder vender rápido

**Criterios**:
- Puedo agregar nombre, precio, stock
- Puedo escanear código de barras existente
- Sistema genera SKU automático si no tiene código
- Puedo asignar categoría

**Pantallas**: ProductFormScreen, InventoryScreen

---

Quiero vender productos escaneando códigos
Para hacer más rápida la atención al cliente

**Criterios**:
- Escanear código agrega producto al carrito
- Puedo buscar producto por nombre si no tiene código
- Carrito muestra productos con cantidades y totales
- Puedo modificar cantidades o eliminar items

**Pantallas**: POSScreen, ScannerScreen

---

Quiero cobrar y registrar ventas
Para mantener control del dinero que entra

**Criterios**:
- Total se calcula automáticamente
- Puedo aplicar descuento (%)
- acepto múltiples métodos de pago
- Venta se guarda con quién vendió y cuándo
- Stock se descuenta automáticamente

**Pantallas**: PaymentScreen

---

Quiero recibir alertas cuando stock está bajo
Para reponer productos a tiempo

**Criterios**:
- Defino stock mínimo por producto
- App muestra indicador visual cuando stock < mínimo
- Puedo ver lista de productos bajo stock

**Pantallas**: InventoryScreen (filtro), DashboardScreen

---

Como empleado de bodega
Quiero acceder con mi PIN
Para que el dueño controle quién vende

**Criterios**:
- Tengo mi propio PIN de 4 dígitos
- Solo puedo realizar ventas (no eliminar productos)
- Todas mis ventas quedan registradas con mi usuario

**Pantallas**: LoginScreen

---

## Funcionalidades Avanzadas

### Como administrador
Quiero imprimir recibos térmica
Para dar comprobante al cliente

**Criterios**:
- Conecto impresoras Bluetooth desde la app
- Recibo incluye: productos, cantidades, precios, total, método pago, tienda
- Puedo reimprimir última venta

**Pantallas**: SettingsScreen → PrinterScreen

---

Quiero ver reportes de ventas
Para saber cómo está mi negocio

**Criterios**:
- Ventas del día, semana, mes
- Productos más vendidos
- Ganancias (requiere precio costo)
- Puedo exportar a PDF

**Pantallas**: ReportsScreen

---

Quiero gestionar múltiples empleados
Para controlar mi equipo

**Criterios**:
- Creo usuarios con rol admin/employee
- Defino PIN para cada uno
- Veo historial de ventas por empleado
- Employee no puede eliminar productos

**Pantallas**: SettingsScreen → UsersScreen

---

Quiero sincronizar datos entre dispositivos
Para tener mi info actualizada en todos lados

**Criterios**:
- Datos se suben cuando hay internet
- Cambio de dispositivo baja últimos datos
- Cola de sync si estoy offline
- Indicador muestra estado de sync

---

Quiero registrar clientes
Para manejar cuenta corriente

**Criterios**:
- Registro nombre, teléfono
- Venta a crédito (queda debiendo)
- Registro de pagos
- Historial por cliente

---

## Monetización (Post-MVP)

### Como usuario free
Quiero probar la app gratis
Para decidir si me sirve

**Criterios**:
- Plan gratuito con límites
- Hasta 50 productos
- Hasta 100 ventas/mes
- Una usuario

---

### Como usuario premium
Quiero功能 completas sin límites
Para usar la app en mi negocio grande

**Criterios**:
- Suscripción mensual
- Productos ilimitados
- Usuarios ilimitados
- Múltiples tiendas
- Reportes avanzados
- Soporte prioritario

---

# ✅ 4. CRITERIOS DE ACEPTACIÓN

## Historia: Registro de Productos

```gherkin
Feature: Registro de Productos

  Scenario: Agregar producto con código de barras
    Given estoy en pantalla "Agregar Producto"
    When ingreso "Coca Cola 1L" en nombre
    And ingreso "5.00" en precio
    And escaneo código de barras "1234567890123"
    And selecciono categoría "Bebidas"
    And presiono "Guardar"
    Then el producto se guarda en base de datos local
    And veo mensaje "Producto guardado"
    And regreso a lista de productos

  Scenario: Agregar producto sin código de barras
    Given estoy en pantalla "Agregar Producto"
    When ingreso "Pan tajado" en nombre
    And ingreso "3.50" en precio
    And no ingreso código de barras
    Then veo SKU auto-generado "BOD-123456"
    When presiono "Guardar"
    Then el producto se guarda con SKU único
```

**Casos Inválidos**:
- Nombre vacío → Error: "El nombre es requerido"
- Precio = 0 → Error: "El precio debe ser mayor a 0"
- Precio negativo → Error: "El precio no puede ser negativo"
- SKU duplicado → Error: "Ya existe un producto con este SKU"
- Código de barras duplicado → Warning: "Ya existe producto con este código"

---

## Historia: Realizar Venta

```gherkin
Feature: Realizar Venta

  Scenario: Venta con un producto
    Given estoy en pantalla de POS
    When escaneo código "1234567890123"
    Then producto se agrega al carrito
    And cantidad es 1
    And total muestra "5.00"
    When presiono "Cobrar"
    Then veo pantalla de pago

  Scenario: Venta con descuento
    Given tengo productos en carrito con total "10.00"
    When presiono "Aplicar Descuento"
    And ingreso "20" en porcentaje
    Then total cambia a "8.00"
    When completo el pago
    Then stock de productos se descuenta
    And venta se registra

  Scenario: Venta con pago en efectivo
    Given total es "8.00"
    And selecciono método "Efectivo"
    When ingreso "10.00" en monto recibido
    Then cambio muestra "2.00"
    When presiono "Confirmar"
    Then venta se completa
    And mensaje "Venta exitosa" aparece
```

**Casos Inválidos**:
- Carrito vacío → Botón "Cobrar" deshabilitado
- Stock insuficiente → Error: "No hay suficiente stock"
- Monto < total → Error: "Monto insuficiente"

---

## Historia: Sincronización

```gherkin
Feature: Sincronización de Datos

  Scenario: Sync cuando hay internet
    Given tengo internet disponible
    And tengo productos pendientes de sync
    When sync se ejecuta automáticamente
    Then productos se suben a Supabase
    And indicador muestra "Sincronizado"

  Scenario: Sync manual
    Given estoy en pantalla de Configuración
    And presiono "Sincronizar ahora"
    Then veo indicador de progreso
    And datos se suben/bajan
    And mensaje "Sync completado" aparece

  Scenario: Intentar sync sin internet
    Given no tengo internet
    When presiono "Sincronizar"
    Then mensaje "Sin conexión. Datos guardados localmente" aparece
```

---

## Historia: Login con PIN

```gherkin
Feature: Autenticación con PIN

  Scenario: Login exitoso
    Given tengo PIN "1234" configurado
    When ingreso "1234" en pantalla de login
    Then acceso al dashboard
    And nombre de usuario se muestra

  Scenario: Login fallido
    Given tengo PIN "1234" configurado
    When ingreso "5678"
    Then mensaje "PIN incorrecto" aparece
    And campo se limpia

  Scenario: Bloqueo tras 3 intentos
    Given ya intenté 3 veces con PIN incorrecto
    When ingreso cualquier PIN
    Then acceso se bloquea por 5 minutos
    And mensaje "Demasiados intentos" aparece
```

---

# 🛠️ 5. BACKLOG TÉCNICO

> **Nota**: Cada tarea sigue la estructura de **Clean Architecture**:
> - **Core**: Configuración, constantes, errores, utilidades
> - **Data**: Repositorios, modelos, fuentes de datos (local/remote)
> - **Domain**: Entidades, casos de uso, interfaces
> - **Presentation**: UI, BLoC, widgets, páginas
> - **Backend**: Endpoints, servicios del lado del servidor

## Prioridad Alta (MVP)

### 1.1 Setup de Proyecto

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T001 | Inicializar Flutter project | P0 | - | Core |
| T002 | Configurar estructura Clean Architecture | P0 | T001 | Core |
| | → Crear carpetas: core/, data/, domain/, presentation/ | | | |
| | → Crear subcarpetas: data/local/, data/remote/, data/repositories/ | | | |
| | → Crear subcarpetas: domain/entities/, domain/usecases/, domain/repositories/ | | | |
| | → Crear subcarpetas: presentation/bloc/, presentation/pages/, presentation/widgets/ | | | |
| T003 | Configurar dependencias (pubspec.yaml) | P0 | T001 | Core |
| T004 | Setup Hive (inicialización, adapters) | P0 | T002 | Data/Local |
| | → lib/data/local/hive_adapters.dart | | | |
| | → lib/data/local/hive_boxes.dart | | | |
| T005 | Crear modelos de datos | P0 | T004 | Data |
| | → lib/data/models/product_model.dart | | | |
| | → lib/data/models/category_model.dart | | | |
| | → lib/data/models/sale_model.dart | | | |
| | → lib/data/models/user_model.dart | | | |
| T006 | Configurar Supabase Client | P0 | T001 | Data/Remote |
| | → lib/data/remote/supabase_client.dart | | | |
| T007 | Implementar Repository Pattern | P0 | T004, T005 | Data |
| | → lib/data/repositories/product_repository_impl.dart | | | |
| | → lib/data/repositories/sale_repository_impl.dart | | | |
| | → lib/domain/repositories/product_repository.dart (interface) | | | |
| | → lib/domain/repositories/sale_repository.dart (interface) | | | |
| T008 | Sistema de logging | P1 | T001 | Core |
| | → lib/core/utils/logger.dart | | | |
| | → lib/core/errors/exceptions.dart | | | |

### 1.2 Autenticación Local

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T010 | Modelo User y Auth service | P0 | T005 | Domain/Data |
| | → lib/domain/entities/user_entity.dart | | | |
| | → lib/data/repositories/auth_repository_impl.dart | | | |
| | → lib/domain/repositories/auth_repository.dart (interface) | | | |
| T011 | Login screen UI | P0 | T010 | Presentation |
| | → lib/presentation/pages/login_page.dart | | | |
| | → lib/presentation/widgets/pin_keyboard.dart | | | |
| T012 | Teclado numérico PIN | P0 | T011 | Presentation |
| | → lib/presentation/widgets/pin_digit.dart | | | |
| T013 | Validación PIN (hash, intentos) | P0 | T010 | Domain |
| | → lib/domain/usecases/validate_pin_usecase.dart | | | |
| T014 | Dashboard screen | P0 | T013 | Presentation |
| | → lib/presentation/pages/dashboard_page.dart | | | |

### 1.3 Inventario

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T020 | Modelo Product, Category | P0 | T005 | Domain |
| | → lib/domain/entities/product_entity.dart | | | |
| | → lib/domain/entities/category_entity.dart | | | |
| T021 | Repository productos | P0 | T020 | Data |
| | → lib/data/repositories/product_repository_impl.dart | | | |
| T022 | Pantalla listado productos | P0 | T021 | Presentation |
| | → lib/presentation/pages/inventory_page.dart | | | |
| | → lib/presentation/widgets/product_card.dart | | | |
| T023 | Buscador y filtros | P1 | T022 | Presentation |
| | → lib/presentation/widgets/search_bar.dart | | | |
| | → lib/presentation/widgets/filter_chip.dart | | | |
| T024 | Pantalla agregar producto | P0 | T021 | Presentation |
| | → lib/presentation/pages/product_form_page.dart | | | |
| T025 | Generador SKU automático | P0 | T024 | Domain |
| | → lib/domain/usecases/generate_sku_usecase.dart | | | |
| T026 | Validaciones producto | P0 | T024 | Domain |
| | → lib/domain/usecases/validate_product_usecase.dart | | | |
| T027 | Pantalla editar producto | P0 | T021 | Presentation |
| | → lib/presentation/pages/product_form_page.dart (reusable) | | | |
| T028 | Eliminar producto | P0 | T021 | Domain |
| | → lib/domain/usecases/delete_product_usecase.dart | | | |
| T029 | Alertas stock mínimo | P1 | T020 | Domain |
| | → lib/domain/usecases/check_low_stock_usecase.dart | | | |

### 1.4 POS / Ventas

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T030 | Modelo Cart, Sale, SaleItem | P0 | T005 | Domain |
| | → lib/domain/entities/cart_entity.dart | | | |
| | → lib/domain/entities/sale_entity.dart | | | |
| | → lib/domain/entities/sale_item_entity.dart | | | |
| T031 | Repository ventas | P0 | T030 | Data |
| | → lib/data/repositories/sale_repository_impl.dart | | | |
| T032 | Pantalla POS principal | P0 | T031 | Presentation |
| | → lib/presentation/pages/pos_page.dart | | | |
| T033 | Carrito de compras | P0 | T032 | Presentation |
| | → lib/presentation/widgets/cart_item.dart | | | |
| | → lib/presentation/widgets/cart_summary.dart | | | |
| T034 | Cálculos (subtotal, descuento, total) | P0 | T033 | Domain |
| | → lib/domain/usecases/calculate_totals_usecase.dart | | | |
| T035 | Descuentos (%) | P0 | T034 | Presentation |
| | → lib/presentation/widgets/discount_dialog.dart | | | |
| T036 | Modelo payment | P0 | T030 | Domain |
| | → lib/domain/entities/payment_entity.dart | | | |
| T037 | Pantalla de pago | P0 | T036 | Presentation |
| | → lib/presentation/pages/payment_page.dart | | | |
| T038 | Finalizar venta | P0 | T037 | Domain |
| | → lib/domain/usecases/complete_sale_usecase.dart | | | |
| T039 | Descuenta stock al vender | P0 | T038 | Domain |
| | → lib/domain/usecases/decrement_stock_usecase.dart | | | |

### 1.5 Escaneo de Códigos

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T040 | Integrar mobile_scanner | P0 | T003 | Core |
| | → pubspec.yaml: mobile_scanner | | | |
| | → lib/core/config/scanner_config.dart | | | |
| T041 | Pantalla escáner | P0 | T040 | Presentation |
| | → lib/presentation/pages/scanner_page.dart | | | |
| | → lib/presentation/widgets/scanner_overlay.dart | | | |
| T042 | Buscar producto por código | P0 | T021 | Domain |
| | → lib/domain/usecases/find_product_by_barcode_usecase.dart | | | |
| T043 | Agregar escaneado al carrito | P0 | T042 | Presentation |
| | → lib/presentation/bloc/cart/cart_bloc.dart | | | |
| T044 | Manejo producto no encontrado (solo mensaje) | P1 | T042 | Presentation |
| | → lib/presentation/widgets/error_product_not_found.dart | | | |
| T045 | Flashlight y cámara | P1 | T040 | Presentation |
| | → lib/presentation/widgets/camera_controls.dart | | | |

### 1.6 Sincronización

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T050 | Schema Supabase | P0 | T006 | Data/Remote |
| | → lib/data/remote/supabase_tables.dart | | | |
| T051 | RLS Policies | P0 | T050 | Data/Remote |
| T052 | Sync Queue (cola) | P0 | T005 | Data/Local |
| | → lib/data/local/sync_queue_box.dart | | | |
| | → lib/data/models/sync_operation_model.dart | | | |
| T053 | Sync service | P0 | T052 | Data |
| | → lib/data/repositories/sync_repository_impl.dart | | | |
| | → lib/domain/repositories/sync_repository.dart | | | |
| T054 | Detección conectividad | P0 | T003 | Core |
| | → lib/core/utils/connectivity_service.dart | | | |
| T055 | Sync automático | P1 | T053, T054 | Data |
| | → lib/data/repositories/auto_sync_service.dart | | | |
| T056 | Sync manual UI | P1 | T055 | Presentation |
| | → lib/presentation/widgets/sync_indicator.dart | | | |

## Prioridad Media (Post-MVP)

### 2.1 Impresión

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T060 | Integrar printer package | P1 | T003 | Core |
| | → pubspec: esc_pos_bluetooth / flutter_thermal_printer_pos | | | |
| T061 | Pantalla config impresora | P1 | T060 | Presentation |
| | → lib/presentation/pages/printer_settings_page.dart | | | |
| T062 | Template receipt ESC/POS | P1 | T061 | Domain |
| | → lib/domain/usecases/generate_receipt_usecase.dart | | | |
| T063 | Impresión post-venta | P1 | T062 | Presentation |
| T064 | Reimpresión | P2 | T063 | Presentation |
| | → lib/presentation/pages/reprint_page.dart | | | |

### 2.2 Usuarios

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T070 | CRUD usuarios | P1 | T010 | Domain |
| | → lib/domain/usecases/create_user_usecase.dart | | | |
| | → lib/domain/usecases/update_user_usecase.dart | | | |
| T071 | Roles (admin/employee) | P1 | T070 | Domain |
| | → lib/domain/entities/role_enum.dart | | | |
| T072 | Restricciones por rol | P1 | T071 | Domain |
| | → lib/domain/usecases/check_permission_usecase.dart | | | |
| T073 | Historial ventas por usuario | P2 | T031 | Data |
| | → lib/data/repositories/sales_by_user_repository.dart | | | |

### 2.3 Reportes

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T080 | Reporte ventas diarias | P1 | T031 | Domain |
| | → lib/domain/usecases/get_daily_sales_usecase.dart | | | |
| T081 | Reporte por período | P1 | T080 | Domain |
| | → lib/domain/usecases/get_sales_by_period_usecase.dart | | | |
| T082 | Productos más vendidos | P1 | T031 | Domain |
| | → lib/domain/usecases/get_top_products_usecase.dart | | | |
| T083 | Ganancias | P1 | T082 | Domain |
| | → lib/domain/usecases/calculate_profits_usecase.dart | | | |
| T084 | Export PDF | P2 | T080 | Presentation |
| | → lib/presentation/pages/reports_page.dart | | | |
| T085 | Export Excel/CSV | P2 | T080 | Presentation |

### 2.4 Serverpod

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T090 | Setup Serverpod | P1 | T003 | Backend |
| | → serverpod/ (carpeta del servidor) | | | |
| | → serverpod/lib/serverpod.dart | | | |
| T091 | Endpoints productos | P1 | T090 | Backend |
| | → serverpod/lib/src/generated/endpoints/ | | | |
| T092 | Endpoints ventas | P1 | T090 | Backend |
| T093 | Integración Supabase | P1 | T091 | Backend |
| | → serverpod/lib/src/integrations/supabase.dart | | | |

## Prioridad Baja (Extras)

| ID | Tarea | Prioridad | Dependencias | Módulo/Capa |
|----|-------|-----------|---------------|--------------|
| T100 | Módulo clientes | P2 | T031 | Domain |
| | → lib/domain/entities/client_entity.dart | | | |
| T101 | Cuenta corriente | P2 | T100 | Domain |
| | → lib/domain/usecases/manage_client_balance_usecase.dart | | | |
| T102 | Importar Excel | P2 | T020 | Data |
| | → lib/data/repositories/import_products_usecase.dart | | | |
| T103 | Dark mode | P2 | T001 | Presentation |
| | → lib/core/theme/app_theme.dart | | | |
| T104 | Notificaciones push | P3 | T090 | Core |
| T105 | Multi-tienda | P3 | T090 | Domain |

---

# 🧪 6. DEFINICIÓN DE "READY TO CODE"

## UI/UX Ready ✓

- [ ] **Design System documentado**: Colores, tipografías, componentes base definidos
- [ ] **Wireframes aprobados**: Todas las pantallas dibujadas y revisadas
- [ ] **Flujos de navegación definidos**: Mapa de navegación completo
- [ ] **Estados UI especificados**: Loading, empty, error, success para cada pantalla
- [ ] **Animaciones decididas**: Cuándo, qué, cuánto dura
- [ ] **Responsive considerado**: Adaptación a diferentes tamaños de pantalla

## Technical Ready ✓

- [ ] **Arquitectura definida**: Clean Architecture con capas separadas
- [ ] **Stack seleccionado**: Flutter, Hive, BLoC, Supabase, Serverpod
- [ ] **Estructura de archivos definida**:
  ```
  lib/
  ├── core/           # Config, constantes, utils, errores
  ├── data/           # Repositorios, modelos, fuentes de datos
  │   ├── local/      # Hive DAOs
  │   ├── remote/     # Supabase/Serverpod clients
  │   └── repositories/
  ├── domain/         # Entidades, casos de uso, interfaces
  └── presentation/  # UI, BLoC, widgets, páginas
  ```
- [ ] **Modelos de datos finalizados**: Product, Sale, User, Category, etc.
- [ ] **API contracts definidos**: Endpoints de Serverpod documentados
- [ ] **Schema SQL completo**: Tablas, índices, foreign keys, RLS
- [ ] **Paquetes evaluados**: Todos los packages verificados y compatibles
- [ ] **Manejo de errores definido**: Cómo mostrar y loguear errores

## Business Ready ✓

- [ ] **Historias de usuario priorizadas**: MVP claramente definido
- [ ] **Casos edge identificados**: Qué hacer con cada caso límite
- [ ] **Reglas de negocio documentadas**: Descuentos, stock, roles, sync
- [ ] **Métricas definidas**: Cómo medir éxito de la app
- [ ] **Monetización clara**: Plan free vs premium

## Development Ready ✓

- [ ] **Entorno configurado**: Flutter SDK, IDE, emuladores
- [ ] **Repo inicializado**: Git con .gitignore apropiado
- [ ] **Dependencies lockeadas**: versiones exactas en pubspec.lock
- [ ] **Code style agreed**: Lint rules configurados
- [ ] **Testing strategy**: Qué testear, cuándo, cómo

---

## Decisiones que NO deben improvisarse

1. **Arquitectura de datos**: Offline-first con Hive como fuente primaria de verdad
2. **Sincronización**: Pattern Dual-DB (Data DB + Sync Queue)
3. **Conflictos**: Last-Write-Wins por defecto
4. **Autenticación**: PIN local + Supabase Auth
5. **Moneda**: Soporte para Bs (bolívares) con decimales

## Reglas que NO deben cambiarse sin impacto

1. **Estructura de archivos**: Clean Architecture debe mantenerse
2. **Estado**: BLoC como único gestor de estado
3. **Offline-first**: Nunca cambiar a online-first
4. **SKU generation**: Formato BOD-XXXXXX no cambiar

## Qué debe existir antes de escribir código

1. **Este documento completo** (este plan)
2. **Wireframes de MVP** (todas las pantallas)
3. **Schema de base de datos** (tablas SQL)
4. **Dependencias resueltas** (pubspec.yaml)
5. **Entorno de desarrollo** funcionando

---

# 📊 ANEXO: MODELO DE DATOS DETALLADO

## Schema Supabase (PostgreSQL)

```sql
-- Extensiones necesarias
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

-- Tabla: Tiendas
CREATE TABLE stores (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name TEXT NOT NULL,
    address TEXT,
    phone TEXT,
    owner_id UUID REFERENCES auth.users(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Usuarios de tienda
CREATE TABLE store_users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    email TEXT UNIQUE,
    pin_hash TEXT NOT NULL,
    role TEXT NOT NULL CHECK (role IN ('admin', 'employee')),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Categorías
CREATE TABLE categories (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Productos
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    sku TEXT NOT NULL,
    barcode TEXT,
    name TEXT NOT NULL,
    description TEXT,
    price DECIMAL(12, 2) NOT NULL,
    cost DECIMAL(12, 2),
    stock INTEGER DEFAULT 0,
    stock_min INTEGER DEFAULT 5,
    category_id UUID REFERENCES categories(id) ON DELETE SET NULL,
    image_url TEXT,
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(store_id, sku),
    UNIQUE(store_id, barcode)
);

-- Tabla: Ventas
CREATE TABLE sales (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    seller_id UUID NOT NULL REFERENCES store_users(id),
    subtotal DECIMAL(12, 2) NOT NULL,
    discount_percent DECIMAL(5, 2) DEFAULT 0,
    discount_amount DECIMAL(12, 2) DEFAULT 0,
    total DECIMAL(12, 2) NOT NULL,
    payment_method TEXT NOT NULL,
    payment_reference TEXT,
    amount_received DECIMAL(12, 2),
    change_given DECIMAL(12, 2),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Items de venta
CREATE TABLE sale_items (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    sale_id UUID NOT NULL REFERENCES sales(id) ON DELETE CASCADE,
    product_id UUID NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL,
    unit_price DECIMAL(12, 2) NOT NULL,
    subtotal DECIMAL(12, 2) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Clientes
CREATE TABLE clients (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    phone TEXT,
    email TEXT,
    balance DECIMAL(12, 2) DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Movimientos de cliente (pagos/deudas)
CREATE TABLE client_movements (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    client_id UUID NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
    sale_id UUID REFERENCES sales(id) ON DELETE SET NULL,
    type TEXT NOT NULL CHECK (type IN ('credit', 'payment')),
    amount DECIMAL(12, 2) NOT NULL,
    reference TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Tabla: Historial de sync (para auditoría)
CREATE TABLE sync_history (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    entity_type TEXT NOT NULL,
    entity_id UUID NOT NULL,
    operation TEXT NOT NULL,
    synced_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Índices para rendimiento
CREATE INDEX idx_products_store ON products(store_id);
CREATE INDEX idx_products_sku ON products(sku);
CREATE INDEX idx_products_barcode ON products(barcode);
CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_sales_store ON sales(store_id);
CREATE INDEX idx_sales_seller ON sales(seller_id);
CREATE INDEX idx_sales_created ON sales(created_at);
CREATE INDEX idx_sale_items_sale ON sale_items(sale_id);
CREATE INDEX idx_sale_items_product ON sale_items(product_id);
CREATE INDEX idx_clients_store ON clients(store_id);

-- Trigger para updated_at automático
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_stores_updated_at
    BEFORE UPDATE ON stores
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_store_users_updated_at
    BEFORE UPDATE ON store_users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_products_updated_at
    BEFORE UPDATE ON products
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_clients_updated_at
    BEFORE UPDATE ON clients
    FOR EACH ROW EXECUTE FUNCTION update_updated_at();
```

## Row Level Security (RLS)

```sql
-- Habilitar RLS
ALTER TABLE stores ENABLE ROW LEVEL SECURITY;
ALTER TABLE store_users ENABLE ROW LEVEL SECURITY;
ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
ALTER TABLE products ENABLE ROW LEVEL SECURITY;
ALTER TABLE sales ENABLE ROW LEVEL SECURITY;
ALTER TABLE sale_items ENABLE ROW LEVEL SECURITY;
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;
ALTER TABLE client_movements ENABLE ROW LEVEL SECURITY;

-- Política: Usuarios ven solo su tienda
CREATE POLICY "Users can only see their store"
    ON stores FOR ALL
    USING (owner_id = auth.uid());

CREATE POLICY "Store users can only see their store"
    ON store_users FOR ALL
    USING (
        store_id IN (
            SELECT id FROM stores WHERE owner_id = auth.uid()
        )
    );

CREATE POLICY "Products visible to store users"
    ON products FOR ALL
    USING (
        store_id IN (
            SELECT store_id FROM store_users 
            WHERE email = auth.jwt()->>'email'
        )
    );

-- Similar para otras tablas...
```

---

# 💰 ANEXO: COSTOS Y INFRAESTRUCTURA

---

## 1. SUPABASE CLOUD (Base de Datos y Auth)

| Plan | Precio | DB | Storage | MAU | Bandwidth | Notas |
|------|--------|-----|---------|-----|-----------|-------|
| **Free** | $0/mes | 500MB | 1GB | 50K | 2GB | Ideal para MVP inicio |
| **Pro** | $25/mes | 8GB | 100GB | 100K | 250GB | Recomendado para escala |
| **Team** | $599/mes | 50GB | 1TB | Unlimited | 2TB | Para empresas |

**Para el MVP**: Plan Free es suficiente inicialmente
- Hasta 50 productos por tienda
- Hasta 100 ventas/mes
- 1 tienda, 1-5 usuarios
- Sin realtime streaming

**Escalando**: Migrar a Pro ($25/mes) cuando:
- Más de 500MB de datos
- Más de 100 ventas/mes
- Necesites realtime

---

## 2. SERVERPOD - OPCIONES DE HOSTING

### Opción A: VPS Compartido (Desarrollo/Inicio)

| Proveedor | Precio | specs | Ubicación | Notas |
|-----------|--------|-------|-----------|-------|
| **DigitalOcean Droplet** | $4-6/mes | 1GB RAM, 1 CPU, 25GB SSD | NYC, SF, Amsterdam | Mejor docs |
| **Linode** | $5/mes | 1GB RAM, 1 CPU, 25GB SSD | Fremont, Atlanta | Buena docs |
| **Contabo** | €4-5/mes | 2GB RAM, 1 CPU, 50GB SSD | Frankfurt |Europa |
| **Hetzner** | €4-6/mes | 2GB RAM, 1 CPU, 20GB SSD | Frankfurt | Excelente precio |

### Opción B: VPS Profesional (Producción)

| Proveedor | Precio | specs | Ubicación | Notas |
|-----------|--------|-------|-----------|-------|
| **DigitalOcean** | $12-20/mes | 2-4GB RAM, 2 CPU | Miami (recomendado) | Baja latencia VE |
| **AWS Lightsail** | $10-20/mes | 2-4GB RAM | N. Virginia | AWS ecosystem |
| **Linode** | $10-20/mes | 2-4GB RAM, 2 CPU | Fremont | |
| **Vultr** | $10-15/mes | 2GB RAM, 1 CPU | Miami | |

### Opción C: Contenedores (Docker)

| Proveedor | Precio | specs | Notas |
|-----------|--------|-------|-------|
| **Railway** | $5-19/mes | 1-4GB RAM | Incluyen PostgreSQL |
| **Render** | $7-25/mon | 2GB RAM | Auto-deploy desde Git |
| **Fly.io** | $5-19/mes | 1-4GB RAM | Global, Docker |
| **Coolify** | Self-hosted | Tu VPS | Alternativa auto-alojada |

---

## 3. INFRAESTRUCTURA COMPLETA - 3 ESCENARIOS

### Escenario A: Solo Supabase Cloud (Más Simple)
```
┌─────────────┐      ┌─────────────┐
│   Flutter   │──────│ Supabase    │
│     App     │      │  (Cloud)    │
└─────────────┘      └─────────────┘
     $0/mes           $0-25/mes
```

| Servicio | Costo | Notas |
|----------|-------|-------|
| Supabase Free | $0 | Plan gratuito |
| **Total** | **$0/mes** | |

---

### Escenario B: Supabase Pro + Serverpod Básico
```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Flutter   │──────│ Serverpod  │──────│ Supabase    │
│     App     │      │  (VPS)     │      │   Pro       │
└─────────────┘      └─────────────┘      └─────────────┘
                           $10/mes           $25/mes
```

| Servicio | Costo | Notas |
|----------|-------|-------|
| Supabase Pro | $25/mes | DB + Auth + Realtime |
| Serverpod (VPS $10) | $10/mes | API Layer |
| Dominio | $5/mes | api.tudominio.com |
| SSL (Let's Encrypt) | $0 | Gratis |
| **Total** | **$40/mes** | |

---

### Escenario C: Infraestructura Propia Completa (Sin Supabase)
```
┌─────────────┐      ┌─────────────┐
│   Flutter   │──────│   Nginx     │
│     App     │      │  (Reverse   │
└─────────────┘      │   Proxy)    │
                     └──────┬──────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
        ┌─────▼─────┐ ┌────▼────┐ ┌───▼────┐
        │ Serverpod  │ │PostgreSQL│ │ Redis  │
        │   (API)   │ │   (DB)   │ │(Cache) │
        └───────────┘ └──────────┘ └────────┘
             $12/mes    $8/mes     $3/mes
```

| Servicio | Costo | Notas |
|----------|-------|-------|
| VPS Backend (Serverpod) | $12/mes | 2GB RAM, 2 CPU |
| VPS DB (PostgreSQL) | $8/mes | 1GB RAM (separada) |
| VPS Redis (cache) | $3/mes | 512MB (opcional) |
| Dominio | $5/mes | .com o .ve |
| SSL | $0 | Let's Encrypt |
| Backup storage | $2/mes | Backblaze B2 |
| **Total** | **$30/mes** | Más control, sin Supabase |

---

## 4. FORMAS DE PAGO EN VENEZUELA

| Método | Disponibilidad | Notas |
|--------|----------------|-------|
| **Zelle** | Muy común | Transferencia USD directa entre bancos |
| **PayPal** | Limitada | Requiere verificación, retiros difíciles |
| **Binance P2P** | Popular | USDT, cryptomonedass |
| **MercadoPago** | Creciendo | En Bs, luego convertir |
| **Banesco/Mercantil** | Universal | Transferencia directa USD |
| **Efectivo USD** | Presencial | Para planes locales |
| **Western Union** | Disponible | Envíos internacionales |

**Recomendación**: Usar **Zelle** como principal para clientes Venezuela, aceptar **USDT** como alternativa.

---

## 5. MODELOS DE COBRO A NEGOCIOS

### Modelo Freemium
| Feature | Límite |
|---------|--------|
| Precio | $0/mes |
| Productos | 50 |
| Ventas/mes | 100 |
| Usuarios | 1 |
| Tiendas | 1 |

### Modelo Básico
| Feature | Límite |
|---------|--------|
| Precio | $5-10/mes |
| Productos | Ilimitados |
| Ventas | Ilimitadas |
| Usuarios | 3 |
| Tiendas | 1 |

### Modelo Profesional
| Feature | Límite |
|---------|--------|
| Precio | $15-25/mes |
| Productos | Ilimitados |
| Ventas | Ilimitadas |
| Usuarios | Ilimitados |
| Tiendas | 3 |
| Reportes | Avanzados |
| Sync | Realtime |

### Modelo Empresarial
| Feature | Límite |
|---------|--------|
| Precio | $50+/mes |
| Todo ilimi | tado |
| Multi-tenant | Sí |
| API access | Sí |
| Soporte | Prioritario |
| On-premise | Opcional |

---

## 6. TIEMPO REAL DE DESARROLLO

### Estimación por Fase (1 desarrollador)

| Fase | Semanas | Horas/Semana | Horas Total |
|------|---------|--------------|------------|
| Setup + Arquitectura | 1-2 | 20-25 | 25-50 |
| Inventario (CRUD) | 1 | 20-25 | 20-25 |
| Ventas (POS) | 1-2 | 20-25 | 25-50 |
| Escaneo códigos | 1 | 20-25 | 20-25 |
| Impresión | 1 | 20-25 | 20-25 |
| Usuarios + Auth | 1 | 20-25 | 20-25 |
| Sincronización | 1-2 | 20-25 | 25-50 |
| Reportes | 1 | 20-25 | 20-25 |
| Testing + Polish | 1 | 20-25 | 20-25 |

### Total MVP: 8-10 semanas (160-250 horas)

### Con equipo de 2 desarrolladores:
- **Tiempo**: 6-8 semanas
- **Horas**: 160-200 horas

---

## 7. CONOCIMIENTOS TÉCNICOS PARA INFRAESTRUCTURA PROPIA

### 7.1 VPS (Servidor Privado Virtual)

**Qué es**: Máquina virtual en la nube que rentás mensualmente

**Conocimientos requeridos**:

| Tema | Nivel | Descripción |
|------|-------|-------------|
| SSH | Básico | Conectar al servidor por terminal |
| Linux (Ubuntu/CentOS) | Básico | Comandos básicos, navegación |
| Terminal | Básico | vim/nano, permisos, paquetes |
| Firewall | Básico | Configurar ufw, puertos |
| Nginx | Intermedio | Reverse proxy, SSL |
| Certbot | Básico | SSL gratuito Let's Encrypt |
| Backups | Intermedio | rsync, scripts automatizados |

**Comandos esenciales**:
```bash
# Conectar por SSH
ssh root@tu-servidor

# Actualizar sistema
sudo apt update && sudo apt upgrade

# Instalar Docker
curl -fsSL get.docker.com | sh

# Configurar firewall
sudo ufw allow 22/tcp   # SSH
sudo ufw allow 80/tcp   # HTTP
sudo ufw allow 443/tcp  # HTTPS

# Monitoreo
htop, top, df -h
```

---

### 7.2 Docker y Contenedores

**Qué es**: Empaquetar aplicaciones en contenedores portables

**Conocimientos requeridos**:

| Tema | Nivel | Descripción |
|------|-------|-------------|
| Imágenes | Básico | Buscar, descargar, eliminar |
| Contenedores | Básico | Crear, iniciar, detener, logs |
| Volumes | Intermedio | Persistencia de datos |
| Docker Compose | Intermedio | Orquestar múltiples servicios |
| Redes | Intermedio | Conectar contenedores |
| Dockerfile | Intermedio | Crear imágenes propias |

**Comandos esenciales**:
```bash
# Ver contenedores activos
docker ps

# Iniciar contenedor
docker start mi-contenedor

# Ver logs
docker logs -f mi-contenedor

# Entrar al contenedor
docker exec -it mi-contenedor bash

# Docker Compose
docker-compose up -d
docker-compose logs -f
docker-compose down
```

**Ejemplo docker-compose.yml para Serverpod**:
```yaml
version: '3.8'

services:
  serverpod:
    image: serverpod/serverpod:latest
    ports:
      - "8080:8080"
      - "8081:8081"
      - "8082:8082"
    environment:
      - DATABASE_HOST=postgres
      - REDIS_HOST=redis
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=tu_password_segura
    volumes:
      - pgdata:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine

volumes:
  pgdata:
```

---

### 7.3 PostgreSQL (Base de Datos)

**Conocimientos requeridos**:

| Tema | Nivel | Descripción |
|------|-------|-------------|
| Conexión | Básico | psql, cliente CLI |
| Tablas | Básico | CREATE, ALTER, DROP |
| Queries | Básico | SELECT, INSERT, UPDATE |
| Índices | Intermedio | Optimizar consultas |
| Backups | Intermedio | pg_dump, pg_restore |
| Usuario/Roles | Intermedio | Permisos, GRANT |
| Replication | Avanzado | Alta disponibilidad |

**Comandos esenciales**:
```bash
# Conectar a DB
psql -U usuario -d database

# Backup
pg_dump -U usuario database > backup.sql

# Restaurar
psql -U usuario database < backup.sql

# Ver tablas
\dt

# Ver tamaño de tablas
SELECT pg_size_pretty(pg_total_relation_size('products'));
```

---

### 7.4 Nginx (Reverse Proxy)

**Para qué**: Exponer Serverpod al exterior, SSL, balanceo

**Conocimientos requeridos**:

| Tema | Nivel | Descripción |
|------|-------|-------------|
| Configuración | Básico | /etc/nginx/nginx.conf |
| Server blocks | Básico | Virtual hosts |
| Proxy pass | Intermedio | Redireccionar a contenedor |
| SSL | Básico | Let's Encrypt + Certbot |
| Headers seguridad | Intermedio | CORS, X-Frame-Options |

**Ejemplo config**:
```nginx
server {
    listen 80;
    server_name api.tudominio.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

### 7.5 Mantenimiento Requerido

| Tarea | Frecuencia | Herramientas |
|-------|-----------|--------------|
| Actualizar sistema | Semanal/Mensual | apt update |
| Backups DB | Diario | cron + pg_dump |
| Backups archivos | Semanal | rsync, tar |
| Monitoreo recursos | Continuo | htop, netdata |
| Logs | Semanal | logrotate |
| SSL renewal | Cada 90 días | certbot auto-renewal |
| Seguridad | Mensual | fail2ban, ufw |

---

### 7.6 Costos Ocultos/Adicionales

| Concepto | Costo | Notas |
|----------|-------|-------|
| Dominio | $5-15/año | .com, .ve |
| Cert SSL | $0 | Let's Encrypt |
| Backup storage | $2-5/mes | S3, Backblaze |
| Monitoreo | $0 | Prometheus, Grafana |
| Dominios extra | $0-5/mes | Subdominios |

---

### 7.7 Escalabilidad

| Nivel | Config | Costo | Capacidad |
|-------|--------|-------|-----------|
| Inicio | 1 VPS (2GB) | $10/mes | 100-500 usuarios |
| Crecimiento | 1 VPS (4GB) + DB separada | $20-25/mes | 500-2000 usuarios |
| Escala | 2+ VPS + Load Balancer | $40-60/mes | 2000-10000 usuarios |
| Enterprise | K8s + Multi-region | $100+/mes | 10000+ usuarios |

---

### 7.8 Checklist Pre-Lanzamiento (Servidor)

- [ ] Servidor configurado con SSH key
- [ ] Docker + Docker Compose instalados
- [ ] PostgreSQL instalado y securizado
- [ ] Redis instalado (si aplica)
- [ ] Serverpod deployado
- [ ] Nginx configurado con SSL
- [ ] Certificados SSL auto-renewal
- [ ] Backups automatizados
- [ ] Monitoreo configurado
- [ ] Firewall configurado (puertos 22, 80, 443)
- [ ] Dominio apuntando
- [ ] Logs rotando

---

## 8. IMPRESORA TÉRMICA

| Modelo | Precio | Notas |
|--------|--------|-------|
| Generic ESC/POS | $30-50 | Chinatown/MercadoLibre |
| Epson TM-T88 | $80-120 | Más confiable |
| 58mm o 80mm | - | Ancho de papel |

---

## 9. RESUMEN MENSUAL (ESCENARIOS)

| Escenario | Costo/Mes | Complejidad | Recomendado para |
|-----------|-----------|-------------|------------------|
| A: Solo Supabase | $0-25 | Muy Baja | Inicio, validación |
| B: Supabase + Serverpod | $40 | Media | Escala media |
| C: Infraestructura propia | $30 | Alta | Control total |

**Para Venezuela**:
- VPS recomendado: **DigitalOcean Miami** (menor latencia)
- Dominio: **.com** o **.ve** (más económico)
- Payment: **Zelle** o **USDT**

---

# 📋 RESUMEN EJECUTIVO

| Métrica | Valor |
|---------|-------|
| **Duración total** | 12 semanas |
| **Semanas MVP** | 8-10 semanas |
| **Horas desarrollo MVP** | 160-250 horas |
| **Features MVP** | Inventario, Ventas, Escaneo, Sync básico |
| **Features Post-MVP** | Impresión, Reportes, Usuarios, Clientes |
| **Stack Frontend** | Flutter + Hive + BLoC |
| **Stack Backend** | Serverpod + Supabase |
| **Costo mensual (Inicio)** | $0-25/mes |
| **Costo mensual (Producción)** | $30-40/mes |
| **Usuarios target** | Negocios pequeños en Venezuela |

---

*Documento generado siguiendo metodología Tech Lead + Product Design*
*Versión: 2.0 - Actualizado con infraestructura detallada*
*Fecha: Marzo 2026*
