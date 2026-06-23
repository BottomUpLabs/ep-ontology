---
concept: Reubicación definitiva por amenaza
status: vigente
steward: Subsecretaría de Espacio Público - Secretaría de Seguridad y Convivencia
last_review: 2025-11-20
next_review: 2026-06-30
sources_clave:
	- [PR-GGOL-012, 2023]
	- [Ley 1801, 2016]
	- [Decreto 2148, 2015]
	- [Metodología DE-GGOL-007, 2023]
coverage: Medellín, Colombia
notes: Novedad que protege a venteros regulados frente a riesgos de seguridad documentados
---

# Concepto: Reubicación definitiva por amenaza

## Contexto y Alcance
Algunos venteros regulados enfrentan amenazas que comprometen su seguridad o la de su familia. La reubicación definitiva es la respuesta institucional para trasladar el permiso y, si aplica, el módulo a otro punto en una comuna distinta, garantizando continuidad económica sin poner en riesgo al titular. Exige denuncia formal ante la Fiscalía y evaluación de disponibilidad en destinos seguros. No cubre traslados motivados por conveniencia comercial.

## Resumen Conceptual
| Campo | Descripción |
| --- | --- |
| Término preferido | Reubicación definitiva por amenaza |
| Etiquetas alternativas | traslado por riesgo, reubicación protegida |
| Definición breve | Novedad que autoriza mover un permiso temporal y su módulo a otro sector cuando el ventero demuestra amenazas documentadas y propone ubicaciones alternativas. |
| Fuentes o evidencias | [PR-GGOL-012, 2023], [Ley 1801, 2016] |
| Ámbito temporal | Lineamientos vigentes desde 2018; reforzados en 2023-06-26. |
| Cobertura territorial | Comunas y corregimientos de Medellín con capacidad de recibir nuevos módulos. |
| Programas o políticas relacionadas | Programa de Reubicación, Intervenciones Estratégicas, Inspectorías, Oferta Institucional. |
| Riesgos o tensiones | Saturación de corredores seguros, revictimización por trámites largos, ausencia de pruebas sólidas, represalias en puntos de origen. |

## Actores y Agentes
| Actor | Tipo | Motivaciones | Capacidades | Relaciones clave |
| --- | --- | --- | --- | --- |
| Ventero regulado en riesgo | Solicitante | Proteger su vida y mantener ingresos. | Presenta denuncia, propone nuevos sitios, participa en visitas. | Fiscalía, Personería. |
| Subsecretaría de Espacio Público (Autorizaciones y Territorio) | Autoridad | Garantizar traslados con evidencia y disponibilidad. | Coordina visitas, actualiza bases, emite oficio. | Inspectorías, Gestión de Módulos. |
| Fiscalía / autoridades de seguridad | Entidad externa | Investigar amenazas y emitir constancias. | Proveen copias de denuncias, orientan medidas de protección. | Secretaría de Seguridad. |
| Facturación municipal | Gestión financiera | Ajustar cobros al nuevo punto. | Actualiza canon según ubicación. | Autorizaciones. |

## Espacios y Escalas
Opera entre comunas: del punto original (micro) al nuevo punto (micro) pasando por decisiones a escala ciudad (macro) para ubicar espacios disponibles fuera de la zona de riesgo. Los traslados consideran mapas de Territorios Públicos y zonas restringidas.

## Documentos, acuerdos y evidencias
- Denuncia formal ante la Fiscalía por amenazas.
- Propuesta de al menos dos ubicaciones alternativas.
- Acta de visita administrativa en el punto original y en los sitios propuestos.
- Oficio de respuesta y actualización en bases (Revalidación, Matriz, SISDEP).

## Procesos, actividades y eventos
1. Radicación de la solicitud con denuncia adjunta.
2. Validación de la evidencia y consulta de disponibilidad de módulos en zonas propuestas.
3. Visitas administrativas para constatar cumplimiento normativo del solicitante y viabilidad de los destinos.
4. Emisión de oficio aprobando o negando la reubicación y coordinación logística del traslado.
5. Actualización de bases de datos y notificación a Facturación.

## Conceptos relacionados y mapa semántico
- **Traslado transitorio por obra**: comparte metodología de visitas pero se basa en afectaciones físicas, no amenazas.
- **Intervenciones en el espacio público**: acompaña operativos de salida del punto en riesgo.
- **Programa de Reubicación**: provee cupos y seguimiento a los venteros trasladados.

## Observables y señales
- Oficios que mencionan números de denuncia y comunas destino.
- Mapas anexos con polígonos de riesgo y rutas sugeridas.
- Registro en Matriz de seguimiento con etiquetado “amenaza”.

## Lenguaje y vocabularios usados
- "Reubicación definitiva" y "amenaza comprobada" son términos formales.
- Se evita "desplazamiento" salvo cuando coincide con la terminología de la Fiscalía.
- "Traslado protegido" se usa en mesas interinstitucionales para comunicar el caso sin exponer detalles.

## Preguntas abiertas y stewardship
- ¿Cómo coordinar medidas de seguridad temporales mientras se completa el traslado?
- ¿Qué criterios definen que las comunas propuestas ofrecen condiciones seguras?
- ¿Se puede crear un protocolo de acompañamiento psicosocial posterior a la reubicación?
La Subsecretaría de Espacio Público articula el concepto con la Secretaría de Seguridad y reporta casos al Comité de Seguimiento de Amenazas.

## Mapeo a ontología (OWL/SKOS)

**Clases relacionadas:**
- `ep:ProgramaReubicacion` - Programa institucional que gestiona reubicaciones
- `ep:PermisoTransitorio` - Permiso temporal durante traslado
- `ep:CambioEstado` - Actividad que representa el cambio a estado "reubicado"
- `vocab:reubicado` - Estado terminal del vendedor reubicado exitosamente

**Propiedades:**
- `ep:participaEn ep:ProgramaReubicacion` - Vendedor participa en programa
- `ep:gestionadoPorPrograma ep:ProgramaReubicacion` - Permiso gestionado por programa
- `ep:fundamentoLegal "Denuncia Fiscalia No. XXX"` - Fundamento legal del traslado
- `ep:motivoCambio "Amenaza documentada"` - Motivo del cambio de estado

**Vocabulario controlado:**
- `vocab:prog-reubicacion` - Tipo de programa (SKOS)
- Sinónimos: "traslado", "reubicacion por amenaza", "reubicacion por obra"

**Ejemplo RDF:**
```turtle
<vendedor/V-456> :estadoActual vocab:reubicado ;
                 :participaEn <programa/PROG-REUB-2025> .

<programa/PROG-REUB-2025> a :ProgramaReubicacion ;
                          :nombrePrograma "Reubicacion por Amenazas 2025" ;
                          :estadoPrograma "en_ejecucion" .

<cambio/20250106-002> a :CambioEstado ;
                      :vendedorAfectado <vendedor/V-456> ;
                      :estadoOrigen vocab:regulado-activo ;
                      :estadoDestino vocab:reubicado ;
                      :fundamentoLegal "Denuncia Fiscalia No. 123456" ;
                      :motivoCambio "Amenaza comprobada - traslado protegido" .
```

Última actualización: 2025-11-20
