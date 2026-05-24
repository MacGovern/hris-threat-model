# 02 — Arquitectura del Sistema y Trust Boundaries

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 3.1 Descripción de la Arquitectura

El sistema HRIS sigue una arquitectura en capas con separación entre el frontend web, el backend de negocio y los sistemas externos. La comunicación entre capas se realiza mediante APIs REST. Los datos sensibles residen en una base de datos Oracle alojada en la red interna.

El sistema es accesible desde Internet (zona pública) para empleados y personal de RRHH autorizados. Los componentes de backend y base de datos se encuentran en una red interna protegida. Las integraciones con servicios externos (sistema de nómina y firma digital) cruzan límites de confianza.

---

## 3.2 Diagrama de Arquitectura

El diagrama de arquitectura se encuentra en:

- **Fuente PlantUML:** [`diagrams/arquitectura-hris.puml`](../diagrams/arquitectura-hris.puml)
- **Imagen renderizada:** [`diagrams/arquitectura-hris.png`](../diagrams/arquitectura-hris.png)

---

## 3.3 Actores del Sistema

| Actor              | Descripción                                      | Privilegios  | Trust Level |
|--------------------|--------------------------------------------------|--------------|-------------|
| Empleado           | Usuario estándar del portal de autoservicio      | Limitados    | Bajo        |
| Personal RRHH      | Gestión de empleados, nóminas y evaluaciones     | Elevados     | Medio       |
| Administrador      | Administración técnica y configuración del sistema | Completos  | Alto        |
| Atacante externo   | Actor malicioso sin acceso previo                | Ninguno      | No confiable|
| Proveedor externo  | Sistemas integrados (nómina, firma digital)      | Limitados    | Medio-Bajo  |

---

## 3.4 Trust Boundaries

Se identifican los siguientes límites de confianza en el sistema:

### TB-1: Zona Pública → Backend (Red Interna)

**Descripción:** Límite entre el frontend web (accesible desde Internet) y el backend Java/Spring en la red interna.

**Riesgo:** Todo tráfico que cruce este límite proviene de actores potencialmente no confiables (empleados remotos, atacantes externos).

**Controles recomendados:**
- Firewall de aplicación web (WAF)
- Validación y sanitización de entradas en el backend
- Autenticación obligatoria (MFA)
- TLS en todas las comunicaciones

---

### TB-2: Backend → Base de Datos Oracle

**Descripción:** Límite entre la capa de lógica de negocio y el almacenamiento de datos sensibles.

**Riesgo:** Acceso indebido a datos de empleados, salarios y contratos si el backend es comprometido.

**Controles recomendados:**
- Conexiones autenticadas con cuentas de servicio con mínimo privilegio
- Cifrado de datos en reposo
- Auditoría de consultas a la base de datos
- Segmentación de red (base de datos en subred aislada)

---

### TB-3: Backend → Servicios Externos

**Descripción:** Límite entre el backend interno y los proveedores externos (sistema de nómina y firma digital).

**Riesgo:** Intercambio de datos sensibles con sistemas externos fuera del control directo de la organización.

**Controles recomendados:**
- Autenticación mutua (mTLS o API keys rotativas)
- Validación de respuestas del servicio externo
- Cifrado en tránsito (TLS 1.2+)
- Registro y monitoreo de todas las transacciones externas

---

### TB-4: Internet → Frontend Web

**Descripción:** Punto de entrada de usuarios desde Internet al portal web.

**Riesgo:** Exposición directa a actores externos no autenticados.

**Controles recomendados:**
- HTTPS obligatorio
- Rate limiting
- Headers de seguridad HTTP (CSP, HSTS, X-Frame-Options)
- Protección contra bots (CAPTCHA en login)

---

## 3.5 Flujos de Datos Principales

| ID  | Flujo                              | Origen         | Destino        | Datos sensibles | Trust Boundary |
|-----|------------------------------------|----------------|----------------|-----------------|----------------|
| F01 | Login de empleado                  | Internet       | Frontend → API | Credenciales    | TB-1, TB-4     |
| F02 | Consulta de nómina                 | Portal RRHH    | API → Oracle   | Salarios, PII   | TB-1, TB-2     |
| F03 | Generación de contrato             | Portal RRHH    | API → Firma    | Contratos, PII  | TB-1, TB-3     |
| F04 | Sincronización con sistema nómina  | Backend        | Nómina ext.    | Datos salariales| TB-3           |
| F05 | Evaluación de desempeño            | Portal RRHH    | API → Oracle   | Evaluaciones    | TB-1, TB-2     |
| F06 | Acceso a perfil de empleado        | Portal Empleado| API → Oracle   | PII             | TB-1, TB-2     |
| F07 | Registro de auditoría              | Backend        | Logs internos  | Eventos sistema | TB-2           |

---

## 3.6 Superficie de Ataque

La superficie de ataque del sistema HRIS incluye:

- **Endpoints públicos:** portal web de empleados y portal RRHH (accesibles desde Internet)
- **API REST:** expuesta al frontend, todos los endpoints deben estar protegidos
- **Integraciones externas:** canales hacia sistema de nómina y proveedor de firma digital
- **Gestión de sesiones:** tokens de sesión, manejo de cookies
- **Módulo de autenticación:** formulario de login, recuperación de contraseñas
- **Base de datos Oracle:** accesible desde el backend, potencial pivot en caso de compromiso del backend
