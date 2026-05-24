# 01 — Inventario de Activos

**Sistema:** HRIS — Plataforma de Gestión de Recursos Humanos  
**Fecha:** Mayo 2026  
**Versión:** 1.0

---

## 1.1 Descripción del Sistema

El sistema analizado es una plataforma de gestión de recursos humanos (HRIS) utilizada por una empresa para administrar procesos relacionados con empleados y RRHH.

La plataforma permite gestionar:
- Nóminas y liquidaciones salariales
- Beneficios de empleados
- Evaluaciones de desempeño
- Onboarding digital
- Generación y firma de contratos
- Acceso de empleados a información personal mediante un portal web

La arquitectura está compuesta por una aplicación web, un backend en Java/Spring, una base de datos Oracle y múltiples integraciones externas, incluyendo sistemas de nómina y servicios de firma digital.

---

## 1.2 Alcance del Análisis

### Componentes incluidos

- Aplicación web de RRHH
- Backend Java/Spring
- Base de datos Oracle
- Integración con sistemas de nómina
- Servicio de firma digital
- Portal de empleados
- Módulo de autenticación y control de acceso

### Componentes fuera de alcance

- Infraestructura física del proveedor cloud
- Dispositivos personales de empleados
- Seguridad interna de proveedores externos
- Redes corporativas ajenas al sistema analizado

---

## 1.3 Supuestos

- El sistema es accesible mediante Internet para empleados autorizados.
- La autenticación se realiza mediante usuario y contraseña.
- Existen distintos niveles de permisos según el rol del usuario.
- Las integraciones externas utilizan APIs.
- La base de datos almacena información sensible de empleados y nóminas.
- El sistema cuenta con mecanismos básicos de logging y auditoría.

---

## 2.1 Activos de Información

| ID  | Activo                          | Tipo de dato    | Clasificación | Propietario |
|-----|---------------------------------|-----------------|---------------|-------------|
| A01 | Datos personales de empleados   | PII (Personally Identifiable Information)             | Alta          | RRHH        |
| A02 | Información salarial y nóminas  | Financiero      | Alta          | RRHH        |
| A03 | Contratos laborales             | Legal / PII     | Alta          | RRHH        |
| A04 | Credenciales de usuarios        | Autenticación   | Alta          | Sistema     |
| A05 | Evaluaciones de desempeño       | Confidencial    | Media / Alta  | RRHH        |

### Descripción de activos de información

**A01 — Datos personales de empleados**  
Incluye nombre completo, DNI/CI, domicilio, fecha de nacimiento, datos de contacto, información bancaria y cualquier dato de naturaleza personal (PII). Su exposición implica violaciones legales y daño reputacional severo.

**A02 — Información salarial y nóminas**  
Datos de remuneraciones, deducciones, liquidaciones y beneficios. Su alteración o exposición puede derivar en fraude financiero, conflictos laborales y pérdidas económicas directas.

**A03 — Contratos laborales**  
Documentos legalmente vinculantes con información sensible sobre condiciones de empleo, cláusulas de confidencialidad y remuneración. Su integridad es crítica para la validez legal.

**A04 — Credenciales de usuarios**  
Hashes de contraseñas, tokens de sesión y cualquier mecanismo de autenticación. Su compromiso permite acceso no autorizado al sistema.

**A05 — Evaluaciones de desempeño**  
Registros de rendimiento, observaciones y calificaciones de empleados. Información confidencial que puede afectar decisiones laborales y generar conflictos si se expone.

---

## 2.2 Activos Tecnológicos

| ID  | Activo                          | Tipo                 | Criticidad | Notas                        |
|-----|---------------------------------|----------------------|------------|------------------------------|
| T01 | Aplicación web HRIS             | Software             | Alta       | Portal principal de acceso   |
| T02 | Backend Java/Spring             | Software             | Alta       | Lógica de negocio central    |
| T03 | Base de datos Oracle            | Base de datos        | Crítica    | Almacena todos los datos sensibles |
| T04 | Integración con nómina          | API / Servicio ext.  | Alta       | Procesamiento salarial       |
| T05 | Servicio de firma digital       | Servicio externo     | Alta       | Firma y validez de contratos |
| T06 | Módulo de autenticación         | Componente software  | Crítica    | Control de acceso al sistema |
| T07 | Sistema de logs y auditoría     | Infraestructura      | Alta       | Trazabilidad de eventos      |

### Descripción de activos tecnológicos

**T01 — Aplicación web HRIS**  
Interfaz principal de interacción para empleados y personal de RRHH. Es el punto de entrada al sistema desde Internet. Su disponibilidad es crítica para la operatoria diaria.

**T02 — Backend Java/Spring**  
Implementa la lógica de negocio: validaciones, reglas de negocio, procesamiento de nóminas y gestión de autorizaciones. Es el componente con mayor superficie de ataque interna.

**T03 — Base de datos Oracle**  
Repositorio central de toda la información del sistema: empleados, salarios, contratos y evaluaciones. Su compromiso representa el mayor riesgo para la organización.

**T04 — Integración con nómina externa**  
API que conecta el HRIS con el sistema de liquidación salarial externo. Punto crítico de integridad financiera.

**T05 — Servicio de firma digital**  
Proveedor externo que gestiona la firma electrónica de contratos. Su compromiso invalidaría la integridad legal de los documentos.

**T06 — Módulo de autenticación**  
Gestiona el proceso de login, sesiones y control de acceso basado en roles. Falla crítica si es comprometido.

**T07 — Sistema de logs y auditoría**  
Registra eventos del sistema para trazabilidad y detección de incidentes. Su alteración impide el no repudio.

---

## 2.3 Activos Intangibles

| Activo intangible              | Descripción                                                      |
|-------------------------------|------------------------------------------------------------------|
| Reputación de la empresa       | Daño reputacional ante empleados y terceros por incidentes      |
| Confianza de los empleados     | Base de la relación laboral; un incidente la erosiona directamente |
| Cumplimiento legal y regulatorio | Obligaciones bajo normativa local de protección de datos (URCDP/GDPR) |
| Disponibilidad operativa       | Continuidad de procesos de RRHH (liquidaciones, onboarding, etc.) |

---

## 2.4 Valoración de Criticidad

| Nivel      | Descripción                                                   |
|------------|---------------------------------------------------------------|
| Crítica    | Compromiso tiene impacto irreversible en la organización      |
| Alta       | Impacto significativo; requiere remediación urgente           |
| Media      | Impacto moderado; puede tolerarse por tiempo limitado         |
| Baja       | Impacto menor; puede planificarse su resolución               |
