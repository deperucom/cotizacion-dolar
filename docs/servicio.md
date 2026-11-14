# Descripción del servicio

## Resumen

El servicio de tipo de cambio referencial de DePeru.com expone un endpoint JSON con valores de compra y venta del dólar. La respuesta de ejemplo también incluye una referencia de dólares por euro, una fecha y un mensaje informativo del proveedor.

## Enlaces

- Página: https://www.deperu.com/tipo_cambio/
- Endpoint: https://deperu.com/api/rest/cotizaciondolar.json
- Contacto: https://deperu.com/contacto

## Propiedades conocidas

- Nombre: Tipo de cambio referencial dólares
- Sitio: DePeru.com
- Formato observado: JSON
- Restricción indicada por el proveedor: solo IP peruana
- Estado informado: el proveedor indica que actualizará la API próximamente
- Frecuencia de actualización de cotizaciones y límite de solicitudes: no especificados en la información disponible

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

## Estructura observada

| Campo | Tipo de ejemplo | Descripción |
| --- | --- | --- |
| `servicio` | Cadena | Nombre del servicio |
| `sitio` | Cadena | Sitio que proporciona la información |
| `enlace` | Cadena (URL) | Página de tipo de cambio |
| `Cotizacion` | Lista de objetos | Contiene los valores de compra y venta |
| `Cotizacion[].Compra` | Número | Cotización de compra del ejemplo |
| `Cotizacion[].Venta` | Número | Cotización de venta del ejemplo |
| `importante` | Cadena | Mensaje informativo y de acceso |
| `DolaresxEuro` | Número | Valor de referencia del ejemplo |
| `fecha` | Cadena | Fecha en formato `DD-MM-YYYY` en el ejemplo |

El ejemplo describe el formato proporcionado; no garantiza que todos los campos permanezcan iguales. Valida la respuesta recibida y contempla campos faltantes o cambios en la API.

## Consideraciones

- Los valores de compra y venta deben interpretarse como datos referenciales de acuerdo con la fuente.
- El proveedor indica que el endpoint solo admite IP peruana.
- El mensaje indica que la API se actualizará próximamente; su estructura podría cambiar.
- No se indica una frecuencia de actualización de datos ni un límite de solicitudes. Consulta al proveedor.
