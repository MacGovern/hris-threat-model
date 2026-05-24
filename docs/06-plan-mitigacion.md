# 06 — Plan de Mitigación

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 7.1 Introducción

El plan de mitigación define los controles de seguridad prioritarios para reducir los riesgos identificados en el análisis STRIDE y cuantificados en la matriz DREAD. Los controles están organizados por tipo (preventivo, detectivo, correctivo) y priorizados según el score de riesgo de las amenazas asociadas.

---

## 7.2 Controles de Seguridad Prioritarios

| ID  | Amenaza asociada | Control de seguridad                                          | Prioridad | Tipo        | Estado    |
|-----|-----------------|---------------------------------------------------------------|-----------|-------------|-----------|
| C01 | TH01            | Implementar MFA para empleados y administradores              | 🔴 Alta   | Preventivo  | Pendiente |
| C02 | TH01            | Políticas de contraseñas robustas y bloqueo por intentos fallidos | 🔴 Alta | Preventivo  | Pendiente |
| C03 | TH04            | Cifrado de datos sensibles en tránsito (TLS 1.2+) y en reposo (Oracle TDE) | 🔴 Alta | Preventivo | Pendiente |
| C04 | TH06            | Revisión y fortalecimiento del modelo RBAC                   | 🔴 Alta   | Preventivo  | Pendiente |
| C05 | TH02 / TH03     | Auditoría de cambios y logging centralizado e inmutable       | 🔴 Alta   | Detectivo   | Pendiente |
| C06 | TH02            | Segregación de funciones en el módulo de nómina              | 🔴 Alta   | Preventivo  | Pendiente |
| C07 | TH06            | Revisión periódica de privilegios administrativos            | 🔴 Alta   | Preventivo  | Pendiente |
| C08 | TH04            | Hardening de Oracle DB y revisión de configuraciones         | 🟡 Media  | Preventivo  | Pendiente |
| C09 | TH05            | Implementar rate limiting y monitoreo de tráfico             | 🟡 Media  | Preventivo  | Pendiente |
| C10 | TH08            | Autenticación mutua con proveedores externos (mTLS / API keys) | 🟡 Media | Preventivo  | Pendiente |
| C11 | TH10            | Filtrado de campos sensibles en respuestas de API            | 🟡 Media  | Preventivo  | Pendiente |
| C12 | TH01            | Detección de accesos anómalos (geolocalización, horario)     | 🟡 Media  | Detectivo   | Pendiente |
| C13 | TH09            | Confirmación explícita y evidencia criptográfica en firma digital | 🟢 Baja | Preventivo  | Pendiente |
| C14 | General         | Capacitación en phishing e ingeniería social                 | 🟡 Media  | Preventivo  | Pendiente |
| C15 | General         | Procedimientos formales de respuesta a incidentes            | 🟡 Media  | Correctivo  | Pendiente |

---

## 7.3 Controles Preventivos

Los controles preventivos buscan reducir la probabilidad de que una amenaza se materialice.

### Autenticación y Acceso

**C01 — Autenticación Multifactor (MFA)**  
Implementar MFA para todos los usuarios del sistema, con prioridad en administradores y personal de RRHH. Opciones recomendadas: TOTP (aplicación autenticadora), hardware token o SMS como mínimo.

**C02 — Política de contraseñas robusta**  
- Longitud mínima: 12 caracteres
- Combinación de mayúsculas, minúsculas, números y símbolos
- Prohibición de reutilización de las últimas 10 contraseñas
- Bloqueo automático tras 5 intentos fallidos
- Reset obligatorio cada 90 días para cuentas privilegiadas

**C04 — Control de acceso basado en roles (RBAC)**  
Revisar y documentar el modelo de roles actuales:
- Empleado estándar: solo acceso a su propio perfil
- RRHH básico: gestión de empleados, sin acceso a nómina
- RRHH nómina: gestión salarial con requerimiento de doble aprobación
- Administrador: acceso técnico; separado del acceso a datos de negocio

**C06 — Segregación de funciones en nómina**  
El empleado que carga datos de nómina no debe poder aprobarlos. Implementar flujo de doble validación para modificaciones salariales.

**C07 — Revisión periódica de accesos**  
Revisión trimestral de cuentas activas y privilegios asignados. Desactivación automática de cuentas de empleados que se desvincularán.

### Protección de Datos

**C03 — Cifrado**  
- En tránsito: TLS 1.2 mínimo (recomendado TLS 1.3) en todas las comunicaciones
- En reposo: Oracle Transparent Data Encryption (TDE) para columnas con datos sensibles (salarios, PII, credenciales)
- Gestión de claves: HSM o servicio de gestión de claves dedicado

**C08 — Hardening de Oracle DB**  
- Auditar y eliminar cuentas de base de datos no utilizadas
- Restringir acceso a la red (firewall de base de datos)
- Deshabilitar funcionalidades no requeridas
- Aplicar parches de seguridad al día

**C11 — Filtrado en respuestas de API**  
Revisar todos los endpoints REST para garantizar que no retornen campos sensibles no necesarios. Implementar serialización selectiva según el rol del solicitante.

### Disponibilidad

**C09 — Rate limiting**  
- Límite de requests por IP: 100 req/min para endpoints públicos
- Límite por usuario autenticado: 500 req/min
- CAPTCHA en formulario de login tras 3 intentos fallidos

**C10 — Integración segura con proveedores externos**  
- Implementar autenticación mutua (mTLS) o API keys con rotación periódica
- Validar estructura y contenido de las respuestas externas
- Timeout agresivo en llamadas a servicios externos

---

## 7.4 Controles Detectivos

Los controles detectivos buscan identificar amenazas en curso o post-incidente.

**C05 — Logging centralizado e inmutable**  
- Registrar todos los eventos críticos: login/logout, cambios en nómina, acceso a contratos, modificación de permisos
- Almacenamiento en sistema append-only (WORM) o SIEM externo
- Retención mínima de 12 meses
- Timestamps sincronizados con NTP

**C12 — Detección de comportamiento anómalo**  
- Alertas por login desde IP o geolocalización inusuales
- Alertas por acceso fuera de horario laboral
- Alertas por volumen inusual de consultas a nómina
- Integración con SIEM para correlación de eventos

---

## 7.5 Controles Correctivos

Los controles correctivos permiten recuperar el estado normal tras un incidente.

**C15 — Plan de respuesta a incidentes**  
Definir procedimientos formales para:
- Detección y clasificación del incidente
- Contención y revocación de accesos comprometidos
- Investigación forense (preservación de logs)
- Comunicación interna y externa
- Recuperación y vuelta a la normalidad
- Revisión post-incidente (lecciones aprendidas)

**Backups y recuperación:**
- Backups completos de Oracle DB: diarios con retención de 30 días
- Backups de logs: semanales con retención de 12 meses
- Prueba de restauración: trimestral
- RTO objetivo: 4 horas / RPO objetivo: 24 horas

---

## 7.6 Capacitación — C14

**Programa de concientización:**
- Capacitación anual obligatoria sobre phishing e ingeniería social para todos los empleados
- Simulaciones de phishing trimestrales
- Capacitación específica para administradores y personal de RRHH sobre riesgos internos
- Canal de reporte de incidentes sospechosos

---

## 7.7 Priorización General

```
Fase 1 — Inmediata (0–30 días):
  C01 — MFA
  C02 — Política de contraseñas
  C03 — Cifrado TLS + TDE
  C04 — Revisión RBAC
  C05 — Logging centralizado

Fase 2 — Corto plazo (30–90 días):
  C06 — Segregación de funciones
  C07 — Revisión de privilegios
  C08 — Hardening Oracle DB
  C09 — Rate limiting
  C11 — Filtrado API
  C12 — Detección anomalías

Fase 3 — Mediano plazo (90–180 días):
  C10 — mTLS con proveedores externos
  C13 — Firma digital robusta
  C14 — Capacitación
  C15 — Plan de respuesta a incidentes
```

---

## 7.8 Resumen Ejecutivo

Las medidas más importantes para reducir el riesgo del sistema HRIS son:

1. **MFA:** mitiga TH01 (amenaza CRÍTICA), la de mayor score DREAD.
2. **RBAC y segregación de funciones:** mitiga TH02 y TH06 (manipulación de nómina y escalada de privilegios).
3. **Logging inmutable:** mitiga TH03 y es base para detección y respuesta.
4. **Cifrado:** mitiga TH04 (exposición de datos sensibles).
5. **Rate limiting:** mitiga TH05 (denegación de servicio).

Estas cinco acciones reducen la superficie de ataque de las amenazas de mayor criticidad identificadas en el análisis.
