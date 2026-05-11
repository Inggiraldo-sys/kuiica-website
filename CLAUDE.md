# Proyecto: Sitio Web Kuiica S.A.S.

## Descripción
Sitio web estático de Kuiica S.A.S. — Laboratorio colombiano de economía circular.
Fabrican maquinaria para reciclaje plástico y productos corporativos hechos de plástico recuperado.

**URL en producción:** https://kuiica.com y https://www.kuiica.com
**Repo GitHub:** https://github.com/Inggiraldo-sys/kuiica-website

---

## Acceso al VPS (servidor)

- **IP:** 46.224.223.222
- **Usuario principal:** juan (tiene sudo sin contraseña)
- **Usuario root:** también habilitado por SSH
- **Llave SSH:** `C:\Users\jggv2\OneDrive\Escritorio\Claude\Vega Lyrae\vega-hetzner`
- **Proveedor:** Hetzner Cloud (Ubuntu 24.04)

```powershell
# Conectarse al VPS:
ssh -i "C:\Users\jggv2\OneDrive\Escritorio\Claude\Vega Lyrae\vega-hetzner" juan@46.224.223.222
```

---

## Estructura del servidor

```
/var/www/kuiica/          <- raíz del sitio web
    index.html            <- página principal
    img/                  <- imágenes del sitio
        prod-agenda.jpg
        prod-llaveros.jpg
        prod-portavasos.jpg
        prod-soporte-celular.jpg
        prod-soporte-portatil.jpg
        prod-trofeo.jpg
        (+ más imágenes hero, taller, nosotros, etc.)
```

---

## Estructura del repo local

```
pagina web/
    kuiica_index.html     <- fuente del index.html desplegado en el VPS
    kuiica_img_web/       <- imágenes web listas para producción
    kuiica_prod_final/    <- fotos producto finales (varias versiones)
    kuiica_catalog_thumbs/  <- miniaturas para catálogo
    kuiica_transparent_test/ <- pruebas de recorte de imágenes
    kuiica_imgs/          <- imágenes fuente (excluidas del repo, muy pesadas)
    kuiica_catalog_imgs/  <- fotos catálogo originales (108 MB, excluidas del repo)
    Fotos/                <- copia de todas las fotos (excluida del repo)
    *.pdf                 <- documentos internos (excluidos del repo)
```

---

## Cómo hacer deploy al VPS

### Actualizar index.html:
```powershell
scp -i "C:\Users\jggv2\OneDrive\Escritorio\Claude\Vega Lyrae\vega-hetzner" `
    "kuiica_index.html" `
    juan@46.224.223.222:/var/www/kuiica/index.html
```

### Actualizar imágenes:
```powershell
scp -i "C:\Users\jggv2\OneDrive\Escritorio\Claude\Vega Lyrae\vega-hetzner" `
    "kuiica_img_web\nombre-imagen.jpg" `
    juan@46.224.223.222:/var/www/kuiica/img/
```

### Sincronizar toda la carpeta img/:
```powershell
# Requiere rsync o WinSCP. Alternativa con scp:
scp -i "C:\Users\jggv2\OneDrive\Escritorio\Claude\Vega Lyrae\vega-hetzner" `
    kuiica_img_web\* `
    juan@46.224.223.222:/var/www/kuiica/img/
```

---

## Nginx en el servidor

Config en: `/etc/nginx/sites-available/kuiica`

El sitio corre en Nginx como servidor estático.
SSL con Let's Encrypt (Certbot) — cubre `kuiica.com` y `www.kuiica.com`.

Para recargar Nginx después de cambios de config:
```bash
sudo nginx -t && sudo systemctl reload nginx
```

---

## Flujo de trabajo recomendado

1. `git pull` antes de empezar
2. Editar `kuiica_index.html` y/o imágenes en `kuiica_img_web/`
3. Probar localmente abriendo `kuiica_index.html` en el navegador
4. Hacer deploy al VPS con `scp`
5. `git add`, `git commit`, `git push`

---

## Estado del proyecto (mayo 2026)

- ✅ Sitio desplegado y funcional en kuiica.com
- ✅ SSL válido para kuiica.com y www.kuiica.com
- ✅ 6 productos visibles en la sección de productos
- ✅ Backup semanal automático en el VPS (cada domingo 3am)
- ✅ Rate limiting en /webhook (Nginx)
- 🔲 Pendiente: agregar más productos al catálogo
- 🔲 Pendiente: formulario de contacto funcional

---

## Otros proyectos en el VPS

El mismo servidor aloja:
- **Meta Bot Kuiica** (`/var/www/meta_bot`) — bot de WhatsApp Business con FastAPI + Gemini
- **Vega Lyrae** — asistente WhatsApp de OpenClaw

Ver sus respectivas carpetas en `C:\Users\jggv2\OneDrive\Escritorio\Claude\` para más detalles.
