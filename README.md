# 🧩 SERA Plugins Marketplace

> Catálogo central descentralizado de plugins para **SERA** (Sistema de Expedientes de Registro Avanzado).

[![Plugins](https://img.shields.io/badge/plugins-1-brightgreen?style=for-the-badge&logo=github)](plugins.json)
[![SERA](https://img.shields.io/badge/SERA-v4.0%2B-blue?style=for-the-badge)](https://github.com/carlosprost/SERA)
[![Licencia](https://img.shields.io/badge/licencia-MIT-orange?style=for-the-badge)](LICENSE)

---

## ¿Qué es esto?

Este repositorio actúa como la **base de datos del catálogo** de plugins verificados para SERA. La app consume el archivo `plugins.json` directamente desde GitHub en caliente cuando el operador abre la pantalla del Marketplace.

**No hay servidores. No hay costos. 100% descentralizado y gratuito de por vida.**

---

## 📦 Estructura del `plugins.json`

Cada entrada del catálogo tiene la siguiente estructura:

```json
{
  "id": "mi-plugin-id",
  "name": "Nombre del Plugin",
  "version": "1.0.0",
  "description": "Descripción clara y concisa de qué hace el plugin.",
  "author": "Tu Nombre o Usuario de GitHub",
  "icon": "nombre_del_icono_material",
  "tags": ["tag1", "tag2"],
  "entry_url": "https://cdn.jsdelivr.net/gh/usuario/repo@version/dist/index.js",
  "style_url": "https://cdn.jsdelivr.net/gh/usuario/repo@version/dist/style.css",
  "repository": "https://github.com/usuario/mi-plugin-repo",
  "verified": false,
  "sera_min_version": "4.0.0"
}
```

### Campos

| Campo | Requerido | Descripción |
|---|---|---|
| `id` | ✅ | Identificador único. Snake-case, sin espacios. |
| `name` | ✅ | Nombre visible en el Marketplace de SERA. |
| `version` | ✅ | Versión semántica (`X.Y.Z`). |
| `description` | ✅ | Descripción visible en la tarjeta del Marketplace. |
| `author` | ✅ | Nombre del desarrollador o equipo. |
| `icon` | ❌ | Nombre de un icono de [Material Icons](https://fonts.google.com/icons). Default: `extension`. |
| `tags` | ❌ | Array de etiquetas para búsqueda y filtrado. |
| `entry_url` | ✅ | URL del bundle JS principal (jsDelivr recomendado). |
| `style_url` | ❌ | URL del archivo CSS de estilos del plugin. |
| `repository` | ❌ | URL del repositorio fuente. |
| `verified` | ❌ | `true` si el plugin fue revisado por el equipo WolfTeI. |
| `sera_min_version` | ❌ | Versión mínima requerida de SERA. |

---

## 🚀 ¿Cómo publicar tu plugin?

### 1. Creá tu repositorio de plugin

Seguí la [Guía para Desarrolladores](https://github.com/carlosprost/sera-plugin-hello-world#readme) usando el plugin de demo oficial como base.

### 2. Publicá con jsDelivr

Una vez que tu repo está en GitHub con un **release/tag** de versión, tu URL de CDN gratuita es:

```
https://cdn.jsdelivr.net/gh/TU_USUARIO/TU_REPO@VERSION/dist/index.js
```

> **Ejemplo:** `https://cdn.jsdelivr.net/gh/juangarcia/sera-mi-plugin@1.0.0/dist/index.js`

### 3. Enviá un Pull Request

Abrí un **PR** a este repositorio agregando tu plugin al `plugins.json`. El equipo revisará:
- ✅ El código no accede directamente a `window.__TAURI__`
- ✅ El plugin usa únicamente la API de `window.SeraAPI`
- ✅ No hay peticiones a servidores de terceros sin documentar

---

## 🛡️ API Disponible para Plugins (`window.SeraAPI`)

```javascript
// NAMESPACE UI - Registro de componentes visuales
SeraAPI.ui.registerRibbonButton({
  id: 'mi-boton-unico',
  label: 'Mi Botón',
  icon: 'star',
  tooltip: 'Descripción del botón',
  action: () => { /* lógica */ }
});

SeraAPI.ui.registerDashboardWidget({
  id: 'mi-widget-unico',
  title: 'Mi Widget',
  colSpan: 1, // 1, 2 o 3 columnas
  render: (element) => {
    element.innerHTML = '<p>¡Hola desde mi plugin!</p>';
  }
});

SeraAPI.ui.registerSidebarTab({
  id: 'mi-tab-unico',
  title: 'Mi Pestaña',
  icon: 'dashboard',
  action: () => { /* lógica al activar */ }
});

// NAMESPACE DATA - Acceso controlado a los datos
const tablas = await SeraAPI.data.getTablas();
const campos = await SeraAPI.data.getCampos('nombre_tabla');
const registros = await SeraAPI.data.getContenido('nombre_tabla');

SeraAPI.data.onBeforeInsert('nombre_tabla', async (record) => {
  // Interceptar y modificar el record antes de insertarlo
  return record;
});

// NAMESPACE ENV - Entorno y notificaciones
SeraAPI.env.showNotification('¡Operación completada!', 'success');
// Tipos: 'info' | 'success' | 'warning' | 'error'
```

---

## 📜 Licencia

Este repositorio es de libre uso bajo la licencia **MIT**.

Los plugins individuales pueden tener sus propias licencias definidas en sus repositorios respectivos.
