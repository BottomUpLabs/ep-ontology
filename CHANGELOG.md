# Changelog

Todas las versiones notables de la Ontología de Espacio Público de Medellín.
Formato basado en [Keep a Changelog](https://keepachangelog.com/) y
[Versionado Semántico](https://semver.org/lang/es/).

## [Unreleased]

### Added (añadido)
- **Dominio `gestiones/gestiones-core.ttl`**: ontología de las gestiones
  operativas de los equipos (fuente `trx_gestiones.xlsx`, app Gestiones) y sus
  evidencias. Define `gestion:Gestion` (sub de `prov:Activity`), **una clase por
  equipo** (`Equipo` + individuos Social, Regulaciones, AEEP, Territorio, PEV y
  las agrupadoras `GestionSocial`…`GestionPEV`), **una clase por tipo de
  gestión** (23 actividades: VisitaAdministrativa, EstudioSocioeconomico,
  ConceptoSocioeconomico, SolicitudIngreso, ResolucionAEEP, etc.), la jerarquía
  `Evidencia` (`EvidenciaArchivo`, `EvidenciaFotografica`, `ListadoAsistencia`,
  sub de `prov:Entity`), el rol `Gestor` y la clase `LugarActividad`
  (tercera especialización concreta de `core:Lugar`). Cada clase hoja declara
  con restricciones OWL sus conexiones explícitas a `core:PersonaNatural`
  (`atiendeVendedor`, `tieneSolicitante`), `core:PuestoVenta` (`enPuestoVenta`,
  `puestoVentaAnterior`), `core:Vivienda` (`enLugarResidencia`), `core:Negocio`
  (`involucraEstablecimiento`) y `LugarActividad` (`enLugarActividad`), según la
  tabla `columnas_relacionamiento` de la fuente.
- **Crosswalk `mappings/gestiones_columnas-fuente_ontologia.csv`** (254 columnas
  de 23 actividades → término ontológico), generado por
  `tools/build_gestiones_mapping.py`.
- Crosswalk de gestiones extendido con la **tercera pata a `ep_raw_data`**
  (columnas `tabla_raw`/`columna_raw`/`nota_raw`; inspección 2026-06-12 de los
  esquemas `cruces_interinstitucionales`, `concepto_socioeconomico`,
  `evidencia_social_v1`, `actas_traslado`, `actos_regulacion`, `acta_reunion`,
  `registro_vendedores`, `remision_oferta_interinstitucional`): informes
  mensuales por estándar, espejo `evidencia_social_v1.gestiones`, histórico
  `gestiones_2`/`v1` (discriminador `gestion_tipo`), tablas atípicas
  (`bd_tras_ago` = visitas de seguimiento, `bd_reg_vend` = BD histórica,
  `actividades_*` = seguimiento por dependencia, tablas por entidad del cruce)
  y 134 columnas solo-raw mapeadas. Convenciones en `mappings/README.md`.
- **Crosswalk de gestiones anclado en `gestiones`/`gestiones_2`** (verificación
  contra `ep_raw_data` en vivo, 2026-06-14): el espejo `evidencia_social_v1.gestiones`
  resultó contener 18 tipos de gestión (equipos Social **y** Regulaciones, no
  solo Social) → `tabla_raw` ahora apunta al espejo en 166 filas app; el informe
  mensual dedicado y el equivalente histórico en `gestiones_2` pasaron a
  `nota_raw`. Las 120 filas solo-raw de otros esquemas se movieron a
  `mappings/gestiones_solo-raw_otros-esquemas.csv`. Corregido además el bug que
  metía etiquetas entre paréntesis en `actividad_nombre` de las filas solo-raw.
- **Fix: resultados del cruce interinstitucional apuntaban a tablas vacías**
  (2026-06-15). Las columnas por entidad (sisbén/salud/pensión/RUNT/SIMIT/…) se
  mapeaban a `cruces_interinstitucionales.{adres,eps,…}` (estructuras destino con
  0 filas); el dato real está en `cruces_interinstitucionales_consultas.*`
  (83–165 filas por entidad, verificado en `ep_raw_data`). Repuntadas las 81
  filas a la actividad `Cruces interinstitucionales` → `core:PersonaNatural`, y
  añadidas las columnas reales que faltaban (`runt.solicitudes`,
  `supernotariado.ciudad`/`inmuebles`). Todo el CSV de otros esquemas quedó
  validado columna a columna contra la base.
- **Principal sin filas vacías**: las 89 columnas de la app de clases
  AEEP/Compra/Territorio cuyo dato vive en esquema dedicado sin curar
  (`resoluciones_aeep_v1`, `pqrsd`, app `solicitudes`) se movieron del crosswalk
  principal a `mappings/gestiones_columnas-pendientes_otros-esquemas.csv`. El
  principal (180 filas) queda 100% anclado en `gestiones`/`gestiones_2`, sin
  ninguna fila con `tabla_raw` vacío.
- Import del módulo de gestiones en la umbrella `espacio-publico.ttl`.

## [0.1.0] — 2026-05-31

Primera publicación abierta de la ontología siguiendo buenas prácticas FAIR/OWL/RDF.

### Added (añadido)
- **IRIs persistentes** bajo `https://w3id.org/medellin/…` con configuración de
  redirección y negociación de contenido (`w3id/medellin/.htaccess`).
- **Módulo `core/roles-institucionales.ttl`** que formaliza el patrón de rol
  (`role:RolInstitucional`, `role:RolInferido`, `role:basadoEn`, `role:estadoActual`,
  `role:esRolActivo`, `role:heredaEstadoDe`, `role:subroganteRol`, etc.), previamente
  referenciado pero no definido.
- **Dominio `control/control-espacio-publico.ttl`**: `IntervencionEspacioPublico`,
  `AprovechamientoEconomico`, `OfertaInstitucional`, `PuntoCritico`, y vocabularios
  controlados de **causales de novedad**, **tipos de concepto técnico**,
  **actividades prohibidas** y **zonas restringidas** (responde a `concepts/ToDo.md`).
- **Vocabulario `clase-venta`** (modalidad de ocupación: estacionario,
  semi-estacionario, ambulante, itinerante, ocasional) con sinónimos para normalización.
- **Vocabulario `normograma`** (marco normativo: leyes, acuerdos, decretos y
  procedimientos internos).
- Alias de compatibilidad `core:Persona` ≡ `core:PersonaNatural`.
- Metadatos FAIR en todos los módulos: `owl:versionIRI`, `vann:preferredNamespacePrefix/Uri`,
  `dcterms:publisher/license/issued`, `bibo:status`; agente publicador `org:Organization`.
- Sinónimos (`skos:altLabel`/`hiddenLabel`) en `core:VendedorInformal` derivados de
  la documentación narrativa (`concepts/`).
- Artefactos publicables (`dist/`): Turtle, RDF/XML, JSON-LD, N-Triples + SHACL.
- Documentación HTML generada con pyLODE (`docs/index.html`).
- Descripción **DCAT + VoID** (`dcat.ttl`) y catálogo OASIS (`ontologies/catalog-v001.xml`).
- Scripts reproducibles de build (`tools/`).

### Changed (modificado)
- **Migración de espacios de nombres** `http://datos.medellin.gov.co/…` y
  `http://vocab.epmodeling.org/…` → `https://w3id.org/medellin/…` (1 559 reemplazos, 35 archivos).
- Versión unificada a **0.1.0** en todos los módulos (antes 1.0.0–3.0.0).
- `regulacion-imports.ttl`: corregidos los `owl:imports` que apuntaban a IRIs
  `…/regulacion/modular/…` inexistentes → `…/regulacion/…`.
- `espacio-publico-shapes.ttl`: reemplazados imports `file://` relativos
  (dependientes de la máquina) por IRIs `w3id.org` portables.

### Fixed (corregido)
- Errores de sintaxis Turtle que impedían el *parsing*:
  - `familias-core.ttl`: espacio en el nombre de propiedad `tieneEstatus Educativo`.
  - `lifecycle-propagation-shapes.ttl`: prefijo `:` no declarado.
  - `event-state-enforcement-shapes.ttl`: espacio en QName y comilla sobrante.
- `vendedor-shapes.ttl`: añadida cabecera `owl:Ontology` (antes sin declarar).
- `versionIRI` malformado `…espacio-publico#/0.1.0` → `…/espacio-publico/0.1.0`.

### Notes
- Todos los archivos `.ttl` (37) parsean sin error y los `owl:imports` resuelven (0 colgantes).
