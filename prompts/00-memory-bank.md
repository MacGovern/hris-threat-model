# Memory Bank — Contexto para IA

Este archivo provee el contexto necesario para que una herramienta de IA (ChatGPT, Claude, Copilot, etc.) comprenda el proyecto y pueda asistir en la generación o revisión de documentos sin necesidad de re-explicar el escenario cada vez.

---

## Escenario

**Nombre:** Sistema de Recursos Humanos (HRIS) — Escenario 6  
**Curso:** Introducción a la Seguridad Informática (ISI) — 2026  
**Grupo:** Luis Andrada, Leonardo Giménez  
**Presentación:** Martes 26 de mayo de 2026

---

## Descripción del Sistema

Plataforma de gestión de RRHH que permite:
- Nóminas y liquidaciones salariales
- Gestión de beneficios de empleados
- Evaluaciones de desempeño
- Portal de empleados (autoservicio)
- Onboarding digital
- Generación y firma digital de contratos

**Stack tecnológico:** Aplicación web · Backend Java/Spring · Base de datos Oracle · Integración con sistema de nómina externo · Proveedor de firma digital externo

---

## Archivos de Contexto Necesarios

Para comprender el proyecto completo, cargar los siguientes archivos:

| Archivo | Contenido |
|---------|-----------|
| `docs/01-inventario-activos.md` | Lista de activos críticos: datos PII, salarios, contratos, credenciales, infraestructura tecnológica |
| `docs/02-arquitectura-trust-boundaries.md` | Arquitectura del sistema y los 4 trust boundaries identificados (TB-1 a TB-4) |
| `docs/03-analisis-stride.md` | Análisis STRIDE completo: 12 amenazas identificadas por componente (TH01–TH12) |
| `docs/04-priorizacion-dread.md` | Matriz DREAD con scoring numérico; TH01 = CRÍTICO (41/50) |
| `docs/05-mapa-attack.md` | Técnicas MITRE ATT&CK mapeadas a las amenazas (T1078, T1566, T1098, etc.) |
| `docs/06-plan-mitigacion.md` | 15 controles priorizados en 3 fases (0–30, 30–90, 90–180 días) |
| `docs/07-controles-nist-iso.md` | Mapeo de controles a NIST SP 800-53 Rev.5 e ISO/IEC 27001:2022 |
| `docs/08-riesgos-residuales.md` | 7 riesgos residuales post-mitigación con nivel y condiciones de aceptación |
| `docs/09-conclusiones.md` | Resumen ejecutivo, hallazgos principales y próximos pasos |
| `diagrams/arquitectura-hris.puml` | Diagrama PlantUML de la arquitectura (fuente) |

---

## Amenazas Identificadas (resumen)

| ID    | Amenaza                          | STRIDE | Score DREAD | Nivel    |
|-------|----------------------------------|--------|-------------|----------|
| TH01  | Credenciales comprometidas       | S      | 41          | CRÍTICO  |
| TH04  | Exposición de datos sensibles    | I      | 38          | ALTO     |
| TH06  | Escalada de privilegios          | E      | 36          | ALTO     |
| TH02  | Manipulación de nómina          | T      | 35          | ALTO     |
| TH05  | Denegación de servicio          | D      | 35          | ALTO     |
| TH07  | Suplantación gestor RRHH        | S      | 33          | ALTO     |
| TH10  | Fuga de datos en API            | I      | 32          | ALTO     |
| TH08  | Manipulación datos en tránsito  | T      | 30          | ALTO     |
| TH03  | Negación de acciones            | R      | 27          | MEDIO    |
| TH09  | Firma sin consentimiento        | R      | 24          | MEDIO    |

---

## Controles Prioritarios

1. **C01:** MFA para todos los usuarios (mitiga TH01 — CRÍTICO)
2. **C03:** Cifrado TLS + Oracle TDE (mitiga TH04)
3. **C04:** Revisión y fortalecimiento RBAC (mitiga TH06)
4. **C05:** Logging centralizado e inmutable (mitiga TH03)
5. **C06:** Segregación de funciones en nómina (mitiga TH02)

---

## Metodologías Utilizadas

- **STRIDE** — Clasificación de amenazas (Microsoft)
- **DREAD** — Priorización cuantitativa de riesgos
- **MITRE ATT&CK** — Mapeo a técnicas de ataque reales
- **NIST SP 800-53 Rev. 5** — Marco de controles de seguridad
- **ISO/IEC 27001:2022** — Gestión de riesgos (Anexo A)

---

## Prompts disponibles en este repositorio

| Archivo | Propósito |
|---------|-----------|
| `prompts/00-memory-bank.md` | Este archivo — contexto general |
| `prompts/01-arquitectura.txt` | Diseño de arquitectura y trust boundaries |
| `prompts/02-stride.txt` | Análisis STRIDE de amenazas |
| `prompts/03-dread.txt` | Priorización DREAD |
| `prompts/04-attack-mapping.txt` | Mapeo MITRE ATT&CK y kill chains |
| `prompts/05-mitigacion-nist-iso.txt` | Plan de mitigación y controles normativos |
| `prompts/06-riesgos-conclusiones.txt` | Riesgos residuales y conclusiones |
| `prompts/07-inventario-activos.txt` | Inventario de activos |
| `prompts/08-refinamiento-repositorio.txt` | Revisión de coherencia y ampliación del repositorio |

## Instrucciones para la IA

Al asistir en este proyecto:
1. Mantener coherencia con las amenazas TH01–TH12 definidas en `docs/03-analisis-stride.md`.
2. Usar los IDs de activos (A01–A05, T01–T07) y controles (C01–C15) definidos.
3. Referenciar los trust boundaries TB-1 a TB-4 cuando se hable de flujos de datos.
4. El sistema usa Java/Spring + Oracle DB; las recomendaciones técnicas deben ser compatibles.
5. Todos los controles deben mapearse a NIST SP 800-53 o ISO 27001 cuando sea posible.