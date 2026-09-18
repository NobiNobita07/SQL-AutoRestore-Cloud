# Informe breve - Restauración automática de una base real en Neon

El sistema utiliza dos bases dentro del mismo proyecto Neon. `neondb` funciona como base de control y almacena una copia lógica única de los datos, además del historial de eventos. La base `bd_gestionbackups` representa la base operativa que puede eliminarse durante la demostración.

Antes de eliminar la base operativa, el backend actualiza el backup en `neondb`. Después ejecuta la eliminación real de `bd_gestionbackups`. El servicio de Render revisa periódicamente si la base existe. Si detecta su ausencia, crea nuevamente `bd_gestionbackups`, reconstruye la tabla `cliente` y restaura los registros desde la copia guardada.

De esta manera se puede demostrar en la consola de Neon que la base desaparece y luego vuelve a aparecer automáticamente, mientras el backup permanece protegido en una base separada.
