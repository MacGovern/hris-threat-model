# 08 — Riesgos Residuales

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 9.1 Introducción

Luego de aplicar los controles de mitigación propuestos en el plan de mitigación, persisten riesgos residuales que no pueden eliminarse completamente. Estos riesgos deben ser identificados, documentados y aceptados formalmente por la organización, estableciendo umbrales de tolerancia y compromisos de monitoreo continuo.

Un riesgo residual es el nivel de riesgo que permanece después de que los controles han sido implementados. Ningún sistema puede alcanzar riesgo cero.

---

## 9.2 Criterios de Evaluación Post-Mitigación

Para evaluar los riesgos residuales se aplica la misma escala de probabilidad e impacto que el análisis DREAD, pero considerando el efecto de los controles implementados:

| Nivel       | Descripción                                               |
|-------------|-----------------------------------------------------------|
| Alta        | Los controles reducen parcialmente el riesgo; puede materializarse |
| Media       | Los controles mitigan significativamente; posible en escenarios específicos |
| Baja        | Los controles son efectivos; ocurrencia poco probable     |
| Muy baja    | Solo posible mediante error humano o falla sistémica      |

---

## 9.3 Registro de Riesgos Residuales

| ID  | Riesgo Residual                             | Amenaza origen | Probabilidad post-mitigation | Impacto | Nivel residual | Justificación |
|-----|---------------------------------------------|----------------|------------------------------|---------|----------------|---------------|
| R01 | Robo de credenciales mediante phishing      | TH01           | Media                        | Alto    | 🔴 ALTO        | Aunque se implemente MFA, persiste el riesgo de ingeniería social avanzada (phishing de MFA, SIM swapping). El factor humano no puede eliminarse. |
| R02 | Acceso indebido por error de configuración  | TH06           | Baja                         | Alto    | 🟡 MEDIO       | Los controles RBAC reducen significativamente el riesgo, pero errores de configuración en actualizaciones o cambios de sistema siguen siendo posibles. |
| R03 | Interrupción temporal del servicio          | TH05           | Media                        | Medio   | 🟡 MEDIO       | Un ataque DoS volumétrico puede afectar parcialmente la disponibilidad antes de que los controles automáticos actúen. |
| R04 | Exposición accidental de información        | TH04           | Baja                         | Alto    | 🟡 MEDIO       | Persiste riesgo asociado a errores operativos, configuraciones inseguras en actualizaciones o mala práctica de usuarios autorizados. |
| R05 | Abuso interno de privilegios                | TH02           | Baja                         | Alto    | 🟡 MEDIO       | Los usuarios privilegiados legítimos (RRHH, administradores) continúan representando un riesgo inherente que no puede eliminarse completamente con controles técnicos. |
| R06 | Compromiso de proveedor externo             | TH08 / TH09    | Muy baja                     | Alto    | 🟢 BAJO        | El proveedor de nómina o firma digital puede ser comprometido. La organización no tiene control directo sobre su seguridad interna. |
| R07 | Fuga de datos por error de programación     | TH10           | Baja                         | Medio   | 🟢 BAJO        | Un cambio en el código puede introducir inadvertidamente una exposición de datos en la API. Los tests de seguridad y code reviews reducen pero no eliminan este riesgo. |

---

## 9.4 Evaluación General Post-Mitigación

### Comparación de riesgo antes y después de mitigación

| Amenaza | Nivel pre-mitigación | Nivel post-mitigación | Reducción |
|---------|---------------------|----------------------|-----------|
| TH01 — Credenciales comprometidas | 🔴 CRÍTICO (41) | 🔴 ALTO (R01) | Significativa |
| TH04 — Exposición de datos          | 🔴 ALTO (38)    | 🟡 MEDIO (R04) | Alta          |
| TH06 — Escalada de privilegios      | 🔴 ALTO (36)    | 🟡 MEDIO (R02) | Alta          |
| TH02 — Manipulación de nómina       | 🔴 ALTO (35)    | 🟡 MEDIO (R05) | Alta          |
| TH05 — Denegación de servicio       | 🔴 ALTO (35)    | 🟡 MEDIO (R03) | Moderada      |
| TH08 — Datos en tránsito            | 🔴 ALTO (30)    | 🟢 BAJO (R06)  | Alta          |
| TH03 — Negación de acciones         | 🟡 MEDIO (27)   | 🟢 BAJO        | Alta          |

---

## 9.5 Riesgos Aceptados Formalmente

Se consideran aceptables (dentro de la tolerancia al riesgo de la organización) los siguientes riesgos residuales, bajo la condición de que se mantengan activos los controles mitigantes:

| ID  | Riesgo aceptado                            | Condición de aceptación                                  |
|-----|--------------------------------------------|----------------------------------------------------------|
| R02 | Error de configuración en RBAC             | Revisión trimestral de permisos activa y documentada     |
| R03 | Interrupción temporal por DoS              | Rate limiting y balanceo implementados y monitoreados    |
| R04 | Exposición accidental de información       | Cifrado en reposo activo; revisión de permisos de API    |
| R06 | Compromiso de proveedor externo            | Contratos de SLA con cláusulas de seguridad y auditoría  |
| R07 | Fuga de datos por error de programación    | Pipeline de CI/CD con análisis de seguridad estático     |

### Riesgo no aceptable (requiere monitoreo continuo):

| ID  | Riesgo                                     | Acción requerida                                         |
|-----|--------------------------------------------|----------------------------------------------------------|
| R01 | Phishing con bypass de MFA                 | Monitoreo continuo de accesos anómalos + capacitación    |
| R05 | Abuso interno de privilegios               | Auditoría continua de acciones de usuarios privilegiados |

---

## 9.6 Plan de Monitoreo de Riesgos Residuales

Para mantener los riesgos residuales dentro de niveles tolerables, se establece el siguiente plan de monitoreo:

| Actividad                                        | Frecuencia   | Responsable  |
|--------------------------------------------------|--------------|--------------|
| Revisión de logs de acceso y eventos anómalos    | Diaria       | IT Security  |
| Revisión de permisos y cuentas activas           | Trimestral   | IT + RRHH    |
| Simulación de phishing                           | Trimestral   | IT Security  |
| Prueba de restauración de backups                | Trimestral   | IT           |
| Auditoría de cambios en módulo de nómina         | Mensual      | RRHH + Audit |
| Revisión de contratos de proveedores externos    | Anual        | Legal + IT   |
| Evaluación completa de riesgos residuales        | Anual        | CISO / IT    |

---

## 9.7 Conclusión sobre Riesgos Residuales

Luego de aplicar los controles propuestos, el nivel de riesgo general del sistema HRIS disminuye considerablemente. Sin embargo, permanecen riesgos residuales asociados principalmente a:

- **Amenazas internas:** el factor humano (errores, conducta intencional) no puede eliminarse con controles técnicos.
- **Ingeniería social:** phishing avanzado y manipulación psicológica persisten como vectores difíciles de mitigar completamente.
- **Disponibilidad del servicio:** ataques DoS pueden generar interrupciones transitorias antes de la activación de controles automáticos.
- **Terceras partes:** la seguridad de proveedores externos está fuera del control directo de la organización.

Estos riesgos no pueden eliminarse completamente, pero pueden mantenerse dentro de niveles aceptables mediante monitoreo continuo, auditorías periódicas y capacitación sostenida de usuarios.

La organización deberá revisar y actualizar este registro de riesgos residuales al menos anualmente o ante cambios significativos en el sistema.
