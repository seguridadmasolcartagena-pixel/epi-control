[README.md](https://github.com/user-attachments/files/29164215/README.md)
# ⛑️ EPI Control — Guía de configuración completa

Sigue estos pasos **en orden**. El proceso completo lleva unos 20 minutos.

---

## RESUMEN DEL SISTEMA

```
Móvil del empleado
      ↓  (abre URL o escanea QR)
GitHub Pages  →  index.html
      ↓  (llamadas a la API)
Google Apps Script
      ↓  (lee y escribe)
Google Sheets  ←  Base de datos
```

---

## PASO 1 — Crear la hoja de cálculo en Google Sheets

1. Ve a [sheets.google.com](https://sheets.google.com) con tu cuenta de empresa
2. Haz clic en **"+"** para crear una nueva hoja
3. Ponle el nombre: **EPI Control**
4. Deja la hoja abierta, la usarás en el paso 2

---

## PASO 2 — Configurar Google Apps Script

### 2a. Abrir el editor de script

1. En tu hoja de cálculo, haz clic en el menú **Extensiones**
2. Haz clic en **Apps Script**
3. Se abrirá el editor en una nueva pestaña
4. Borra todo el código que aparece por defecto

### 2b. Pegar el código

1. Abre el archivo **`Code.gs`** que hay junto a este README
2. Selecciona todo el contenido (Ctrl+A) y cópialo
3. Pégalo en el editor de Apps Script
4. Haz clic en el icono de **guardar** (💾) o pulsa Ctrl+S
5. Ponle nombre al proyecto: **EPI Control**

### 2c. Inicializar las hojas

1. En el editor, selecciona la función **`inicializarHojas`** en el desplegable de funciones (arriba, junto al botón "Ejecutar")
2. Haz clic en **"Ejecutar"**
3. La primera vez pedirá permisos — haz clic en **"Revisar permisos"**
4. Elige tu cuenta de Google
5. Verás un aviso de "Google no ha verificado esta aplicación" — haz clic en **"Configuración avanzada"** y luego en **"Ir a EPI Control (no seguro)"**
6. Haz clic en **"Permitir"**
7. Vuelve al editor y ejecuta `inicializarHojas` de nuevo
8. Deberías ver un mensaje de éxito

### 2d. Cargar los EPIs por defecto

1. En el desplegable, selecciona la función **`cargarEPIsDefecto`**
2. Haz clic en **"Ejecutar"**
3. Espera a que termine (puede tardar 30-60 segundos porque crea muchas filas)
4. Al terminar verás el mensaje de éxito

### 2e. Desplegar como aplicación web

1. Haz clic en el botón **"Implementar"** (arriba a la derecha)
2. Selecciona **"Nueva implementación"**
3. Haz clic en el icono ⚙️ junto a "Tipo" y selecciona **"Aplicación web"**
4. Configura así:
   - **Descripción:** EPI Control v1
   - **Ejecutar como:** Yo (tu cuenta)
   - **Quién tiene acceso:** Cualquier usuario
5. Haz clic en **"Implementar"**
6. Copia la **URL de la aplicación web** — es larga y empieza por `https://script.google.com/macros/s/...`

> ⚠️ **Guarda esta URL, la necesitarás en el paso 4.**

---

## PASO 3 — Publicar en GitHub Pages

### 3a. Crear cuenta y repositorio

1. Ve a [github.com](https://github.com) y crea una cuenta gratuita si no tienes
2. Haz clic en **"New repository"** (botón verde)
3. Nombre: `epi-control`
4. Selecciona **"Public"**
5. Haz clic en **"Create repository"**

### 3b. Subir el index.html

1. En tu repositorio, haz clic en **"uploading an existing file"**
2. Arrastra el archivo **`index.html`** a la zona de carga
3. Escribe un comentario: `Versión inicial`
4. Haz clic en **"Commit changes"**

### 3c. Activar GitHub Pages

1. Ve a **Settings** (pestaña del repositorio)
2. En el menú lateral, haz clic en **"Pages"**
3. En **"Source"**, selecciona `Deploy from a branch`
4. En **"Branch"**, selecciona `main` y carpeta `/ (root)`
5. Haz clic en **"Save"**
6. Espera 2 minutos y recarga la página
7. Aparecerá tu URL: `https://TU-USUARIO.github.io/epi-control/`

---

## PASO 4 — Conectar la app con la base de datos

Este es el paso clave: hay que decirle al `index.html` cuál es la URL de tu Apps Script.

1. Descarga el `index.html` de tu repositorio (haz clic en él y luego en el botón de descarga ⬇)
2. Ábrelo con el **Bloc de notas** (Windows) o **TextEdit** (Mac)
3. Busca esta línea (está muy arriba, en las primeras líneas):
   ```
   const API_URL = 'PEGA_AQUI_TU_URL_DE_APPS_SCRIPT';
   ```
4. Sustituye `PEGA_AQUI_TU_URL_DE_APPS_SCRIPT` por la URL que copiaste en el paso 2e, **manteniendo las comillas simples**:
   ```
   const API_URL = 'https://script.google.com/macros/s/XXXXXXX/exec';
   ```
5. Guarda el archivo
6. Vuelve a tu repositorio en GitHub
7. Haz clic en el `index.html` ya subido
8. Haz clic en el icono de lápiz ✏️ para editarlo
9. Selecciona todo (Ctrl+A), borra, y pega el contenido del archivo modificado
10. Haz clic en **"Commit changes"**

Espera 2 minutos y abre tu URL de GitHub Pages — la app debería cargar y conectarse.

---

## PASO 5 — Crear el código QR

1. Ve a [qrcode-monkey.com](https://www.qrcode-monkey.com)
2. Pega tu URL de GitHub Pages
3. Personaliza el color si quieres (verde: #0F6E56)
4. Descarga el QR en PNG
5. Imprímelo y pégalo en la puerta del almacén

---

## PASO 6 — Configurar el trigger mensual (opcional)

Para que el resumen mensual se envíe automáticamente:

1. En el editor de Apps Script, haz clic en el icono de **reloj** ⏰ (Activadores / Triggers) en el menú lateral
2. Haz clic en **"+ Añadir activador"**
3. Configura:
   - **Función:** `resumenMensualAuto`
   - **Tipo de evento:** `Temporizado`
   - **Tipo de tiempo:** `Temporizador mensual`
   - **Día del mes:** 1 (o el día que prefieras)
4. Haz clic en **"Guardar"**

---

## MANTENIMIENTO

### Actualizar la aplicación (cuando haya mejoras)
1. Descarga el nuevo `index.html`
2. Edita la primera línea con tu URL de Apps Script (igual que el paso 4)
3. Súbelo al repositorio de GitHub sustituyendo el anterior

### Si necesitas actualizar el script (Code.gs)
1. Abre Apps Script
2. Pega el nuevo código
3. Guarda (Ctrl+S)
4. **Importante:** haz clic en "Implementar" → "Gestionar implementaciones"
5. Haz clic en el icono de lápiz ✏️ de tu implementación existente
6. En "Versión" selecciona **"Nueva versión"**
7. Haz clic en "Implementar"
8. La URL no cambia — el nuevo código queda activo de inmediato

### Copia de seguridad
La propia hoja de Google Sheets es la copia de seguridad. Google la guarda automáticamente con historial de versiones.

---

## CONTRASEÑA DEL GESTOR

La contraseña por defecto es `admin123`.  
Cámbiala desde la app: **Panel gestor → Ajustes → Cambiar contraseña**.

---

## PROBLEMAS FRECUENTES

| Problema | Solución |
|---|---|
| "Error al conectar" al abrir la app | Comprueba que la URL en `API_URL` es correcta y que el script está desplegado como "Cualquier usuario" |
| La app carga pero no muestra datos | Ejecuta `inicializarHojas` y `cargarEPIsDefecto` desde el editor |
| "Google no ha verificado esta aplicación" al dar permisos | Es normal — haz clic en "Configuración avanzada" y "Continuar" |
| Los cambios en GitHub no se ven | Espera 2 minutos y fuerza la recarga con Ctrl+Shift+R |
| El email de alerta no llega | Comprueba que has guardado los emails desde Ajustes y que el script tiene permiso de correo |
