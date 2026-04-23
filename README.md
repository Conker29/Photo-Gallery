# **Aplicación Móvil - Galería de Fotos**

### Descripción
Esta aplicación móvil desarrollada con Ionic permite capturar, guardar y visualizar fotos dentro de una galería organizada por pestañas (tabs). Además, incluye funcionalidades como almacenamiento automático en la galería del dispositivo y alertas personalizadas.

## Funcionalidades
### 1. Mostrar fotos en el Tab 3

Las imágenes capturadas se muestran en el apartado Fotos (Tab 3) en formato de galería.
<img width="702" height="1600" alt="WhatsApp Image 2026-04-17 at 4 39 44 PM (1)" src="https://github.com/user-attachments/assets/a87e20aa-01df-4411-934b-b25973296f71" />

#### 2. Guardar fotos con apellido

Las fotos se almacenan incluyendo el apellido en el nombre del archivo.
<img width="702" height="1600" alt="WhatsApp Image 2026-04-17 at 4 39 44 PM (2)" src="https://github.com/user-attachments/assets/c054c590-1fa7-48aa-aaed-15d3fc45176d" />

### 3. Mostrar alerta en Tab 1

Se implementó un método que muestra una alerta con un mensaje predeterminado al presionar un botón.
<img width="1080" height="2460" alt="WhatsApp Image 2026-04-17 at 4 39 43 PM" src="https://github.com/user-attachments/assets/29fc141e-0694-490e-95ab-3ba4617eabe8" />

### 4. Guardar fotos en la galería del teléfono

Se utilizó la propiedad:
const capturedPhoto = await Camera.getPhoto({
  resultType: CameraResultType.Uri,
  source: CameraSource.Camera,
  quality: 100,
  saveToGallery: true,
});

### Comandos utilizados

Después de realizar cambios:
ionic build
ionic cap copy

### Integrantes
Emilio Erazo
Jhosselin Naula

# Ionic App — Guía de configuración de Splash Screen & Icon

## Tabla de contenidos

- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Splash Screen personalizado](#splash-screen-personalizado)
- [Ícono de aplicación personalizado](#ícono-de-aplicación-personalizado)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Scripts disponibles](#scripts-disponibles)
- [Personalización de marca](#personalización-de-marca)

---

## Requisitos previos

| Herramienta | Versión mínima |
|-------------|---------------|
| Node.js     | 18.x          |
| npm         | 9.x           |
| Ionic CLI   | 7.x           |
| Angular CLI | 17.x          |

```bash
npm install -g @ionic/cli @angular/cli
```

---

## Instalación

```bash
# 1. Clonar el repositorio
git clone <URL_DEL_REPO>
cd <nombre-del-proyecto>

# 2. Instalar dependencias
npm install

# 3. Ejecutar en el navegador
ionic serve
```

---

## Splash Screen personalizado

Esta app incluye un **splash screen animado** construido como componente Angular/Ionic nativo. No requiere plugins de Capacitor para el comportamiento básico.

### Archivos relevantes

```
src/app/
└── splash/
    ├── splash.page.html   ← Estructura visual
    ├── splash.page.scss   ← Animaciones y estilos
    ├── splash.page.ts     ← Lógica de duración y navegación
    └── splash.module.ts   ← Módulo y rutas
```

### Cómo funciona

1. `app.component.ts` redirige inmediatamente a `/splash` al iniciar.
2. `SplashPage` ejecuta `initializeApp()` en paralelo con un temporizador mínimo.
3. Cuando ambos terminan, activa la animación `fade-out` y navega a `/tabs/tab1`.

### Personalizar la duración

Edita las constantes en `splash.page.ts`:

```typescript
// Tiempo total que el usuario ve el splash (ms)
private readonly SPLASH_DURATION = 3000;

// Duración de la animación de salida (debe coincidir con el CSS)
private readonly FADE_DURATION = 500;
```

### Reemplazar el logo

Abre `splash.page.html` y sustituye el bloque `<svg>` dentro de `.logo-icon` por tu imagen o ícono:

```html
<!-- Opción A: imagen PNG/SVG propia -->
<img src="assets/icon/logo.png" alt="Logo" />

<!-- Opción B: ion-icon de Ionicons -->
<ion-icon name="rocket-outline" style="font-size:40px;color:white;"></ion-icon>
```

### Cambiar colores del splash

En `splash.page.scss` ajusta las variables al inicio del archivo:

```scss
$primary:   #6C63FF;   // ← tu color principal
$secondary: #48CFAD;   // ← tu color de acento
$dark-bg:   #0F0E17;   // ← color de fondo
```

### Agregar lógica de inicialización

Descomenta o añade código en el método `initializeApp()` dentro de `splash.page.ts`:

```typescript
private async initializeApp(): Promise<void> {
  const token = localStorage.getItem('auth_token');
  if (token) {
    await this.authService.validateToken(token);
  }
  await this.configService.loadRemoteConfig();
}
```

El splash esperará automáticamente a que esta promesa se resuelva antes de continuar.

---

## Ícono de aplicación personalizado

### Opción recomendada: `@capacitor/assets` (automático)

Esta es la forma más sencilla. Genera todos los tamaños necesarios para iOS y Android a partir de un solo archivo fuente.

#### Paso 1 — Instalar la herramienta

```bash
npm install @capacitor/assets --save-dev
```

#### Paso 2 — Preparar tu imagen fuente

Crea la carpeta y coloca tu imagen:

```
assets/
├── icon-only.png        ← ícono sin fondo (1024×1024 px, PNG con transparencia)
├── icon-foreground.png  ← capa frontal para Android adaptive icon (1024×1024 px)
└── icon-background.png  ← capa de fondo para Android adaptive icon (1024×1024 px)
```

> **Requisitos de la imagen:**
> - Formato PNG
> - Tamaño mínimo: **1024 × 1024 píxeles**
> - Para `icon-only.png`: fondo transparente, diseño centrado con ~20% de margen seguro
> - Para el splash nativo (Capacitor): también se admite `splash.png` (2732×2732 px)

#### Paso 3 — Generar todos los íconos

```bash
npx @capacitor/assets generate --iconBackgroundColor '#0F0E17' --iconBackgroundColorDark '#0F0E17'
```

Esto generará automáticamente los íconos en:
- `ios/App/App/Assets.xcassets/AppIcon.appiconset/`
- `android/app/src/main/res/mipmap-*/`

#### Paso 4 — Sincronizar con Capacitor

```bash
npx cap sync
```

---

### Opción manual

Si prefieres hacerlo a mano:

#### Android

Reemplaza los archivos en estas carpetas con tu ícono en el tamaño correspondiente:

| Carpeta                                  | Tamaño     |
|------------------------------------------|-----------|
| `android/app/src/main/res/mipmap-mdpi`   | 48×48 px  |
| `android/app/src/main/res/mipmap-hdpi`   | 72×72 px  |
| `android/app/src/main/res/mipmap-xhdpi`  | 96×96 px  |
| `android/app/src/main/res/mipmap-xxhdpi` | 144×144 px|
| `android/app/src/main/res/mipmap-xxxhdpi`| 192×192 px|

Archivos a reemplazar: `ic_launcher.png`, `ic_launcher_round.png`, `ic_launcher_foreground.png`

#### iOS

Reemplaza los archivos en:
```
ios/App/App/Assets.xcassets/AppIcon.appiconset/
```

Debes proveer todos los tamaños definidos en `Contents.json`. Los más importantes:

| Nombre del archivo       | Tamaño      |
|--------------------------|-------------|
| `AppIcon-20@2x.png`      | 40×40 px    |
| `AppIcon-20@3x.png`      | 60×60 px    |
| `AppIcon-60@2x.png`      | 120×120 px  |
| `AppIcon-60@3x.png`      | 180×180 px  |
| `AppIcon-1024.png`       | 1024×1024 px|

---

### Ícono en el navegador (PWA / favicon)

Reemplaza el archivo en:
```
src/assets/icon/favicon.png
```
Tamaño recomendado: **512×512 px** (se escala automáticamente).

---

## Estructura del proyecto

```
src/
├── app/
│   ├── splash/            ← Splash screen (NUEVO)
│   │   ├── splash.page.html
│   │   ├── splash.page.scss
│   │   ├── splash.page.ts
│   │   └── splash.module.ts
│   ├── tabs/              ← Contenedor de pestañas
│   ├── tab1/              ← Pestaña 1
│   ├── tab2/              ← Pestaña 2
│   ├── tab3/              ← Pestaña 3
│   ├── app-routing.module.ts
│   ├── app.component.ts
│   └── app.module.ts
├── assets/
│   └── icon/
│       └── favicon.png    ← Ícono PWA/navegador
├── theme/
│   └── variables.scss     ← Variables de color de Ionic
└── global.scss            ← Estilos globales
```

---

## Scripts disponibles

```bash
# Desarrollo en navegador con hot-reload
ionic serve

# Build de producción
ionic build --prod

# Agregar plataforma nativa
ionic cap add android
ionic cap add ios

# Sincronizar cambios web a nativo
npx cap sync

# Abrir en Android Studio
npx cap open android

# Abrir en Xcode
npx cap open ios
```

---

## Personalización de marca

Para cambiar el nombre de la aplicación:

### Android
Edita `android/app/src/main/res/values/strings.xml`:
```xml
<string name="app_name">NombreDeTuApp</string>
```

### iOS
Edita el campo `Bundle Display Name` en `ios/App/App/Info.plist`:
```xml
<key>CFBundleDisplayName</key>
<string>NombreDeTuApp</string>
```

### Web / PWA
Edita el `<title>` en `src/index.html`:
```html
<title>NombreDeTuApp</title>
```
