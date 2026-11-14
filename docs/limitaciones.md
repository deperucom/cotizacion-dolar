# Restricciones y políticas del servicio

## Acceso geográfico

El campo `importante` de la respuesta de ejemplo indica que el acceso es solo para IP peruana. Una aplicación alojada fuera de Perú puede no poder consultar directamente el endpoint. Si tienes dudas sobre acceso autorizado, consulta a DePeru.com en https://deperu.com/contacto.

## Actualización de la API y de los datos

El mensaje del proveedor dice que la API se actualizará próximamente. Esto se refiere a una posible actualización de la API; no se especifica la frecuencia con que se actualizan las cotizaciones. Confirma la información vigente con el proveedor antes de depender del servicio.

## Límite de solicitudes

No se ha especificado un rate limit en la información disponible. Evita solicitudes innecesarias y pregunta al proveedor por los límites autorizados antes de ejecutar integraciones con alto volumen.

## Buenas prácticas

- Validar y manejar errores de conexión, respuestas HTTP y JSON.
- Evitar polling frecuente y no crear loops de consultas sin pausa.
- Consultar al proveedor sobre permisos, frecuencia de actualización y límites de uso.
- Si el servicio se bloquea, confirmar que la IP de salida esté dentro de las condiciones permitidas; no intentar evadir restricciones.
- No asumir SLA ni garantías de disponibilidad.

## Importante

Esta documentación es informativa y se basa en la respuesta de ejemplo proporcionada. La disponibilidad, la política de acceso, los límites y la estructura pueden cambiar. La información oficial del servicio corresponde a DePeru.com.
