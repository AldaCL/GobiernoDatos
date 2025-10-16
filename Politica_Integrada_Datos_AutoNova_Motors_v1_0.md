# Política Integrada de Datos — AutoNova Motors
**Calidad • Clasificación • Acceso**  
**Versión 1.0** · **Fecha:** 16 de octubre de 2025 · **Propietario:** CDO (Chief Data Officer)

---

## Resumen Ejecutivo
Esta política integra y normaliza los lineamientos de **Calidad**, **Clasificación** y **Acceso** para asegurar la **confidencialidad, integridad, disponibilidad y trazabilidad** de los datos de AutoNova Motors. Se establecen principios claros, niveles de sensibilidad (1–4), criterios de calidad, controles de acceso (RBAC), retención/eliminación segura, auditoría y responsabilidades (RACI) a fin de habilitar su traducción inmediata a controles técnicos.

---

## 1. Propósito y Alcance
**Propósito:** Proteger y gobernar los datos de AutoNova Motors mediante reglas corporativas uniformes que reduzcan riesgos regulatorios, operativos y reputacionales.  
**Alcance:** Aplica a toda información en formato digital o físico, en instalaciones *on‑premise* o nube, así como repositorios documentales y sistemas analíticos. Es de cumplimiento obligatorio para empleados, contratistas y terceros con acceso a información corporativa.  
*Fuente principal: Clasificación / Múltiples*

---

## 2. Definiciones Clave
- **Proveedor:** Persona moral o física con relación comercial con AutoNova Motors.  
- **Empleado:** Persona física que presta sus servicios a AutoNova Motors.  
- **Licitación:** Procedimiento de competencia para adjudicación de contratos.  
- **Cumplimiento Regulatorio:** Conjunto de acciones y controles para dar cumplimiento a regulaciones aplicables.  
- **Data Steward:** Responsable de un conjunto de datos (propiedad operativa, clasificación, permisos y trazabilidad).  
*Fuente principal: Clasificación*

---

## 3. Principios Rectores
**3.1 Responsabilidad y Propiedad (Accountability):** Cada conjunto de datos crítico tendrá un **Data Steward** registrado en el Catálogo de Datos.  
**3.2 Privilegio mínimo y segregación de funciones:** El acceso se otorga según necesidad de negocio; se separan solicitud, aprobación, implementación y auditoría.  
**3.3 Autenticación robusta:** La información de niveles **3–4** requiere **MFA** y controles reforzados.  
**3.4 Trazabilidad:** Toda acción de lectura/escritura en niveles **3–4** debe quedar registrada en bitácoras seguras.  
**3.5 Seguridad por Diseño:** La clasificación y los controles se incorporan desde el inicio del **SDLC** y el diseño de procesos.  
*Fuentes: Clasificación / Acceso — Equipo 1*

---

## 4. Clasificación de Datos
Se establecen cuatro niveles de sensibilidad con descripción y ejemplos:
- **Nivel 1 — Público:** Información que puede divulgarse sin riesgo.  
- **Nivel 2 — Interno:** Información operativa y administrativa sin PII, impacto bajo si se divulga.  
- **Nivel 3 — Sensible:** PII, datos financieros o estratégicos de impacto moderado.  
- **Nivel 4 — Altamente Sensible:** PII o confidencial de alto impacto (salarios, nómina, secretos técnicos).  

Todo activo (tablas, vistas, archivos, reportes) debe portar **etiqueta de clasificación** (p. ej., `CLASS=LEVEL{1..4}`) en su metadato o nombre; revisión **anual** o ante cambios de proceso.  
*Fuente principal: Clasificación*

---

## 5. Control de Acceso (RBAC)
- Todo objeto de información debe tener **nivel (1–4)** asociado.  
- Accesos a **niveles 3–4** requieren **autorización formal del CDO** y **MFA** en sistemas críticos.  
- **Revocación automática** al fin de la relación laboral/contractual; **prohibido** compartir credenciales o usar cuentas genéricas.  
- **Logs de acceso** se conservan **12 meses**; auditoría interna revisa su integridad y patrón de uso.  
- **Ejemplos:** Nómina (N4), Contratos (N3), Compliance (N3), Manuales/Reportes operativos (N2).  
*Fuente principal: Acceso — Equipo 1*

---

## 6. Calidad de Datos
- **Dimensiones:** Completitud, Precisión, Consistencia, Unicidad y Actualidad; se definen umbrales por entidad y campo crítico.  
- **Gobernanza operativa:** El **Comité de Gobernanza** mide y reporta la calidad; las **áreas propietarias** corrigen y previenen recurrencias.  
- **Proceso de incidentes:** Detección → Reporte → Evaluación → Corrección → Verificación → Cierre con lecciones aprendidas.  
- **Áreas/tablas críticas (ejemplos):** Empleados, Clientes, Proveedores, Vehículos en Inventario, Ventas, con riesgos asociados por dato incorrecto o desactualizado.  
*Fuente principal: Calidad*

---

## 7. Retención y Eliminación
**Objetivo:** Conservar la información el **tiempo legal/operativo** necesario y eliminarla de forma **segura** al cierre del ciclo de vida.  
**Reglas:** Asignar plazos por entidad y tipo de dato, métodos de eliminación (borrado seguro, **anonimización**, hash) y roles responsables; los datos altamente sensibles se destruyen o anonimizan al finalizar su uso.  
*Fuentes: Clasificación / Acceso — Equipo 1*

---

## 8. Monitoreo, Auditoría y Sanciones
- **Monitoreo técnico:** **DLP** para salida no autorizada; **SIEM** para correlación y detección de accesos anómalos en N3–N4.  
- **Auditorías:** Anual por Auditoría Interna; revisiones **semestrales** de accesos por Stewards; informes al Comité de Gobernanza y dirección ejecutiva.  
- **Incumplimientos:** Definición de violación y medidas proporcionales (advertencia, suspensión, terminación en casos maliciosos).  
*Fuente principal: Clasificación*

---

## 9. Roles y Responsabilidades (RACI)
| Rol               | Clasificación | Acceso | Calidad        | Auditoría/Monitoreo |
|-------------------|---------------|--------|----------------|----------------------|
| **CDO**           | A/R           | A/R    | C              | C                    |
| **Data Steward**  | R             | R      | R              | C                    |
| **Ingeniería/TI** | C             | R      | C              | R                    |
| **CISO/Seguridad**| C             | C      | C              | R                    |
| **Auditoría Interna** | I         | C      | I              | A/R                  |
| **Usuarios/Gerencias** | I        | I      | C (corrige)    | I                    |

*A = Aprueba, R = Responsable, C = Consulta, I = Informa*  
*Fuentes: Acceso — Equipo 1 / Clasificación / Calidad*

---

## 10. Gestión de Excepciones y Solicitudes
Las **excepciones** deben documentarse con justificación, vigencia y controles compensatorios; requieren **aprobación del CDO**. Todas las **solicitudes de acceso** (altas/cambios/bajas) se registran y conservan como evidencia de cumplimiento.

---

## 11. Capacitación y Concientización
Se establecen **capacitaciones anuales** obligatorias para todo el personal sobre identificación, clasificación y manejo seguro de información según su nivel. Materiales y política estarán disponibles en la intranet corporativa.  
*Fuente principal: Clasificación*

---

## 12. Gobernanza de la Política (Revisión y Cambios)
La política se **revisará anualmente** o ante cambios regulatorios/organizacionales relevantes. La versión vigente y su historial de cambios estarán publicados en la intranet.

---

## 13. Notas de Referenciación
- **Clasificación:** “AutoNova Motors.docx” — niveles 1–4, Seguridad por Diseño, DLP/SIEM, roles y ejemplos por dominio.  
- **Acceso — Equipo 1:** “Política de Acceso a Datos – AutoNova Motors (Equipo 1)” — MFA, privilegio mínimo, segregación de funciones, logs 12 meses, revocación automática, ejemplos.  
- **Calidad:** “AutoNova Motors” (calidad) — dimensiones de calidad, entidades/columnas clave y proceso de incidentes.

---

## Apéndices

### A. Convenciones de Etiquetado Técnico
- Metadato `CLASS=LEVEL{1..4}` aplicado en catálogo, esquema y objeto (tabla, vista, archivo BI).  
- Nomenclatura de esquemas/carpetas por sensibilidad (ej., `L2_ops`, `L3_sensitive`).

### B. Flujo de Solicitud de Acceso (resumen)
1. Solicitud  
2. Validación de necesidad y nivel  
3. Aprobación (CDO para N3–N4)  
4. Implementación (TI)  
5. Registro y notificación  
6. Revisión periódica  
7. Baja/Revocación

### C. Flujo de Incidente de Calidad de Datos
Detección → Reporte → Evaluación (clasificación de severidad) → Corrección por área propietaria → Verificación → Cierre y prevención.
