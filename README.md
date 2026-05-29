# Presupuestador ProRatto

Single-page app desplegada vía GitHub Pages para vendedores de Ratto Hnos.

## URL

https://proratto.rattohnos.com.ar

## Cómo se actualiza

El HTML que vive acá (`index.html`) se genera con el build script de
`/Users/lucasivancevich/Documents/Presupuestador/build_v21.py`.

Para publicar una versión nueva:

```bash
cd /Users/lucasivancevich/Documents/Presupuestador
python3 build_v21.py
cp Presupuestador_ProRatto_v<N>.html pages-dist/index.html
cd pages-dist
git add index.html
git commit -m "Deploy v<N>"
git push
```

GitHub Pages publica automáticamente en cada push a `main`.

## Auth

El frontend usa **MSAL.js (Microsoft Entra)** para que solo usuarios
`@rattohnos.com.ar` puedan llamar a los workers de email y PDF.

Config en `build_v21.py`:
- `SPA_TENANT_ID`: Tenant ID de Ratto (`394fd463-92ba-4fde-9bc4-b5661afdf620`)
- `SPA_CLIENT_ID`: Application (Client) ID de la app SPA registrada en Entra

Mientras `SPA_CLIENT_ID` esté vacío, MSAL.js no se activa y la app
funciona sin auth (el worker está en soft mode).

## Worker email + PDF

- URL: `https://presupuestos-email.lucas-ivancevich.workers.dev`
- Endpoints: `/send`, `/pdf`, `/whoami`
- Código: `/Users/lucasivancevich/Documents/Presupuestador/worker-email/`
