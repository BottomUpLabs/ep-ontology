# Reporte de Validación — v0.1.0

Ejecutar: `python tools/validate.py` (resultado abajo, todos los checks en verde).

## Resultados

| # | Verificación | Resultado |
|---|---|---|
| 1 | Todos los archivos `.ttl` (39) parsean (Turtle) | ✅ PASS |
| 2 | No quedan IRIs legados (`datos.medellin.gov.co` / `vocab.epmodeling.org`) | ✅ PASS |
| 3 | No hay `owl:imports` colgantes (todos resuelven a una ontología declarada) | ✅ PASS |
| 4 | Las 29 ontologías declaran `owl:versionInfo` + `dcterms:license` | ✅ PASS |
| 5 | Las serializaciones hacen *round-trip* (10 060 tripletas en TTL/RDF-XML/JSON-LD/N-Triples) | ✅ PASS |
| 6 | Las SHACL shapes cargan y son meta-válidas (52 NodeShapes) | ✅ PASS |
| 7 | La instancia de ejemplo conforma a las SHACL shapes (validación de datos real) | ✅ PASS |

**7/7 verificaciones superadas.**

## Métricas del release

| | |
|---|---|
| Ontologías OWL (módulos) | 23 |
| Clases | 132 |
| Conceptos SKOS | 476 |
| Propiedades de objeto / datos / anotación | 116 / 388 / 23 |
| Tripletas (grafo de ontología integrado) | 10 060 |
| Tripletas (grafo de SHACL shapes) | 1 688 |
| NodeShapes SHACL | 52 |
| Restricciones `sh:sparql` con `sh:prefixes` | 56/56 |

## Herramientas

- **rdflib 7.6** — *parsing*, serialización, *round-trip*.
- **pySHACL** — meta-validación de shapes y validación de la instancia de ejemplo.
- **pyLODE 3.4** — documentación HTML.

## Validaciones realizadas

1. **Sintaxis Turtle**: los 39 archivos `.ttl` parsean sin error. Se corrigieron 3
   errores de sintaxis preexistentes (ver `CHANGELOG.md`).
2. **Integridad de IRIs**: 0 referencias a los hosts legados; todos los recursos
   bajo `https://w3id.org/medellin/…`.
3. **Cierre de imports**: los 21 destinos de `owl:imports` resuelven a una de las
   29 ontologías declaradas. El catálogo OASIS (`ontologies/catalog-v001.xml`)
   permite la resolución offline en Protégé.
4. **Metadatos**: todas las ontologías declaran versión y licencia; la umbrella
   incluye el conjunto FAIR completo (versionIRI, vann, publisher, cite-as, keywords).
5. **Serializaciones**: las cuatro representaciones contienen exactamente las mismas
   10 060 tripletas (sin pérdida; se usó el serializador `xml` y no `pretty-xml`,
   que descartaba axiomas con listas RDF).
6. **SHACL**: las 52 NodeShapes son meta-válidas; las 56 restricciones `sh:sparql`
   declaran sus prefijos; la instancia `examples/ejemplo-vendedor.ttl` conforma.

## Limitaciones conocidas (para versiones futuras)

- **Consistencia OWL DL**: la validación estructural y SHACL pasa; se recomienda
  además verificar la consistencia lógica con un razonador DL (HermiT/Pellet) en
  Protégé. rdflib no realiza razonamiento DL.
- **Doble núcleo**: coexisten dos vocabularios núcleo (`core:` de espacio-publico y
  `ep:`/`epd:` de los dominios caracterizacion/familias/politica_publica). Ambos son
  válidos y persistentes bajo w3id; su consolidación se evaluará en 0.2.
- **Vocabularios `actividad-prohibida` / `zona-restringida`**: listas **preliminares
  y no exhaustivas**; requieren confirmación con el normograma completo
  (ver `concepts/ToDo.md`).
- **Cobertura de `vann`**: presente en la umbrella y los módulos con namespace por
  defecto; los módulos `ep:` (prefijo nombrado) se completarán en 0.2.
