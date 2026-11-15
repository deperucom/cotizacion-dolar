![Logo del proyecto](cotizacion-dolar_.png)
# Tipo de cambio referencial del dólar — API de DePeru.com

Repositorio informativo sobre el servicio de tipo de cambio referencial del dólar de [DePeru.com](https://www.deperu.com/tipo_cambio/) y su endpoint JSON. Reúne la URL, el formato de respuesta de ejemplo, las restricciones conocidas y ejemplos sencillos para consultar los datos desde distintas plataformas.

> **Este repositorio documenta el endpoint de DePeru.com; no es un servicio intermediario ni una API independiente.** El proveedor indica que esta API será actualizada próximamente y que el acceso requiere una IP peruana. Consulta la [página de tipo de cambio](https://www.deperu.com/tipo_cambio/) y el contacto del proveedor para información actualizada.

## Enlaces

- **Página de tipo de cambio:** https://www.deperu.com/tipo_cambio/
- **Página de API:** https://www.deperu.com/api/
- **Endpoint JSON:** https://deperu.com/api/rest/cotizaciondolar.json
- **Contacto de DePeru.com:** https://deperu.com/contacto

## ¿Qué ofrece el servicio?

El endpoint entrega una respuesta JSON con una cotización referencial del dólar, incluyendo valores de compra y venta, una referencia de dólares por euro y una fecha. La respuesta de ejemplo también incluye un aviso del proveedor sobre una próxima actualización de la API y su restricción geográfica.

### Características conocidas

| Propiedad | Detalle |
| --- | --- |
| Servicio | Tipo de cambio referencial dólares |
| Sitio | DePeru.com |
| Actualización del endpoint | El proveedor indica que la API se actualizará próximamente; no se especifica una frecuencia de actualización de los datos |
| Restricción geográfica | Solo IP peruana, según el mensaje de la respuesta |
| Límite de solicitudes | No especificado en la información disponible; consulta al proveedor |
| Formato observado | JSON |

Estas características reflejan la información disponible y el ejemplo proporcionado. El contenido, disponibilidad, políticas y formato pueden cambiar; verifica la respuesta del endpoint y la información publicada por el proveedor.

## Ejemplo de respuesta

```json
{
  "servicio": "Tipo de cambio referencial dolares",
  "sitio": "DePeru.com",
  "enlace": "https://www.deperu.com/tipo_cambio/",
  "Cotizacion": [
    {
      "Compra": 3.398,
      "Venta": 3.404
    }
  ],
  "importante": "Pronto actualizaremos esta API - solo IP peruana - deperu.com/contacto",
  "DolaresxEuro": 1.1052,
  "fecha": "14-11-2026"
}
```

El ejemplo es ilustrativo; los valores y la fecha pueden variar. El nombre de la propiedad `Cotizacion` usa mayúscula inicial y contiene una lista de objetos. `Compra` y `Venta` son valores numéricos en el ejemplo. Confirma la estructura que devuelve actualmente el endpoint antes de usarla en producción.

## Identifica tu aplicación con User-Agent

Se recomienda enviar un encabezado `User-Agent` personalizado en las solicitudes. Incluye el dominio o nombre de la aplicación que consulta los datos para que el proveedor pueda identificar el origen de las peticiones. Por ejemplo:

```http
User-Agent: GuiaPlaza.com_EuroxDolar/1.0
```

Sustituye `GuiaPlaza.com_EuroxDolar/1.0` por el dominio o nombre y versión de tu propio sitio o aplicación. No uses un identificador que no te pertenezca. Los ejemplos de la [guía técnica](docs/guia-tecnica.md) muestran cómo configurarlo.

## Consulta rápida

Prueba desde una IP peruana, cambiando el User-Agent por el de tu aplicación:

```bash
curl --fail --silent --show-error \
  --max-time 10 \
  --user-agent 'GuiaPlaza.com_EuroxDolar/1.0' \
  https://deperu.com/api/rest/cotizaciondolar.json
```

Para ejemplos con manejo de errores en PHP, Python, Node.js y Java, consulta la [guía técnica](docs/guia-tecnica.md).

## Recomendaciones de consumo

- Envía un `User-Agent` que identifique tu dominio o aplicación y su versión; reemplaza el ejemplo por tus propios datos.
- El mensaje del endpoint indica que el acceso es solo para IP peruana. Una aplicación alojada fuera de Perú podría no poder consultarlo directamente.
- No se ha especificado un límite de solicitudes ni una frecuencia de actualización de los datos en la información disponible. Consulta al proveedor y evita solicitudes innecesarias.
- Maneja errores de red, respuestas HTTP no exitosas y JSON inválido.
- Valida que `Cotizacion` sea una lista y que `Compra` y `Venta` tengan los tipos esperados antes de operar con ellos.
- Trata `fecha` como una cadena con formato `DD-MM-YYYY` en el ejemplo; verifica el formato real recibido.
- No uses el servicio para aplicaciones críticas ni asumas garantías de disponibilidad (SLA).

## Uso responsable y atribución

Al mostrar los datos, conserva una atribución visible a **DePeru.com** y, cuando corresponda, enlaza a https://www.deperu.com/tipo_cambio/. Antes de redistribuir los datos o incorporarlos a un producto, revisa las condiciones de uso y los derechos aplicables del proveedor. Este repositorio documenta el endpoint; no concede derechos sobre los datos ni garantiza el acceso.

## Documentación

- [Guía técnica de uso: cURL, PHP, Python, Node.js y Java](docs/guia-tecnica.md)
- [Descripción del servicio y estructura de la respuesta](docs/servicio.md)
- [Restricciones y buenas prácticas](docs/limitaciones.md)

## Aviso

Este proyecto es informativo. No es un proxy ni una garantía de disponibilidad del servicio. El proveedor indica que la API será actualizada próximamente, por lo que la URL, respuesta y políticas aquí descritas pueden cambiar sin previo aviso; confirma la información con DePeru.com antes de depender de ella.
