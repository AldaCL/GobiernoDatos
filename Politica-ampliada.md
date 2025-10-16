# 🚗 Política Integrada de Datos — AutoNova Motors

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

---

## 14. Matriz de Controles por Nivel (N1–N4)

> **Objetivo:** Alinear controles organizativos y técnicos con el nivel de sensibilidad para asegurar proporcionalidad y cumplimiento.

| Categoría de control | N1 — Público | N2 — Interno | N3 — Sensible | N4 — Altamente Sensible |
|---|---|---|---|---|
| **Identidad y autenticación** | Cuenta nominal; autenticación básica | Cuenta nominal obligatoria; expiración de contraseñas | **MFA obligatorio** en sistemas críticos; política de contraseñas reforzada | **MFA + políticas de riesgo** (geolocalización, dispositivo); SSO corporativo |
| **Autorización (RBAC/ABAC)** | Roles genéricos por área | RBAC por función | RBAC + **principio de privilegio mínimo**; **segregación de funciones** | RBAC+ABAC por contexto; **aprobación CDO** para altas/cambios |
| **Seguridad de datos en tránsito** | TLS 1.2+ recomendado | **TLS 1.2+ requerido** | **TLS 1.2+ requerido** con *cipher suites* fuertes | **TLS 1.3** donde esté disponible; pinning cuando aplique |
| **Seguridad de datos en reposo** | Cifrado at‑rest recomendado | Cifrado at‑rest recomendado | **Cifrado at‑rest obligatorio** (KMS) | **Cifrado at‑rest obligatorio** con gestión de llaves segregada |
| **Control a nivel de datos** | n/a | Enmascaramiento estático opcional | **Enmascaramiento dinámico**, **Column‑Level Security (CLS)** | **Row‑Level Security (RLS)** + CLS + tokenización/seudonimización |
| **Trazabilidad (logging)** | Accesos administrativos | Accesos y cambios relevantes | **Lectura/escritura** por usuario, origen y propósito; **retención ≥12m** | Lo de N3 + **integridad de logs** (WORM/append‑only) |
| **Monitoreo y detección** | Alertas básicas | SIEM con reglas estándar | **SIEM** con reglas de comportamiento; **DLP** para exfiltración | **DLP avanzado** + correlación con IAM/EDR; *playbooks* de respuesta |
| **Backups y recuperación** | Backups programados | Backups con pruebas de restauración | RPO/RTO definidos; cifrado de backups | RPO/RTO estrictos; restauración validada y segregada |
| **Retención y destrucción** | Según cuadro de retención | Según cuadro de retención | Según cuadro de retención + **anonimización/purge** al cierre | Según cuadro de retención + **anonimización/expurgo seguro** |
| **Transferencia/compartición** | Publicación aprobada | Solo internos; banner de uso | Con terceros: **NDA**, canal seguro, registro de entrega | **Prohibido por defecto**; excepción con **CDO+CISO** |
| **Desarrollo (SDLC)** | Clasificación opcional | Clasificación temprana | **Seguridad por Diseño**; *threat modeling* | Lo de N3 + **Privacy Impact Assessment** y pruebas de seguridad reforzadas |

*Fuentes: Acceso — Equipo 1 / Clasificación / Calidad*

---

## 15. RACI Ampliado por Dominio

> **Objetivo:** Clarificar responsabilidades por dominio de negocio para acelerar decisiones y auditoría.

| Dominio / Rol | CDO | Data Steward (del dominio) | Propietario del Proceso (Dueño de área) | Ingeniería/TI (Plataforma de Datos) | CISO/Seguridad | Compliance/Legal | Auditoría Interna | Gerencia de Área | Usuarios Finales |
|---|---|---|---|---|---|---|---|---|---|
| **Proveedores (Compras)** | A | R | A/R | C/R (controles técnicos) | C | C | I | C | I |
| **Empleados (RRHH/Nómina)** | A | R | A/R | C/R | C | C | I | C | I |
| **Ventas & CRM** | A | R | A/R | C/R | C | I | I | C | I |
| **Inventario & Vehículos** | A | R | A/R | C/R | C | I | I | C | I |
| **Licitaciones/Compras** | A | R | A/R | C/R | C | C | I | C | I |
| **Cumplimiento/Legal** | A | R | A/R | C | C/R (controles de seguridad) | A/R | I | C | I |
| **BI & Analytics** | A | R | A | C/R (governance técnico) | C | I | I | C | I |

*A = Aprueba, R = Responsable, C = Consulta, I = Informa*  
*Fuentes: Acceso — Equipo 1 / Clasificación / Calidad*

---
