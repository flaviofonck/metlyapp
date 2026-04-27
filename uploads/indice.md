<!-- markdownlint-disable -->
# 📚 Índice de Arquitectura - GeastionApp

> **🚀 Navegación Rápida:** Enlaces directos a la documentación técnica completa y resúmenes ejecutivos por módulo.

---

## 📄 Documentación Principal

### [📖 APP.md - Informe Técnico Completo](APP.md)
> Análisis exhaustivo de 32 blueprints, 128 modelos ORM, y 700+ endpoints con trazabilidad funcional completa.

---

## 🗂️ Blueprints y Módulos

### 📦 [ACCIONES](APP.md#acciones) `(6 endpoints)`
> Gestión de acciones correctivas y preventivas del SG-SST

### 📦 [ADMIN](APP.md#admin) `(70 endpoints)`
> Administración de usuarios, permisos y configuración del sistema

### 📦 [AUDITORIAS](APP.md#auditorias) `(12 endpoints)`
> Auditorías internas y externas del Sistema de Gestión

### 📦 [AUSENTISMO](APP.md#ausentismo) `(9 endpoints)`
> Control y análisis de ausentismo laboral

### 📦 [AUTH](APP.md#auth) `(9 endpoints)`
> Autenticación, autorización y gestión de sesiones

### 📦 [BUGS](APP.md#bugs) `(7 endpoints)`
> Sistema de reporte y gestión de errores

### 📦 [CALIDAD_SGC](APP.md#calidad_sgc) `(121 endpoints)`
> Sistema de Gestión de Calidad ISO 9001:2015

### 📦 [CAPACITACIONES](APP.md#capacitaciones) `(29 endpoints)`
> Programa de capacitación y entrenamiento SST

### 📦 [COMITES](APP.md#comites) `(29 endpoints)`
> Comités COPASST y gestión paritaria

### 📦 [CONFIGURACION](APP.md#configuracion) `(26 endpoints)`
> Configuración empresarial y perfiles de clientes

### 📦 [CONVIVENCIA](APP.md#convivencia) `(21 endpoints)`
> Comité de Convivencia Laboral y gestión social

### 📦 [CUMPLIMIENTO](APP.md#cumplimiento) `(14 endpoints)`
> Matriz de cumplimiento legal y normativo

### 📦 [DASHBOARD](APP.md#dashboard) `(7 endpoints)`
> Panel de control e indicadores ejecutivos

### 📦 [DIAGNOSTICO](APP.md#diagnostico) `(30 endpoints)`
> Diagnósticos iniciales y evaluaciones

### 📦 [DOCUMENTOS](APP.md#documentos) `(18 endpoints)`
> Control documental y gestión de archivos

### 📦 [DOTACION_EPP](APP.md#dotacion_epp) `(11 endpoints)`
> Módulo del sistema

### 📦 [EMPLEADOS](APP.md#empleados) `(20 endpoints)`
> Gestión de recursos humanos y personal

### 📦 [FIRMAS](APP.md#firmas) `(10 endpoints)`
> Firmas digitales y autenticación de documentos

### 📦 [FORMULARIOS_PERSONALIZADOS](APP.md#formularios_personalizados) `(0 endpoints)`
> Formularios dinámicos y personalizables

### 📦 [GESTION_AMBIENTAL](APP.md#gestion_ambiental) `(10 endpoints)`
> Módulo del sistema

### 📦 [INDICADORES](APP.md#indicadores) `(6 endpoints)`
> KPIs, métricas e indicadores de gestión

### 📦 [INFORME_GESTION](APP.md#informe_gestion) `(7 endpoints)`
> Informes ejecutivos con IA generativa

### 📦 [INFORMES_ENTIDADES](APP.md#informes_entidades) `(11 endpoints)`
> Reportes a entidades gubernamentales

### 📦 [INSPECCIONES](APP.md#inspecciones) `(25 endpoints)`
> Inspecciones de seguridad e infraestructura

### 📦 [MEDICINA_LABORAL](APP.md#medicina_laboral) `(17 endpoints)`
> Medicina del trabajo y vigilancia epidemiológica

### 📦 [NOTIFICACIONES](APP.md#notificaciones) `(5 endpoints)`
> Sistema de alertas y comunicaciones

### 📦 [PESV](APP.md#pesv) `(33 endpoints)`
> Plan Estratégico de Seguridad Vial

### 📦 [PLAN_TRABAJO](APP.md#plan_trabajo) `(15 endpoints)`
> Planificación y cronograma anual SST

### 📦 [PLATAFORMA](APP.md#plataforma) `(14 endpoints)`
> Módulo del sistema

### 📦 [PORTAL_PROVEEDORES](APP.md#portal_proveedores) `(0 endpoints)`
> Gestión de proveedores y contratistas

### 📦 [PRIVACIDAD](APP.md#privacidad) `(16 endpoints)`
> Protección de datos personales y HABEAS DATA

### 📦 [PROVEEDORES](APP.md#proveedores) `(29 endpoints)`
> Evaluación y control de proveedores

### 📦 [REHABILITACION](APP.md#rehabilitacion) `(14 endpoints)`
> Módulo del sistema

### 📦 [REPORTES](APP.md#reportes) `(16 endpoints)`
> Reportes de incidentes y accidentes laborales

### 📦 [RIESGOS](APP.md#riesgos) `(15 endpoints)`
> Identificación y gestión de peligros y riesgos

### 📦 [RLCPD](APP.md#rlcpd) `(13 endpoints)`
> Módulo del sistema

### 📦 [SANEAMIENTO](APP.md#saneamiento) `(6 endpoints)`
> Módulo del sistema

### 📦 [SGA_AMBIENTAL](APP.md#sga_ambiental) `(64 endpoints)`
> Módulo del sistema

### 📦 [SIG](APP.md#sig) `(13 endpoints)`
> Módulo del sistema

### 📦 [TENANT](APP.md#tenant) `(3 endpoints)`
> Módulo del sistema

### 📦 [VIGILANCIA_EPI](APP.md#vigilancia_epi) `(13 endpoints)`
> Módulo del sistema

### 📦 [VISITAS](APP.md#visitas) `(10 endpoints)`
> Visitas de inspección y seguimiento SST


---

## 🗄️ Arquitectura de Base de Datos 

### [📊 Resumen de Arquitectura](APP.md#2-arquitectura-de-base-de-datos-sqlalchemy-orm)
> 128 modelos ORM, 265 relaciones, 214 foreign keys en arquitectura PostgreSQL

### [🌐 Diagrama de Relaciones Centrales](APP.md#diagrama-de-relaciones-centrales)  
> Diagramas ASCII detallados de ecosistemas PESV, SST e Inspecciones

### [📋 Tabla de Modelos Principales](APP.md#tabla-de-modelos-principales)
> Análisis de entidades centrales con conteo de relaciones y propósito funcional  

### [🔗 Grupos de Modelos por Dominio](APP.md#grupos-de-modelos-por-dominio)
> Organización funcional: SST, PESV, Calidad ISO 9001, Medicina Laboral, etc.

---

## 🎯 Enlaces Rápidos por Funcionalidad

| **Dominio** | **Blueprints Relacionados** | **Modelos Clave** |
|-------------|-----------------------------|--------------------|
| **🛡️ SG-SST** | `reportes`, `acciones`, `riesgos`, `auditorias` | ReporteIncidente, AccionMejora, MatrizRiesgo |
| **🔍 Inspecciones** | `inspecciones`, `visitas` | Inspeccion, InspeccionBotiquin, InspeccionExtintor |  
| **🚗 Seguridad Vial** | `pesv` | PESV, VehiculoPESV, ConductorPESV, SiniestroPESV |
| **🩺 Medicina Laboral** | `medicina_laboral`, `ausentismo` | ExamenMedico, EvaluacionMedica, Incapacidad |
| **📊 Calidad ISO** | `calidad_sgc` | ProcesoSGC, RiesgoCalidad, EvaluacionProveedorCalidad |
| **👥 Recursos Humanos** | `empleados`, `capacitaciones`, `comites` | Empleado, SesionCapacitacion, ComiteCOPASST |

---

## 📈 Estadísticas del Sistema

- **📦 Blueprints Totales:** 42
- **🔗 Endpoints Totales:** 804
- **🗄️ Modelos ORM:** 172
- **🎨 Plantillas HTML:** 395


> **🔄 Actualización Automática:** Este índice se regenera automáticamente al ejecutar `python scripts/actualizar_app_md.py`

> **📅 Última Actualización:** 21/04/2026 20:48:31

---

*⚡ **Tip de Navegación:** Usa Ctrl+F en APP.md para buscar modelos, rutas o blueprints específicos*
