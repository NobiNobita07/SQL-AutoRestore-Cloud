# SQL AutoRestore Cloud - base real en Neon

Demo de backup y restauración automática con Render + Neon PostgreSQL.

## Arquitectura

- `neondb`: base de control. Conserva `cloud_backup` y `cloud_events`.
- `bd_gestionbackups`: base real de demostración. Contiene `public.cliente`.

El botón **Eliminar base real (prueba)** guarda primero el backup actual y luego ejecuta un `DROP DATABASE bd_gestionbackups WITH (FORCE)`. En el siguiente ciclo, el backend detecta que la base ya no existe, la crea nuevamente y restaura sus datos desde `neondb.cloud_backup`.

## Variables en Render

- `DATABASE_URL`: URL de conexión a `neondb` en Neon.
- `DEMO_TOKEN`: clave para las acciones de demostración.
- `INTERVAL_SECONDS`: opcional, por defecto 60.
- `APP_DB_NAME`: opcional, por defecto `bd_gestionbackups`.

## Flujo de demostración

1. Abrir el frontend y Neon > Databases.
2. Confirmar que existen `neondb` y `bd_gestionbackups`.
3. En el frontend, pulsar **Eliminar base real (prueba)**.
4. Refrescar Neon: `bd_gestionbackups` debe desaparecer.
5. Esperar el siguiente ciclo o pulsar **Ejecutar ciclo ahora**.
6. Refrescar Neon: `bd_gestionbackups` debe reaparecer.
7. El frontend vuelve a mostrar los clientes restaurados.

> No elimines `neondb`: ahí vive la copia de seguridad que permite recrear la base de demostración.
