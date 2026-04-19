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
