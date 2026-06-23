---
concept: Solicitud de permiso temporal por vendedor no regulado
status: vigente
steward: Subsecretaría de Espacio Público - Secretaría de Seguridad y Convivencia
last_review: 2025-11-20
next_review: 2026-06-30
sources_clave:
	- [PR-GGOL-012, 2023]
	- [PR-GGOL-016, 2022]
	- [Ley 1801, 2016]
coverage: Medellín, Colombia
notes: Novedad que se responde negando permisos nuevos cuando no hay proceso de formalización abierto
---

# Concepto: Solicitud de permiso temporal por vendedor no regulado

## Contexto y Alcance
Aunque PR-GGOL-012 se enfoca en venteros regulados, recibe solicitudes de personas sin permiso vigente que buscan autorización temporal. El trámite verifica bases de datos y, si no hay convocatorias abiertas, responde negando la petición y orientando a la oferta institucional. Este concepto describe ese flujo negativo para evitar falsas expectativas y mantener trazabilidad.

## Resumen Conceptual
| Campo | Descripción |
| --- | --- |
| Término preferido | Solicitud de permiso temporal de no regulado |
| Etiquetas alternativas | petición de autorización nueva, solicitud fuera de convocatoria |
| Definición breve | Novedad registrada cuando una persona sin estatus regulado pide un permiso temporal; la Subsecretaría verifica sus bases y responde que no se otorgan nuevos permisos por fuera de los procesos de formalización vigentes. |
| Fuentes o evidencias | [PR-GGOL-012, 2023], [PR-GGOL-016, 2022] |
| Ámbito temporal | Vigente desde 2018, confirmado en 2023-06-26. |
| Cobertura territorial | Todo Medellín, pues la respuesta aplica sin importar la ubicación solicitada. |
| Programas o políticas relacionadas | Autorización temporal de ocupación, Oferta Institucional, PQRSD. |
| Riesgos o tensiones | Expectativas incumplidas, saturación de radicados repetitivos, actores que insisten sin vías formales, percepción de inequidad. |

## Actores y Agentes
| Actor | Tipo | Motivaciones | Capacidades | Relaciones clave |
| --- | --- | --- | --- | --- |
| Vendedor no regulado | Solicitante | Obtener permiso sin pasar por convocatoria formal. | Radica oficio, aporta información básica. | Mesas comunitarias, líderes barriales. |
| Subsecretaría de Espacio Público (Autorizaciones) | Autoridad | Garantizar cumplimiento de la política de cupos. | Consulta SISDEP, Revalidación, TETHA; emite oficio negativo. | Oferta Institucional, Territorios Públicos. |
| Oferta Institucional / programas sociales | Alternativa | Orientar al solicitante hacia procesos de formalización o apoyo social. | Remisiones, convocatorias específicas. | Subsecretaría, Secretaría de Desarrollo Económico. |

## Espacios y Escalas
Se registra digitalmente en Mercurio y las bases de seguimiento. Territorialmente puede abarcar cualquier barrio donde se identifique la intención de ocupar el espacio público sin autorización.

## Documentos, acuerdos y evidencias
- Radicado en Mercurio con la solicitud y respuesta negativa.
- Consultas en SISDEP, Revalidación, Hoja de vida, TETHA y Facturación para confirmar que no es regulado.
- Oficio orientador que remite a la oferta institucional o indica futuros procesos.

## Procesos, actividades y eventos
1. Recepción de la solicitud y verificación de estatus.
2. Emisión de respuesta negativa con fundamento normativo.
3. Actualización en bases para evitar trámites duplicados.
4. Remisión (cuando aplica) a programas que acompañan vendedores no regulados.

## Conceptos relacionados y mapa semántico
- **Novedades de regulados**: clasifica esta solicitud como novedad aunque el solicitante no esté formalizado.
- **Autorización temporal de ocupación**: procedimiento al que eventualmente podría acceder si se abre convocatoria.
- **Oferta institucional**: destino sugerido para canalizar la necesidad.

## Observables y señales
- Incremento de solicitudes en épocas pre-electorales o de crisis económica.
- Radicados con notas “no regulado” y enlaces a remisiones sociales.
- Estadísticas que muestran porcentaje de negativas frente a solicitudes de módulo.

## Lenguaje y vocabularios usados
- "No se otorgan nuevos permisos" se cita textualmente en los oficios.
- "Oferta institucional" agrupa programas de apoyo.
- Se evita "rechazo" para mantener tono pedagógico; se usa "respuesta negativa con orientación".

## Preguntas abiertas y stewardship
- ¿Cómo comunicar proactivamente las fechas de futuras convocatorias para reducir solicitudes fuera de ciclo?
- ¿Qué datos mínimos deben capturarse para facilitar contacto cuando se abra un proceso?
- ¿Puede integrarse la remisión automática a programas sociales digitalmente?
La Subsecretaría de Espacio Público mantiene la política y coordina con Oferta Institucional para consolidar estadísticas semestrales de solicitudes negadas.

## Mapeo a ontología (OWL/SKOS)

**Clases relacionadas:**
- `ep:OfertaInstitucional` - Programa institucional de orientación y acompañamiento
- `ep:ProgramaFormalizacion` - Subclase para formalización comercial
- `vocab:negado` - Estado cuando se niega la solicitud
- `vocab:desistido` - Estado cuando el vendedor desiste

**Propiedades:**
- `ep:requierePrograma ep:OfertaInstitucional` - Vendedor es remitido a oferta
- `ep:participaEn ep:ProgramaFormalizacion` - Vinculación a programa específico
- `ep:motivoCambio "Solicitud negada - no hay cupos disponibles"` - Justificación

**Vocabulario controlado:**
- `vocab:prog-formalizacion` - Programa de formalización (SKOS)
- `vocab:prog-capacitacion` - Programa de capacitación
- `vocab:prog-apoyo-social` - Programa de apoyo social
- `vocab:oferta-integral` - Meta-concepto que agrupa todos los programas

**Flujo típico:**
```turtle
# Vendedor caracterizado solicita permiso
<vendedor/V-789> :estadoActual vocab:caracterizado-socialmente .

# Solicitud negada - no hay convocatoria abierta
<cambio/20250106-003> a :CambioEstado ;
                      :vendedorAfectado <vendedor/V-789> ;
                      :estadoOrigen vocab:caracterizado-socialmente ;
                      :estadoDestino vocab:negado ;
                      :motivoCambio "No se otorgan nuevos permisos por fuera de convocatorias" ;
                      :fundamentoLegal "PR-GGOL-012 - Politica de cupos" .

# Remisión a oferta institucional
<vendedor/V-789> :requierePrograma <programa/PROG-OFER-2025> .

<programa/PROG-OFER-2025> a :OfertaInstitucional ;
                          :nombrePrograma "Oferta Integral Vendedores 2025" ;
                          :descripcionPrograma "Formalizacion, capacitacion y apoyo social" .
```

Última actualización: 2025-11-20
