# 03 — Análisis de Amenazas: Metodología STRIDE

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 4.1 Introducción a STRIDE

STRIDE es una metodología de análisis de amenazas desarrollada por Microsoft que clasifica las amenazas en seis categorías, cada una asociada a un objetivo de seguridad afectado:

| Categoría | Objetivo de seguridad afectado | Pregunta clave |
|-----------|-------------------------------|----------------|
| **S**poofing | Autenticación | ¿Puede alguien hacerse pasar por otro? |
| **T**ampering | Integridad | ¿Puede alguien modificar datos o código? |
| **R**epudiation | No repudio | ¿Puede alguien negar haber realizado una acción? |
| **I**nformation Disclosure | Confidencialidad | ¿Puede alguien acceder a información que no debería ver? |
| **D**enial of Service | Disponibilidad | ¿Puede alguien impedir el uso del sistema? |
| **E**levation of Privilege | Autorización | ¿Puede alguien obtener permisos que no le corresponden? |

---

## 4.2 Aplicación de STRIDE al HRIS

| Categoría              | Objetivo afectado   | Ejemplo en el HRIS                          |
|------------------------|---------------------|---------------------------------------------|
| Spoofing               | Autenticación       | Uso de credenciales robadas para acceder    |
| Tampering              | Integridad          | Modificación de registros de nómina         |
| Repudiation            | No repudio          | Negación de haber firmado un contrato       |
| Information Disclosure | Confidencialidad    | Exposición de salarios de empleados         |
| Denial of Service      | Disponibilidad      | Caída del portal de empleados               |
| Elevation of Privilege | Autorización        | Acceso indebido a funciones administrativas |

---

## 4.3 Matriz de Amenazas por Componente

| ID    | Flujo / Componente           | STRIDE | Amenaza                                  | Contramedida inicial        |
|-------|------------------------------|--------|------------------------------------------|-----------------------------|
| TH01  | Portal Empleado → Backend    | S      | Uso de credenciales comprometidas        | MFA                         |
| TH02  | Backend → Oracle DB          | T      | Manipulación de registros salariales     | RBAC + auditoría            |
| TH03  | Portal RRHH                  | R      | Negación de modificaciones realizadas    | Logs inmutables             |
| TH04  | Oracle DB                    | I      | Exposición de datos sensibles            | Cifrado                     |
| TH05  | API REST                     | D      | Saturación de requests (DoS)             | Rate limiting               |
| TH06  | Backend Java/Spring          | E      | Escalada de privilegios                  | Validación de roles         |
| TH07  | Portal RRHH → Backend        | S      | Suplantación de identidad de gestor RRHH | MFA + alertas de acceso     |
| TH08  | Backend → Sistema Nómina ext.| T      | Manipulación de datos en tránsito        | TLS + validación de respuesta|
| TH09  | Módulo de firma digital      | R      | Firma de contrato sin consentimiento     | Confirmación explícita      |
| TH10  | API REST                     | I      | Fuga de datos en respuestas de API       | Filtrado de campos sensibles|

---

## 4.4 Detalle de Amenazas Principales

---

### AMENAZA TH01 — Acceso mediante credenciales comprometidas

**Categoría STRIDE:** Spoofing  
**Componente afectado:** Portal de Empleados → Backend API

**Descripción:**  
Un atacante utiliza credenciales robadas o reutilizadas para acceder al portal de empleados o al portal de RRHH. Las credenciales pueden obtenerse mediante phishing, relleno de credenciales (credential stuffing) o compra en mercados de datos robados. Una vez dentro, el atacante accede a información sensible y funcionalidades internas sin generar alertas inmediatas.

**Activos afectados:** A01, A02, A03, A04

**Probabilidad:** Alta  
**Impacto:** Alto

**Vector de ataque típico:**
1. Atacante obtiene credenciales mediante phishing a empleado
2. Accede al portal web con credenciales válidas
3. Navega por información salarial, contratos y datos de otros empleados
4. Exfiltra información sin disparar alertas (parece comportamiento legítimo)

**Mitigaciones propuestas:**
- Implementar MFA para todos los accesos
- Políticas de contraseñas robustas (longitud mínima, complejidad, no reutilización)
- Detección de accesos anómalos (geolocalización, horario, dispositivo)
- Bloqueo automático por intentos fallidos
- Notificaciones de login al usuario legítimo

**Técnicas ATT&CK relacionadas:** T1078 (Valid Accounts), T1566 (Phishing)

---

### AMENAZA TH02 — Manipulación de información salarial

**Categoría STRIDE:** Tampering  
**Componente afectado:** Backend → Oracle DB (Módulo Nómina)

**Descripción:**  
Un usuario con permisos indebidos —ya sea por acceso comprometido o por falla en el modelo de autorización— modifica salarios, beneficios o información de nómina. El ataque puede ser oportunista (aprovechando un error de configuración) o intencional (insider threat con acceso legítimo pero abusivo).

**Activos afectados:** A02, A05, T04

**Probabilidad:** Media  
**Impacto:** Alto

**Vector de ataque típico:**
1. Usuario con acceso RRHH (o atacante con credenciales comprometidas) accede al módulo de nómina
2. Modifica valores salariales directamente en el sistema
3. El cambio pasa a procesamiento sin revisión adicional
4. Los logs pueden no capturar la modificación si el sistema tiene fallas de auditoría

**Mitigaciones propuestas:**
- Validaciones de integridad en el backend (rangos salariales, doble aprobación)
- Segregación de funciones (quien ingresa la nómina ≠ quien la aprueba)
- Auditoría completa de todos los cambios en datos financieros
- Control de acceso basado en roles (RBAC) estricto

**Técnicas ATT&CK relacionadas:** T1098 (Account Manipulation), T1078 (Valid Accounts)

---

### AMENAZA TH03 — Negación de acciones realizadas

**Categoría STRIDE:** Repudiation  
**Componente afectado:** Portal RRHH / Sistema de Logs

**Descripción:**  
Un usuario o administrador niega haber realizado modificaciones sobre contratos, evaluaciones o liquidaciones. Esto es posible cuando los registros de auditoría son insuficientes, alterables o no cubren todas las acciones críticas del sistema. También aplica a la firma de contratos cuando el proceso no deja evidencia suficientemente robusta.

**Activos afectados:** A05, T01, T02

**Probabilidad:** Media  
**Impacto:** Medio

**Mitigaciones propuestas:**
- Logs centralizados e inmutables (almacenamiento append-only)
- Timestamps sincronizados con NTP confiable
- Registro de acciones críticas (quién, qué, cuándo, desde dónde)
- Firma digital de contratos con evidencia criptográfica
- SIEM para correlación y preservación de eventos

**Técnicas ATT&CK relacionadas:** T1070 (Indicator Removal on Host)

---

### AMENAZA TH04 — Exposición de datos sensibles

**Categoría STRIDE:** Information Disclosure  
**Componente afectado:** Oracle DB / API REST

**Descripción:**  
Información confidencial de empleados, contratos y evaluaciones es expuesta debido a fallas de autorización, configuraciones inseguras, errores en la API (exceso de datos en respuestas) o accesos indebidos a la base de datos. Esta amenaza puede materializarse tanto por actores externos como por insiders.

**Activos afectados:** A01, A02, A03, A05

**Probabilidad:** Media  
**Impacto:** Alto

**Vector de ataque típico (externo):**
1. Atacante identifica endpoint de API sin validación de autorización adecuada
2. Realiza queries con IDs de otros usuarios (IDOR - Insecure Direct Object Reference)
3. Obtiene datos salariales o personales de múltiples empleados

**Vector de ataque típico (interno):**
1. Empleado con acceso legítimo exporta datos más allá de sus responsabilidades
2. Exfiltración silenciosa sin alertas

**Mitigaciones propuestas:**
- Cifrado de datos en tránsito (TLS 1.2+) y en reposo (Oracle TDE)
- Principio de mínimo privilegio en permisos de base de datos
- Hardening de base de datos Oracle
- Revisión periódica de permisos de acceso
- Filtrado de campos sensibles en respuestas de API (no exponer campos innecesarios)
- Protección contra IDOR con validación de ownership

**Técnicas ATT&CK relacionadas:** T1005 (Data from Local System), T1213 (Data from Information Repositories)

---

### AMENAZA TH05 — Denegación de servicio sobre la plataforma HRIS

**Categoría STRIDE:** Denial of Service  
**Componente afectado:** API REST / Aplicación Web

**Descripción:**  
Un atacante genera un volumen excesivo de solicitudes HTTP o consume recursos críticos del servidor, afectando la disponibilidad del portal de empleados y los servicios internos. Esto puede impactar directamente en procesos críticos como el cierre de nómina o el onboarding de nuevos empleados.

**Activos afectados:** T01, T02, T03

**Probabilidad:** Media  
**Impacto:** Medio

**Mitigaciones propuestas:**
- Rate limiting por IP y por usuario
- Balanceo de carga y escalado automático
- Monitoreo continuo de tráfico y métricas de recursos
- Alertas automáticas ante anomalías de tráfico
- CDN para absorber tráfico en el frontend

**Técnicas ATT&CK relacionadas:** T1499 (Endpoint Denial of Service)

---

### AMENAZA TH06 — Escalada de privilegios

**Categoría STRIDE:** Elevation of Privilege  
**Componente afectado:** Backend Java/Spring

**Descripción:**  
Un usuario obtiene permisos superiores a los autorizados debido a errores de configuración en el modelo de autorización, vulnerabilidades en el backend o explotación de funcionalidades no protegidas adecuadamente. Un empleado podría acceder a funciones de RRHH o un usuario de RRHH podría acceder a funciones administrativas.

**Activos afectados:** T01, T02, T03

**Probabilidad:** Media  
**Impacto:** Alto

**Mitigaciones propuestas:**
- Revisión y auditoría del modelo de roles y permisos
- Validación de autorización en el backend (no solo en el frontend)
- Principio de mínimo privilegio aplicado a cada rol
- Revisión periódica de accesos privilegiados
- Tests de penetración sobre el módulo de autorización

**Técnicas ATT&CK relacionadas:** T1068 (Exploitation for Privilege Escalation), T1078 (Valid Accounts)

---

## 4.5 Resumen de la Matriz STRIDE

| Categoría STRIDE       | Amenazas identificadas           | Nivel de riesgo predominante |
|------------------------|----------------------------------|------------------------------|
| Spoofing               | TH01, TH07                       | Alto / Crítico               |
| Tampering              | TH02, TH08                       | Alto                         |
| Repudiation            | TH03, TH09                       | Medio                        |
| Information Disclosure | TH04, TH10                       | Alto                         |
| Denial of Service      | TH05                             | Medio / Alto                 |
| Elevation of Privilege | TH06                             | Alto                         |
