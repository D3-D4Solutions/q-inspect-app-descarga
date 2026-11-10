# Página de descarga · Q-Inspect (Vercel)

**Fecha:** 10 nov 2026 · **Dueño:** Sistemas D3-D4 Solutions
**Qué es:** página estática para que planta descargue e instale la versión más reciente de la app Q-Inspect en tablets Android, con la guía de instalación y de vinculación.

---

## 1. Contenido de la página

| Bloque | Qué muestra |
|---|---|
| Barra de marca | «D3-D4 Solutions» en tipografía **Space Age** |
| Hero (Navy) | Logo de Q-Inspect, indicador de **versión más reciente**, chip «Solo disponible en Android», botón de descarga animado, fecha, tamaño y Android mínimo |
| Tarjeta lateral | Equipo recomendado: **Samsung Galaxy Tab A11** |
| Aviso ámbar | Aparece solo si se abre desde un equipo que no es Android |
| Instalar la app | FAQ de 6 preguntas: descargar, apps desconocidas, Play Protect, permisos, actualizar, otros dispositivos |
| Vincular la tablet | 4 pasos (Panel › Planta › Tablets › Vincular → abrir app → escanear QR de 15 min y un uso → confirmar) + FAQ: código vencido, mover de área, tablet desactivada |

## 2. Estados del botón de descarga

| Estado | Animación |
|---|---|
| `listo` | Flecha con rebote suave que invita a tocar |
| `descargando` | Flecha cayendo en bucle, barra de llenado y % real (APK en el mismo dominio) |
| `indeterminado` | Barrido continuo (APK en otro dominio, p. ej. GitHub Releases) |
| `completo` | Círculo verde con «pop» y palomita trazada |
| `error` | Ámbar, «Toca para intentar de nuevo» |

Con `prefers-reduced-motion` se apagan todas las animaciones.

## 3. Estructura

```
qinspect-descarga/
├── index.html            Página completa (HTML + CSS + JS)
├── version.json          ← lo único que se edita en cada versión
├── vercel.json           Encabezados: APK como descarga, version.json sin caché
├── assets/logo-qinspect.png   Espacio del logo (PNG cuadrado 512×512)
├── apk/                  Aquí va el APK firmado
└── fonts/                Space Age, Instrument Sans, Inter (locales, sin CDN)
```

## 4. Publicar una versión nueva

1. Copia el APK firmado a `apk/q-inspect-X.Y.Z.apk`.
2. Edita `version.json`:
   ```json
   {
     "version": "X.Y.Z",
     "fecha": "AAAA-MM-DD",
     "apk_url": "apk/q-inspect-X.Y.Z.apk",
     "apk_nombre": "q-inspect-X.Y.Z.apk",
     "tamano_mb": 48.2,
     "min_android": "Android 10 o superior",
     "demo": false
   }
   ```
   `tamano_mb` y `min_android` se ocultan si van vacíos. `demo: true` simula la descarga para revisar la animación.
3. `git push`: Vercel despliega solo.

**El APK debe firmarse siempre con la misma llave.** Si cambia la firma, Android no instala encima y habría que desinstalar, lo que borra la vinculación de la tablet.

## 5. Despliegue en Vercel

1. Sube la carpeta a un repo (p. ej. `D3-D4Solutions/qinspect-descarga`).
2. En Vercel: **Add New › Project**, importa el repo, framework **Other**, sin comando de build y con la raíz como carpeta de salida.
3. Dominio sugerido: `descargas.q-inspect.tech` (registro CNAME en Hostinger hacia Vercel).

## 6. Pendientes antes de salir

- [ ] **Fuente Space Age:** agregar `fonts/SpaceAge.woff2` (o `.ttf`) y **revisar su licencia de uso comercial**; varias versiones gratuitas son solo para uso personal. Agregar también Instrument Sans e Inter (ambas con licencia OFL).
- [ ] **Versión real:** `version.json` trae `6.1.0` de ejemplo; poner la de `pubspec.yaml`.
- [ ] **Android mínimo:** tomarlo de `minSdk` en `android/app/build.gradle` (Chris).
- [ ] **Plan de Vercel:** el plan Hobby es para uso personal no comercial; para una empresa corresponde Pro. Confirmarlo antes de publicar.
- [ ] **Tamaño del APK:** si supera el límite de archivo de Vercel, alojarlo en GitHub Releases y poner esa URL en `apk_url` (la página cambia sola a animación indeterminada).
- [ ] **Acceso:** la página es pública. Si no se quiere que cualquiera baje el APK, proteger con contraseña de Vercel o servirla solo desde el panel.

## 7. Diseño

- Paleta D3-D4 (`DISENO_QINSPECT_V6.md` §2): Navy `#091F5B`, Cloud `#EFEFEF`, Silver `#D0D0D2`, Graphite `#545454`, Ink `#161A24`, Success `#16845B`, ámbar `#F2A900` solo para avisos.
- El único rojo visible es el del logo. Diseño plano: sin gradientes ni sombras decorativas.
- Áreas táctiles de 60 px o más; probado a 1340 px (tablet horizontal), 1280 px y 390 px.
# q-inspect-app-descarga
