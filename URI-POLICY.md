# Política de IRIs persistentes

Esta política describe cómo se identifican, versionan y resuelven los recursos de
la Ontología de Espacio Público de Medellín, conforme a las *Best Practice Recipes
for Publishing RDF Vocabularies* (W3C) y a las recomendaciones FAIR.

## 1. Base persistente

Todos los recursos se identifican bajo el dominio persistente **w3id.org**:

```
https://w3id.org/medellin/espacio-publico       ← ontología umbrella (identidad principal)
https://w3id.org/medellin/espacio-publico/<modulo>
https://w3id.org/medellin/espacio-publico/vocab/<vocabulario>
https://w3id.org/medellin/espacio-publico/shapes/<shape>
https://w3id.org/medellin/ep/...                ← vocabulario epmodeling complementario
```

w3id.org garantiza estabilidad a largo plazo independientemente del proveedor de
alojamiento. Se eligió frente al dominio institucional `datos.medellin.gov.co`
para asegurar resolución permanente y control comunitario del redireccionamiento.

## 2. Hash vs. slash

- Los **términos** (clases, propiedades) viven en namespaces *hash* por módulo,
  p. ej. `…/core#PersonaNatural`. Esto permite recuperar todo el módulo con una
  sola petición y evita una regla de redirección por término.
- Los **módulos, vocabularios y shapes** usan namespaces *slash*
  (`…/core`, `…/vocab/clase-venta`), que se negocian individualmente.

## 3. Versionado

- `owl:versionInfo` sigue **Versionado Semántico** (`MAJOR.MINOR.PATCH`).
- `owl:versionIRI` = `<IRI>/<versión>` (p. ej. `…/espacio-publico/0.1.0`).
- El IRI sin versión (`…/espacio-publico`) siempre apunta a la **última** versión.
- Cada release conserva sus artefactos versionados en `dist/`.

## 4. Negociación de contenido

El archivo `w3id/medellin/.htaccess` (registrado en
[perma-id/w3id.org](https://github.com/perma-id/w3id.org)) redirige con `303 See Other`:

| `Accept` | Recurso servido |
|---|---|
| `text/html` (navegador) | `docs/index.html` (documentación pyLODE) |
| `text/turtle` | `dist/espacio-publico-0.1.0.ttl` |
| `application/rdf+xml` | `dist/espacio-publico-0.1.0.rdf` |
| `application/ld+json` | `dist/espacio-publico-0.1.0.jsonld` |
| `application/n-triples` | `dist/espacio-publico-0.1.0.nt` |

El destino físico es **GitHub Pages**
(`https://medellin-datos.github.io/ep-ontology/`). Para cambiar de proveedor,
basta editar el `HOST` en el `.htaccess`.

## 5. Pasos para activar la resolución

1. Publicar este repositorio en GitHub y habilitar **GitHub Pages** (rama `main`, raíz).
2. Abrir un *pull request* a `perma-id/w3id.org` añadiendo `medellin/.htaccess`.
3. Verificar:
   ```bash
   curl -sIL -H "Accept: text/turtle" https://w3id.org/medellin/espacio-publico
   curl -sIL -H "Accept: text/html"   https://w3id.org/medellin/espacio-publico
   ```

## 6. Mientras la resolución no esté activa

El catálogo OASIS `ontologies/catalog-v001.xml` permite que Protégé y la OWLAPI
resuelvan los `owl:imports` a los archivos locales sin acceso a la red.
