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
| TH05  | Denegación de servicio               | 7 | 7 | 6 | 8 | 7 | **35**| 🔴 ALTO    |
| TH07  | Suplantación gestor RRHH             | 8 | 6 | 6 | 7 | 6 | **33**| 🔴 ALTO    |
| TH08  | Manipulación datos en tránsito       | 8 | 5 | 5 | 7 | 5 | **30**| 🔴 ALTO    |
| TH10  | Fuga de datos en respuestas API      | 7 | 6 | 6 | 7 | 6 | **32**| 🔴 ALTO    |
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

---

## 5.5 Ranking de Priorización Final

```
Prioridad 1 (Remediación inmediata):
  TH01 — Credenciales comprometidas       [Score: 41 — CRÍTICO]

Prioridad 2 (Alta urgencia):
  TH04 — Exposición de datos sensibles    [Score: 38 — ALTO]
  TH06 — Escalada de privilegios          [Score: 36 — ALTO]
  TH02 — Manipulación de nómina          [Score: 35 — ALTO]
  TH05 — Denegación de servicio          [Score: 35 — ALTO]
  TH07 — Suplantación gestor RRHH        [Score: 33 — ALTO]
  TH10 — Fuga de datos en API            [Score: 32 — ALTO]
  TH08 — Manipulación datos en tránsito  [Score: 30 — ALTO]

Prioridad 3 (Mitigación planificada):
  TH03 — Negación de acciones            [Score: 27 — MEDIO]
  TH09 — Firma sin consentimiento        [Score: 24 — MEDIO]
```

---

## 5.6 Resumen Ejecutivo

Las amenazas con mayor criticidad identificadas en el análisis DREAD son:

1. **Robo de credenciales (TH01):** única amenaza CRÍTICA. La ausencia de MFA combinada con la exposición pública del portal genera un riesgo inaceptable.
2. **Exposición de datos sensibles (TH04):** dado el volumen y sensibilidad de los datos (PII, salarios, contratos), su exposición tiene consecuencias legales y reputacionales severas.
3. **Escalada de privilegios (TH06) y manipulación de nómina (TH02):** afectan la integridad del sistema y la confianza en los procesos de RRHH.

Las medidas prioritarias se enfocan en fortalecer autenticación (MFA), autorización (RBAC) y monitoreo de accesos.
