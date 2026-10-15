# Odoo para demos

Entorno de demostración de Odoo 18 con Docker Compose y PostgreSQL. Los datos se guardan en directorios del proyecto para conservarlos al detener o recrear los contenedores.

> Este entorno está pensado para demos locales; no está endurecido para producción.

## Requisitos

- Docker instalado y en ejecución.
- Docker Compose v2 (comando `docker compose`).

## Preparación inicial

Desde la raíz del proyecto, prepara los archivos locales de entorno y configuración:

```bash
cp env_example .env
cp config/odoo.conf.example config/odoo.conf
mkdir -p addons
```

Edita `config/odoo.conf` y cambia `admin_passwd` por una contraseña maestra propia. Esta contraseña se usa para operaciones administrativas de bases de datos en Odoo; no es la contraseña del usuario de Odoo. No compartas ni publiques los archivos locales `.env` y `config/odoo.conf`.

El archivo `.env` de ejemplo conecta Odoo con el servicio PostgreSQL definido en Compose (`demo-db`). Si cambias las credenciales allí, usa los mismos valores para las variables de conexión de Odoo y las variables `POSTGRES_*`.

## Iniciar Odoo

Levanta Odoo y PostgreSQL en segundo plano:

```bash
docker compose up -d
```

Comprueba que los contenedores estén activos y consulta los registros si lo necesitas:

```bash
docker compose ps
docker compose logs -f demo-odoo
```

Abre [http://localhost:8369](http://localhost:8369). En el primer inicio, crea una base de datos desde la pantalla de Odoo. Usa la contraseña maestra configurada en `config/odoo.conf` cuando Odoo la solicite y define allí las credenciales del usuario administrador de la base de datos.

## Detener el entorno

Para detener y eliminar los contenedores sin borrar los datos persistidos:

```bash
docker compose down
```

Los datos de PostgreSQL y Odoo permanecen en `odoo-db-data/` y `odoo-web-data/`, respectivamente. Para volver a iniciar el entorno, ejecuta `docker compose up -d`.

## Puertos

- Odoo: `http://localhost:8369` (puerto interno 8069).
- Longpolling de Odoo: puerto local `8372` (puerto interno 8072).
- PostgreSQL: puerto local `5434` (puerto interno 5432).