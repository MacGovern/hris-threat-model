# 07 — Controles de Seguridad: NIST SP 800-53 e ISO 27001

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 8.1 Introducción

Este documento mapea los controles de seguridad propuestos en el plan de mitigación con los marcos normativos de referencia:

- **NIST SP 800-53 Rev. 5:** Catálogo de controles de seguridad del gobierno de EE.UU., ampliamente adoptado como estándar internacional.
- **ISO/IEC 27001:2022 (Anexo A):** Controles de gestión de la seguridad de la información del estándar internacional.

El mapeo permite verificar la cobertura normativa del sistema y facilitar auditorías de cumplimiento.

---

## 8.2 Controles NIST SP 800-53

| ID Control | Control                         | Descripción                                            | Amenaza mitigada |
|------------|---------------------------------|--------------------------------------------------------|------------------|
| AC-1       | Access Control Policy           | Definición y documentación de políticas de control de acceso | TH06        |
| AC-2       | Account Management              | Ciclo de vida de cuentas: creación, revisión, desactivación | TH01, TH06  |
| AC-3       | Access Enforcement              | Implementación del modelo de control de acceso (RBAC) | TH06            |
| AC-6       | Least Privilege                 | Asignación del mínimo privilegio necesario por rol    | TH02, TH06       |
| AC-7       | Unsuccessful Login Attempts     | Bloqueo automático tras intentos fallidos de login    | TH01             |
| AC-17      | Remote Access                   | Control y monitoreo del acceso remoto al sistema      | TH01, TH07       |
| IA-2       | Identification and Authentication | Implementación de autenticación multifactor (MFA)  | TH01             |
| IA-5       | Authenticator Management        | Gestión de contraseñas y tokens; política de rotación | TH01            |
| AU-2       | Event Logging                   | Definición de eventos que deben registrarse           | TH03             |
| AU-3       | Content of Audit Records        | Contenido mínimo de cada registro de auditoría        | TH03             |
| AU-6       | Audit Record Review             | Revisión y análisis periódico de logs                 | TH03, TH05       |
| AU-9       | Protection of Audit Information | Protección de integridad de los logs                  | TH03             |
| SC-8       | Transmission Confidentiality    | Protección de datos en tránsito mediante TLS          | TH04, TH08       |
| SC-28      | Protection of Info. at Rest     | Cifrado de información almacenada en base de datos    | TH04             |
| SC-5       | Denial-of-Service Protection    | Controles para mitigar ataques de denegación de servicio | TH05          |
| SI-4       | System Monitoring               | Monitoreo continuo de eventos de seguridad            | TH01, TH05       |
| SI-10      | Information Input Validation    | Validación de entradas en todos los puntos de la API  | TH06, TH10       |
| CP-9       | System Backup                   | Backups periódicos y verificados de base de datos     | TH05             |
| CP-10      | System Recovery                 | Plan de recuperación ante incidentes                  | TH05             |
| IR-4       | Incident Handling               | Procedimientos de respuesta a incidentes              | General          |
| PS-4       | Personnel Termination           | Revocación inmediata de accesos al desvincular empleo | TH01, TH02       |
| SA-11      | Developer Testing               | Testing de seguridad en el ciclo de desarrollo        | TH06, TH10       |

---

## 8.3 Relación con ISO/IEC 27001:2022

### Controles del Anexo A aplicables

| Ref. ISO 27001 | Control                                       | Relación con el HRIS                              |
|----------------|-----------------------------------------------|---------------------------------------------------|
| A.5.14         | Information transfer                          | Protección de datos en tránsito con proveedores externos |
| A.5.15         | Access control                                | Política de control de acceso (RBAC)              |
| A.5.16         | Identity management                           | Gestión de identidades de empleados y administradores |
| A.5.17         | Authentication information                   | Política de contraseñas y MFA                     |
| A.5.18         | Access rights                                 | Principio de mínimo privilegio y revisión periódica |
| A.5.23         | Information security for cloud services      | Gestión de proveedores externos (nómina, firma)    |
| A.5.26         | Response to information security incidents   | Plan de respuesta a incidentes                     |
| A.5.28         | Collection of evidence                       | Preservación de logs para investigación            |
| A.5.33         | Protection of records                        | Integridad y disponibilidad de registros de auditoría |
| A.6.3          | Information security awareness training      | Capacitación en phishing e ingeniería social       |
| A.7.1          | Physical and environmental security perimeters | Protección del entorno donde reside Oracle DB    |
| A.8.2          | Privileged access rights                     | Gestión y revisión de cuentas privilegiadas        |
| A.8.5          | Secure authentication                        | MFA e identificación robusta                       |
| A.8.7          | Protection against malware                   | Protección en componentes de la aplicación         |
| A.8.11         | Data masking                                 | Enmascaramiento de datos sensibles en entornos no productivos |
| A.8.12         | Data leakage prevention                      | DLP para prevenir exfiltración de datos            |
| A.8.15         | Logging                                      | Registro de eventos críticos del sistema           |
| A.8.17         | Clock synchronization                        | Sincronización NTP para trazabilidad de logs       |
| A.8.24         | Use of cryptography                          | Cifrado TLS y Oracle TDE                           |

---

## 8.4 Áreas de Control por Objetivo de Seguridad

| Área de control            | Objetivo de seguridad | Controles NIST              | Controles ISO 27001          |
|----------------------------|-----------------------|-----------------------------|------------------------------|
| Control de acceso          | Autenticación         | AC-2, AC-3, AC-7, IA-2, IA-5 | A.5.15, A.5.16, A.5.17, A.8.5 |
| Gestión de privilegios     | Autorización          | AC-6, AC-17                 | A.5.18, A.8.2                |
| Protección de datos        | Confidencialidad      | SC-8, SC-28                 | A.8.24, A.8.12, A.8.11       |
| Logging y monitoreo        | Detectabilidad        | AU-2, AU-3, AU-6, AU-9, SI-4 | A.8.15, A.8.17, A.5.28      |
| Continuidad operativa      | Disponibilidad        | CP-9, CP-10, SC-5           | (no mapeado específicamente) |
| Gestión de incidentes      | Respuesta             | IR-4                        | A.5.26                       |
| Seguridad de RRHH          | Prevención interna    | PS-4                        | A.6.3                        |
| Proveedores externos       | Cadena de suministro  | —                           | A.5.23                       |

---

## 8.5 Gap Analysis (Brechas Identificadas)

Los siguientes controles normativos no están actualmente implementados y representan brechas de cumplimiento:

| Brecha | Control faltante               | Normativa | Amenaza relacionada | Prioridad |
|--------|-------------------------------|-----------|---------------------|-----------|
| G01    | MFA no implementado           | IA-2 / A.8.5 | TH01           | 🔴 Alta   |
| G02    | Logs sin protección de integridad | AU-9 / A.8.15 | TH03        | 🔴 Alta   |
| G03    | Sin cifrado en reposo         | SC-28 / A.8.24 | TH04        | 🔴 Alta   |
| G04    | RBAC no revisado/documentado  | AC-3 / A.5.15 | TH06         | 🔴 Alta   |
| G05    | Sin plan formal de incidentes | IR-4 / A.5.26 | General      | 🟡 Media  |
| G06    | Sin capacitación documentada  | — / A.6.3   | TH01, TH07     | 🟡 Media  |
| G07    | Sin gestión de proveedores ext.| — / A.5.23 | TH08, TH09    | 🟡 Media  |

---

## 8.6 Resumen

Los controles seleccionados se enfocan principalmente en:

- **Autenticación y autorización:** bloque fundamental dado que TH01 (credenciales comprometidas) es la amenaza más crítica.
- **Protección de información sensible:** cifrado en tránsito y en reposo para datos PII, salariales y contractuales.
- **Monitoreo y auditoría:** capacidad de detección y trazabilidad de eventos para no repudio y respuesta a incidentes.
- **Continuidad operativa:** backups, recuperación y procedimientos de respuesta.

La implementación de estos controles permite alinear el sistema HRIS con los marcos normativos NIST SP 800-53 e ISO 27001, reduciendo el riesgo operativo y mejorando la postura de seguridad general de la organización.
