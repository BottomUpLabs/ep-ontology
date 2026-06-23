# Guía Conceptual para Vocabularios y Ontologías

Aplica esta guía a todos los documentos dentro de `vocabulary/`. El propósito es capturar el significado de los conceptos, sus relaciones y el lenguaje que las comunidades usan para describirlos, sin atarlos a modelos de datos específicos. Prioriza descripciones narrativas, ejemplos y vínculos contextuales.

## 1. Estructura

1. Bloque de metadatos obligatorio en la parte superior (ver sección siguiente).
2. `# Concepto: <Nombre>` – título con el nombre más reconocido del concepto.
2. `## Contexto y Alcance` – dónde aparece el concepto, por qué importa y qué queda explícitamente fuera.
3. `## Resumen Conceptual` – tabla breve de metadatos (ver sección siguiente).
4. `## Actores y Agentes` – personas o instituciones que activan, regulan o viven el concepto.
5. `## Espacios y Escalas` – lugares físicos o administrativos donde se manifiesta.
6. `## Documentos, acuerdos y evidencias` – soportes formales o informales asociados.
7. `## Procesos, actividades y eventos` – dinámicas que involucran al concepto a lo largo del tiempo.
8. `## Conceptos relacionados y mapa semántico` – relaciones clave con otros conceptos.
9. `## Observables y señales (opcional)` – pistas cualitativas o cuantitativas que permiten identificar el concepto.
10. `## Lenguaje y vocabularios usados` – expresiones comunes, sinónimos, términos sensibles.
11. `## Preguntas abiertas y stewardship` – temas pendientes, responsables y próximos pasos.


## 2. Metadatos obligatorios

Cada documento debe iniciar con un bloque YAML (sin sangrías extras) que capture la trazabilidad del concepto. Ejemplo:

```
---
concept: Vendedor informal
steward: IPES - Direccion de Formalizacion
last_review: 2025-11-20
sources_clave:
	- [Alcaldia de Bogota, 2022 Politica de Espacio Publico]
	- [IPES, 2023 Registro de Vendedores]
coverage: Bogota D.C.
notes: En coordinacion con Mesa Distrital de Espacio Publico
---
```

Campos requeridos:

- `concept`: nombre oficial.
- `steward`: rol o equipo responsable.
- `last_review`: fecha de última revisión en formato `YYYY-MM-DD`.
- `sources_clave`: lista corta (máx. 5) con referencias clave.
- `coverage`: ámbito territorial principal (especifica ciudad/área; por defecto Medellín para este vocabulario a menos que se indique lo contrario).


## 3. Resumen Conceptual

Incluye una tabla con campos redactados en lenguaje natural:

| Campo | Descripción |
| --- | --- |
| Término preferido | Nombre central del concepto. |
| Etiquetas alternativas | Alias, variaciones locales o históricas separados por `, `. |
| Definición breve | Máximo tres oraciones que capturen esencia, límites y ejemplo típico. |
| Fuentes o evidencias | Normas, artículos, planes, estudios o testimonios (formato `[Entidad, Año]`). |
| Ámbito temporal | Periodo de vigencia, momento de origen o caducidad. |
| Cobertura territorial | Escala donde se aplica (ciudad, barrio, corredor, nacional). |
| Programas o políticas relacionadas | Iniciativas que usan el concepto o lo convierten en requisito. |
| Riesgos o tensiones | Controversias, vacíos, sesgos u oportunidades en discusión. |

## 4. Actores y Agentes

Describe los actores con narrativas o tablas. Cuando uses tablas, sugiere columnas `Actor o colectivo`, `Tipo`, `Motivaciones`, `Capacidades`, `Relaciones clave`. Evita códigos técnicos; prioriza cómo se autodenominan y cómo los reconocen otras partes. Añade ejemplos situacionales.

## 5. Espacios y Escalas

Explica los lugares donde el concepto cobra vida: calles, plazas, instituciones, rutas administrativas, instancias de participación. Señala las escalas (micro, barrio, ciudad, región) y menciona referencias cartográficas solo si aclaran el contexto. Describe restricciones, oportunidades o tensiones espaciales.

## 6. Documentos, acuerdos y evidencias

Enumera los soportes que materializan el concepto: registros, actas, testimonios, piezas audiovisuales, resoluciones o acuerdos comunitarios. Indica quién los emite, para qué sirven y qué partes de la realidad dejan por fuera. Distingue entre evidencia formal e informal cuando ayude a entender el concepto.

## 7. Procesos, actividades y eventos

Relaciona rutinas periódicas (caracterizaciones, mercados, comités) y eventos puntuales (operativos, emergencias, mesas de diálogo). Para cada proceso describe qué lo activa, quién participa, qué cambia antes y después, y cuáles indicadores sociales o narrativos se observan.

## 8. Conceptos relacionados y mapa semántico

Construye un pequeño grafo textual o tabla con relaciones como `es parte de`, `contrasta con`, `depende de`, `habilita`, `es regulado por`. Explica por qué existe la relación y cita ejemplos concretos. Si conectas con ontologías externas, aclara cómo se alinean los significados.

## 9. Observables y señales (opcional)

Cuando sea útil, describe señales que permiten identificar el concepto en campo o en registros: testimonios, artefactos visuales, horas del día, patrones de movilidad, documentos habituales. No uses notación técnica; enfócate en cómo los equipos reconocerían el fenómeno.

## 10. Lenguaje y vocabularios usados

Documenta expresiones populares, sinónimos, eufemismos, términos oficiales y palabras en disputa. Explica quién usa cada término y en qué contexto. Señala términos que deben evitarse por sesgos o estigmas. Cuando existan listas controladas, menciona su propósito sin describir estructuras de datos.

## 11. Preguntas abiertas y stewardship

Registra dudas vigentes, decisiones pendientes y quién lidera la conversación. Incluye ventanas de tiempo deseadas, foros donde se discutirán y dependencias entre decisiones.

## 12. Estilo y buenas prácticas

- Usa prosa clara, ejemplos y testimonios breves.
- Evita nombres en `snake_case`, códigos técnicos o referencias a columnas salvo que sean indispensables para aclarar el concepto (y, aun así, descríbelos en lenguaje natural).
- Mantén los textos en español neutro y menciona traducciones solo si añaden contexto.
- Limita las tablas a aquello que complemente la narrativa; prioriza explicaciones.
- Cita fuentes con referencias cortas `[Entidad, Año]` y agrega URL opcional cuando exista.
- Cierra cada documento con la fecha de última actualización.

## 13. Ejemplo mínimo (solo guía visual)

```
# Concept: Vendedor informal
Última actualización: 2025-11-20
## Contexto y Alcance
...
```

No reutilices el texto del ejemplo; úsalo únicamente para recordar el orden de las secciones.
