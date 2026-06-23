# Registro en w3id.org

Esta carpeta contiene la configuración de redirección para los IRIs persistentes
`https://w3id.org/medellin/…`.

## Cómo activarla

1. Haz *fork* de [perma-id/w3id.org](https://github.com/perma-id/w3id.org).
2. Copia `medellin/.htaccess` (de esta carpeta) a `medellin/.htaccess` en tu *fork*.
3. Ajusta, si es necesario, el `HOST` del `.htaccess` (por defecto apunta a
   `https://medellin-datos.github.io/ep-ontology`).
4. Abre un *pull request* al repositorio `perma-id/w3id.org`.

Una vez aceptado, los IRIs `https://w3id.org/medellin/espacio-publico…` resolverán
con negociación de contenido (HTML para navegadores, RDF para clientes de datos).

## Verificación

```bash
curl -sIL -H "Accept: text/turtle" https://w3id.org/medellin/espacio-publico
curl -sIL -H "Accept: text/html"   https://w3id.org/medellin/espacio-publico
```
