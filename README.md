# 🔐 Threat Model — Sistema HRIS (Escenario 6)

## Descripción del Proyecto

Este repositorio contiene el análisis de riesgos y modelado de amenazas de una plataforma de gestión de recursos humanos (HRIS). El trabajo fue desarrollado en el marco de la asignatura **Introducción a la Seguridad Informática (ISI)** — Tarea 2, Escenario 6.

**Grupo:** Luis Andrada, Leonardo Giménez  
**Presentación:** Martes 26 de mayo de 2026  
**Docente:** Andrés Pastorini

---

## 🏢 Escenario: Sistema de Recursos Humanos (HRIS)

La plataforma analizada gestiona procesos críticos de RRHH de una organización:

- Nóminas y liquidaciones salariales
- Gestión de beneficios de empleados
- Evaluaciones de desempeño
- Portal de empleados (autoservicio)
- Onboarding digital
- Generación y firma digital de contratos

**Stack tecnológico:** Aplicación web · Backend Java/Spring · Base de datos Oracle · Integración con sistemas de nómina · Proveedor de firma digital

---

## 📁 Estructura del Repositorio

```
hris-threat-model/
├── README.md                          # Este archivo
├── docs/
│   ├── 01-inventario-activos.md       # Activos de información y tecnológicos
│   ├── 02-arquitectura-trust-boundaries.md  # Arquitectura y límites de confianza
│   ├── 03-analisis-stride.md          # Análisis STRIDE completo
│   ├── 04-priorizacion-dread.md       # Priorización de amenazas con DREAD
│   ├── 05-mapa-attack.md              # Técnicas MITRE ATT&CK mapeadas
│   ├── 06-plan-mitigacion.md          # Plan de mitigación priorizado
│   ├── 07-controles-nist-iso.md       # Controles NIST SP 800-53 e ISO 27001
│   ├── 08-riesgos-residuales.md       # Riesgos residuales post-mitigación
│   └── 09-conclusiones.md             # Conclusiones y recomendaciones
├── diagrams/
│   ├── arquitectura-hris.puml         # Diagrama PlantUML (fuente)
│   └── arquitectura-hris.png          # Diagrama renderizado
├── prompts/
│   ├── 00-memory-bank.md              # Contexto de IA para el proyecto
│   ├── 01-arquitectura.txt            # Prompt: diseño de arquitectura
│   ├── 02-stride.txt                  # Prompt: análisis STRIDE
│   └── 03-dread.txt                   # Prompt: priorización DREAD
└── references/
    └── referencias.md                 # Referencias normativas y bibliografía
```

---

## ✅ Checklist de Entregables

| Entregable | Archivo | Estado |
|------------|---------|--------|
| ☑ Diagrama de arquitectura con trust boundaries | `diagrams/` + `docs/02-arquitectura-trust-boundaries.md` | ✅ Completo |
| ☑ Inventario de activos (información y tecnológico) | `docs/01-inventario-activos.md` | ✅ Completo |
| ☑ Matriz STRIDE completa | `docs/03-analisis-stride.md` | ✅ Completo |
| ☑ Análisis DREAD de amenazas prioritarias | `docs/04-priorizacion-dread.md` | ✅ Completo |
| ☑ Mapa de técnicas ATT&CK | `docs/05-mapa-attack.md` | ✅ Completo |
| ☑ Plan de mitigación priorizado | `docs/06-plan-mitigacion.md` | ✅ Completo |
| ☑ Controles documentados (NIST/ISO) | `docs/07-controles-nist-iso.md` | ✅ Completo |
| ☑ Riesgos residuales identificados | `docs/08-riesgos-residuales.md` | ✅ Completo |
| ☑ Conclusiones y recomendaciones | `docs/09-conclusiones.md` | ✅ Completo |

---

## 🤖 Herramientas de IA Utilizadas

- **ChatGPT (GPT-5.5)** — Generación del borrador inicial del análisis de riesgos y matrices STRIDE/DREAD
- **Claude (Anthropic)** — Organización del repositorio, refinamiento de documentación y generación de estructura Git

> Los prompts utilizados se encuentran documentados en la carpeta `prompts/`, incluyendo el archivo de contexto `00-memory-bank.md`.

---

## 📚 Metodologías Aplicadas

| Metodología | Uso en este proyecto |
|-------------|---------------------|
| **STRIDE** | Categorización de amenazas por componente |
| **DREAD** | Priorización cuantitativa de riesgos |
| **MITRE ATT&CK** | Mapeo de técnicas de ataque reales |
| **NIST SP 800-53** | Marco de controles de seguridad |
| **ISO 27001** | Gestión de riesgos y controles organizacionales |

---

## 🔗 Referencias Rápidas

- [OWASP Top 10 (2021)](https://owasp.org/Top10/)
- [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/)
- [NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/final)
- [ISO/IEC 27001](https://www.iso.org/isoiec-27001-information-security.html)
- Ver referencias completas en [`references/referencias.md`](references/referencias.md)
