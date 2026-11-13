# Harem Club · Caja Puerta

Web para celulares: registro de entradas, pagos (efectivo/Yape/tarjeta), anulaciones con motivo, auditoría, cierre de caja y descarga a Excel (CSV).

## 1. Subir a GitHub Pages
1. Crea un repositorio en GitHub (ej. `harem-caja`).
2. Sube `index.html`, `manifest.json` y este `README.md` a la raíz.
3. En el repositorio: **Settings → Pages → Branch: `main` / carpeta `/ (root)` → Save**.
4. En 1–2 minutos tendrás el link: `https://TU-USUARIO.github.io/harem-caja/`
5. En el celular abre el link y usa **Menú ⋮ → Instalar app / Añadir a pantalla de inicio**.

## 2. Guardar los datos en la nube (compartido entre celulares)
GitHub Pages solo muestra la página; **no guarda datos**. Sin este paso, los registros quedan solo en el celular donde se hicieron.

1. Crea una cuenta gratis en https://supabase.com y un proyecto nuevo.
2. En **SQL Editor** ejecuta:

```sql
create table registros (
  id text primary key,
  kind text,
  data jsonb not null,
  created_at timestamptz default now()
);
alter table registros enable row level security;
create policy "acceso_app" on registros for all using (true) with check (true);
```

3. En **Project Settings → API** copia `Project URL` y la clave `anon public`.
4. En `index.html`, sección `CONFIG`, pega:
   `SUPABASE_URL: 'https://xxxx.supabase.co'` y `SUPABASE_KEY: 'tu-clave-anon'`.
5. Haz commit. El indicador de arriba dirá **Guardado en la nube**.

## 3. Cambiar usuarios, PIN y series
En `CONFIG.USUARIOS` de `index.html`. **Cambia los PIN de ejemplo (1234 y 1111) antes de usarlo.**

## Limitaciones de seguridad (importante)
- Los PIN están en el código de la página, así que sirven para ordenar el acceso del personal, no para frenar a alguien técnico. Para seguridad real, el siguiente paso es usar Supabase Auth con login por correo y políticas por usuario.
- La política `acceso_app` de arriba permite leer y escribir a quien tenga la clave anon. No publiques el link junto con la clave en lugares abiertos.
- La auditoría es registro de operaciones, pero un administrador técnico de la base de datos podría modificarla.
- Las boletas se numeran por serie de usuario (WA02, AD02…); dos celulares con el mismo usuario a la vez podrían repetir número. Usa un usuario por celular.
