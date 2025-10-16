# Manual de Accionables Técnicos en **OpenMetadata**  
**Derivado de la Política Integrada de Datos — AutoNova Motors (v1.0)**

> Este manual traduce la política (Calidad • Clasificación • Acceso) a configuraciones reproducibles en **OpenMetadata**. Incluye artefactos YAML/JSON, comandos de CLI, y recomendaciones de despliegue y SSO. Al final, se agrega un **Glosario**.

---

## 0) Resumen para ingeniería
- **Objetivo:** materializar N1–N4, RBAC, DQ y Lineage en OpenMetadata (OM).  
- **Resultado esperado:** catálogo con **clasificación** (tags), **glosario**, **roles y policies**, **tests de calidad**, **lineage** y **SSO** conectados a las fuentes (DB/BI/Pipelines).  
- **Alcance técnico:** configuración reproducible vía **YAML + CLI** y/o UI; integración con **Airflow** u orquestador externo; autenticación **OIDC** (Okta/Azure AD).

---

## 1) Componentes y prerrequisitos
**Arquitectura base** (resumen):  
- **Servidor OpenMetadata** + **Ingestion Framework** (conectores).  
- **Base de metadatos**: **MySQL** o **PostgreSQL**.  
- **Motor de búsqueda**: **Elasticsearch** u **OpenSearch**.  
- **UI Web** y **API** para manejar entidades (tablas, dashboards, pipelines, tags, glossaries, policies, roles).

**Requisitos mínimos** (producción): DB 4 vCPU / 16 GiB RAM; ES 2 vCPU / 8 GiB RAM (ajustar según volumen).  
**Despliegues típicos**: Docker Compose, Helm en Kubernetes (AKS/EKS/GKE).

**Referencias**: ver enlaces en la sección *Fuentes* al final de este documento.

---

## 2) Mapeo de la política N1–N4 a OpenMetadata
- **Niveles N1–N4 → Clasificación (Tags):** Crear una **Classification** `DataClassification` con tags `Level1`, `Level2`, `Level3`, `Level4` y aplicarlos a **tablas/columnas**.  
- **Privacidad/PII → Tags adicionales** (p. ej., `PII.Email`, `PII.Salary`) para granularidad por columna.  
- **RBAC → Roles & Policies:** Definir **Roles** (p. ej., `L2_Internal_Reader`, `L3_Sensitive_Custodian`, `Data_Steward`, `OM_Admin`) y asociarles **Policies** con **Rules** (recursos/operaciones).  
- **DQ → Profiler + Test Suites:** Configurar **Profiler** y **tests** por tabla/columna (notNull, pattern, min/max, freshness).  
- **Lineage → OpenLineage/Conectores:** Ingerir lineage desde **dbt**, **Airflow**, **Spark**, etc.  
- **SSO/MFA → IdP OIDC/SAML:** Integrar Okta/Azure AD (MFA se aplica en el IdP).

---

## 3) Clasificación (Tags) — Taxonomía N1–N4
### 3.1. Crear la Classification y Tags (API/SDK)
**JSON: Create Classification** (`/api/v1/classifications`):  
```json
{
  "name": "DataClassification",
  "displayName": "Data Classification",
  "description": "Sensitivity levels per AutoNova policy (N1–N4).",
  "mutuallyExclusive": true,
  "enableMigration": true
}
```
**JSON: Create Tag** (`/api/v1/tags`), por cada nivel:  
```json
{
  "name": "Level3",
  "displayName": "Level 3 - Sensible",
  "description": "Sensitive: PII y datos financieros de impacto moderado.",
  "classification": "DataClassification"
}
```
> Alternativa: **Python SDK** para crear Classification + Tags y aplicarlos a entidades.

### 3.2. Auto‑clasificación (mapeo de tags)
- Configurar **Tag Mapping** para que ciertos tags (p. ej., `PII.Personal`) apliquen automáticamente `DataClassification.Level3` o `Level4`.  
- Útil para **propagación** consistente. Ver ejemplos en *Fuentes* (Auto Classification).

### 3.3. Convenciones
- Etiquetar **columnas sensibles** (e.g., `salary`, `email`), y **tablas** completas cuando apliquen políticas a nivel dataset.  
- Usar prefijos por dominio (p. ej., `HR.*`, `Finance.*`) para búsquedas y paneles.

---

## 4) Glosario de negocio (Business Glossary)
### 4.1. Crear Glosario y Términos
- Crear **Glossary** `AutoNovaGlossary` y **Glossary Terms** (`Empleado`, `Proveedor`, `Licitación`, etc.).  
- Enlazar **sinónimos**, **términos relacionados**, **owner** y **reviewers**.  
- Asociar términos del glosario con **activos** (tablas/columnas) para semántica y descubrimiento.

**JSON: Create GlossaryTerm** (extracto):  
```json
{
  "name": "Empleado",
  "displayName": "Empleado",
  "description": "Persona que presta servicios a AutoNova.",
  "glossary": "AutoNovaGlossary",
  "synonyms": ["Colaborador", "Trabajador"]
}
```

---

## 5) RBAC — Roles, Policies y Rules
### 5.1. Principios
- **Flat RBAC:** Usuarios → **Roles**; **Roles** → **Policies**; **Policies** → **Rules** (resource + operations allow/deny).  
- Recursos comunes: `Table`, `Database`, `Dashboard`, `Pipeline`, `Tag`, `Glossary`, `Search`, `Lineage`, etc.

### 5.2. Roles recomendados (ejemplo)
- `OM_Admin`: administración total.  
- `Data_Steward`: crear/editar tags, glosarios, owners, aprobar cambios.  
- `L2_Internal_Reader`: lectura de N1–N2.  
- `L3_Sensitive_Custodian`: acceso y edición de metadatos en N3; aprobar etiquetas; lanzar DQ.  
- `L4_HighlySensitive_Custodian`: igual que N3 + gobierno ampliado sobre N4.

### 5.3. Policy (JSON) — ejemplo mínimo
```json
{
  "name": "policy_l3_sensitive",
  "displayName": "Policy - Level3 Sensitive",
  "rules": [
    {
      "name": "read_table_metadata",
      "resources": ["table"],
      "operations": ["ViewAll"],
      "effect": "allow"
    },
    {
      "name": "edit_tags_glossary",
      "resources": ["tag", "glossary"],
      "operations": ["EditAll"],
      "effect": "allow"
    },
    {
      "name": "deny_admin_ops",
      "resources": ["table", "database", "pipeline"],
      "operations": ["Delete", "HardDelete"],
      "effect": "deny"
    }
  ]
}
```
> Asignar la **Policy** al **Role** `L3_Sensitive_Custodian` y mapearlo a grupos del IdP (Okta/Azure).

---

## 6) Ingesta de metadatos (DB/BI/Pipelines)
### 6.1. YAML de **metadata ingestion** (PostgreSQL)
```yaml
source:
  type: postgres
  serviceName: pg-autonova
  serviceConnection:
    config:
      type: Postgres
      username: om_reader
      password: ${POSTGRES_PASSWORD}
      hostPort: postgres-host:5432
      database: autonova_dw
  sourceConfig:
    config:
      includeTables: true
      includeViews: true

sink:
  type: metadata-rest
  config: {}

workflowConfig:
  openMetadataServerConfig:
    hostPort: http://openmetadata:8585/api
    authProvider: basic
    securityConfig:
      jwtToken: ${OM_TOKEN}
```
**CLI**:  
```bash
metadata ingest -c postgres_metadata.yaml
```
> Para **Profiler** se usa `metadata profile` (ver sección 7).

### 6.2. Uso (usage) y otros conectores
- **Usage Workflow** para popular estadísticas de uso/queries.  
- Conectores para **S3/Datalake**, **BI** (Power BI/Looker), **Pipelines** (Airflow/Prefect).

---

## 7) Data Quality y Profiler
### 7.1. Profiler + Tests vía YAML
```yaml
source:
  type: postgres
  serviceName: pg-autonova
  sourceConfig:
    config:
      type: Profiler
      tableFilterPattern:
        includes:
          - autonova_hr.empleados

processor:
  type: orm-profiler
  config:
    tableConfig:
      - fullyQualifiedName: pg-autonova.autonova_hr.empleados
        profileSample: 0.3
        columnConfig:
          - columnName: salary
            metrics: [min, max, mean, stddev, nullCount]
          - columnName: email
            metrics: [nullCount, distinctCount]
        tests:
          - tableTests:
              - name: row_count_non_zero
                testDefinitionName: tableRowCountToBeBetween
                parameterValues:
                  - name: minValue
                    value: 1
          - columnTests:
              - columnName: email
                testDefinitionName: columnValuesToMatchRegex
                parameterValues:
                  - name: regex
                    value: "^[^@\s]+@[^@\s]+\.[^@\s]+$"
              - columnName: salary
                testDefinitionName: columnValuesToBeBetween
                parameterValues:
                  - name: minValue
                    value: 1
                  - name: maxValue
                    value: 1000000

sink:
  type: metadata-rest
  config: {}

workflowConfig:
  openMetadataServerConfig:
    hostPort: http://openmetadata:8585/api
    authProvider: basic
    securityConfig:
      jwtToken: ${OM_TOKEN}
```
**CLI**:  
```bash
metadata profile -c profiler_empleados.yaml
```
> Los **tests soportados** incluyen table/column tests (nulls, ranges, regex, uniqueness, freshness, etc.).

---

## 8) Lineage (dbt, Airflow, Spark) con **OpenLineage**
### 8.1. Ingesta desde OpenLineage
- Configurar pipelines para **emitir eventos** OpenLineage; OpenMetadata integra el conector para recibir/consultar lineage.  
- **Spark** (ejemplo `spark-submit` o `spark-defaults.conf`):
```properties
spark.extraListeners=io.openlineage.spark.agent.OpenLineageSparkListener
spark.openlineage.transport.type=http
spark.openlineage.transport.url=http://openlineage-backend:5000
spark.openlineage.namespace=autonova_spark
```
- **Airflow/dbt**: habilitar **OpenLineage** en operadores y ejecutar el conector de OM para sincronizar lineage.

---

## 9) Autenticación (SSO) y MFA
- **OIDC/SAML** con **Okta** o **Azure AD**. Configurar flujo **Auth Code** (apps confidenciales) o **implícito** (SPA) según topología.  
- **MFA** se **aplica en el IdP** y repercute en el inicio de sesión de OM. Mapear **grupos** a **Roles** de OM (N2/N3/N4, Stewards, Admin).

---

## 10) Operación y scheduling
- **UI de OM**: creación de *Services*, *Connections* y *Ingestion Pipelines* (programación periódica).  
- **Airflow managed APIs** o contenedor `openmetadata/ingestion` cuando se orquesta desde la UI.  
- **CLI**: `metadata ingest` (metadatos), `metadata profile` (profiler + tests).  
- **Buenas prácticas**: versionar YAML, *secrets* externos (ENV/Secret Manager), pipelines separados (metadata/usage/profiler/lineage).

---

## 11) Plantillas rápidas (copiar/pegar)

### 11.1. Classification N1–N4 (JSON)
```json
{
  "name": "DataClassification",
  "displayName": "Data Classification",
  "description": "Sensitivity levels (N1–N4) per AutoNova policy.",
  "mutuallyExclusive": true
}
```
```json
{ "name": "Level1", "displayName": "Level 1 - Público", "classification": "DataClassification" }
```
```json
{ "name": "Level2", "displayName": "Level 2 - Interno", "classification": "DataClassification" }
```
```json
{ "name": "Level3", "displayName": "Level 3 - Sensible", "classification": "DataClassification" }
```
```json
{ "name": "Level4", "displayName": "Level 4 - Altamente Sensible", "classification": "DataClassification" }
```

### 11.2. Policy mínima (deny‑delete)
```json
{
  "name": "policy_read_write_metadata_no_delete",
  "rules": [
    { "name": "view_all", "resources": ["*"], "operations": ["ViewAll"], "effect": "allow" },
    { "name": "edit_tags", "resources": ["tag", "glossary"], "operations": ["EditAll"], "effect": "allow" },
    { "name": "no_delete", "resources": ["*"], "operations": ["Delete", "HardDelete"], "effect": "deny" }
  ]
}
```

### 11.3. Test Suite (tabla `empleados`)
- **notNull:** `email`  
- **regex:** `email` patrón correo  
- **range:** `salary` 1..1,000,000  
- **rowCount:** `>= 1`  

> Ver YAML completo en §7.1.

### 11.4. Spark → OpenLineage (props)
```properties
spark.extraListeners=io.openlineage.spark.agent.OpenLineageSparkListener
spark.openlineage.transport.type=http
spark.openlineage.transport.url=http://openlineage-backend:5000
spark.openlineage.namespace=autonova_spark
```

---

## 12) Glosario (herramientas y conceptos)
- **OpenMetadata (OM):** Plataforma unificada OSS para catálogo, gobierno y observabilidad de datos.  
- **Entity Store:** DB relacional para entidades/relaciones.  
- **Search Engine:** Elasticsearch/OpenSearch para indexación y búsqueda.  
- **Service / Connector:** Definición de conexión a fuente (DB/BI/Pipeline/Storage).  
- **Classification / Tag:** Taxonomía de etiquetas; se aplica a entidades/columnas.  
- **Glossary / GlossaryTerm:** Términos de negocio vinculados a activos.  
- **Role / Policy / Rule:** Construcciones RBAC para permisos en **metadatos**.  
- **Profiler & Data Quality:** Métricas y pruebas (tabla/columna) ejecutadas por el Ingestion Framework.  
- **OpenLineage:** Estándar abierto para eventos de lineage; OM consume/integra lineage emitido por pipelines.  
- **OIDC/SSO:** Integración con IdP (Okta/Azure AD) para autenticación centralizada y MFA.  
- **CLI `metadata`:** Ejecuta *ingest* y *profile* con archivos YAML.

---

## 13) Sugerencias de operación (AutoNova)
- Versionar **YAML/JSON** en un repo `governance/` con carpetas por conector (db/bi/pipeline).  
- Usar **nombres de servicio** estandarizados (`pg-autonova`, `bq-autonova`, `powerbi-autonova`).  
- Pipeline de **auto‑clasificación**: al detectar `PII.*` en columnas, aplicar `DataClassification.Level3/4`.  
- **Dashboards** en OM: paneles por dominio (`RRHH`, `Proveedores`, `Ventas`) y por nivel (N2/N3/N4).  
- **Auditoría**: exportar resultados de **tests DQ** y **cambios de tags** a SIEM; alertas por fallas críticas.

---

## 14) Fuentes (en línea)
- **RBAC (Roles/Policies/Rules):** Docs OM — Roles & Policies (Authorization).  
- **Ingestion Workflows (YAML):** Esquema de *workflow* y *usage*; CLI `metadata ingest`.  
- **Data Quality & Profiler:** Tests en YAML, Profiler Workflow.  
- **Glossary / Create Terms:** Guía de términos; API `createGlossaryTerm`.  
- **Classification / Tags:** Overview, API `createClassification`, `createTag`, Python SDK for Tags.  
- **Lineage (OpenLineage):** Conector OM + guías Spark/OpenLineage.  
- **SSO:** Azure AD, Okta, OIDC.  
- **Requisitos & Arquitectura:** Requisitos mínimos, diseño de alto nivel, helm/docker.

> Para facilitar auditoría interna, mantiene estos enlaces en la wiki de AutoNova y actualízalos al ritmo de upgrades de OM.
