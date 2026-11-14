# Guía técnica de uso

Esta guía muestra cómo consultar el endpoint desde una aplicación. La URL documentada es:

```text
https://deperu.com/api/rest/cotizaciondolar.json
```

El mensaje incluido en la respuesta de ejemplo indica que el acceso es solo para IP peruana y que la API será actualizada próximamente. No se especifica una frecuencia de actualización de los datos ni un límite de solicitudes. Consulta al proveedor para conocer las políticas vigentes. Ejecuta los ejemplos desde un entorno autorizado y evita llamadas innecesarias.

## User-Agent personalizado

Identifica las solicitudes con un encabezado `User-Agent` que indique el dominio o nombre de la aplicación cliente y, de ser posible, su versión. Esto ayuda al proveedor a reconocer el origen de las consultas. Por ejemplo:

```http
User-Agent: GuiaPlaza.com_EuroxDolar/1.0
```

En los ejemplos siguientes se usa ese valor. **Reemplázalo por el dominio o nombre de tu propio sitio o aplicación**; no copies un identificador que no te pertenezca.

## cURL

```bash
curl --fail --silent --show-error \
  --max-time 10 \
  --user-agent 'GuiaPlaza.com_EuroxDolar/1.0' \
  https://deperu.com/api/rest/cotizaciondolar.json
```

`--user-agent` configura el encabezado `User-Agent`. `--fail` indica error para respuestas HTTP no exitosas y `--max-time` limita el tiempo de espera del cliente.

## Python

Requiere Python 3.8 o posterior. Usa solo la biblioteca estándar:

```python
import json
from urllib.error import HTTPError, URLError
from urllib.request import Request, urlopen

URL = "https://deperu.com/api/rest/cotizaciondolar.json"
USER_AGENT = "GuiaPlaza.com_EuroxDolar/1.0"  # Cámbialo por el dominio/nombre de tu app
request = Request(
    URL,
    headers={
        "Accept": "application/json",
        "User-Agent": USER_AGENT,
    },
)

try:
    with urlopen(request, timeout=10) as response:
        data = json.loads(response.read().decode("utf-8"))
except HTTPError as error:
    raise SystemExit(f"El servidor respondió HTTP {error.code}") from error
except (URLError, TimeoutError) as error:
    raise SystemExit(f"No se pudo consultar el servicio: {error}") from error
except json.JSONDecodeError as error:
    raise SystemExit(f"La respuesta no contiene JSON válido: {error}") from error

print("Servicio:", data.get("servicio", "sin nombre"))
print("Sitio:", data.get("sitio", "sin sitio"))
print("Fecha:", data.get("fecha", "sin fecha"))

cotizaciones = data.get("Cotizacion", [])
if isinstance(cotizaciones, list):
    for cotizacion in cotizaciones:
        if not isinstance(cotizacion, dict):
            continue
        compra = cotizacion.get("Compra")
        venta = cotizacion.get("Venta")
        print(f"Compra: {compra}" if compra is not None else "Compra: no disponible")
        print(f"Venta: {venta}" if venta is not None else "Venta: no disponible")

if "DolaresxEuro" in data:
    print("Dólares por euro:", data["DolaresxEuro"])
if data.get("importante"):
    print("Aviso:", data["importante"])
```

## PHP

Requiere PHP con la extensión cURL habilitada:

```php
<?php
$url = 'https://deperu.com/api/rest/cotizaciondolar.json';
$userAgent = 'GuiaPlaza.com_EuroxDolar/1.0'; // Cámbialo por el dominio/nombre de tu app
$curl = curl_init($url);

curl_setopt_array($curl, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_CONNECTTIMEOUT => 5,
    CURLOPT_TIMEOUT => 10,
    CURLOPT_USERAGENT => $userAgent,
    CURLOPT_HTTPHEADER => ['Accept: application/json'],
]);

$body = curl_exec($curl);
$status = curl_getinfo($curl, CURLINFO_HTTP_CODE);
$error = curl_error($curl);
curl_close($curl);

if ($body === false) {
    throw new RuntimeException("Error de conexión: {$error}");
}
if ($status < 200 || $status >= 300) {
    throw new RuntimeException("El servidor respondió HTTP {$status}");
}

$data = json_decode($body, true);
if (!is_array($data) || json_last_error() !== JSON_ERROR_NONE) {
    throw new RuntimeException('La respuesta no contiene JSON válido.');
}

printf("Servicio: %s\n", $data['servicio'] ?? 'sin nombre');
printf("Fecha: %s\n", $data['fecha'] ?? 'sin fecha');
foreach (($data['Cotizacion'] ?? []) as $cotizacion) {
    if (!is_array($cotizacion)) {
        continue;
    }
    printf("Compra: %s\n", $cotizacion['Compra'] ?? 'no disponible');
    printf("Venta: %s\n", $cotizacion['Venta'] ?? 'no disponible');
}
if (isset($data['DolaresxEuro'])) {
    printf("Dólares por euro: %s\n", $data['DolaresxEuro']);
}
if (!empty($data['importante'])) {
    printf("Aviso: %s\n", $data['importante']);
}
```

## Node.js

Requiere Node.js 18 o posterior, que incluye `fetch`:

```js
const url = "https://deperu.com/api/rest/cotizaciondolar.json";
const userAgent = "GuiaPlaza.com_EuroxDolar/1.0"; // Cámbialo por el dominio/nombre de tu app

async function main() {
  let response;
  try {
    response = await fetch(url, {
      headers: {
        Accept: "application/json",
        "User-Agent": userAgent,
      },
      signal: AbortSignal.timeout(10_000),
    });
  } catch (error) {
    throw new Error(`No se pudo consultar el servicio: ${error.message}`);
  }

  if (!response.ok) {
    throw new Error(`El servidor respondió HTTP ${response.status}`);
  }

  let data;
  try {
    data = await response.json();
  } catch {
    throw new Error("La respuesta no contiene JSON válido.");
  }

  console.log(`Servicio: ${data.servicio ?? "sin nombre"}`);
  console.log(`Fecha: ${data.fecha ?? "sin fecha"}`);
  for (const cotizacion of data.Cotizacion ?? []) {
    if (!cotizacion || typeof cotizacion !== "object" || Array.isArray(cotizacion)) continue;
    console.log(`Compra: ${cotizacion.Compra ?? "no disponible"}`);
    console.log(`Venta: ${cotizacion.Venta ?? "no disponible"}`);
  }
  if (data.DolaresxEuro !== undefined) {
    console.log(`Dólares por euro: ${data.DolaresxEuro}`);
  }
  if (data.importante) console.log(`Aviso: ${data.importante}`);
}

main().catch((error) => {
  console.error(error.message);
  process.exitCode = 1;
});
```

> Algunos entornos gestionados pueden controlar o restringir encabezados de solicitud. Si tu runtime no permite establecer `User-Agent` con `fetch`, utiliza un cliente HTTP que permita configurarlo y revisa la documentación de ese entorno.

## Java

Ejemplo para Java 11 o posterior usando `HttpClient`:

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

public class CotizacionDolar {
    public static void main(String[] args) throws Exception {
        var client = HttpClient.newBuilder()
                .connectTimeout(Duration.ofSeconds(5))
                .build();
        var request = HttpRequest.newBuilder()
                .uri(URI.create("https://deperu.com/api/rest/cotizaciondolar.json"))
                .timeout(Duration.ofSeconds(10))
                .header("Accept", "application/json")
                .header("User-Agent", "GuiaPlaza.com_EuroxDolar/1.0") // Sustituye por tu dominio/app
                .GET()
                .build();

        var response = client.send(request, HttpResponse.BodyHandlers.ofString());
        if (response.statusCode() < 200 || response.statusCode() >= 300) {
            throw new IllegalStateException("El servidor respondió HTTP " + response.statusCode());
        }
        // La respuesta es JSON. Usa una biblioteca JSON (por ejemplo, Jackson o Gson)
        // para analizarla y contempla los campos Cotizacion, DolaresxEuro y fecha.
        System.out.println(response.body());
    }
}
```

## Manejo de respuesta y errores

- Comprueba el código HTTP antes de interpretar el cuerpo.
- Usa un timeout para que una conexión lenta no bloquee indefinidamente tu aplicación.
- Valida que el JSON tenga la estructura esperada y que `Cotizacion` sea una lista de objetos.
- Comprueba la existencia y el tipo de `Compra`, `Venta`, `DolaresxEuro` y `fecha` antes de procesarlos.
- El proveedor indica que el acceso es solo desde IP peruana. Si recibes una respuesta de bloqueo, verifica que tu entorno cumpla las condiciones y contacta al proveedor; no intentes evadir restricciones geográficas o de acceso.
- Usa un `User-Agent` que identifique tu dominio o aplicación, en lugar de dejar un identificador genérico o copiar el ejemplo sin adaptarlo.

## Frecuencia y caché

La información disponible no especifica cada cuánto se actualizan las cotizaciones ni el límite de solicitudes. Consulta a DePeru.com para conocer esos detalles y, mientras tanto, evita solicitudes frecuentes o innecesarias. Los ejemplos son demostrativos y no incluyen una biblioteca de caché.
