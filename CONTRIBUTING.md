# Cómo contribuir

Gracias por tu interés en mejorar la Ontología de Espacio Público de Medellín.

## Flujo de trabajo

1. Abre un *issue* describiendo el cambio (nuevo término, corrección, alineación).
2. Crea una rama y edita los archivos `.ttl` fuente (no edites `dist/` ni `docs/`,
   que son generados).
3. Ejecuta los scripts de build y validación antes de proponer el cambio:
   ```bash
   python tools/build_release.py
   python tools/build_catalog.py
   python tools/build_docs.py
   python tools/validate.py
   ```
4. Asegúrate de que `tools/validate.py` reporte **0 errores de parsing** y
   **0 imports colgantes**.
5. Abre un *pull request* enlazando el *issue*.

## Convenciones de modelado

- **Nomenclatura:** clases en `UpperCamelCase`, propiedades en `lowerCamelCase`,
  conceptos SKOS en `kebab-case`.
- **Etiquetas:** toda clase/propiedad nueva debe tener `rdfs:label`@es y @en, y una
  definición (`skos:definition` o `rdfs:comment`).
- **Procedencia:** usa `rdfs:isDefinedBy` apuntando al IRI de la ontología y
  `dcterms:source` para citar la norma o el documento de `concepts/`/`context/`.
- **Reutiliza** vocabularios estándar (Dublin Core, SKOS, FOAF, schema.org, PROV)
  antes de crear términos nuevos.
- **IRIs:** usa siempre la base `https://w3id.org/medellin/…`. Si migras IRIs
  legados, usa `tools/migrate_iris.py`.
- **Metadatos de módulo:** todo `owl:Ontology` nuevo debe declarar `owl:versionInfo`,
  `owl:versionIRI`, `dcterms:title/description/license/publisher` y
  `vann:preferredNamespacePrefix/Uri`.

## Versionado

Sigue [SemVer](https://semver.org/lang/es/). Cambios incompatibles → *major*;
términos nuevos compatibles → *minor*; correcciones → *patch*. Actualiza
`CHANGELOG.md` y los `owl:versionInfo`/`owl:versionIRI`.

## Documentación narrativa

Los directorios `concepts/` y `context/` contienen la base conceptual. Si tu cambio
formaliza un concepto narrativo, enlaza el término con su documento mediante
`dcterms:source`.
