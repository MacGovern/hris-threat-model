# 05 — Mapa de Técnicas MITRE ATT&CK

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 6.1 Introducción a MITRE ATT&CK

MITRE ATT&CK (Adversarial Tactics, Techniques & Common Knowledge) es un marco de conocimiento sobre tácticas y técnicas de ataque utilizadas por actores maliciosos reales. Su uso en threat modeling permite vincular las amenazas abstractas del análisis STRIDE con comportamientos de ataque documentados en el mundo real, facilitando la selección de controles de detección y mitigación más efectivos.

El marco organiza las técnicas en **tácticas**, que representan el objetivo del atacante en cada fase del ataque.

---

## 6.2 Técnicas ATT&CK Identificadas

| ID ATT&CK | Técnica                       | Táctica              | Relación con el HRIS                            | Amenaza vinculada |
|-----------|-------------------------------|----------------------|-------------------------------------------------|-------------------|
| T1078     | Valid Accounts                | Initial Access       | Uso de credenciales válidas robadas             | TH01, TH02, TH06  |
| T1566     | Phishing                      | Initial Access       | Robo de credenciales de empleados               | TH01, TH07        |
| T1098     | Account Manipulation          | Persistence          | Modificación de cuentas y permisos en el HRIS   | TH02              |
| T1005     | Data from Local System        | Collection           | Acceso a datos sensibles desde el sistema       | TH04              |
| T1213     | Data from Info. Repositories  | Collection           | Extracción de datos desde Oracle DB             | TH04, TH10        |
| T1499     | Endpoint Denial of Service    | Impact               | Saturación de servicios del HRIS                | TH05              |
| T1068     | Exploitation for Priv. Escal. | Privilege Escalation | Escalada de permisos administrativos            | TH06              |
| T1070     | Indicator Removal on Host     | Defense Evasion      | Eliminación o alteración de logs de auditoría   | TH03              |
| T1190     | Exploit Public-Facing App.    | Initial Access       | Explotación de vulnerabilidades en el portal web| TH06, TH04        |
| T1552     | Unsecured Credentials         | Credential Access    | Credenciales expuestas en código o configuración| TH01, TH04        |

---

## 6.3 Cadenas de Ataque (Kill Chains)

### Cadena 1: Robo de credenciales → Acceso → Exfiltración de datos

```
[Reconocimiento]
    └─ T1566 Phishing → empleado recibe correo malicioso
         ↓
[Initial Access]
    └─ T1078 Valid Accounts → atacante usa credenciales robadas
         ↓
[Collection]
    └─ T1213 Data from Information Repositories → extrae nóminas y datos PII
         ↓
[Exfiltration]
    └─ Datos sensibles fuera de la organización
```

**Amenazas STRIDE vinculadas:** TH01 (S), TH04 (I)  
**Score DREAD combinado:** CRÍTICO

---

### Cadena 2: Insider malicioso → Manipulación de nómina

```
[Initial Access]
    └─ T1078 Valid Accounts → usuario legítimo con acceso a nómina
         ↓
[Privilege Escalation (opcional)]
    └─ T1068 Exploitation for Privilege Escalation → acceso a módulos adicionales
         ↓
[Impact]
    └─ T1098 Account Manipulation → modifica datos salariales
         ↓
[Defense Evasion]
    └─ T1070 Indicator Removal → altera o elimina logs de auditoría
```

**Amenazas STRIDE vinculadas:** TH02 (T), TH03 (R), TH06 (E)  
**Score DREAD combinado:** ALTO

---

### Cadena 3: Escalada de privilegios → Control del sistema

```
[Initial Access]
    └─ T1190 Exploit Public-Facing Application → vulnerabilidad en portal web
         ↓
[Privilege Escalation]
    └─ T1068 Exploitation for Privilege Escalation → acceso de administrador
         ↓
[Collection + Impact]
    └─ T1005 Data from Local System → acceso irrestricto a toda la base de datos
    └─ T1499 Endpoint DoS (opcional) → interrupción del servicio
```

**Amenazas STRIDE vinculadas:** TH06 (E), TH04 (I), TH05 (D)  
**Score DREAD combinado:** ALTO

---

## 6.4 Relación entre Amenazas y Técnicas ATT&CK

| Amenaza STRIDE                        | Técnicas ATT&CK relacionadas        | Táctica principal       |
|---------------------------------------|-------------------------------------|-------------------------|
| TH01 — Credenciales comprometidas     | T1078, T1566, T1552                 | Initial Access          |
| TH02 — Manipulación de nómina        | T1098, T1078                        | Persistence / Impact    |
| TH03 — Negación de acciones          | T1070                               | Defense Evasion         |
| TH04 — Exposición de datos sensibles  | T1005, T1213                        | Collection              |
| TH05 — Denegación de servicio        | T1499                               | Impact                  |
| TH06 — Escalada de privilegios        | T1068, T1078, T1190                 | Privilege Escalation    |
| TH07 — Suplantación gestor RRHH      | T1566, T1078                        | Initial Access          |
| TH08 — Manipulación datos en tránsito| T1557 (AiTM)                        | Collection              |
| TH09 — Firma sin consentimiento      | T1098                               | Persistence             |
| TH10 — Fuga de datos en API          | T1213                               | Collection              |

---

## 6.5 Mitigaciones por Técnica ATT&CK

| Técnica   | Mitigación recomendada                                                     |
|-----------|----------------------------------------------------------------------------|
| T1078     | MFA obligatorio, detección de accesos anómalos, revisión de cuentas       |
| T1566     | Filtros anti-phishing, capacitación en concientización, DMARC/SPF         |
| T1098     | Auditoría de cambios en cuentas, alertas en tiempo real                   |
| T1005     | Principio de mínimo privilegio, DLP (Data Loss Prevention)                |
| T1213     | Control de acceso a repositorios, cifrado de datos en reposo              |
| T1499     | Rate limiting, WAF, balanceo de carga, monitoreo de disponibilidad        |
| T1068     | Hardening del backend, validación de autorización en servidor             |
| T1070     | Logs inmutables (append-only), SIEM centralizado, backup de logs          |
| T1190     | WAF, gestión de vulnerabilidades, pentesting periódico                    |
| T1552     | Gestión segura de secretos (Vault), revisión de configuraciones           |

---

## 6.6 Resumen

Las técnicas ATT&CK identificadas reflejan que los principales riesgos del sistema HRIS se concentran en:

- **Acceso indebido mediante cuentas válidas (T1078):** técnica más prevalente, presente en múltiples cadenas de ataque.
- **Exposición de información sensible (T1213, T1005):** dado el volumen de datos PII y financieros, la exfiltración es un objetivo atractivo.
- **Abuso de privilegios internos (T1068, T1098):** el riesgo de insider threat es alto en sistemas de RRHH con acceso a datos salariales.

Este perfil es típico de aplicaciones corporativas con alta dependencia de autenticación, autorización y protección de datos personales y financieros.