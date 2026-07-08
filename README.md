# K&M Control | GitHub-ready

App financiera familiar para controlar disponible real, flujo proyectado, registro diario, cuentas, ingresos, presupuesto, gastos fijos, tarjetas/deudas, metas y reportes.

## Estructura

```text
km-control/
├─ index.html
├─ css/
│  └─ styles.css
├─ js/
│  ├─ config.js
│  ├─ storage.js
│  ├─ formulas.js
│  ├─ engine.js
│  ├─ validators.js
│  └─ app.js
└─ sql/
   └─ supabase_schema.sql
```

## Como funciona hoy

- `index.html` es la pagina principal.
- `css/styles.css` controla todo el diseño visual responsive.
- `js/storage.js` guarda datos en localStorage y tiene modo Supabase preparado.
- `js/formulas.js` concentra calculos financieros.
- `js/engine.js` pinta dashboard, tablas, tarjetas y reportes.
- `js/validators.js` valida formularios.
- `js/app.js` conecta eventos, formularios, edicion, eliminacion e importacion/exportacion.

## Modo local

Por defecto, `js/config.js` trae:

```js
cloud: {
  enabled: false
}
```

Eso significa que los datos se guardan en el navegador/dispositivo donde se use la pagina.

Importante: en modo local, Mau y Karla no comparten datos automaticamente. Para compartir datos en la misma liga, hay que activar Supabase.

## Publicar en GitHub Pages

### Opcion A: desde navegador

1. Entra a GitHub.
2. Crea un repositorio nuevo llamado `km-control`.
3. Sube todos los archivos de esta carpeta.
4. Ve a **Settings > Pages**.
5. En **Build and deployment**, selecciona:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/root`
6. Guarda.
7. GitHub generara una liga parecida a:

```text
https://TU_USUARIO.github.io/km-control/
```

### Opcion B: desde la computadora con Git

```bash
git init
git add .
git commit -m "K&M Control app inicial"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/km-control.git
git push -u origin main
```

Despues activa GitHub Pages desde **Settings > Pages**.

## Activar modo Supabase compartido

### 1. Crear proyecto en Supabase

Crea un proyecto en Supabase.

### 2. Ejecutar SQL

En Supabase, abre **SQL Editor** y ejecuta:

```text
sql/supabase_schema.sql
```

### 3. Crear usuarios

En Supabase Auth, crea usuarios para Mau y Karla.

### 4. Crear workspace

En SQL Editor:

```sql
insert into public.workspaces (name)
values ('K&M Control')
returning id;
```

Copia el `id` generado.

### 5. Agregar miembros

Busca los `id` de Mau y Karla en `auth.users` y registra miembros:

```sql
insert into public.workspace_members (workspace_id, user_id, role)
values ('WORKSPACE_ID_AQUI', 'USER_ID_MAU', 'owner');

insert into public.workspace_members (workspace_id, user_id, role)
values ('WORKSPACE_ID_AQUI', 'USER_ID_KARLA', 'editor');
```

### 6. Editar `js/config.js`

Cambia:

```js
cloud: {
  enabled: true,
  supabaseUrl: 'https://TU-PROYECTO.supabase.co',
  supabaseAnonKey: 'TU_ANON_KEY_PUBLICA',
  workspaceId: 'WORKSPACE_ID_AQUI',
  realtime: true
}
```

### 7. Subir cambios a GitHub

```bash
git add .
git commit -m "Activar Supabase compartido"
git push
```

Desde ese momento, Mau y Karla entran con usuario y contrasena y ven la misma informacion.

## Respaldo

Aunque uses Supabase, usa **Exportar respaldo JSON** de forma semanal. Es el seguro contra el caos operativo.
