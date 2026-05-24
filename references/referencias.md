# Referencias y Bibliografía

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026

---

## Normativas y Estándares

| Referencia | Descripción | URL |
|------------|-------------|-----|
| NIST SP 800-30 Rev. 1 | Guide for Conducting Risk Assessments | https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-30r1.pdf |
| NIST SP 800-53 Rev. 5 | Security and Privacy Controls for Information Systems and Organizations | https://csrc.nist.gov/pubs/sp/800/53/r5/final |
| ISO/IEC 27001:2022 | Information security management systems — Requirements | https://www.iso.org/isoiec-27001-information-security.html |
| ISO/IEC 27002:2022 | Information security controls | https://www.iso.org/standard/75652.html |
| Marco de Ciberseguridad 5.0 (Agesic) | Guía de implementación — GR1: Adoptar metodología de evaluación de riesgo | https://www.gub.uy/agencia-gobierno-electronico-sociedad-informacion-conocimiento/comunicacion/publicaciones/guia-implementacion-del-mcu-50/gestion-riesgos/gr1-adoptar-metodologia |

---

## Metodologías de Threat Modeling

| Referencia | Descripción | URL |
|------------|-------------|-----|
| STRIDE (Microsoft) | Threat Modeling Methodology | https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats |
| OWASP Threat Modeling | Community guide to threat modeling | https://owasp.org/www-community/Threat_Modeling |
| DREAD | Risk Rating Methodology | https://owasp.org/www-community/OWASP_Risk_Rating_Methodology |
| Microsoft Threat Modeling Tool | Desktop tool for STRIDE threat modeling | https://aka.ms/threatmodelingtool |
| OWASP Threat Dragon | Open-source threat modeling tool | https://threatdragon.org/ |

---

## MITRE ATT&CK

| Referencia | Descripción | URL |
|------------|-------------|-----|
| MITRE ATT&CK Framework | Adversarial Tactics, Techniques & Common Knowledge | https://attack.mitre.org/ |
| ATT&CK Navigator | Herramienta de visualización de técnicas | https://mitre-attack.github.io/attack-navigator/ |
| T1078 — Valid Accounts | Técnica de acceso con cuentas válidas | https://attack.mitre.org/techniques/T1078/ |
| T1566 — Phishing | Técnica de phishing para acceso inicial | https://attack.mitre.org/techniques/T1566/ |
| T1098 — Account Manipulation | Modificación de cuentas y permisos | https://attack.mitre.org/techniques/T1098/ |
| T1005 — Data from Local System | Recolección de datos desde el sistema local | https://attack.mitre.org/techniques/T1005/ |
| T1213 — Data from Information Repositories | Extracción de datos de repositorios | https://attack.mitre.org/techniques/T1213/ |
| T1499 — Endpoint Denial of Service | Ataque de denegación de servicio | https://attack.mitre.org/techniques/T1499/ |
| T1068 — Exploitation for Privilege Escalation | Escalada de privilegios por explotación | https://attack.mitre.org/techniques/T1068/ |
| T1070 — Indicator Removal on Host | Borrado de indicadores / logs | https://attack.mitre.org/techniques/T1070/ |
| T1190 — Exploit Public-Facing Application | Explotación de aplicaciones expuestas públicamente | https://attack.mitre.org/techniques/T1190/ |
| T1552 — Unsecured Credentials | Credenciales no protegidas | https://attack.mitre.org/techniques/T1552/ |

---

## OWASP

| Referencia | Descripción | URL |
|------------|-------------|-----|
| OWASP Top 10 (2021) | Diez riesgos más críticos en aplicaciones web | https://owasp.org/Top10/ |
| OWASP API Security Top 10 (2023) | Riesgos en APIs | https://owasp.org/API-Security/editions/2023/en/0x00-toc/ |
| OWASP Cheat Sheet — Authentication | Buenas prácticas de autenticación | https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html |
| OWASP Cheat Sheet — Authorization | Control de acceso y autorización | https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html |
| OWASP Cheat Sheet — Logging | Prácticas de logging seguro | https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html |
| OWASP Insider Threat | Recursos sobre amenazas internas | https://owasp.org/www-community/attacks/ |

---

## Recursos de Seguridad para RRHH e Insider Threat

| Referencia | Descripción | URL |
|------------|-------------|-----|
| NIST SP 800-53 Familia SI | System and Information Integrity Controls | https://csrc.nist.gov/pubs/sp/800/53/r5/final |
| ISO 27001 Anexo A.6 — People Controls | Controles orientados a personas | https://www.iso.org/isoiec-27001-information-security.html |
| CERT Insider Threat Center | Recursos sobre amenazas internas (CISO) | https://www.cisa.gov/topics/physical-security/insider-threat-mitigation |
| URCDP — Ley 18.331 | Ley de Protección de Datos Personales (Uruguay) | https://www.urcdp.gub.uy/inicio/normativa |

---

## Herramientas Utilizadas en el Análisis

| Herramienta | Tipo | Uso en este proyecto |
|-------------|------|---------------------|
| ChatGPT (GPT-5.5) | IA generativa | Generación del borrador de análisis STRIDE/DREAD |
| Claude (Anthropic) | IA generativa | Organización del repositorio y refinamiento de documentación |
| PlantUML | Diagramación | Diagrama de arquitectura (fuente .puml) |
| PlantUML Web Editor (https://editor.plantuml.com/) | Visualización | Generación del diagrama PNG de arquitectura |

---

## Recursos Adicionales Consultados

- NIST Cybersecurity Framework (CSF) 2.0: https://www.nist.gov/cyberframework
- Oracle Database Security Guide: https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/
- Spring Security Reference Documentation: https://docs.spring.io/spring-security/reference/
- Have I Been Pwned (verificación de credenciales comprometidas): https://haveibeenpwned.com/