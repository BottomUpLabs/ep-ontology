# PQRS y Novedades de Vendedores Regulados

## Descripción General

Este módulo ontológico define el sistema de **PQRS (Peticiones, Quejas, Reclamos y Sugerencias)** aplicado específicamente a **novedades administrativas** que afectan el estado de vendedores informales regulados con permisos vigentes.

**Archivo**: `pqrs-novedades.ttl`  
**Basado en**: Diagrama "Ciclo de Vida - Candidato de Autorización" (Decision Point "Novedad")  
**Fecha**: 2026-01-07

---

## Contexto del Negocio

Cuando un **VendedorInformalRegulado** (vendedor con permiso vigente) experimenta una situación que afecta su capacidad de operar o la vigencia de su permiso, debe radicar una **SolicitudPQRS** para notificar la novedad a la Subsecretaría de Espacio Público.

Estas novedades están representadas en el diagrama de ciclo de vida como el **Decision Point "Novedad"**, que desencadena diferentes flujos según el tipo:

- **Persona Fallece** → NovedadFallecimiento
- **Persona Cambia de Titular** → NovedadCambioTitular
- **Estado Suspendido** → NovedadSuspension

---

## Clases Principales

### 1. SolicitudPQRS (Clase Base)

**Hereda de**: `:Tramite`  
**Propiedades clave**:
- `:numeroRadicado` - Identificador único del trámite (ej: RAD-2025-00456)
- `:fechaRadicacion` - Fecha de presentación
- `:tipoNovedad` - Clasificación (fallecimiento, cambio_titular, suspensión)
- `:causalNovedad` - Motivo específico

**Características**:
- Todas las novedades generan un número de radicado para trazabilidad
- Afectan directamente el estado del `VendedorInformalRegulado`
- Pueden afectar el estado del `PermisoOcupacion`
- Requieren verificación documental rigurosa

---

### 2. NovedadFallecimiento

**Desencadenante**: Deceso del titular de un permiso vigente  
**Solicitante**: Familiar directo, conviviente o entidad (no el titular)  
**Documento requerido**: Certificado de defunción (Registraduría)

#### Flujo de Vida

```
1. Radicación → 2. Verificación Documental → 3. Evaluación de Cesión → 4. Decisión
```

#### Estados del Ciclo de Vida

| Estado | Descripción |
|--------|-------------|
| `radicado` | PQRS presentada con certificado de defunción |
| `en_verificacion_documental` | Validando autenticidad del certificado |
| `verificacion_parentesco` | Verificando identidad y relación del solicitante |
| `beneficiario_identificado` | Beneficiario elegible propuesto |
| `en_evaluacion_cesion` | ConceptoTecnico evalúa viabilidad de cesión |
| `resuelto_favorable_cesion` | Cesión APROBADA → genera nuevo permiso |
| `resuelto_desfavorable_sin_cesion` | Cesión NEGADA → termina permiso |

#### Impacto en Entidades

**Si NO hay cesión** (sin beneficiario o cesión negada):
```turtle
VendedorInformalRegulado (titular) → estado: 'fallecido' (TERMINAL)
PermisoOcupacion → estado: 'revocado'
Carne → estado: 'destruido_con_trazabilidad'
Modulo → estado: 'recuperado' (retorna a inventario)
```

**Si HAY cesión aprobada**:
```turtle
VendedorInformalRegulado (fallecido) → estado: 'fallecido' (TERMINAL)
PermisoOcupacion (original) → estado: 'revocado'
NUEVO VendedorInformalRegulado (beneficiario) → estado: 'activo'
NUEVO PermisoOcupacion → estado: 'activo'
NUEVO Carne → estado: 'entregado_con_constancia'
Modulo → estado: 'reasignado' (sin cambio de ubicación)
```

#### Requisitos para Cesión

- **Beneficiario elegible**: cónyuge, hijo mayor de edad, conviviente >5 años
- **Caracterización previa**: FO-GGOL-048 completado
- **Sin permisos vigentes** en otro punto
- **Sin antecedentes** de revocatorias previas
- **Paz y salvo**: cumplir obligaciones pendientes del titular fallecido

#### Evento Resultante

Si aprobado: **`:CesionPermisoFallecimiento`**

---

### 3. NovedadCambioTitular

**Desencadenante**: Titular vigente desea ceder permiso voluntariamente  
**Solicitante**: El titular actual del permiso  
**Documento requerido**: Formulario de cesión, consentimiento expreso, certificado de parentesco

#### Flujo de Vida

```
1. Radicación → 2. Verificación Consentimiento → 3. Evaluación Elegibilidad → 4. Decisión
```

#### Estados del Ciclo de Vida

| Estado | Descripción |
|--------|-------------|
| `radicado` | Solicitud presentada por titular |
| `verificacion_consentimiento` | Validando firma y consentimiento del titular |
| `verificacion_parentesco` | Verificando relación familiar con beneficiario |
| `verificacion_paz_salvo` | Verificando ausencia de deudas |
| `beneficiario_validado` | Beneficiario cumple requisitos de parentesco |
| `en_evaluacion_elegibilidad` | ConceptoTecnico evalúa elegibilidad del beneficiario |
| `resuelto_favorable_cesion_aprobada` | Cesión APROBADA → nuevo permiso |
| `resuelto_desfavorable_cesion_negada` | Cesión NEGADA → titular mantiene permiso |

#### Diferencias con NovedadFallecimiento

| Aspecto | NovedadFallecimiento | NovedadCambioTitular |
|---------|---------------------|---------------------|
| **Solicitante** | Familiar/tercero | Titular actual |
| **Voluntariedad** | NO (forzado por deceso) | SÍ (decisión voluntaria) |
| **Estado titular** | 'fallecido' (irreversible) | 'cancelado' (voluntario) |
| **Reversibilidad** | NO (no puede revertirse) | Parcial (puede desistir antes de aprobación) |
| **Requisito clave** | Certificado de defunción | Consentimiento expreso |

#### Causales de Negación

- Beneficiario no cumple parentesco requerido (debe ser cónyuge, hijo, conviviente >5 años)
- Beneficiario tiene permiso vigente en otro punto
- Beneficiario con antecedentes de revocatoria
- Titular actual con obligaciones pendientes (deudas)
- Permiso suspendido o con proceso sancionatorio abierto

#### Evento Resultante

Si aprobado: **`:CesionPermisoVoluntaria`**

---

### 4. NovedadSuspension

**Desencadenante**: Titular solicita pausa temporal por causal justificada  
**Solicitante**: El titular del permiso  
**Documentos requeridos**: Certificado médico, denuncia Fiscalía, certificado RUV, etc. (según causal)

#### Flujo de Vida

```
1. Radicación → 2. Verificación Causal → 3. Evaluación Técnica → 4. Decisión → 5. Reactivación
```

#### Estados del Ciclo de Vida

| Estado | Descripción |
|--------|-------------|
| `radicado` | Solicitud presentada con documentos soporte |
| `verificacion_causal` | Validando autenticidad de la causal |
| `en_evaluacion_tecnica` | ConceptoTecnico evalúa pertinencia y duración |
| `resuelto_favorable_suspension_aprobada` | Suspensión APROBADA (30-180 días) |
| `suspension_vigente` | Permiso suspendido, sin cobros |
| `suspension_vencida` | Plazo de suspensión terminado |
| `reactivacion_solicitada` | Titular solicita reactivar antes de vencimiento |
| `reactivado` | Permiso reactivado exitosamente |

#### Causales Aceptadas

| Causal | Documento Requerido | Duración Típica |
|--------|-------------------|----------------|
| **Enfermedad grave** | Certificado médico >30 días | 60-120 días |
| **Amenaza documentada** | Denuncia Fiscalía/Policía | 90-180 días |
| **Desplazamiento temporal** | Certificado RUV | 60-120 días |
| **Calamidad doméstica** | Certificado defunción, hospitalización | 30-90 días |
| **Atención dependiente** | Certificado discapacidad/enfermedad | 60-120 días |

#### Impacto en Entidades (si aprobada)

```turtle
VendedorInformalRegulado → estado: 'suspendido'
PermisoOcupacion → estado: 'suspendido'
Carne → estado: 'vencido' (debe renovarse al reactivar)
Modulo → estado: 'retirado_temporalmente' o 'en_mantenimiento'
Cobros de ocupación → SUSPENDIDOS durante periodo
```

#### Diferencia Clave: Suspensión vs Revocatoria

- **Suspensión**: Medida temporal de protección (NO es sanción)
- **Revocatoria**: Terminación definitiva por incumplimiento grave (ES sanción)

#### Evento Resultante

Tras vencimiento/solicitud: **`:ReactivacionSuspension`**

---

## Relaciones con Otras Entidades

### Relación con VendedorInformalRegulado

Todas las novedades PQRS modifican el estado del vendedor regulado:

```turtle
:NovedadFallecimiento → VendedorInformalRegulado: 'fallece' → 'fallecido'
:NovedadCambioTitular → VendedorInformalRegulado: 'cambia_titular' → 'cancelado'
:NovedadSuspension → VendedorInformalRegulado: 'activo' → 'suspendido'
```

### Relación con PermisoOcupacion

```turtle
:NovedadFallecimiento → PermisoOcupacion (original): 'activo' → 'revocado'
:NovedadCambioTitular → PermisoOcupacion (original): 'activo' → 'revocado'
:NovedadSuspension → PermisoOcupacion: 'activo' → 'suspendido' → 'activo'
```

### Relación con AutorizacionTemporalEspacioPublico

**DIFERENCIA CLAVE**:
- **AutorizacionTemporalEspacioPublico**: Trámite para OBTENER permiso (vendedor NO regulado → regulado)
- **SolicitudPQRS (Novedades)**: Trámite para MODIFICAR estado de permiso existente (vendedor YA regulado)

```
Flujo Completo:
1. Persona → radica AutorizacionTemporalEspacioPublico (FO-GGOL-026)
2. Si aprobado → se crea VendedorInformalRegulado + PermisoOcupacion
3. Durante operación → puede surgir novedad
4. Vendedor Regulado → radica SolicitudPQRS (NovedadFallecimiento, etc.)
5. Novedad resuelta → modifica estado vendedor/permiso
```

---

## Propiedades Clave

### Object Properties

| Propiedad | Dominio | Rango | Descripción |
|-----------|---------|-------|-------------|
| `:afectaVendedorRegulado` | SolicitudPQRS | VendedorInformalRegulado | PQRS afecta estado del vendedor |
| `:afectaPermisoOcupacion` | SolicitudPQRS | PermisoOcupacion | PQRS afecta estado del permiso |
| `:beneficiarioCesion` | NovedadFallecimiento, NovedadCambioTitular | Persona | Beneficiario propuesto |
| `:generaNuevoPermiso` | CesionPermisoFallecimiento, CesionPermisoVoluntaria | PermisoOcupacion | Cesión aprobada genera nuevo permiso |
| `:transicionaEstadoVendedor` | SolicitudPQRS | VendedorInformalRegulado | Desencadena transición de estado |
| `:reactivaPermiso` | ReactivacionSuspension | PermisoOcupacion | Reactiva permiso suspendido |

### Datatype Properties (NovedadFallecimiento)

- `:certificadoDefuncionNumero` - Número del certificado oficial
- `:fechaDefuncion` - Fecha del deceso
- `:beneficiarioNombre` - Nombre del beneficiario propuesto
- `:parentescoBeneficiario` - Tipo de relación familiar
- `:cesionAprobada` - Boolean: aprobada o negada
- `:fechaDecisionCesion` - Fecha de la decisión

### Datatype Properties (NovedadCambioTitular)

- `:consentimientoTitular` - Boolean: titular consintió
- `:motivoCesionVoluntaria` - Razón de la cesión
- `:pazSalvoTitular` - Boolean: sin deudas
- `:beneficiarioDocumento` - Cédula del beneficiario

### Datatype Properties (NovedadSuspension)

- `:causalSuspension` - Tipo de causal (enfermedad_grave, amenaza, etc.)
- `:duracionSuspensionSolicitada` - Días solicitados (30-180)
- `:documentoSoporteCausal` - Número del certificado/denuncia
- `:fechaInicioSuspension` - Inicio efectivo de la suspensión
- `:fechaFinSuspension` - Fin programado
- `:reactivacionSolicitada` - Boolean: reactivación solicitada

---

## Eventos Administrativos

### CesionPermisoFallecimiento

**Trigger**: NovedadFallecimiento aprobada con beneficiario elegible  
**Genera**:
1. Nuevo PermisoOcupacion para beneficiario
2. Nueva Resolución administrativa
3. Nuevo Carne
4. Nuevo rol VendedorInformalRegulado (beneficiario)

**Termina**:
- Rol VendedorInformalRegulado (fallecido)
- PermisoOcupacion (original)
- Carne (original)

### CesionPermisoVoluntaria

**Trigger**: NovedadCambioTitular aprobada  
**Genera**:
1. Nuevo PermisoOcupacion para beneficiario
2. Nueva Resolución
3. Nuevo Carne
4. Nuevo rol VendedorInformalRegulado (beneficiario)

**Termina**:
- Rol VendedorInformalRegulado (titular actual)
- PermisoOcupacion (original)
- Carne (original)

### ReactivacionSuspension

**Trigger**: Vencimiento de suspensión o solicitud anticipada aprobada  
**Reactiva**:
1. VendedorInformalRegulado ('suspendido' → 'activo')
2. PermisoOcupacion ('suspendido' → 'activo')
3. Modulo ('retirado_temporalmente' → 'en_uso')

**Genera**:
- Nuevo Carne (el anterior venció durante suspensión)

---

## Diagrama de Transiciones de Estado

### VendedorInformalRegulado (estados afectados por PQRS)

```
[activo] 
   ↓ (NovedadFallecimiento radicada)
[fallece] 
   ↓ (Cesión aprobada → beneficiario)
[fallecido] (TERMINAL)

[activo]
   ↓ (NovedadCambioTitular radicada)
[cambia_titular]
   ↓ (Cesión aprobada)
[cancelado] (TERMINAL)

[activo]
   ↓ (NovedadSuspension aprobada)
[suspendido]
   ↓ (Reactivación)
[activo] (retorna)
```

---

## Competency Questions

1. **Q**: ¿Qué documentos se requieren para radicar una NovedadFallecimiento?  
   **A**: Certificado de defunción (Registraduría), cédula del solicitante, certificado de parentesco

2. **Q**: ¿Puede un beneficiario de NovedadFallecimiento tener permiso vigente en otro punto?  
   **A**: NO, es causal de negación de la cesión

3. **Q**: ¿Cuál es la diferencia entre NovedadFallecimiento y NovedadCambioTitular?  
   **A**: NovedadFallecimiento es involuntaria (titular fallecido), NovedadCambioTitular es voluntaria (titular decide ceder)

4. **Q**: ¿La suspensión por NovedadSuspension es una sanción?  
   **A**: NO, es una medida temporal de protección, NO es sanción

5. **Q**: ¿Cuánto tiempo puede durar una suspensión aprobada?  
   **A**: Entre 30 y 180 días (máximo 6 meses)

6. **Q**: ¿Se paga ocupación durante una suspensión aprobada?  
   **A**: NO, los cobros se suspenden durante el periodo de suspensión

7. **Q**: ¿Qué pasa con el módulo cuando el titular fallece sin beneficiario elegible?  
   **A**: El módulo retorna a inventario disponible (estado: 'recuperado')

8. **Q**: ¿Puede revertirse una NovedadCambioTitular después de aprobada?  
   **A**: NO, una vez ejecutada la cesión es irreversible (titular actual → 'cancelado')

---

## Fuentes Normativas

- **Ley 1755 de 2015**: Derecho de petición
- **Acuerdo 076 de 2014**: Política Pública de Vendedores Informales (criterios de cesión)
- **Decreto 883 de 2015**: Requisitos de autorización
- **Proceso PR-GGOL-012**: Gestión de novedades de regulados
- **Diagrama de Ciclo de Vida**: Decision Point "Novedad" (2026-01-07)

---

## Uso en Consultas SPARQL

### Ejemplo 1: Listar todas las cesiones por fallecimiento aprobadas en 2025

```sparql
PREFIX : <http://datos.medellin.gov.co/def/espacio-publico/core#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?novedad ?fechaDefuncion ?beneficiario ?fechaCesion
WHERE {
    ?novedad a :NovedadFallecimiento ;
             :cesionAprobada true ;
             :fechaDefuncion ?fechaDefuncion ;
             :beneficiarioNombre ?beneficiario ;
             :fechaDecisionCesion ?fechaCesion .
    
    FILTER (YEAR(?fechaCesion) = 2025)
}
ORDER BY ?fechaCesion
```

### Ejemplo 2: Vendedores con suspensiones activas

```sparql
PREFIX : <http://datos.medellin.gov.co/def/espacio-publico/core#>

SELECT ?vendedor ?novedad ?causal ?fechaInicio ?fechaFin
WHERE {
    ?vendedor a :VendedorInformalRegulado ;
              :estadoRolRegulado "suspendido" .
    
    ?novedad a :NovedadSuspension ;
             :afectaVendedorRegulado ?vendedor ;
             :suspensionAprobada true ;
             :causalSuspension ?causal ;
             :fechaInicioSuspension ?fechaInicio ;
             :fechaFinSuspension ?fechaFin .
    
    FILTER (?fechaFin >= NOW())
}
```

---

## Integración con DMP

Las PQRS de novedades NO están directamente en el DMP (a1, a2, b1, etc.), pero se relacionan así:

| DMP Estándar | Relación con PQRS |
|--------------|-------------------|
| **caracterizacion_v1** | Vendedores caracterizados son elegibles como beneficiarios de cesión |
| **resoluciones_aeep_v1** | Cada cesión aprobada genera nueva resolución |
| **familias_v1** | Datos de parentesco validan beneficiarios en cesiones |
| **modulos_v1** | Estados de módulos afectados por novedades (recuperado, reasignado) |

---

## Próximos Pasos

1. **Agregar SHACL Shapes** para validar requisitos de cesión
2. **Crear vocabulario SKOS** para causales de suspensión
3. **Definir OWL Restrictions** en elegibilidad de beneficiarios
4. **Generar JSON Cards** para RAG sobre novedades
5. **Integrar con sistema de carnetización** (destrucción/emisión de carnés)

---

**Última actualización**: 2026-01-07  
**Autor**: Subsecretaría de Espacio Público - Secretaría de Seguridad y Convivencia  
**Versión Ontología**: 1.0
