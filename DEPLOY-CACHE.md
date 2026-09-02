# Cabeceras de caché — DigitalOcean App Platform

## El problema

Lighthouse marca **"Usar tiempos de vida de caché eficientes" (~257 KiB)** porque
los sitios estáticos de **DigitalOcean App Platform** sirven todos los archivos con
`Cache-Control: max-age=10` (10 segundos) y **no permiten personalizar esa cabecera**
desde el panel ni desde el App Spec. Es una limitación conocida de la plataforma.

No se puede arreglar tocando el HTML/CSS/JS. Hay dos caminos:

---

## Opción A — Cloudflare por delante (recomendada, ~10 min, sin re-deploy)

1. Crear cuenta gratis en Cloudflare y añadir el dominio `patiolaesperanza.com.mx`.
2. Cambiar los **nameservers** del dominio a los que indique Cloudflare.
3. En Cloudflare, dejar el registro que apunta a la app de DO **proxied** (nube naranja).
4. **Rules → Cache Rules → Create rule:**
   - Nombre: `Assets estáticos`
   - Si: `URI Path` `starts with` `/assets/`
   - Entonces:
     - *Cache eligibility*: `Eligible for cache`
     - *Edge TTL*: `Override origin` → `1 month`
     - *Browser TTL*: `Override origin` → `1 year`
5. (Opcional) Segunda regla para `.avif .webp .jpg .png .woff2 .css .js` con el mismo TTL.

Cloudflare reescribe el `Cache-Control` que envía DO y el aviso de Lighthouse desaparece.
Además añade CDN global y compresión Brotli gratis.

> Al publicar cambios, purgar la caché de Cloudflare (Caching → Configuration → Purge Everything)
> o usar nombres de archivo versionados.

---

## Opción B — Servir con nginx (componente "Web Service" en vez de "Static Site")

Cambia el tipo de componente en DO. Añadir estos dos archivos a la raíz del repo y,
en la app de DO, borrar el componente *Static Site* y crear uno *Web Service* (Dockerfile).

**`Dockerfile`**

```dockerfile
FROM nginx:1.27-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY . /usr/share/nginx/html
```

**`nginx.conf`**

```nginx
server {
    listen 8080;
    root /usr/share/nginx/html;
    index index.html;

    # Brotli/gzip lo aplica el edge de DO automáticamente

    location / {
        try_files $uri $uri/ =404;
        error_page 404 /404.html;
    }

    # HTML: siempre revalidar
    location ~* \.html$ {
        add_header Cache-Control "public, max-age=0, must-revalidate";
    }

    # Estáticos con hash de contenido implícito: cache larga
    location ~* \.(avif|webp|jpg|jpeg|png|gif|svg|woff2|css|js|mp4)$ {
        add_header Cache-Control "public, max-age=31536000, immutable";
        access_log off;
    }
}
```

DO expone el puerto `8080` por defecto para web services. Tras el deploy, los
estáticos se sirven con `max-age=31536000` y el aviso desaparece.

---

## Estado del resto de avisos (ya corregidos en código)

| Aviso Lighthouse | Estado |
|---|---|
| Visualización de fuentes (~40 ms) | ✅ Font Awesome eliminado; las fuentes propias ya usan `font-display: swap` |
| Árbol de dependencia de red | ✅ Eliminada la petición a `cdnjs.cloudflare.com` (Font Awesome → iconos SVG en línea) |
| Mejorar la entrega de imágenes (~87 KiB) | ✅ Versiones AVIF del hero y de la galería + `image-set()` / `<source type="image/avif">` |
| Usar tiempos de vida de caché eficientes (~257 KiB) | ⚠️ Requiere Opción A o B de este documento (límite de DO App Platform) |
