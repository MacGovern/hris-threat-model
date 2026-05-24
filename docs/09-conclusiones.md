# 09 — Conclusiones y Recomendaciones

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 10.1 Resumen Ejecutivo

El análisis de riesgos realizado sobre la plataforma HRIS permitió identificar amenazas relevantes relacionadas con acceso indebido mediante credenciales comprometidas, exposición de información sensible, manipulación de datos de nómina y abuso de privilegios internos.

Las amenazas de mayor criticidad afectan principalmente la **confidencialidad e integridad** de los datos de empleados, debido a la naturaleza sensible de la información gestionada: datos personales (PII), información salarial y contratos laborales.

La aplicación de las metodologías **STRIDE** y **DREAD** permitió clasificar y priorizar los riesgos identificados de manera sistemática y cuantitativa, mientras que el uso de **MITRE ATT&CK** permitió relacionar las amenazas con técnicas de ataque reales documentadas en entornos corporativos similares.

---

## 10.2 Hallazgos Principales

### Amenaza crítica identificada

**TH01 — Credenciales comprometidas (DREAD Score: 41/50 — CRÍTICO)**  
La ausencia de autenticación multifactor en el sistema expone el portal a ataques de credential stuffing, phishing y robo de contraseñas. Dado que el portal es accesible desde Internet con credenciales simples (usuario/contraseña), cualquier conjunto de credenciales robadas permite acceso completo a datos de empleados, nóminas y contratos. Esta es la brecha de seguridad más urgente a resolver.

### Amenazas de alto impacto

- **TH04 — Exposición de datos sensibles (Score: 38):** La base de datos Oracle contiene información altamente sensible sin garantías documentadas de cifrado en reposo. Fallas en el modelo de autorización de la API podrían permitir acceso indebido a datos de otros empleados.
- **TH06 — Escalada de privilegios (Score: 36):** El modelo de roles no está suficientemente documentado ni verificado, lo que genera incertidumbre sobre la efectividad de los controles de autorización.
- **TH02 — Manipulación de nómina (Score: 35):** La ausencia de segregación de funciones y doble aprobación en modificaciones salariales representa un riesgo financiero directo.

### Riesgos de continuidad

- **TH05 — Denegación de servicio (Score: 35):** El portal carece de controles explícitos de rate limiting, haciéndolo vulnerable durante períodos críticos como el cierre mensual de nómina.

---

## 10.3 Logros del Análisis

Este trabajo permitió:

1. **Inventariar activos** de información y tecnológicos del HRIS, clasificándolos por criticidad.
2. **Mapear la arquitectura** del sistema e identificar cuatro trust boundaries con sus respectivos controles.
3. **Aplicar STRIDE** de forma sistemática a cada componente, identificando 10 amenazas.
4. **Cuantificar los riesgos** con DREAD, obteniendo un ranking que prioriza la inversión en seguridad.
5. **Vincular amenazas con técnicas ATT&CK** reales, haciendo el análisis accionable para equipos de detección y respuesta.
6. **Proponer 15 controles** clasificados por tipo y prioridad, mapeados a NIST SP 800-53 e ISO 27001.
7. **Identificar 7 riesgos residuales** con su nivel post-mitigación y condiciones de aceptación.

---

## 10.4 Recomendaciones Prioritarias

### Inmediatas (0–30 días)

1. **Implementar autenticación multifactor (MFA)** para todos los accesos al sistema, comenzando por administradores y personal de RRHH. Es la medida con mayor retorno en reducción de riesgo.

2. **Revisar y documentar el modelo RBAC** actual. Confirmar que cada rol tiene únicamente los permisos mínimos necesarios y que no existen cuentas con permisos excesivos.

3. **Implementar logging centralizado** con protección de integridad. Sin registros confiables, es imposible detectar incidentes ni garantizar el no repudio.

4. **Habilitar cifrado en tránsito** (TLS 1.2+ en todas las comunicaciones) y confirmar el estado de cifrado en reposo en Oracle DB.

### Corto plazo (30–90 días)

5. **Aplicar segregación de funciones** en el módulo de nómina: separar el rol de carga de datos del rol de aprobación.

6. **Implementar rate limiting** en la API REST y en el formulario de login.

7. **Realizar hardening de Oracle DB**: auditar cuentas activas, restringir acceso de red y aplicar parches pendientes.

8. **Revisar respuestas de la API** para asegurar que no exponen campos sensibles innecesarios (prevención de IDOR y fuga de datos).

### Mediano plazo (90–180 días)

9. **Formalizar el plan de respuesta a incidentes** con procedimientos documentados, contactos de escalación y ejercicios de simulación.

10. **Implementar programa de capacitación** en phishing e ingeniería social para todos los empleados, con simulaciones periódicas.

11. **Revisar contratos de proveedores externos** (nómina, firma digital) incluyendo cláusulas de seguridad, SLA y derechos de auditoría.

12. **Realizar pruebas de penetración** sobre el módulo de autenticación y la API REST para validar los controles implementados.

---

## 10.5 Próximos Pasos

| Acción                                          | Responsable   | Plazo        |
|-------------------------------------------------|---------------|--------------|
| Implementar MFA                                 | IT / Seguridad | 30 días     |
| Auditoría y documentación de RBAC               | IT + RRHH     | 30 días      |
| Configurar SIEM y logging centralizado          | IT / Seguridad | 30 días     |
| Confirmar y habilitar cifrado Oracle TDE        | DBA           | 30 días      |
| Implementar rate limiting en API                | Desarrollo    | 60 días      |
| Segregación de funciones en nómina              | IT + RRHH     | 60 días      |
| Hardening de base de datos Oracle               | DBA           | 60 días      |
| Plan de respuesta a incidentes documentado      | CISO / IT     | 90 días      |
| Capacitación phishing — primera sesión          | IT / RRHH     | 90 días      |
| Pruebas de penetración sobre autenticación y API| Equipo externo| 120 días     |
| Re-evaluación de riesgos post-implementación    | CISO          | 180 días     |

---

## 10.6 Conclusión Final

El sistema HRIS presenta riesgos típicos de plataformas corporativas que manejan información altamente sensible y procesos críticos de negocio. Las principales brechas identificadas —ausencia de MFA, falta de cifrado en reposo documentado, modelo RBAC sin revisión formal y logging insuficiente— son solucionables con controles estándar de la industria sin necesidad de rediseño arquitectónico.

Mediante la implementación priorizada de los controles propuestos, es posible:
- Reducir el score de la amenaza CRÍTICA (TH01) de 41 a un nivel ALTO residual manejable.
- Llevar todas las amenazas ALTAS a nivel MEDIO o BAJO post-mitigación.
- Alcanzar una postura de seguridad alineada con NIST SP 800-53 e ISO 27001.

El riesgo no puede eliminarse completamente, pero con los controles adecuados puede mantenerse dentro de niveles aceptables para la organización, garantizando la confidencialidad, integridad y disponibilidad de los datos de empleados que el sistema gestiona.

---

## 10.7 Limitaciones del Análisis

- El análisis se basó en la descripción del sistema proporcionada; no se realizó inspección técnica directa del código ni de la infraestructura.
- Los scores DREAD son estimaciones basadas en el contexto descrito y pueden variar con información técnica adicional.
- El análisis no cubre amenazas físicas ni seguridad de la infraestructura cloud del proveedor.
- Se recomienda complementar este análisis con pruebas de penetración profesionales y revisión de código.
