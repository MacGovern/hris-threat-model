# 04 — Priorización de Amenazas: Metodología DREAD

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 5.1 Introducción a DREAD

DREAD es una metodología de priorización de riesgos que permite asignar un puntaje numérico a cada amenaza identificada en el análisis STRIDE. El puntaje total permite ordenar y priorizar las amenazas para enfocar los recursos de mitigación.

### Criterios DREAD

| Criterio             | Sigla | Descripción                                           |
|----------------------|-------|-------------------------------------------------------|
| Damage               | D     | Daño potencial si la amenaza se concreta              |
| Reproducibility      | R     | Facilidad para reproducir el ataque                   |
| Exploitability       | E     | Facilidad de explotación (recursos, conocimiento)     |
| Affected Users       | A     | Cantidad de usuarios o activos afectados              |
| Discoverability      | D     | Facilidad para descubrir la vulnerabilidad            |

### Escala de puntuación

| Valor | Descripción                         |
|-------|-------------------------------------|
| 1–3   | Bajo impacto / difícil              |
| 4–6   | Impacto moderado / dificultad media |
| 7–9   | Alto impacto / relativamente fácil  |
| 10    | Máximo impacto / trivial            |

**Score total:** suma de los 5 criterios (rango: 5–50)

---

## 5.2 Matriz de Riesgos DREAD

| ID    | Amenaza                              | D | R | E | A | D | Total | Nivel      |
|-------|--------------------------------------|---|---|---|---|---|-------|------------|
| TH01  | Credenciales comprometidas           | 9 | 8 | 7 | 9 | 8 | **41**| 🔴 CRÍTICO |
| TH04  | Exposición de datos sensibles        | 9 | 7 | 6 | 9 | 7 | **38**| 🔴 ALTO    |
| TH06  | Escalada de privilegios              | 9 | 6 | 7 | 8 | 6 | **36**| 🔴 ALTO    |
| TH02  | Manipulación de nómina               | 9 | 6 | 6 | 8 | 6 | **35**| 🔴 ALTO    |
| TH05  | Denegación de servicio (volumétrico) | 7 | 7 | 6 | 8 | 7 | **35**| 🔴 ALTO    |
| TH07  | Suplantación gestor RRHH             | 8 | 6 | 6 | 7 | 6 | **33**| 🔴 ALTO    |
| TH10  | Fuga de datos en respuestas API      | 7 | 6 | 6 | 7 | 6 | **32**| 🔴 ALTO    |
| TH12  | Manipulación parámetros de sesión    | 9 | 6 | 6 | 5 | 5 | **31**| 🔴 ALTO    |
| TH08  | Manipulación datos en tránsito       | 8 | 5 | 5 | 7 | 5 | **30**| 🔴 ALTO    |
| TH11  | DoS lógico por consultas costosas    | 7 | 6 | 5 | 6 | 5 | **29**| 🟡 MEDIO   |
| TH03  | Negación de acciones                 | 6 | 5 | 5 | 6 | 5 | **27**| 🟡 MEDIO   |
| TH09  | Firma sin consentimiento             | 7 | 4 | 4 | 5 | 4 | **24**| 🟡 MEDIO   |

---

## 5.3 Escala de Severidad

| Rango   | Severidad   | Color | Acción recomendada                        |
|---------|-------------|-------|-------------------------------------------|
| 40–50   | CRÍTICO     | 🔴    | Remediación inmediata (sprint actual)     |
| 30–39   | ALTO        | 🔴    | Prioridad alta (próximo sprint/mes)       |
| 20–29   | MEDIO       | 🟡    | Mitigar en siguientes iteraciones         |
| 10–19   | BAJO        | 🟢    | Monitorear, bajo riesgo aceptable         |
| 1–9     | MÍNIMO      | ⚪    | Riesgo aceptable sin acción inmediata     |

---

## 5.4 Análisis Detallado por Amenaza

---

### TH01 — Credenciales comprometidas (Score: 41 — CRÍTICO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 9         | Acceso total a datos sensibles de empleados, nóminas y contratos |
| Reproducibility  | 8         | Credential stuffing automatizable con herramientas públicas |
| Exploitability   | 7         | Requiere solo credenciales válidas; sin necesidad de explotar código |
| Affected Users   | 9         | Todos los empleados de la organización quedan expuestos |
| Discoverability  | 8         | Portal de login público y visible; fácil de identificar como objetivo |

**Conclusión:** Es la amenaza de mayor prioridad. La ausencia de MFA la convierte en un vector de ataque de bajo costo y alto impacto.

---

### TH04 — Exposición de datos sensibles (Score: 38 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 9         | Violación de privacidad, consecuencias legales, daño reputacional |
| Reproducibility  | 7         | Reproducible si existe IDOR o endpoint sin autorización correcta |
| Exploitability   | 6         | Requiere cierto conocimiento técnico (enumerar IDs, analizar respuestas API) |
| Affected Users   | 9         | Toda la base de empleados potencialmente expuesta |
| Discoverability  | 7         | Las vulnerabilidades de autorización en APIs son relativamente comunes |

---

### TH06 — Escalada de privilegios (Score: 36 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 9         | Acceso completo al sistema; modificación de cualquier dato |
| Reproducibility  | 6         | Depende de la vulnerabilidad específica; no siempre reproducible |
| Exploitability   | 7         | Errores de configuración en RBAC son frecuentes |
| Affected Users   | 8         | Un administrador comprometido afecta a toda la organización |
| Discoverability  | 6         | Requiere reconocimiento interno del sistema |

---

### TH02 — Manipulación de nómina (Score: 35 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 9         | Fraude financiero directo; impacto legal y económico inmediato |
| Reproducibility  | 6         | Requiere acceso previo con permisos de nómina |
| Exploitability   | 6         | Posible mediante insider con acceso legítimo o credenciales robadas |
| Affected Users   | 8         | Todos los empleados con nómina activa |
| Discoverability  | 6         | Detectable solo con auditoría activa |

---

### TH05 — Denegación de servicio (Score: 35 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 7         | Interrupción de operaciones; crítico en períodos de cierre de nómina |
| Reproducibility  | 7         | Herramientas de DoS ampliamente disponibles |
| Exploitability   | 6         | Requiere conocimiento básico de ataques de red |
| Affected Users   | 8         | Todos los usuarios del sistema durante el ataque |
| Discoverability  | 7         | Portal público con URL conocida |

### TH07 — Suplantación de gestor RRHH (Score: 33 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 8         | Acceso a nóminas, contratos y PII de todos los empleados con permisos de modificación |
| Reproducibility  | 6         | Requiere credenciales de un rol específico; más difícil que atacar un empleado estándar |
| Exploitability   | 6         | Spear phishing dirigido a RRHH es técnicamente accesible para atacantes con motivación |
| Affected Users   | 7         | Impacta a todos los empleados cuyos datos son gestionados por ese gestor |
| Discoverability  | 6         | El personal de RRHH es identificable públicamente (LinkedIn, directorio corporativo) |

**Conclusión:** La diferencia respecto a TH01 radica en que el rol RRHH permite operaciones masivas y modificaciones de datos financieros. El spear phishing dirigido eleva la probabilidad de éxito frente a un ataque genérico.

---

### TH10 — Fuga de datos en respuestas de API (Score: 32 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 7         | Exposición de datos PII y salariales a usuarios no autorizados |
| Reproducibility  | 6         | Reproducible si existe IDOR; basta con modificar el ID en la URL |
| Exploitability   | 6         | Conocimiento técnico básico de APIs REST; herramientas como Burp Suite son accesibles |
| Affected Users   | 7         | Puede afectar a múltiples empleados si se enumera sistemáticamente |
| Discoverability  | 6         | La API es el canal principal de la aplicación; es el primer lugar que un atacante inspecciona |

**Conclusión:** La excesiva verbosidad de las APIs es un error de diseño frecuente que se convierte en vulnerabilidad de alto impacto en sistemas con datos sensibles.

---

### TH08 — Manipulación de datos en tránsito (Score: 30 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 8         | Datos salariales alterados pueden resultar en transferencias fraudulentas |
| Reproducibility  | 5         | Requiere posición de red privilegiada; no trivial para un atacante externo |
| Exploitability   | 5         | Necesita compromiso de la red o del proveedor; mayor barrera técnica |
| Affected Users   | 7         | Todos los empleados procesados en el ciclo de nómina afectado |
| Discoverability  | 5         | Depende de la configuración del canal; no visible desde fuera sin acceso a la red |

**Conclusión:** La baja exploitability la diferencia de TH01 y TH04, pero el daño potencial sobre datos financieros justifica su clasificación como ALTO y su inclusión en el plan de mitigación de corto plazo.

---

### TH03 — Negación de acciones realizadas (Score: 27 — MEDIO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 6         | Disputas legales sobre contratos o liquidaciones; daño en procesos de RRHH |
| Reproducibility  | 5         | Solo posible si los logs son insuficientes o alterables; depende del estado del sistema |
| Exploitability   | 5         | Requiere conocimiento del sistema de logging; más sencillo si el sistema no tiene auditoría robusta |
| Affected Users   | 6         | Impacta principalmente al empleado involucrado en la disputa y al área legal |
| Discoverability  | 5         | Las deficiencias de auditoría no son evidentes desde afuera; se descubren en incidentes |

**Conclusión:** Aunque el score es MEDIO, la implicancia legal de no poder demostrar quién realizó una acción en contratos laborales puede tener consecuencias desproporcionadas. La mitigación (logs inmutables) es de bajo costo relativo.

---

### TH11 — DoS lógico por consultas costosas (Score: 29 — MEDIO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 7         | Inoperatividad del sistema en períodos críticos (cierre de nómina, onboarding masivo) |
| Reproducibility  | 6         | Reproducible si el endpoint existe; automatizable con un script simple |
| Exploitability   | 5         | Requiere acceso autenticado y conocimiento de los endpoints de reporte |
| Affected Users   | 6         | Todos los usuarios activos durante el ataque se ven afectados |
| Discoverability  | 5         | Los endpoints de exportación/reporte suelen ser conocidos por usuarios internos |

**Conclusión:** Su score lo ubica en MEDIO, pero el contexto temporal importa: si el ataque ocurre durante el cierre mensual de nómina, el impacto operativo supera lo que el número sugiere. La mitigación con paginación y timeouts es de implementación simple.

---

### TH09 — Firma de contrato sin consentimiento (Score: 24 — MEDIO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 7         | Contrato firmado sin voluntad del empleado; implicaciones legales y laborales serias |
| Reproducibility  | 4         | Requiere acceso al módulo de contratos con permisos de gestor RRHH |
| Exploitability   | 4         | Necesita acceso interno previo con rol específico; no es un ataque de bajo costo |
| Affected Users   | 5         | Impacta individualmente a cada empleado afectado; no es masivo por naturaleza |
| Discoverability  | 4         | El flujo de firma es interno; no visible desde el exterior del sistema |

**Conclusión:** Menor score por su naturaleza targeted y sus requisitos de acceso. Sin embargo, la consecuencia legal de un contrato firmado fraudulentamente es severa y la mitigación (confirmación por canal independiente) es de implementación directa.

---

### TH12 — Manipulación de parámetros de sesión (Score: 31 — ALTO)

| Criterio         | Puntuación | Justificación |
|------------------|-----------|---------------|
| Damage           | 9         | Acceso administrativo completo si la escalada tiene éxito |
| Reproducibility  | 6         | Reproducible si el JWT está mal firmado; herramientas como jwt_tool automatizan el ataque |
| Exploitability   | 6         | Requiere conocimiento de JWT y análisis del token; al alcance de atacantes con experiencia media |
| Affected Users   | 5         | Afecta principalmente al sistema y a los datos accedidos por la sesión escalada |
| Discoverability  | 5         | La debilidad en la validación JWT no es visible sin acceso al token y prueba activa |

**Conclusión:** Amenaza de alto daño potencial que depende fuertemente de la correcta implementación de la firma JWT en el backend Spring. Un error de configuración (uso de algoritmo "none" o clave débil) la convierte en trivialmente explotable.

---

## 5.5 Ranking de Priorización Final

```
Prioridad 1 — Remediación inmediata:
  TH01 — Credenciales comprometidas           [Score: 41 — CRÍTICO]

Prioridad 2 — Alta urgencia (30–90 días):
  TH04 — Exposición de datos sensibles        [Score: 38 — ALTO]
  TH06 — Escalada de privilegios              [Score: 36 — ALTO]
  TH02 — Manipulación de nómina              [Score: 35 — ALTO]
  TH05 — Denegación de servicio volumétrico  [Score: 35 — ALTO]
  TH07 — Suplantación gestor RRHH            [Score: 33 — ALTO]
  TH12 — Manipulación parámetros de sesión   [Score: 31 — ALTO]
  TH10 — Fuga de datos en API                [Score: 32 — ALTO]
  TH08 — Manipulación datos en tránsito      [Score: 30 — ALTO]

Prioridad 3 — Mitigación planificada (90–180 días):
  TH11 — DoS lógico por consultas costosas   [Score: 29 — MEDIO]
  TH03 — Negación de acciones               [Score: 27 — MEDIO]
  TH09 — Firma sin consentimiento           [Score: 24 — MEDIO]
```

---

## 5.6 Resumen Ejecutivo

El análisis DREAD sobre las 12 amenazas identificadas en STRIDE revela un perfil de riesgo consistente con sistemas corporativos que manejan datos sensibles de RRHH sin controles modernos de autenticación y autorización. A continuación, los hallazgos clave:

**Una amenaza CRÍTICA:** TH01 (Credenciales comprometidas, score 41) es la única en rango crítico. La combinación de portal público accesible sin MFA, datos de alta sensibilidad y herramientas de credential stuffing ampliamente disponibles crea un riesgo inaceptable que debe resolverse antes que cualquier otro control. Cada día sin MFA es una ventana de ataque abierta.

**Ocho amenazas en rango ALTO:** Seis de las doce amenazas obtienen score ≥ 30, lo que indica que el sistema tiene una superficie de ataque amplia sin un único punto débil dominante. TH04 (datos sensibles, 38) y TH06 (escalada de privilegios, 36) son las siguientes en urgencia, ambas relacionadas con fallas en el modelo de autorización. TH12 (manipulación de sesión JWT, 31) merece atención especial porque una mala configuración de Spring Security podría hacerla trivialmente explotable. TH07 (suplantación de gestor RRHH, 33) amplifica el impacto de TH01: si el objetivo del phishing es RRHH en lugar de un empleado común, el daño potencial se multiplica.

**Tres amenazas en rango MEDIO:** TH11, TH03 y TH09 tienen scores entre 24 y 29. Aunque no son urgentes, ignorarlas sería un error: TH03 (repudio de acciones) tiene consecuencias legales que el score numérico no captura completamente, y TH09 (firma sin consentimiento) puede generar conflictos laborales graves con muy baja probabilidad pero alto impacto individual.

**Patrón de ataque dominante:** Nueve de las doce amenazas tienen como precondición o vector relacionado el acceso mediante credenciales válidas (T1078). Esto confirma que fortalecer la autenticación —MFA, política de contraseñas, detección de anomalías— es la intervención de mayor retorno sobre la inversión en seguridad para este sistema.

**Recomendación de priorización de inversión:**
- 60% de los recursos de seguridad → autenticación (MFA, detección de anomalías, políticas de contraseñas)
- 25% → autorización (RBAC, validación JWT, segregación de funciones)
- 15% → monitoreo y logging (SIEM, logs inmutables, alertas)

Con este perfil de inversión se mitigan las 9 amenazas de rango ALTO/CRÍTICO en menos de 90 días.
