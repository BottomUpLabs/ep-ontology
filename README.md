# Ontología de Espacio Público de Medellín

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version](https://img.shields.io/badge/version-0.1.0-green.svg)](CHANGELOG.md)
[![Ontology](https://img.shields.io/badge/OWL-2-orange.svg)](https://www.w3.org/TR/owl2-overview/)

Ontología OWL modular y vocabularios SKOS para representar la **gestión del
espacio público de Medellín**: actores, trámites, resoluciones, roles, ciclos de
vida y los dominios de **regulación de vendedores informales**, **propaganda
electoral** y **control y aprovechamiento del espacio público**.

- **IRI persistente:** <https://w3id.org/medellin/espacio-publico>
- **Versión:** `0.1.0` (primera publicación abierta)
- **Licencia:** [CC BY 4.0](LICENSE)
- **Documentación HTML:** [`docs/index.html`](docs/index.html)

---

## 1. ¿Qué contiene?

| Métrica | Valor |
|---|---|
| Ontologías OWL (módulos) | 23 |
| Clases | 132 |
| Conceptos SKOS | 476 |
| Propiedades de objeto | 116 |
| Propiedades de datos | 388 |
| Tripletas (grafo integrado) | 10 060 |
| SHACL shapes (tripletas) | 1 546 |

## 2. Arquitectura

La ontología sigue una arquitectura de **dos capas** (núcleo abstracto + dominios):

```
ontologies/
├── espacio-publico.ttl                  ← UMBRELLA: importar SOLO este archivo
├── catalog-v001.xml                      ← catálogo OASIS (resolución offline en Protégé)
├── core/                                 ← núcleo abstracto reutilizable
│   ├── entidades-primer-orden.ttl        Lugar, Negocio, Familia, Vivienda
│   ├── persona-natural.ttl               PersonaNatural, MiembroFamiliar y caracterización
│   ├── institutional-core-entities.ttl   Tramite, Resolucion, ConceptoTecnico, Programa, PQRS
│   ├── roles-institucionales.ttl         patrón de rol: RolInstitucional / RolInferido
│   └── rule-names-vocabulary.ttl         vocabulario SKOS de reglas de calidad (DMP)
└── domains/
    ├── regulacion/                       vendedores informales (roles, trámites, permisos, módulos…)
    ├── propaganda_electoral/             propaganda electoral
    ├── control/                          control, aprovechamiento, puntos críticos (NUEVO en 0.1)
    ├── caracterizacion/ · familias/ · politica_publica/   dominios complementarios (ns ep:)

vocabularies/   ← vocabularios SKOS independientes
  tipologia-venta · clase-venta (NUEVO) · normograma (NUEVO) · estado-vendedor ·
  tipo-programa-institucional · propaganda-electoral-vocabularios

shapes/         ← validación SHACL (importar espacio-publico-shapes.ttl)
concepts/ · context/   ← documentación narrativa de soporte (insumo de modelado)
dist/           ← artefactos publicables (TTL, RDF/XML, JSON-LD, N-Triples)
docs/           ← documentación HTML (pyLODE)
tools/          ← scripts reproducibles de build
w3id/           ← configuración de redirección w3id.org
```

## 3. Espacios de nombres (namespaces)

| Prefijo | IRI | Contenido |
|---|---|---|
| `ep` | `https://w3id.org/medellin/espacio-publico#` | ontología umbrella |
| `core` | `https://w3id.org/medellin/espacio-publico/core#` | entidades base e institucionales |
| `role` | `https://w3id.org/medellin/espacio-publico/roles#` | patrón de rol |
| `reg` | `https://w3id.org/medellin/espacio-publico/regulacion#` | vendedores informales |
| `pe` | `https://w3id.org/medellin/espacio-publico/propaganda-electoral#` | propaganda electoral |
| `ctrl` | `https://w3id.org/medellin/espacio-publico/control#` | control y aprovechamiento |
| `vocab` | `https://w3id.org/medellin/espacio-publico/vocab/` | vocabularios SKOS |
| `ep*` (epd-…) | `https://w3id.org/medellin/ep/…` | dominios complementarios |

## 4. Cómo usarla

### Protégé
`File ▸ Open ▸ ontologies/espacio-publico.ttl`. El archivo `catalog-v001.xml`
resuelve los `owl:imports` a los archivos locales mientras los IRIs `w3id.org`
no estén activos en la web.

### Python (rdflib)
```python
from rdflib import Graph
g = Graph().parse("dist/espacio-publico-0.1.0.ttl", format="turtle")
print(len(g), "tripletas")
```

### SPARQL (ejemplo)
```sparql
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX vocab: <https://w3id.org/medellin/espacio-publico/vocab/>
SELECT ?modalidad ?label WHERE {
  ?modalidad skos:inScheme vocab:clase-venta ; skos:prefLabel ?label .
  FILTER(lang(?label) = "es")
}
```

### Validación SHACL
```bash
pyshacl -s shapes/espacio-publico-shapes.ttl -df turtle datos.ttl
```

## 5. Artefactos publicables (`dist/`)

| Archivo | Formato | Media type |
|---|---|---|
| `espacio-publico-0.1.0.ttl` | Turtle | `text/turtle` |
| `espacio-publico-0.1.0.rdf` | RDF/XML | `application/rdf+xml` |
| `espacio-publico-0.1.0.jsonld` | JSON-LD | `application/ld+json` |
| `espacio-publico-0.1.0.nt` | N-Triples | `application/n-triples` |
| `espacio-publico-shapes-0.1.0.ttl` | SHACL (Turtle) | `text/turtle` |

Las cuatro serializaciones de la ontología son equivalentes (10 060 tripletas, verificado por *round-trip*).

## 6. Reproducir el build

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install rdflib pyshacl pylode
python tools/build_release.py     # serializaciones + resumen
python tools/build_docs.py        # documentación HTML (pyLODE)
python tools/build_catalog.py     # catálogo OASIS para Protégé
python tools/validate.py          # validación y reporte QA
```

## 7. Publicación e IRIs persistentes

Ver [`URI-POLICY.md`](URI-POLICY.md). En resumen: los IRIs `https://w3id.org/medellin/…`
son la identidad permanente; se resuelven mediante negociación de contenido
(`w3id/medellin/.htaccess`) hacia GitHub Pages (`docs/` para HTML, `dist/` para RDF).

## 8. Buenas prácticas aplicadas (FAIR / OWL / RDF)

- IRIs persistentes y dereferenciables vía **w3id.org** + negociación de contenido (303).
- Metadatos de ontología completos: `owl:versionIRI`, `owl:versionInfo`,
  `vann:preferredNamespacePrefix/Uri`, `dcterms:publisher/creator/license/issued`,
  `dcterms:bibliographicCitation`, `bibo:status`, palabras clave DCAT.
- Etiquetas y definiciones bilingües (es/en) y `rdfs:isDefinedBy` en términos nuevos.
- Reutilización de vocabularios estándar: Dublin Core, FOAF, schema.org, SKOS, PROV, DCAT, VoID, ORG, BIBO, VANN.
- Múltiples serializaciones + documentación HTML + descripción DCAT/VoID.
- Validación sintáctica (todos los `.ttl` parsean) y SHACL.

## 9. Citar

> Subsecretaría de Espacio Público de Medellín (2026). *Ontología de Espacio
> Público de Medellín*, versión 0.1.0. <https://w3id.org/medellin/espacio-publico>.
> Licencia CC BY 4.0.

## 10. Estado y limitaciones

`0.1.0` es una **primera publicación**. Ver [`CHANGELOG.md`](CHANGELOG.md) y la
sección *Limitaciones conocidas* de [`VALIDATION.md`](VALIDATION.md). Cómo
contribuir: [`CONTRIBUTING.md`](CONTRIBUTING.md).

## 11. Licencia

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).
© 2026 Alcaldía de Medellín — Subsecretaría de Espacio Público.
