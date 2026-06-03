# Lab-visualizaciones-DB

Repositorio para el lab de visualizaciones de datos - grupo 4 (Marketing).

Stack: **PostgreSQL 15** + **Metabase** vía Docker Compose.

credenciales Metabase:
    Correo: calificar@uvg.edu.gt
    Contraseña: secret123+ 

---

## Requisitos

- Docker y Docker Compose instalados

---

## Setup

### 1. Clonar el repositorio

```bash
git clone https://github.com/Pxdro-410/Lab-visualizaciones-DB.git
cd Lab-visualizaciones-DB
```

### 2. Crear archivo de variables de entorno

```bash
cp .env.example .env
```

Editar `.env` con las credenciales deseadas:
Se adjuntan por este medio por motivos academicos

```env
# Credenciales de PostgreSQL
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres_password
POSTGRES_DB=retailmax

# Puertos locales (Host)
POSTGRES_LOCAL_PORT=5432
METABASE_LOCAL_PORT=3007
```

### 3. Crear docker-compose.yml

```bash
cp docker-compose.example.yml docker-compose.yml
```

> `docker-compose.yml` está en `.gitignore`. Usar el `.example` como base.

### 4. Levantar los contenedores

```bash
docker compose up -d
```

Primera vez: PostgreSQL ejecuta automáticamente `init/01_DDL.sql` y `init/02_DATA.sql` para crear y poblar la base de datos.

### 5. Verificar que la DB tiene datos (opcional)

```bash
docker exec -it retailmax_db psql -U postgres -d retailmax -c "\dt"
```

---

## Acceder a Metabase

Abrir en el navegador: `http://localhost:3007` (o el puerto configurado en `METABASE_LOCAL_PORT`).

### Conectar Metabase a la base de datos

Al entrar por primera vez, Metabase pedirá configurar una fuente de datos. Usar:

| Campo    | Valor              |
|----------|--------------------|
| Tipo     | PostgreSQL         |
| Host     | `db`               |
| Puerto   | `5432`             |
| Base de datos | `retailmax`   |
| Usuario  | `postgres`         |
| Contraseña | `postgres_password` |

> El host es `db` (nombre del servicio en docker-compose), **no** `localhost`.

---

## Reiniciar desde cero

Para eliminar todos los datos y re-ejecutar los scripts de init:

```bash
docker compose down -v
docker compose up -d
```

El flag `-v` elimina los volúmenes persistentes de PostgreSQL.

---

### Link del video: https://youtu.be/oT_xfBCjhu8

## Estructura del proyecto

```
.
├── init/
│   ├── 01_DDL.sql          # Creación de tablas
│   └── 02_DATA.sql         # Datos de prueba
├── metabase-data/          # Persistencia de configuración de Metabase
├── .env                    # Variables de entorno (no versionado)
├── .env.example            # Plantilla de variables de entorno
├── docker-compose.yml      # Compose activo (no versionado)
└── docker-compose.example.yml  # Plantilla de compose
```