# Plan Actualizado - Plataforma twinsublime
## "Design Lab: Identidad a tu medida"

### ANÁLISIS DE SKETCHES Y ESPECIFICACIONES

#### Visual 1: Design Lab Layout
- **Box 1**: Visor web lateral izquierdo
- **Área de trabajo**: Canvas central responsive
- **Box 2**: Panel derecho con información
- **Banner blur**: Selector empresa/particular con efectos visuales

#### Visual 2-3: Catálogo y Selección
- **Grid de productos**: Lanyard, pulsera, cinta, llavero, sombrero, cojín
- **Panel información**: Detalles técnicos, envío, cantidad
- **Aspectos del diseño**: Patrón, imágenes adjuntas, variables (1/2 caras, medidas)

#### Visual 4: Editor 2D
- **Plantilla delimitadora**: Ejemplo lanyard QR 25mm con guías
- **Cara A/B**: Alternancia entre caras del producto
- **Información contextual**: Límites de plantillas, usuario 3D

#### Visual 5: Visor 3D
- **Panel izquierdo**: Diseño y acceso móvil/aplicaciones 2D
- **Centro**: Visor 3D con rotación 360°
- **Controles**: Botones rotar izq/der, cámara del producto
- **Funciones limitadas**: Información generada durante proceso

### IMPLEMENTACIÓN ACTUALIZADA

## FASE 1: CATÁLOGO REAL DE PRODUCTOS

### 1.1 Productos Base (Datos Reales)
```typescript
// Estructura de productos actualizada
interface Product {
  id: string;
  name: string;
  category: 'lanyard' | 'pulsera' | 'llavero' | 'cinta' | 'cojin' | 'parche' | 'placa';
  technique: 'sublimacion' | 'dtf_uv' | 'dtv' | 'dtf_uv_relieve';
  material: string;
  dimensions: {
    width: number;
    length?: number;
    thickness?: number;
  };
  printArea: {
    width: number;
    height: number;
  };
  faces: 1 | 2;
  minimumOrder: number;
  estimatedDays: number;
  basePrice: number;
  extras: ProductExtra[];
  template: string; // Plantilla descargable
  warnings: string[]; // Sangrado, colores, etc.
}
```

### 1.2 Catálogo Completo
- **Lanyard estándar 20mm**: Poliéster, sublimación integral, 90cm
  - Extras: Mosquetón metálico/seguridad, rompefuerzas, doble mosquetón, porta-tarjetas PVC
- **Lanyard premium 25mm**: Satinado, DTF UV + barniz selectivo
- **Pulsera festival 15mm**: Sublimación integral, cierre aluminio/plástico
  - Extras: Numeración, QR, cierre inviolable
- **Pulsera DTV 20mm**: DTV a color, cierre botón, tiradas cortas
- **Llavero con forma**: DTF UV sobre acrílico 3mm, corte perimetral (40/60mm)
  - Extras: Anilla premium, cadena corta
- **Llavero textil tag**: 130×30mm, sublimación doble cara, ribete opcional
- **Cojín recortable**: DTV/sublimación, funda + relleno (35/45cm)
  - Extras: Cremallera oculta, doble cara
- **Cinta sombrero/medalla**: 25mm sublimación, largo a medida
- **Parche textil**: DTV/DTF UV, backing termo-adhesivo/velcro
- **Placa acrílica**: DTF UV con relieve, 30-50mm, pin/imán

## FASE 2: DESIGN LAB - LAYOUT PRINCIPAL

### 2.1 Estructura Visual según Sketch 1
```
┌─────────────────────────────────────────────────────────┐
│ Header: twinsublime + navegación                        │
├─────────┬─────────────────────────────┬─────────────────┤
│ BOX 1   │     ÁREA DE TRABAJO         │     BOX 2       │
│ Visor   │     (Canvas/Catálogo)       │ Información     │
│ Web     │                             │ Producto        │
│         │                             │                 │
│ - Cat.  │                             │ - Detalles      │
│ - Filt. │                             │ - Precio        │
│ - Prev. │                             │ - Extras        │
└─────────┴─────────────────────────────┴─────────────────┘
│ Banner Blur: ¿Para quién vamos a diseñar?              │
│ [Empresa] [Particular] + datos personales/pedido       │
└─────────────────────────────────────────────────────────┘
```

### 2.2 Componentes del Layout
- **Header**: Logo twinsublime, navegación, carrito
- **Sidebar izquierdo (Box 1)**: Categorías, filtros, vista previa
- **Área central**: Canvas responsive para catálogo/editor/visor
- **Sidebar derecho (Box 2)**: Información producto, configuración
- **Banner inferior**: Selector empresa/particular con blur effect

## FASE 3: EDITOR 2D FUNCIONAL (Visual 4)

### 3.1 Canvas con Plantillas Reales
- **Plantilla delimitadora**: Guías específicas por producto
- **Ejemplo lanyard QR 25mm**: Área imprimible, márgenes de seguridad
- **Cara A/B**: Alternancia visual con preview
- **Safe area**: Límites de impresión claramente marcados

### 3.2 Herramientas de Edición (MVP Funcional)
```typescript
// Stack: Fabric.js/Konva para canvas
interface EditorTools {
  text: {
    add: () => void;
    edit: () => void;
    curve: boolean; // Texto curvo
    fonts: string[];
  };
  images: {
    upload: () => void;
    position: () => void;
    scale: () => void;
    logos: boolean;
  };
  colors: {
    picker: () => void;
    recolor: () => void;
    palette: string[];
  };
  layers: {
    order: () => void;
    visibility: () => void;
    lock: () => void;
  };
  export: {
    png: () => void; // Con sangrado
    json: () => void; // Proyecto
    validation: () => boolean; // DPI/bordes
  };
}
```

### 3.3 Validaciones y Export
- **DPI check**: Mínimo 300 DPI para imágenes
- **Sangrado**: Export con área de sangrado automática
- **Preview real-scale**: Vista a tamaño real
- **JSON proyecto**: Guardado completo del diseño

## FASE 4: VISOR 3D MOCK FUNCIONAL (Visual 5)

### 4.1 Layout Tripartito
```
┌─────────┬─────────────────────────────┬─────────────────┐
│ Diseño  │        VISOR 3D             │   Información   │
│ y       │                             │   del Pedido    │
│ Acceso  │    [Modelo 3D Mock]         │                 │
│ Móvil   │                             │ - Técnica       │
│         │  ← [360°] →                 │ - Material      │
│ 2D      │     ↕                       │ - Tiempo        │
│ Editor  │   Zoom                      │ - Precio        │
│         │                             │ - Extras        │
└─────────┴─────────────────────────────┴─────────────────┘
```

### 4.2 Mock 3D Convincente
- **Modelo base**: Geometría simple por producto
- **Orbit controls**: Rotación 360° con botones
- **Material swap**: Aplicación visual del diseño 2D
- **Animaciones**: Transiciones suaves
- **Hooks preparados**: Para Three.js/Babylon posterior

### 4.3 Controles de Navegación
- **Botones direccionales**: Rotar izquierda/derecha
- **Zoom**: Acercar/alejar con límites
- **Vista 360°**: Cámara orbital automática
- **Reset view**: Volver a posición inicial

## FASE 5: CHECKOUT FUNCIONAL BÁSICO

### 5.1 Flujo de Compra
1. **Configuración**: Cantidad, extras, envío
2. **Datos cliente**: Empresa/particular, facturación
3. **Carrito**: Resumen con precios calculados
4. **Checkout**: Stripe test + envío
5. **Confirmación**: Orden con mock producción

### 5.2 Generación de Archivos
- **ID pedido**: Archivo único generado
- **Preview adjunto**: Imágenes del diseño 3D
- **Datos técnicos**: Especificaciones para producción
- **Enlace externo**: Confirmación y seguimiento

## ESPECIFICACIONES TÉCNICAS ACTUALIZADAS

### Identidad Visual twinsublime
- **Colores**: Blanco/Negro base, grises, azul eléctrico, violeta profundo
- **Tipografías**: Orbitron (títulos), Roboto (texto)
- **Efectos**: Blur selectivo, transiciones suaves, microinteracciones

### Stack Tecnológico
- **Frontend**: Next.js 15+ TypeScript (existente)
- **Styling**: Tailwind CSS + shadcn/ui (existente)
- **Canvas 2D**: Fabric.js o Konva.js
- **3D Mock**: CSS 3D transforms + animaciones
- **Forms**: React Hook Form (existente)
- **Payments**: Stripe test mode

### Estructura de Archivos Actualizada
```
src/
├── app/
│   ├── page.tsx (Design Lab principal)
│   ├── editor/page.tsx (Editor 2D)
│   ├── preview/page.tsx (Visor 3D)
│   └── checkout/page.tsx (Compra)
├── components/
│   ├── layout/
│   │   ├── DesignLabLayout.tsx
│   │   ├── Sidebar.tsx
│   │   └── BlurBanner.tsx
│   ├── catalog/
│   │   ├── ProductGrid.tsx
│   │   ├── ProductCard.tsx
│   │   └── CategoryFilter.tsx
│   ├── editor/
│   │   ├── Canvas2D.tsx
│   │   ├── ToolPanel.tsx
│   │   ├── LayerPanel.tsx
│   │   └── TemplateLoader.tsx
│   ├── viewer3d/
│   │   ├── Mock3DViewer.tsx
│   │   ├── OrbitControls.tsx
│   │   └── MaterialSwapper.tsx
│   └── checkout/
│       ├── OrderSummary.tsx
│       ├── CustomerForm.tsx
│       └── PaymentForm.tsx
├── lib/
│   ├── products-real.ts (catálogo completo)
│   ├── editor-engine.ts (Fabric.js wrapper)
│   ├── 3d-mock.ts (CSS 3D utilities)
│   └── checkout.ts (Stripe integration)
└── styles/
    └── twinsublime.css (tema actualizado)
```

## ORDEN DE DESARROLLO ACTUALIZADO

### Sprint 1 (Semana 1): Foundation + Catálogo Real
- [x] Base Next.js existente
- [ ] Catálogo real de productos con datos completos
- [ ] Design Lab layout tripartito
- [ ] Identidad visual twinsublime aplicada

### Sprint 2 (Semana 2): Editor 2D Funcional
- [ ] Canvas con Fabric.js/Konva
- [ ] Plantillas reales por producto
- [ ] Herramientas básicas (texto, imagen, color)
- [ ] Validación y export (PNG + JSON)

### Sprint 3 (Semana 3): Visor 3D Mock + Integración
- [ ] Mock 3D convincente con CSS transforms
- [ ] Controles orbit y zoom
- [ ] Aplicación diseño 2D sobre modelo
- [ ] Hooks preparados para Three.js

### Sprint 4 (Semana 4): Checkout + Refinamiento
- [ ] Flujo compra completo
- [ ] Stripe test integration
- [ ] Generación archivos pedido
- [ ] Polish y microinteracciones

## NOTAS CRÍTICAS

### Prioridades
1. **Funcionalidad real**: No maquetas, MVP funcional
2. **Datos reales**: Catálogo twinsublime completo desde día 1
3. **UX fluida**: Transiciones entre vistas sin cortes
4. **Responsive**: Mobile-first, especialmente editor

### Preparación Futura
- **Three.js hooks**: Arquitectura lista para motor 3D real
- **API endpoints**: Estructura preparada para backend
- **Escalabilidad**: Componentes modulares y reutilizables
- **Testing**: Hooks para Jest + React Testing Library

### Validación Continua
- **Sketches compliance**: Cada vista debe coincidir con bocetos
- **Flujo usuario**: Test continuo del journey completo
- **Performance**: Optimización de canvas y 3D mock
- **Accesibilidad**: WCAG compliance en todos los componentes
