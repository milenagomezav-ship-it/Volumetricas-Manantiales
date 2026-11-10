# Registro de volumétricas · Manantiales

App web instalable para registrar cada día la lectura **inicial** y **final** de las 9 volumétricas.
Consumo en m³ = (final − inicial) × factor (10 en general; 1 en V8, pozo profundo). La inicial de cada día
se precarga con la última final guardada.

## Publicar en Vercel (unos 15 minutos)

### 1. Subir el proyecto
**Opción A, con GitHub (recomendada):**
1. Crea un repositorio privado en GitHub y sube el contenido de esta carpeta.
2. En vercel.com: **Add New → Project** → importa el repositorio.
3. Deja *Framework Preset* en **Other**, sin comando de build. Pulsa **Deploy**.

**Opción B, con la terminal:**
```bash
npm i -g vercel
cd volumetricas-vercel
vercel login
vercel --prod
```

### 2. Crear la base de datos
En el proyecto de Vercel: **Storage → Create Database → Neon (Postgres)** y conéctala al proyecto.
Vercel crea la variable `DATABASE_URL` automáticamente. Las tablas se crean solas en el primer uso.

### 3. Definir los códigos de acceso
En **Settings → Environment Variables** crea estas dos variables (valores largos y distintos entre sí):

| Variable | Para qué sirve |
|---|---|
| `APP_PIN` | Código del personal de campo: registrar y consultar lecturas |
| `ADMIN_PIN` | Código de administración: además, cambiar nombres y factores |

Después, en **Deployments**, pulsa **Redeploy** para que se apliquen.

### 4. Llevar las lecturas ya registradas
Abre el **SQL Editor** de la base en Neon, pega el contenido de `db/migracion.sql` y ejecútalo.
Carga las lecturas del 1 y 2 de noviembre de 2026. Se puede repetir sin duplicar datos.

### 5. Instalarla en el celular
Abre la dirección que te dio Vercel, entra con tu nombre y el código de acceso y:
- **Android (Chrome):** menú ⋮ → *Instalar app* o *Añadir a pantalla de inicio*.
- **iPhone (Safari):** botón Compartir → *Añadir a pantalla de inicio*.

## Cómo funciona
- Cada persona escribe su nombre y el código al entrar; el nombre queda como autor de lo que guarda.
  El navegador recuerda la sesión en ese dispositivo.
- Los datos se leen del servidor al abrir, cada minuto y al volver a la app.
- Guardar requiere conexión. Si no hay señal, lo escrito se conserva en pantalla hasta poder guardar.
- Para cambiar un código: cambia la variable en Vercel y haz Redeploy. Todos los dispositivos pedirán el nuevo.

## Notas
- **Plan de Vercel:** el plan gratuito (Hobby) es para uso personal no comercial. Para uso de la empresa
  corresponde el plan Pro. Confirma las condiciones vigentes en vercel.com/pricing.
- **Seguridad:** el acceso es por código compartido. Es suficiente para una herramienta interna de campo, pero
  quien conozca el código puede registrar lecturas. No se publican nombres ni datos sin el código.
- **Cambiar un factor** recalcula también los días ya registrados.

## Estructura
```
public/          página, manifest, service worker e íconos
api/             funciones del servidor (auth, data, save, config)
db/migracion.sql esquema y lecturas ya registradas
vercel.json      configuración de despliegue
```
