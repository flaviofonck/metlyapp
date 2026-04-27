<!-- markdownlint-disable -->
# Informe Técnico Muy Avanzado: Arquitectura de GeastionApp

> **Nota del Análisis:** Este formato actualizado escanea directamente los metadatos y **hace lectura profunda sobre el código fuente de los endpoints de la aplicación.** Al no existir un *docstring* en la funcion, la inteligencia heurística recorre la función infiriendo las bases de datos consultadas, los componentes enviados a render y el objetivo algorítmico, garantizando que puedas entender qué hace el sistema **endpoint por endpoint.**

> **🗄️ Estructura de Base de Datos:** Incluye análisis completo de 128 modelos ORM, 265 relaciones, y 214 foreign keys organizados por dominios funcionales (SST, PESV, Calidad, etc.) con diagramas de relaciones centrales.

---

## 1. Módulos y Blueprints (Endpoints y Enrutamiento Profundo)

### 📦 Blueprint / Módulo: `ACCIONES`

#### 🔗 Ruta Servidor: `/acciones/`
- **Nombre de Función (Backend Handler):** `acciones.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `AccionMejora.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `acciones/lista.html`

#### 🔗 Ruta Servidor: `/acciones/<int:id>`
- **Nombre de Función (Backend Handler):** `acciones.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `acciones/detalle.html`

#### 🔗 Ruta Servidor: `/acciones/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `acciones.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True`
  - Plantillas HTML Parseadas: `acciones/form.html`

#### 🔗 Ruta Servidor: `/acciones/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `acciones.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/acciones/<int:id>/exportar`
- **Nombre de Función (Backend Handler):** `acciones.exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/acciones/nueva`
- **Nombre de Función (Backend Handler):** `acciones.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Acciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `acciones/form.html`

---

### 📦 Blueprint / Módulo: `ADMIN`

#### 🔗 Ruta Servidor: `/admin/api/entidades-seg-social`
- **Nombre de Función (Backend Handler):** `admin.api_entidades_seg_social`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** API: devuelve lista de entidades activas por tipo — para datalist en formularios [asesor+]
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EntidadSegSocial`
    - Filtros Aplicados (WHERE): `activo=True, tipo=tipo`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/admin/api/permisos/<perfil>/<codigo_permiso>/toggle`
- **Nombre de Función (Backend Handler):** `admin.api_toggle_permiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** API para activar/desactivar un permiso específico [superadmin]
- **Comentarios Descriptivos:** ─── API PARA GESTIÓN DINÁMICA DE PERMISOS ───────────────────
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PerfilPermiso, Permiso`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Permiso, PerfilPermiso`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilPermiso.permiso → Permiso`
    - Filtros Aplicados (WHERE): `codigo=codigo_permiso, activo=True, 
        perfil=perfil,
        permiso_id=permiso.id
    `
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/admin/api/permisos/estado`
- **Nombre de Función (Backend Handler):** `admin.api_estado_permisos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** API para obtener el estado actual de permisos del usuario actual [todos]
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/admin/auditoria-usuarios`
- **Nombre de Función (Backend Handler):** `admin.auditoria_usuarios`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Auditoría de usuarios:
    - Superadmin: ve todos los usuarios del sistema con su último login y actividad.
    - Cliente: ve únicamente los usuarios de su empresa.
    - Admin: acceso denegado (ruta restringida a superadmin/cliente).
- **Comentarios Descriptivos:** ─── AUDITORÍA DE USUARIOS ────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_usuarios' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=current_user.cliente_id, 
                RegistroActividad.usuario_id.in_(user_ids, RegistroActividad.usuario_id.in_(user_ids`
  - Plantillas HTML Parseadas: `admin/auditoria_usuarios.html`

#### 🔗 Ruta Servidor: `/admin/auditoria-usuarios/<int:id>`
- **Nombre de Función (Backend Handler):** `admin.auditoria_usuario_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Historial completo de actividad de un usuario específico.
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroActividad, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroActividad`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `usuario_id=id, accion=accion, modulo=modulo... (+1 más)`
  - Plantillas HTML Parseadas: `admin/auditoria_usuario_detalle.html`

#### 🔗 Ruta Servidor: `/admin/auditoria-usuarios/<int:id>/limpiar-log`
- **Nombre de Función (Backend Handler):** `admin.auditoria_limpiar_log_usuario`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** [Superadmin] Elimina todos los registros de actividad de un usuario.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_limpiar_log_usuario' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroActividad, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroActividad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RegistroActividad.usuario → Usuario`
    - Filtros Aplicados (WHERE): `usuario_id=id`

#### 🔗 Ruta Servidor: `/admin/auditoria-usuarios/purgar-log`
- **Nombre de Función (Backend Handler):** `admin.auditoria_purgar_log`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** [Superadmin] Purga el log global: elimina registros más antiguos que N días, o todos.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_purgar_log' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroActividad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroActividad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `query`
    - Relaciones Navegadas (JOINS): `RegistroActividad.usuario → Usuario`
    - Filtros Aplicados (WHERE): `RegistroActividad.fecha < corte`

#### 🔗 Ruta Servidor: `/admin/bandeja-correos`
- **Nombre de Función (Backend Handler):** `admin.bandeja_correos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de correos enviados, consultada desde HistorialNotificacion en BD.
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'bandeja_correos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialNotificacion`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `HistorialNotificacion.estado == estado, 
            db.or_(
                HistorialNotificacion.destinatario.ilike(f'%{q}%'`
  - Plantillas HTML Parseadas: `admin/bandeja_correos.html`

#### 🔗 Ruta Servidor: `/admin/bandeja-correos/<int:registro_id>`
- **Nombre de Función (Backend Handler):** `admin.bandeja_correo_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Detalle de un correo individual desde HistorialNotificacion.
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialNotificacion`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `admin/bandeja_correo_detalle.html`

#### 🔗 Ruta Servidor: `/admin/clientes`
- **Nombre de Función (Backend Handler):** `admin.clientes`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── CLIENTES ────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'clientes' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `admin/clientes.html`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/datos`
- **Nombre de Función (Backend Handler):** `admin.cliente_datos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** [superadmin] Panel de gestión de datos — ver conteos y borrar módulos selectivamente.
- **Comentarios Descriptivos:** ─── GESTIÓN DE DATOS DEL CLIENTE (SUPERADMIN) ────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_datos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AuditoriaSST, AutoevaluacionSST, ChecklistDocumentosSST, Cliente, ConductorPESV, ConfigNotificacion, DatoBaseIndicador, DocumentoSST, Empleado, EvaluacionMedica, ExamenMedico, Incapacidad, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PlanAnualSST, PoliticaSST, RegistroAusentismo, ReintegroLaboral, ReporteIncidente, RequisitoEmpresa, RespuestaBateria, ResultadoPVE, SesionCapacitacion, TemaCapacitacion, TipoIncidente, VehiculoPESV, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AccionMejora, Empleado, Area, TemaCapacitacion, SesionCapacitacion, MatrizRiesgo, PlanAnualSST, AuditoriaSST, DocumentoSST, ChecklistDocumentosSST, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, RequisitoEmpresa, PESV, VehiculoPESV, ConductorPESV, TipoIncidente, ConfigNotificacion, ExamenMedico, Incapacidad, ReintegroLaboral, EvaluacionMedica, RegistroAusentismo, ResultadoPVE, RespuestaBateria`
    - Relaciones Navegadas (JOINS): `Cliente.visitas → Visita, Cliente.reportes → ReporteIncidente, Cliente.empleados → Empleado, Cliente.areas → Area, Visita.cliente → Cliente, Inspeccion.visita → Visita, Empleado.cliente → Cliente, Empleado.area → Area, Area.cliente → Cliente, Area.empleados → Empleado, Area.reportes → ReporteIncidente, TipoIncidente.cliente → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.area → Area, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, TemaCapacitacion.sesiones → SesionCapacitacion, SesionCapacitacion.cliente → Cliente, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, ConfigNotificacion.cliente → Cliente, DocumentoSST.cliente → Cliente, AuditoriaSST.cliente → Cliente, PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado, AutoevaluacionSST.cliente → Cliente, ChecklistDocumentosSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, Incapacidad.reintegro → ReintegroLaboral, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, RegistroAusentismo.incapacidad → Incapacidad, EvaluacionMedica.cliente → Cliente, EvaluacionMedica.empleado → Empleado, EvaluacionMedica.examen → ExamenMedico, ResultadoPVE.cliente → Cliente, ResultadoPVE.empleado → Empleado, RespuestaBateria.cliente → Cliente, RespuestaBateria.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=id, cliente_id=id, cliente_id=id... (+30 más)`
  - Plantillas HTML Parseadas: `admin/cliente_datos.html`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/datos/borrar`
- **Nombre de Función (Backend Handler):** `admin.cliente_datos_borrar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** [superadmin] Eliminación selectiva de datos por módulo.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_datos_borrar' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AsistenteCapacitacion, AuditoriaSST, AutoevaluacionSST, ChecklistDocumentosSST, Cliente, ConductorPESV, ConfigNotificacion, DatoBaseIndicador, DocumentoSST, Empleado, EvaluacionMedica, EvidenciaItemDiagnostico, ExamenMedico, Incapacidad, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PlanAnualSST, PoliticaSST, RegistroAusentismo, ReintegroLaboral, ReporteIncidente, RequisitoEmpresa, RespuestaBateria, ResultadoPVE, SesionCapacitacion, TemaCapacitacion, TipoIncidente, VehiculoPESV, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroAusentismo, PESV, VehiculoPESV, ConductorPESV, AuditoriaSST, AccionMejora, Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AsistenteCapacitacion, SesionCapacitacion, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, DocumentoSST, ChecklistDocumentosSST, EvidenciaItemDiagnostico, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, RequisitoEmpresa, Empleado, Area, TipoIncidente, ConfigNotificacion, EvaluacionMedica, ReintegroLaboral, ExamenMedico, Incapacidad, ResultadoPVE, RespuestaBateria`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(`
    - Relaciones Navegadas (JOINS): `Cliente.visitas → Visita, Cliente.reportes → ReporteIncidente, Cliente.empleados → Empleado, Visita.cliente → Cliente, Visita.inspecciones → Inspeccion, Visita.fotos → FotoVisita, Inspeccion.visita → Visita, Empleado.cliente → Cliente, Area.cliente → Cliente, Area.empleados → Empleado, Area.reportes → ReporteIncidente, TipoIncidente.cliente → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.encuesta → EncuestaInvestigacion, ReporteIncidente.furat → FuratDetalle, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, TemaCapacitacion.sesiones → SesionCapacitacion, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.asistentes → AsistenteCapacitacion, AsistenteCapacitacion.sesion → SesionCapacitacion, AsistenteCapacitacion.empleado → Empleado, MatrizRiesgo.cliente → Cliente, MatrizRiesgo.filas → FilaMatrizRiesgo, PlanAnualSST.cliente → Cliente, PlanAnualSST.actividades → ActividadPlan, ConfigNotificacion.cliente → Cliente, DocumentoSST.cliente → Cliente, DocumentoSST.descargas → DescargaDocumento, AuditoriaSST.cliente → Cliente, AuditoriaSST.hallazgos → HallazgoAuditoria, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora, PESV.cliente → Cliente, PESV.actividades → ActividadPESV, PESV.siniestros → SiniestroPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, VehiculoPESV.inspecciones_preop → InspeccionPreOperacional, VehiculoPESV.siniestros → SiniestroPESV, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado, AutoevaluacionSST.cliente → Cliente, EvidenciaItemDiagnostico.cliente → Cliente, ChecklistDocumentosSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.accion_mejora → AccionMejora, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, RegistroAusentismo.incapacidad → Incapacidad, EvaluacionMedica.cliente → Cliente, EvaluacionMedica.empleado → Empleado, EvaluacionMedica.examen → ExamenMedico, ResultadoPVE.cliente → Cliente, ResultadoPVE.empleado → Empleado, RespuestaBateria.cliente → Cliente, RespuestaBateria.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=id, cliente_id=id, cliente_id=id... (+37 más)`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/datos/preview`
- **Nombre de Función (Backend Handler):** `admin.cliente_datos_preview`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Retorna JSON con una muestra de registros (máx. 5) de un módulo.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_datos_preview' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaSST, AutoevaluacionSST, ChecklistDocumentosSST, Cliente, ConfigNotificacion, DatoBaseIndicador, DocumentoSST, Empleado, ExamenMedico, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PlanAnualSST, PoliticaSST, RegistroAusentismo, ReporteIncidente, RequisitoEmpresa, RespuestaBateria, ResultadoPVE, TemaCapacitacion, TipoIncidente, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AccionMejora, Empleado, TipoIncidente, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, AuditoriaSST, DocumentoSST, ChecklistDocumentosSST, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, RequisitoEmpresa, ConfigNotificacion, PESV, ExamenMedico, RegistroAusentismo, ResultadoPVE, RespuestaBateria`
    - Relaciones Navegadas (JOINS): `Cliente.visitas → Visita, Cliente.reportes → ReporteIncidente, Cliente.empleados → Empleado, Visita.cliente → Cliente, Inspeccion.visita → Visita, Empleado.cliente → Cliente, Empleado.area → Area, TipoIncidente.cliente → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.area → Area, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, ConfigNotificacion.cliente → Cliente, DocumentoSST.cliente → Cliente, AuditoriaSST.cliente → Cliente, PESV.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, ChecklistDocumentosSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, ResultadoPVE.cliente → Cliente, ResultadoPVE.empleado → Empleado, RespuestaBateria.cliente → Cliente, RespuestaBateria.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=id, cliente_id=id, cliente_id=id... (+23 más)`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.editar_cliente`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Cliente.usuarios → Usuario`
    - Filtros Aplicados (WHERE): `_C.slug == slug_clean, _C.id != cliente.id`
  - Plantillas HTML Parseadas: `admin/cliente_form.html, admin/cliente_form.html`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.eliminar_cliente`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** [admin/superadmin] Elimina el cliente y TODOS sus datos de forma irreversible.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AuditoriaSST, AutoevaluacionSST, BugReport, ChecklistDocumentosSST, Cliente, ClienteObjetivoVisita, ConductorPESV, ConfigNotificacion, DatoBaseIndicador, DescargaDocumento, DocumentoSST, Empleado, EvaluacionMedica, EvidenciaItemDiagnostico, ExamenMedico, HistorialNotificacion, IdeaRoadmap, Incapacidad, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PerfilEmpresa, PlanAnualSST, PoliticaSST, RegistroAusentismo, ReintegroLaboral, ReporteIncidente, RequisitoEmpresa, RespuestaBateria, ResultadoPVE, TemaCapacitacion, TipoIncidente, Usuario, VehiculoPESV, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario, RequisitoEmpresa, RegistroAusentismo, PESV, VehiculoPESV, ConductorPESV, AuditoriaSST, AccionMejora, Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, DocumentoSST, ChecklistDocumentosSST, EvidenciaItemDiagnostico, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, TipoIncidente, ConfigNotificacion, EvaluacionMedica, ReintegroLaboral, ExamenMedico, Incapacidad, ResultadoPVE, RespuestaBateria, HistorialNotificacion, Empleado, Area, PerfilEmpresa, ClienteObjetivoVisita, DescargaDocumento, BugReport, IdeaRoadmap`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(`
    - Relaciones Navegadas (JOINS): `ClienteObjetivoVisita.cliente → Cliente, Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, Usuario.creado_por → Usuario, Cliente.usuarios → Usuario, Cliente.empleados → Empleado, Visita.cliente → Cliente, Visita.asesor → Usuario, Empleado.cliente → Cliente, Area.cliente → Cliente, Area.empleados → Empleado, TipoIncidente.cliente → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.reportado_por → Usuario, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, ConfigNotificacion.cliente → Cliente, HistorialNotificacion.cliente → Cliente, DocumentoSST.cliente → Cliente, DescargaDocumento.usuario → Usuario, AuditoriaSST.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora, PESV.cliente → Cliente, VehiculoPESV.cliente → Cliente, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado, PerfilEmpresa.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, EvidenciaItemDiagnostico.cliente → Cliente, EvidenciaItemDiagnostico.subido_por → Usuario, ChecklistDocumentosSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.accion_mejora → AccionMejora, BugReport.usuario → Usuario, IdeaRoadmap.usuario → Usuario, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, RegistroAusentismo.incapacidad → Incapacidad, EvaluacionMedica.cliente → Cliente, EvaluacionMedica.empleado → Empleado, ResultadoPVE.cliente → Cliente, ResultadoPVE.empleado → Empleado, RespuestaBateria.cliente → Cliente, RespuestaBateria.empleado → Empleado`
    - Filtros Aplicados (WHERE): `rol='superadmin', activo=True, rol='superadmin', activo=True, cliente_id=id... (+45 más)`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/modulos`
- **Nombre de Función (Backend Handler):** `admin.cliente_modulos`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Activa o desactiva módulos contratados por una empresa.
- **Comentarios Descriptivos:** ─── MÓDULOS ACTIVOS POR CLIENTE ────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_modulos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `admin/cliente_modulos.html`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/objetivos`
- **Nombre de Función (Backend Handler):** `admin.cliente_objetivos`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Permite al superadmin habilitar/deshabilitar tipos de visita por empresa [superadmin]
- **Comentarios Descriptivos:** ─── CONFIG DE OBJETIVOS POR EMPRESA ────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_objetivos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ClienteObjetivoVisita, TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoObjetivoVisita, ClienteObjetivoVisita`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ClienteObjetivoVisita.cliente → Cliente, ClienteObjetivoVisita.objetivo → TipoObjetivoVisita, Visita.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, cliente_id=id`
  - Plantillas HTML Parseadas: `admin/cliente_objetivos.html`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/resumen-eliminacion`
- **Nombre de Función (Backend Handler):** `admin.cliente_resumen_eliminacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** [admin/superadmin] Retorna JSON con resumen de datos del cliente para confirmación de eliminación.
- **Comentarios Descriptivos:** ─── ELIMINACIÓN COMPLETA DE CLIENTE ─────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cliente_resumen_eliminacion' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AuditoriaSST, AutoevaluacionSST, Cliente, DatoBaseIndicador, DocumentoSST, Empleado, ExamenMedico, Incapacidad, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PlanAnualSST, PoliticaSST, RegistroAusentismo, ReporteIncidente, RequisitoEmpresa, TemaCapacitacion, Usuario, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario, Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AccionMejora, Empleado, Area, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, AuditoriaSST, DocumentoSST, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, RequisitoEmpresa, PESV, ExamenMedico, Incapacidad, RegistroAusentismo`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, Visita.cliente → Cliente, Empleado.cliente → Cliente, Area.cliente → Cliente, ReporteIncidente.cliente → Cliente, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, DocumentoSST.cliente → Cliente, AuditoriaSST.cliente → Cliente, PESV.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, ExamenMedico.cliente → Cliente, Incapacidad.cliente → Cliente, RegistroAusentismo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=id, cliente_id=id, cliente_id=id... (+21 más)`

#### 🔗 Ruta Servidor: `/admin/clientes/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.toggle_cliente`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/clientes/crear-test`
- **Nombre de Función (Backend Handler):** `admin.crear_cliente_test`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** [superadmin] Crea un cliente de prueba con datos realistas según sector.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_cliente_test' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.add, session.add, session.add`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente`
    - Filtros Aplicados (WHERE): `nit=nit, email=email_u`
  - Plantillas HTML Parseadas: `admin/crear_cliente_test.html, admin/crear_cliente_test.html, admin/crear_cliente_test.html, admin/crear_cliente_test.html, admin/crear_cliente_test.html, admin/crear_cliente_test.html`

#### 🔗 Ruta Servidor: `/admin/clientes/importar`
- **Nombre de Función (Backend Handler):** `admin.importar_clientes`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Importación masiva de clientes desde archivo Excel o CSV.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_clientes' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente`
  - Plantillas HTML Parseadas: `admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html, admin/importar_clientes.html`

#### 🔗 Ruta Servidor: `/admin/clientes/nuevo`
- **Nombre de Función (Backend Handler):** `admin.nuevo_cliente`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Cliente.usuarios → Usuario`
  - Plantillas HTML Parseadas: `admin/cliente_form.html, admin/cliente_form.html`

#### 🔗 Ruta Servidor: `/admin/clientes/plantilla-importacion`
- **Nombre de Función (Backend Handler):** `admin.plantilla_importacion_clientes`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y devuelve un Excel con la plantilla para importar clientes.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'plantilla_importacion_clientes' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/admin/configuracion-codigos`
- **Nombre de Función (Backend Handler):** `admin.configuracion_codigos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista las configuraciones de código documental por empresa [superadmin]
- **Comentarios Descriptivos:** ─── CONFIGURACIÓN DE CÓDIGOS DOCUMENTALES ───────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'configuracion_codigos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, CodigoDocumentalConfig`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `CodigoDocumentalConfig.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True`
  - Plantillas HTML Parseadas: `admin/configuracion_codigos.html`

#### 🔗 Ruta Servidor: `/admin/configuracion-codigos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.configuracion_codigo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una configuración de código documental [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CodigoDocumentalConfig`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/configuracion-codigos/nueva`
- **Nombre de Función (Backend Handler):** `admin.configuracion_codigo_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crea o actualiza la configuración de código para un tipo y empresa [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, CodigoDocumentalConfig`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, CodigoDocumentalConfig`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CodigoDocumentalConfig.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, 
            cliente_id=cliente_id, tipo_documento=tipo_documento
        `
  - Plantillas HTML Parseadas: `admin/configuracion_codigo_form.html, admin/configuracion_codigo_form.html`

#### 🔗 Ruta Servidor: `/admin/documentos`
- **Nombre de Función (Backend Handler):** `admin.documentos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'documentos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DocumentoCodigo`
    - Filtros Aplicados (WHERE): `documento=doc_id`
  - Plantillas HTML Parseadas: `admin/documentos.html`

#### 🔗 Ruta Servidor: `/admin/documentos/<doc_id>`
- **Nombre de Función (Backend Handler):** `admin.documento_codigos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'documento_codigos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `documento=doc_id`
  - Plantillas HTML Parseadas: `admin/documento_codigos.html`

#### 🔗 Ruta Servidor: `/admin/documentos/<doc_id>/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.documento_codigo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `admin/documento_codigo_form.html`

#### 🔗 Ruta Servidor: `/admin/documentos/<doc_id>/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.documento_codigo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/documentos/<doc_id>/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.documento_codigo_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/documentos/<doc_id>/nuevo`
- **Nombre de Función (Backend Handler):** `admin.documento_codigo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoCodigo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DocumentoCodigo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `documento=doc_id, codigo=codigo`
  - Plantillas HTML Parseadas: `admin/documento_codigo_form.html, admin/documento_codigo_form.html, admin/documento_codigo_form.html`

#### 🔗 Ruta Servidor: `/admin/empresa-asesores`
- **Nombre de Función (Backend Handler):** `admin.empresa_asesores`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista asesores asociados a la empresa del usuario actual
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa_asesores' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente`
    - Filtros Aplicados (WHERE): `Usuario.rol == 'asesor', 
            Usuario.rol == 'asesor',
            db.or_(
                Usuario.cliente_id == current_user.cliente_id,
                Usuario.clientes_asignados.any(Cliente.id == current_user.cliente_id`
  - Plantillas HTML Parseadas: `admin/empresa_asesores.html`

#### 🔗 Ruta Servidor: `/admin/empresa-asesores/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.empresa_editar_asesor`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Editar asesor asociado a la empresa
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, 
                Cliente.id.in_([int(i`
  - Plantillas HTML Parseadas: `admin/empresa_asesor_form.html`

#### 🔗 Ruta Servidor: `/admin/empresa-asesores/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.empresa_toggle_asesor`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activar/desactivar asesor asociado a la empresa
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente`

#### 🔗 Ruta Servidor: `/admin/empresa-asesores/nuevo`
- **Nombre de Función (Backend Handler):** `admin.empresa_nuevo_asesor`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crear nuevo asesor asociado a la empresa
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, email=email, 
                Cliente.id.in_([int(i`
  - Plantillas HTML Parseadas: `admin/empresa_asesor_form.html, admin/empresa_asesor_form.html, admin/empresa_asesor_form.html`

#### 🔗 Ruta Servidor: `/admin/entidades-seg-social`
- **Nombre de Función (Backend Handler):** `admin.entidades_seg_social`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista el catálogo de entidades de seguridad social [superadmin]
- **Comentarios Descriptivos:** ─── CATÁLOGO EPS / AFP / ARL / CAJAS ────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'entidades_seg_social' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EntidadSegSocial`
    - Filtros Aplicados (WHERE): `tipo=tipo_filtro, tipo=t`
  - Plantillas HTML Parseadas: `admin/entidades_seg_social.html`

#### 🔗 Ruta Servidor: `/admin/entidades-seg-social/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.entidad_seg_social_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Edita una entidad existente [superadmin]
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EntidadSegSocial`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `
            EntidadSegSocial.tipo == entidad.tipo,
            EntidadSegSocial.nombre == nombre,
            EntidadSegSocial.id != id
        `
  - Plantillas HTML Parseadas: `admin/entidad_seg_social_form.html, admin/entidad_seg_social_form.html, admin/entidad_seg_social_form.html`

#### 🔗 Ruta Servidor: `/admin/entidades-seg-social/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.entidad_seg_social_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una entidad [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/entidades-seg-social/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.entidad_seg_social_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa/desactiva una entidad [superadmin]
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/entidades-seg-social/nueva`
- **Nombre de Función (Backend Handler):** `admin.entidad_seg_social_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crea una nueva entidad de seguridad social [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntidadSegSocial`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EntidadSegSocial`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `tipo=tipo, nombre=nombre`
  - Plantillas HTML Parseadas: `admin/entidad_seg_social_form.html, admin/entidad_seg_social_form.html, admin/entidad_seg_social_form.html`

#### 🔗 Ruta Servidor: `/admin/login-page`
- **Nombre de Función (Backend Handler):** `admin.login_page`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Vista principal para administrar el contenido de la pantalla de login [superadmin].
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.modulos → LoginPageModulo, LoginPageConfig.stats → LoginPageStat`
    - Filtros Aplicados (WHERE): `activo=True, activo=True`
  - Plantillas HTML Parseadas: `admin/login_page.html`

#### 🔗 Ruta Servidor: `/admin/login-page/modulo/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.login_page_modulo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.modulos → LoginPageModulo`
  - Plantillas HTML Parseadas: `admin/login_page_modulo_form.html`

#### 🔗 Ruta Servidor: `/admin/login-page/modulo/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.login_page_modulo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/login-page/modulo/nuevo`
- **Nombre de Función (Backend Handler):** `admin.login_page_modulo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.modulos → LoginPageModulo`
    - Filtros Aplicados (WHERE): `config_id=cfg.id`
  - Plantillas HTML Parseadas: `admin/login_page_modulo_form.html`

#### 🔗 Ruta Servidor: `/admin/login-page/modulo/reordenar`
- **Nombre de Función (Backend Handler):** `admin.login_page_modulo_reordenar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `LoginPageModulo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/login-page/stat/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.login_page_stat_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `admin/login_page_stat_form.html`

#### 🔗 Ruta Servidor: `/admin/login-page/stat/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.login_page_stat_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/login-page/stat/nuevo`
- **Nombre de Función (Backend Handler):** `admin.login_page_stat_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `config_id=cfg.id`
  - Plantillas HTML Parseadas: `admin/login_page_stat_form.html`

#### 🔗 Ruta Servidor: `/admin/manuales`
- **Nombre de Función (Backend Handler):** `admin.manuales`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de Manuales de Usuario — modo presentación tipo tab APP [todos]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'manuales' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `admin/manuales.html`

#### 🔗 Ruta Servidor: `/admin/mapa-app`
- **Nombre de Función (Backend Handler):** `admin.mapa_app`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Mapa visual interactivo de la app: organigrama expandible con áreas, módulos y funciones.
    Visible para todos los roles autenticados.
- **Comentarios Descriptivos:** ── MAPA DE LA APP ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'mapa_app' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, Cliente.visitas → Visita, Cliente.reportes → ReporteIncidente, Cliente.empleados → Empleado, Cliente.areas → Area, Visita.cliente → Cliente, Visita.asesor → Usuario, Visita.inspecciones → Inspeccion, Inspeccion.visita → Visita, Empleado.cliente → Cliente, Empleado.area → Area, PESV.cliente → Cliente, PESV.actividades → ActividadPESV`
  - Plantillas HTML Parseadas: `admin/mapa_app.html`

#### 🔗 Ruta Servidor: `/admin/permisos`
- **Nombre de Función (Backend Handler):** `admin.gestionar_permisos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista de perfiles disponibles para gestionar permisos [superadmin]
- **Comentarios Descriptivos:** ─── GESTIÓN DE PERMISOS POR PERFIL ──────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'gestionar_permisos' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Permiso.perfiles → PerfilPermiso, PerfilPermiso.permiso → Permiso`
    - Filtros Aplicados (WHERE): `
            PerfilPermiso.perfil == perfil,
            PerfilPermiso.activo == True
        `
  - Plantillas HTML Parseadas: `admin/permisos_lista.html`

#### 🔗 Ruta Servidor: `/admin/permisos/<perfil>`
- **Nombre de Función (Backend Handler):** `admin.permisos_perfil`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Gestión de permisos específicos de un perfil [superadmin] o vista propia [admin]
- **Comentarios Descriptivos:** Verificar acceso según rol
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'permisos_perfil' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PerfilPermiso.permiso → Permiso`
    - Filtros Aplicados (WHERE): `activo=True, 
        PerfilPermiso.perfil == perfil,
        PerfilPermiso.activo == True
    `
  - Plantillas HTML Parseadas: `admin/permisos_perfil.html`

#### 🔗 Ruta Servidor: `/admin/permisos/<perfil>/actualizar`
- **Nombre de Función (Backend Handler):** `admin.actualizar_permisos_perfil`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Actualiza los permisos de un perfil específico [solo superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_permisos_perfil' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PerfilPermiso, Permiso`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Permiso, PerfilPermiso`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilPermiso.permiso → Permiso`
    - Filtros Aplicados (WHERE): `activo=True, 
            perfil=perfil,
            permiso_id=permiso.id
        `

#### 🔗 Ruta Servidor: `/admin/permisos/reset/<perfil>`
- **Nombre de Función (Backend Handler):** `admin.reset_permisos_perfil`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Restablece los permisos por defecto de un perfil [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reset_permisos_perfil' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Permiso.perfiles → PerfilPermiso, PerfilPermiso.permiso → Permiso`
    - Filtros Aplicados (WHERE): `perfil=perfil`

#### 🔗 Ruta Servidor: `/admin/roadmap`
- **Nombre de Función (Backend Handler):** `admin.roadmap`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de roadmap con pendientes del task.md [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'roadmap' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IdeaRoadmap`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `admin/roadmap.html`

#### 🔗 Ruta Servidor: `/admin/roadmap/ideas/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.roadmap_idea_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Eliminar una idea del roadmap [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IdeaRoadmap`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/admin/roadmap/ideas/<int:id>/gestionar`
- **Nombre de Función (Backend Handler):** `admin.roadmap_idea_gestionar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Cambiar estado, fase destino y notas de una idea [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'roadmap_idea_gestionar' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IdeaRoadmap`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/roadmap/ideas/nueva`
- **Nombre de Función (Backend Handler):** `admin.roadmap_idea_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crear nueva idea para el roadmap [asesor+superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IdeaRoadmap`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `IdeaRoadmap.usuario → Usuario`

#### 🔗 Ruta Servidor: `/admin/tipos-objetivo`
- **Nombre de Función (Backend Handler):** `admin.tipos_objetivo`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista todos los tipos de objetivo de visita [superadmin]
- **Comentarios Descriptivos:** ─── TIPOS DE OBJETIVO DE VISITA ─────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'tipos_objetivo' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `admin/tipos_objetivo.html`

#### 🔗 Ruta Servidor: `/admin/tipos-objetivo/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.tipo_objetivo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Visita.tipo_objetivo → TipoObjetivoVisita, Visita.inspecciones → Inspeccion, Visita.fotos → FotoVisita`
  - Plantillas HTML Parseadas: `admin/tipo_objetivo_form.html`

#### 🔗 Ruta Servidor: `/admin/tipos-objetivo/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.tipo_objetivo_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Visita.tipo_objetivo → TipoObjetivoVisita`

#### 🔗 Ruta Servidor: `/admin/tipos-objetivo/nuevo`
- **Nombre de Función (Backend Handler):** `admin.tipo_objetivo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoObjetivoVisita`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Visita.tipo_objetivo → TipoObjetivoVisita, Visita.inspecciones → Inspeccion, Visita.fotos → FotoVisita`
    - Filtros Aplicados (WHERE): `codigo=codigo`
  - Plantillas HTML Parseadas: `admin/tipo_objetivo_form.html, admin/tipo_objetivo_form.html, admin/tipo_objetivo_form.html`

#### 🔗 Ruta Servidor: `/admin/usuarios`
- **Nombre de Función (Backend Handler):** `admin.usuarios`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** El admin solo ve sus propias empresas para filtros
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'usuarios' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, Usuario.rol == filtro_rol, Usuario.activo.is_(True... (+3 más)`
  - Plantillas HTML Parseadas: `admin/usuarios.html`

#### 🔗 Ruta Servidor: `/admin/usuarios/<int:id>/asignaciones`
- **Nombre de Función (Backend Handler):** `admin.usuario_asignaciones`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Previsualiza todo lo asignado al usuario y permite reasignarlo antes de eliminar.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'usuario_asignaciones' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, PlanAnualSST, ReporteIncidente, Usuario, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Visita, PlanAnualSST, CentroTrabajo, ReporteIncidente`
    - Relaciones Navegadas (JOINS): `Visita.asesor → Usuario, ReporteIncidente.reportado_por → Usuario, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `asesor_id=id, asignado_a_asesor_id=id, asesor_asignado_id=id... (+2 más)`
  - Plantillas HTML Parseadas: `admin/usuario_asignaciones.html`

#### 🔗 Ruta Servidor: `/admin/usuarios/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `admin.editar_usuario`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, Usuario.creado_por → Usuario, Cliente.usuarios → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, 
            creado_por_id=admin_creador_id, activo=True
        , Usuario.email == nuevo_email, Usuario.id != id... (+1 más)`
  - Plantillas HTML Parseadas: `admin/usuario_form.html, admin/usuario_form.html, admin/usuario_form.html, admin/usuario_form.html`

#### 🔗 Ruta Servidor: `/admin/usuarios/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `admin.eliminar_usuario`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Eliminar un usuario del sistema.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaAcceso, BugReport, DescargaDocumento, EvidenciaItemDiagnostico, IdeaRoadmap, RegistroActividad, ReporteIncidente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario, ReporteIncidente, EvidenciaItemDiagnostico, DescargaDocumento, RegistroActividad, AuditoriaAcceso, BugReport, IdeaRoadmap`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.reportado_por → Usuario, DescargaDocumento.usuario → Usuario, EvidenciaItemDiagnostico.cliente → Cliente, EvidenciaItemDiagnostico.subido_por → Usuario, BugReport.usuario → Usuario, IdeaRoadmap.usuario → Usuario, AuditoriaAcceso.usuario → Usuario, AuditoriaAcceso.cliente → Cliente, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `reportado_por_id=id, subido_por_id=id, usuario_id=id... (+5 más)`

#### 🔗 Ruta Servidor: `/admin/usuarios/<int:id>/reasignar`
- **Nombre de Función (Backend Handler):** `admin.usuario_reasignar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Reasigna registros del usuario a otro y lo elimina.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'usuario_reasignar' - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaAcceso, BugReport, CentroTrabajo, DescargaDocumento, EvidenciaItemDiagnostico, IdeaRoadmap, PlanAnualSST, RegistroActividad, ReporteIncidente, Usuario, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario, Visita, PlanAnualSST, CentroTrabajo, ReporteIncidente, EvidenciaItemDiagnostico, DescargaDocumento, RegistroActividad, AuditoriaAcceso, BugReport, IdeaRoadmap`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, Visita.cliente → Cliente, Visita.asesor → Usuario, ReporteIncidente.cliente → Cliente, ReporteIncidente.reportado_por → Usuario, CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.cliente → Cliente, PlanAnualSST.asesor_asignado → Usuario, DescargaDocumento.usuario → Usuario, EvidenciaItemDiagnostico.cliente → Cliente, EvidenciaItemDiagnostico.subido_por → Usuario, BugReport.usuario → Usuario, IdeaRoadmap.usuario → Usuario, AuditoriaAcceso.usuario → Usuario, AuditoriaAcceso.cliente → Cliente, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `asesor_id=id, asesor_id=id, asignado_a_asesor_id=id... (+10 más)`

#### 🔗 Ruta Servidor: `/admin/usuarios/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `admin.toggle_usuario`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/admin/usuarios/nuevo`
- **Nombre de Función (Backend Handler):** `admin.nuevo_usuario`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** El admin solo puede asignar usuarios a sus propias empresas
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Admin**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, Usuario.creado_por → Usuario, Cliente.usuarios → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, email=email, 
                        Cliente.id.in_([int(i... (+1 más)`
  - Plantillas HTML Parseadas: `admin/usuario_form.html`

---

### 📦 Blueprint / Módulo: `AUDITORIAS`

#### 🔗 Ruta Servidor: `/auditorias/`
- **Nombre de Función (Backend Handler):** `auditorias.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, estado=estado_f, tipo=tipo_f... (+1 más)`
  - Plantillas HTML Parseadas: `auditorias/lista.html`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>`
- **Nombre de Función (Backend Handler):** `auditorias.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AuditoriaSST.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST`
    - Filtros Aplicados (WHERE): `
            cliente_id=auditoria.cliente_id,
            estado='abierta'
        `
  - Plantillas HTML Parseadas: `auditorias/detalle.html`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `auditorias.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `auditorias/form.html`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `auditorias.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/estado`
- **Nombre de Función (Backend Handler):** `auditorias.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `auditorias.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR EXCEL  [login_required — asesor y cliente ven el suyo] | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `auditorias.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR PDF  [login_required — asesor y cliente ven el suyo] | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/auditorias/<int:id>/hallazgo/nuevo`
- **Nombre de Función (Backend Handler):** `auditorias.nuevo_hallazgo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST, HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora`

#### 🔗 Ruta Servidor: `/auditorias/hallazgo/<int:hid>/eliminar`
- **Nombre de Función (Backend Handler):** `auditorias.eliminar_hallazgo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `HallazgoAuditoria.auditoria → AuditoriaSST`

#### 🔗 Ruta Servidor: `/auditorias/hallazgo/<int:hid>/generar-accion`
- **Nombre de Función (Backend Handler):** `auditorias.generar_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_accion' - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora`

#### 🔗 Ruta Servidor: `/auditorias/hallazgo/<int:hid>/vincular-accion`
- **Nombre de Función (Backend Handler):** `auditorias.vincular_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'vincular_accion' - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora`

#### 🔗 Ruta Servidor: `/auditorias/nueva`
- **Nombre de Función (Backend Handler):** `auditorias.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Auditorias**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `auditorias/form.html, auditorias/form.html`

---

### 📦 Blueprint / Módulo: `AUSENTISMO`

#### 🔗 Ruta Servidor: `/ausentismo/`
- **Nombre de Función (Backend Handler):** `ausentismo.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** LISTA PRINCIPAL | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroAusentismo`
    - Relaciones Navegadas (JOINS): `RegistroAusentismo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, RegistroAusentismo.fecha_inicio >= fecha_desde, RegistroAusentismo.fecha_inicio <= fecha_hasta... (+2 más)`
  - Plantillas HTML Parseadas: `ausentismo/lista.html`

#### 🔗 Ruta Servidor: `/ausentismo/<int:id>`
- **Nombre de Función (Backend Handler):** `ausentismo.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado`
    - Filtros Aplicados (WHERE): `empleado_id=registro.empleado_id`
  - Plantillas HTML Parseadas: `ausentismo/detalle.html`

#### 🔗 Ruta Servidor: `/ausentismo/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `ausentismo.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad, RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, Incapacidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, RegistroAusentismo.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid`
  - Plantillas HTML Parseadas: `ausentismo/form.html`

#### 🔗 Ruta Servidor: `/ausentismo/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `ausentismo.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RegistroAusentismo.cliente → Cliente`

#### 🔗 Ruta Servidor: `/ausentismo/ajax/empleados`
- **Nombre de Función (Backend Handler):** `ausentismo.ajax_empleados`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ajax_empleados' - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True`

#### 🔗 Ruta Servidor: `/ausentismo/ajax/incapacidades`
- **Nombre de Función (Backend Handler):** `ausentismo.ajax_incapacidades`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ajax_incapacidades' - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Incapacidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Incapacidad`
    - Relaciones Navegadas (JOINS): `Incapacidad.empleado → Empleado`
    - Filtros Aplicados (WHERE): `empleado_id=empleado_id`

#### 🔗 Ruta Servidor: `/ausentismo/analisis`
- **Nombre de Función (Backend Handler):** `ausentismo.analisis`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ANÁLISIS DE CAUSAS (MOD-44 funcionalidad 5) | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'analisis' - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroAusentismo`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cid, db.extract('year', RegistroAusentismo.fecha_inicio... (+1 más)`
  - Plantillas HTML Parseadas: `ausentismo/analisis.html`

#### 🔗 Ruta Servidor: `/ausentismo/exportar/excel`
- **Nombre de Función (Backend Handler):** `ausentismo.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTACIÓN EXCEL (MOD-44 funcionalidad 6) | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/ausentismo/nuevo`
- **Nombre de Función (Backend Handler):** `ausentismo.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Ausentismo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad, RegistroAusentismo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, Incapacidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.empleado → Empleado, RegistroAusentismo.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid`
  - Plantillas HTML Parseadas: `ausentismo/form.html`

---

### 📦 Blueprint / Módulo: `AUTH`

#### 🔗 Ruta Servidor: `/auth/cambiar-contexto`
- **Nombre de Función (Backend Handler):** `auth.cambiar_contexto`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Cambiar contexto del superadmin sin hacer logout
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_contexto' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Cliente.usuarios → Usuario`
  - Plantillas HTML Parseadas: `auth/cambiar_contexto.html`

#### 🔗 Ruta Servidor: `/auth/cambiar-contrasena`
- **Nombre de Función (Backend Handler):** `auth.cambiar_contrasena`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_contrasena' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `auth/cambiar_contrasena.html`

#### 🔗 Ruta Servidor: `/auth/login`
- **Nombre de Función (Backend Handler):** `auth.login`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** MOD-41 · Mostrar aviso si la sesión expiró
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, LoginPageConfig, RegistroActividad, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente, LoginPageConfig.modulos → LoginPageModulo, LoginPageConfig.stats → LoginPageStat`
    - Filtros Aplicados (WHERE): `email=email, activo=True, usuario_id=user.id, accion='login', id=_cfg.id... (+3 más)`
  - Plantillas HTML Parseadas: `tenant/login.html`

#### 🔗 Ruta Servidor: `/auth/logout`
- **Nombre de Función (Backend Handler):** `auth.logout`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Limpiar contexto del superadmin | Limpiar empresa seleccionada del asesor
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'logout' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/auth/recuperar-contrasena`
- **Nombre de Función (Backend Handler):** `auth.recuperar_contrasena`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Paso 1 — el usuario ingresa su email y recibe un enlace de reset.
- **Comentarios Descriptivos:** ────────────────────────────────────────────────────────────── | RECUPERACIÓN DE CONTRASEÑA (flujo transaccional por email) | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'recuperar_contrasena' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `email=email, activo=True`
  - Plantillas HTML Parseadas: `auth/recuperar_contrasena.html, auth/recuperar_contrasena.html`

#### 🔗 Ruta Servidor: `/auth/restablecer-contrasena/<token>`
- **Nombre de Función (Backend Handler):** `auth.restablecer_contrasena`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Paso 2 — el usuario establece su nueva contraseña usando el token del email.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'restablecer_contrasena' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `reset_token=token`
  - Plantillas HTML Parseadas: `auth/restablecer_contrasena.html`

#### 🔗 Ruta Servidor: `/auth/seleccionar-contexto`
- **Nombre de Función (Backend Handler):** `auth.seleccionar_contexto`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Selección de contexto para superadmin al hacer login
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'seleccionar_contexto' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `auth/seleccionar_contexto.html`

#### 🔗 Ruta Servidor: `/auth/seleccionar-empresa`
- **Nombre de Función (Backend Handler):** `auth.seleccionar_empresa`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Selector obligatorio de empresa para asesores con múltiples asignaciones.
- **Comentarios Descriptivos:** Solo asesores normales (no superadmin, no cliente)
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'seleccionar_empresa' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `auth/seleccionar_empresa.html`

#### 🔗 Ruta Servidor: `/auth/simular-rol`
- **Nombre de Función (Backend Handler):** `auth.simular_rol`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Cambio rápido de rol simulado para superadmin en modo empresa
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'simular_rol' - asociado al sistema maestro de **Auth**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

---

### 📦 Blueprint / Módulo: `BUGS`

#### 🔗 Ruta Servidor: `/bugs/admin`
- **Nombre de Función (Backend Handler):** `bugs.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── GET/POST /bugs/admin  [superadmin] ─────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport, Usuario`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `BugReport.usuario → Usuario`
    - Filtros Aplicados (WHERE): `estado=estado_filtro, usuario_id=int(usuario_filtro`
  - Plantillas HTML Parseadas: `bugs/lista.html`

#### 🔗 Ruta Servidor: `/bugs/admin/<int:bug_id>`
- **Nombre de Función (Backend Handler):** `bugs.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── GET /bugs/admin/<id>  [superadmin] ────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `bugs/detalle.html`

#### 🔗 Ruta Servidor: `/bugs/admin/<int:bug_id>/eliminar`
- **Nombre de Función (Backend Handler):** `bugs.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── POST /bugs/admin/<id>/eliminar  [superadmin] ────────────────────────── | Eliminar screenshot si existe
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/bugs/admin/<int:bug_id>/estado`
- **Nombre de Función (Backend Handler):** `bugs.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── POST /bugs/admin/<id>/estado  [superadmin] ────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/bugs/mis-reportes`
- **Nombre de Función (Backend Handler):** `bugs.mis_reportes`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════════════════════════ | ── GET /bugs/mis-reportes  [todos los usuarios autenticados] ─────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'mis_reportes' - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `BugReport`
    - Relaciones Navegadas (JOINS): `BugReport.usuario → Usuario`
    - Filtros Aplicados (WHERE): `usuario_id=current_user.id, usuario_id=current_user.id, BugReport.estado == estado_filtro... (+2 más)`
  - Plantillas HTML Parseadas: `bugs/mis_reportes.html`

#### 🔗 Ruta Servidor: `/bugs/mis-reportes/<int:bug_id>`
- **Nombre de Función (Backend Handler):** `bugs.seguimiento`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── GET /bugs/mis-reportes/<id>  [solo el reporter de ese bug] ─────────── | Solo el reporter (o superadmin) puede ver el seguimiento
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'seguimiento' - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `BugReport.usuario → Usuario`
  - Plantillas HTML Parseadas: `bugs/seguimiento.html`

#### 🔗 Ruta Servidor: `/bugs/reportar`
- **Nombre de Función (Backend Handler):** `bugs.reportar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── POST /bugs/reportar  [todos los usuarios autenticados] ──────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reportar' - asociado al sistema maestro de **Bugs**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `BugReport`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `BugReport.usuario → Usuario`

---

### 📦 Blueprint / Módulo: `CALIDAD_SGC`

#### 🔗 Ruta Servidor: `/calidad-sgc/`
- **Nombre de Función (Backend Handler):** `calidad_sgc.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** DASHBOARD / ÍNDICE DEL MÓDULO | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AlcanceSGC, AnalisisContextoSGC, AuditoriaCertificacionSGC, AuditoriaSST, AutoevaluacionISO9001, CambioPlaneadoSGC, CompetenciaSGC, ComunicacionSGC, ControlProduccionSGC, DisenoDesarrolloSGC, DocumentoSST, EvaluacionProveedorCalidad, FichaProceso, IndicadorCalidadSGC, IniciativaMejora, LiberacionProducto, ObjetivoCalidad, ParteInteresadaSGC, PoliticaCalidad, ProcesoSGC, RequisitoProducto, RetroalimentacionCliente, RiesgoCalidad, RolSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AnalisisContextoSGC, ParteInteresadaSGC, ProcesoSGC, AlcanceSGC, PoliticaCalidad, RolSGC, ObjetivoCalidad, RiesgoCalidad, CambioPlaneadoSGC, AccionMejora, CompetenciaSGC, ComunicacionSGC, DocumentoSST, RequisitoProducto, DisenoDesarrolloSGC, EvaluacionProveedorCalidad, ControlProduccionSGC, LiberacionProducto, IndicadorCalidadSGC, RetroalimentacionCliente, AuditoriaCertificacionSGC, AuditoriaSST, IniciativaMejora`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, DocumentoSST.cliente → Cliente, AuditoriaSST.cliente → Cliente, Proveedor.cliente → Cliente, AnalisisContextoSGC.cliente → Cliente, ParteInteresadaSGC.cliente → Cliente, ProcesoSGC.cliente → Cliente, AlcanceSGC.cliente → Cliente, PoliticaCalidad.cliente → Cliente, ObjetivoCalidad.cliente → Cliente, RolSGC.cliente → Cliente, RiesgoCalidad.cliente → Cliente, RiesgoCalidad.proceso → ProcesoSGC, CambioPlaneadoSGC.cliente → Cliente, ComunicacionSGC.cliente → Cliente, CompetenciaSGC.cliente → Cliente, FichaProceso.proceso → ProcesoSGC, RequisitoProducto.cliente → Cliente, RequisitoProducto.proceso → ProcesoSGC, DisenoDesarrolloSGC.cliente → Cliente, DisenoDesarrolloSGC.proceso → ProcesoSGC, EvaluacionProveedorCalidad.cliente → Cliente, EvaluacionProveedorCalidad.proveedor → Proveedor, EvaluacionProveedorCalidad.proceso → ProcesoSGC, ControlProduccionSGC.cliente → Cliente, ControlProduccionSGC.proceso → ProcesoSGC, LiberacionProducto.cliente → Cliente, LiberacionProducto.proceso → ProcesoSGC, AuditoriaCertificacionSGC.cliente → Cliente, RetroalimentacionCliente.cliente → Cliente, IndicadorCalidadSGC.cliente → Cliente, AutoevaluacionISO9001.cliente → Cliente, IniciativaMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, anio=anio, 
        cliente_id=cliente_id, activa=True, 
        cliente_id=cliente_id, activo=True... (+23 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/index.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/alcance`
- **Nombre de Función (Backend Handler):** `calidad_sgc.alcance_ver`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ALCANCE DEL SGC (cláusula 4.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'alcance_ver' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AlcanceSGC`
    - Relaciones Navegadas (JOINS): `AlcanceSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, vigente=True, 
        cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/alcance_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/alcance/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.alcance_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AlcanceSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/alcance/guardar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.alcance_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'alcance_guardar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AlcanceSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AlcanceSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, vigente=True`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────── LISTA ────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, AuditoriaCertificacionSGC.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────── DETALLE ──────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente, AuditoriaCertificacionSGC.evaluaciones → EvaluacionClausulaSGC`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/clausula/<codigo_cl>/evaluar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_clausula_evaluar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_clausula_evaluar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaCertificacionSGC, EvaluacionClausulaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EvaluacionClausulaSGC, AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AuditoriaCertificacionSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        auditoria_id=id, clausula_codigo=codigo_cl, 
            cliente_id=aud.cliente_id,
            descripcion_hallazgo=texto_desc
        `

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────── EDITAR ───────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────── ELIMINAR ─────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/finalizar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_finalizar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────── FINALIZAR ────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_cert_finalizar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/presentacion`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_presentacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────── MODO PRESENTACIÓN ───────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_cert_presentacion' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente, AuditoriaCertificacionSGC.evaluaciones → EvaluacionClausulaSGC`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_presentacion.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/<int:id>/revision`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_revision`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────── MODO REVISIÓN ───────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_cert_revision' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente, AuditoriaCertificacionSGC.evaluaciones → EvaluacionClausulaSGC`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_revision.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditoria-certificacion/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_cert_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────── NUEVA ────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaCertificacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaCertificacionSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_cert_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CLÁUSULA 9.2 · AUDITORÍAS INTERNAS SGC (reutiliza AuditoriaSST) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AuditoriaSST`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo_sistema='calidad',
        tipo='interna'
    , estado=estado_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_interna_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_interna_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_interna_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/<int:id>/hallazgo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_hallazgo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_interna_sgc_hallazgo' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaSST, HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AuditoriaSST.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/<int:id>/hallazgo/<int:hid>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_hallazgo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HallazgoAuditoria`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `HallazgoAuditoria.auditoria → AuditoriaSST`

#### 🔗 Ruta Servidor: `/calidad-sgc/auditorias-internas-sgc/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.auditoria_interna_sgc_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/auditoria_interna_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── Lista de autoevaluaciones ─────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/autoeval_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── Detalle de autoevaluación ─────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente, AutoevaluacionISO9001.items → ItemAutoevaluacionISO`
  - Plantillas HTML Parseadas: `calidad_sgc/autoeval_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── Eliminar ──────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/evaluar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_evaluar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ── Evaluar ítems ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'autoeval_evaluar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente, AutoevaluacionISO9001.items → ItemAutoevaluacionISO`
  - Plantillas HTML Parseadas: `calidad_sgc/autoeval_evaluar.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── Exportar Excel ────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── Exportar PDF ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/finalizar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_finalizar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── Finalizar / Reabrir ───────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'autoeval_finalizar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/generar-acciones`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_generar_acciones`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── Generar acciones correctivas desde ítems no conformes ─────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'autoeval_generar_acciones' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AutoevaluacionISO9001.cliente → Cliente, AutoevaluacionISO9001.items → ItemAutoevaluacionISO`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/<int:id>/reabrir`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_reabrir`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'autoeval_reabrir' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/autoevaluacion-iso9001/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.autoeval_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ── Nueva autoevaluación ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionISO9001`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AutoevaluacionISO9001`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionISO9001.cliente → Cliente, AutoevaluacionISO9001.items → ItemAutoevaluacionISO`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, anio=anio_actual, cliente_id=cliente_id, anio=anio`
  - Plantillas HTML Parseadas: `calidad_sgc/autoeval_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/cambios-planeados`
- **Nombre de Función (Backend Handler):** `calidad_sgc.cambios_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PLANIFICACIÓN DE CAMBIOS (cláusula 6.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CambioPlaneadoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CambioPlaneadoSGC`
    - Relaciones Navegadas (JOINS): `CambioPlaneadoSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, estado=estado_filtro, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/cambios_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/cambios-planeados/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.cambios_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CambioPlaneadoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CambioPlaneadoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/cambios_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/cambios-planeados/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.cambios_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CambioPlaneadoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `CambioPlaneadoSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/cambios-planeados/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.cambios_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CambioPlaneadoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CambioPlaneadoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/cambios_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/competencias`
- **Nombre de Función (Backend Handler):** `calidad_sgc.competencias_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** COMPETENCIA DEL PERSONAL (cláusula 7.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompetenciaSGC, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CompetenciaSGC, SesionCapacitacion`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, CompetenciaSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activa=True, 
        cliente_id=cliente_id, tipo_sistema='calidad'
    , 
        cliente_id=cliente_id, activa=True... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/competencias_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/competencias/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.competencias_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompetenciaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CompetenciaSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/competencias_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/competencias/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.competencias_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompetenciaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `CompetenciaSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/competencias/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.competencias_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompetenciaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CompetenciaSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/competencias_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/comunicaciones`
- **Nombre de Función (Backend Handler):** `calidad_sgc.comunicaciones_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** COMUNICACIONES SGC (cláusulas 7.3–7.4) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ComunicacionSGC`
    - Relaciones Navegadas (JOINS): `ComunicacionSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo=tipo_filtro, canal=canal_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/comunicaciones_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/comunicaciones/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.comunicaciones_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ComunicacionSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/comunicaciones_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/comunicaciones/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.comunicaciones_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ComunicacionSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/comunicaciones/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.comunicaciones_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComunicacionSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/comunicaciones_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/contexto`
- **Nombre de Función (Backend Handler):** `calidad_sgc.contexto_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ANÁLISIS DE CONTEXTO (cláusula 4.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AnalisisContextoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AnalisisContextoSGC`
    - Relaciones Navegadas (JOINS): `AnalisisContextoSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/contexto_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/contexto/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.contexto_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AnalisisContextoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AnalisisContextoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/contexto_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/contexto/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.contexto_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AnalisisContextoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AnalisisContextoSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/contexto/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.contexto_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AnalisisContextoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AnalisisContextoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/contexto_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/control-produccion`
- **Nombre de Función (Backend Handler):** `calidad_sgc.control_produccion_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CONTROL DE PRODUCCIÓN / PRESTACIÓN DEL SERVICIO (cláusula 8.5) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ControlProduccionSGC, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ControlProduccionSGC, ProcesoSGC`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, ControlProduccionSGC.cliente → Cliente, ControlProduccionSGC.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, estado=estado_filtro, proceso_id=proceso_filtro... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/control_produccion_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/control-produccion/<int:id>/detalle`
- **Nombre de Función (Backend Handler):** `calidad_sgc.control_produccion_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ControlProduccionSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ControlProduccionSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/control_produccion_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/control-produccion/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.control_produccion_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, ControlProduccionSGC, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, ProcesoSGC.cliente → Cliente, ControlProduccionSGC.cliente → Cliente, ControlProduccionSGC.proceso → ProcesoSGC, ControlProduccionSGC.accion_mejora → AccionMejora`
    - Filtros Aplicados (WHERE): `
        cliente_id=registro.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/control_produccion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/control-produccion/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.control_produccion_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ControlProduccionSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ControlProduccionSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/control-produccion/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.control_produccion_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, ControlProduccionSGC, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, ProcesoSGC.cliente → Cliente, ControlProduccionSGC.cliente → Cliente, ControlProduccionSGC.proceso → ProcesoSGC, ControlProduccionSGC.accion_mejora → AccionMejora`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/control_produccion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/diseno-desarrollo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.diseno_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** DISEÑO Y DESARROLLO (cláusula 8.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DisenoDesarrolloSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DisenoDesarrolloSGC`
    - Relaciones Navegadas (JOINS): `DisenoDesarrolloSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, estado=estado_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/diseno_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/diseno-desarrollo/<int:id>/detalle`
- **Nombre de Función (Backend Handler):** `calidad_sgc.diseno_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DisenoDesarrolloSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DisenoDesarrolloSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/diseno_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/diseno-desarrollo/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.diseno_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DisenoDesarrolloSGC, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, DisenoDesarrolloSGC.cliente → Cliente, DisenoDesarrolloSGC.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=diseno.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/diseno_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/diseno-desarrollo/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.diseno_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DisenoDesarrolloSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `DisenoDesarrolloSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/diseno-desarrollo/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.diseno_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DisenoDesarrolloSGC, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, DisenoDesarrolloSGC.cliente → Cliente, DisenoDesarrolloSGC.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/diseno_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/documentos-calidad`
- **Nombre de Función (Backend Handler):** `calidad_sgc.documentos_calidad_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** INFORMACIÓN DOCUMENTADA (cláusula 7.5) — Reutiliza DocumentoSST | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DocumentoSST`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo_sistema='calidad', tipo=tipo_filtro, estado=estado_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/documentos_calidad_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/fichas-proceso`
- **Nombre de Función (Backend Handler):** `calidad_sgc.fichas_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** FICHAS DE PROCESO | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FichaProceso, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC, FichaProceso`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, FichaProceso.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True, proceso_id=proceso_filtro, FichaProceso.proceso_id.in_(proceso_ids`
  - Plantillas HTML Parseadas: `calidad_sgc/fichas_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/fichas-proceso/<int:id>/detalle`
- **Nombre de Función (Backend Handler):** `calidad_sgc.fichas_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FichaProceso`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `FichaProceso.proceso → ProcesoSGC`
  - Plantillas HTML Parseadas: `calidad_sgc/fichas_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/fichas-proceso/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.fichas_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FichaProceso, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, FichaProceso.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=ficha.proceso.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/fichas_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/fichas-proceso/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.fichas_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FichaProceso`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `FichaProceso.proceso → ProcesoSGC`

#### 🔗 Ruta Servidor: `/calidad-sgc/fichas-proceso/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.fichas_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FichaProceso, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, FichaProceso.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/fichas_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/iniciativas-mejora`
- **Nombre de Función (Backend Handler):** `calidad_sgc.iniciativas_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IniciativaMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `IniciativaMejora`
    - Relaciones Navegadas (JOINS): `IniciativaMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo=tipo_f, estado=estado_f... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/iniciativas_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/iniciativas-mejora/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.iniciativa_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IniciativaMejora`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `IniciativaMejora.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/iniciativa_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/iniciativas-mejora/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.iniciativa_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IniciativaMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `IniciativaMejora.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/iniciativa_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/iniciativas-mejora/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.iniciativa_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IniciativaMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `IniciativaMejora.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/iniciativas-mejora/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.iniciativa_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IniciativaMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `IniciativaMejora.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/iniciativa_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/kpis-calidad`
- **Nombre de Función (Backend Handler):** `calidad_sgc.kpis_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CLÁUSULA 9.1 · KPIs DE CALIDAD (IndicadorCalidadSGC) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IndicadorCalidadSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `IndicadorCalidadSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio`
  - Plantillas HTML Parseadas: `calidad_sgc/kpis_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/kpis-calidad/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.kpis_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IndicadorCalidadSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `IndicadorCalidadSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/kpis_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/kpis-calidad/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.kpis_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IndicadorCalidadSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `IndicadorCalidadSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/kpis-calidad/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.kpis_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IndicadorCalidadSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `IndicadorCalidadSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `IndicadorCalidadSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id_form, anio=anio, mes=mes`
  - Plantillas HTML Parseadas: `calidad_sgc/kpis_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/liberacion-productos`
- **Nombre de Función (Backend Handler):** `calidad_sgc.liberacion_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** LIBERACIÓN DE PRODUCTOS / SERVICIOS (cláusula 8.5.6 / 8.6) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LiberacionProducto, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `LiberacionProducto, ProcesoSGC`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, LiberacionProducto.cliente → Cliente, LiberacionProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, resultado=resultado_filtro, proceso_id=proceso_filtro... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/liberacion_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/liberacion-productos/<int:id>/detalle`
- **Nombre de Función (Backend Handler):** `calidad_sgc.liberacion_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LiberacionProducto`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `LiberacionProducto.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/liberacion_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/liberacion-productos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.liberacion_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LiberacionProducto, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, LiberacionProducto.cliente → Cliente, LiberacionProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=lib.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/liberacion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/liberacion-productos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.liberacion_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LiberacionProducto`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `LiberacionProducto.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/liberacion-productos/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.liberacion_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LiberacionProducto, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, LiberacionProducto.cliente → Cliente, LiberacionProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/liberacion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/objetivos-calidad`
- **Nombre de Función (Backend Handler):** `calidad_sgc.objetivos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** OBJETIVOS DE CALIDAD (cláusula 6.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoCalidad`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ObjetivoCalidad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio`
  - Plantillas HTML Parseadas: `calidad_sgc/objetivos_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/objetivos-calidad/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.objetivos_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoCalidad, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, ObjetivoCalidad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=obj.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/objetivos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/objetivos-calidad/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.objetivos_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ObjetivoCalidad.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/objetivos-calidad/<int:id>/seguimiento`
- **Nombre de Función (Backend Handler):** `calidad_sgc.objetivos_seguimiento`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'objetivos_seguimiento' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ObjetivoCalidad.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/objetivos-calidad/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.objetivos_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoCalidad, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, ObjetivoCalidad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/objetivos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/oportunidades`
- **Nombre de Función (Backend Handler):** `calidad_sgc.oportunidades_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** OPORTUNIDADES DE MEJORA (cláusula 6.1) → AccionMejora | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/oportunidades_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/oportunidades/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.oportunidades_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, ProcesoSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/oportunidades_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/partes-interesadas`
- **Nombre de Función (Backend Handler):** `calidad_sgc.partes_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PARTES INTERESADAS (cláusula 4.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ParteInteresadaSGC`
    - Relaciones Navegadas (JOINS): `ParteInteresadaSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activa=True, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/partes_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/partes-interesadas/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.partes_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ParteInteresadaSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/partes_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/partes-interesadas/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.partes_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ParteInteresadaSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/partes-interesadas/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.partes_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ParteInteresadaSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/partes_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/politica-calidad`
- **Nombre de Función (Backend Handler):** `calidad_sgc.politica_ver`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** POLÍTICA DE CALIDAD (cláusula 5.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica_ver' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaCalidad`
    - Relaciones Navegadas (JOINS): `PoliticaCalidad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/politica_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/politica-calidad/guardar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.politica_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica_guardar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaCalidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PoliticaCalidad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`

#### 🔗 Ruta Servidor: `/calidad-sgc/procesos`
- **Nombre de Función (Backend Handler):** `calidad_sgc.procesos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** MAPA DE PROCESOS (cláusula 4.4) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/procesos_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/procesos/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.procesos_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/procesos_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/procesos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.procesos_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/procesos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/procesos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.procesos_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/procesos/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.procesos_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/procesos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/proveedores-externos`
- **Nombre de Función (Backend Handler):** `calidad_sgc.proveedores_calidad_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista integrada: proveedores del cliente + sus evaluaciones de calidad.
- **Comentarios Descriptivos:** CONTROL DE PROVEEDORES EXTERNOS (cláusula 8.4) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvaluacionProveedorCalidad, Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Proveedor, EvaluacionProveedorCalidad`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, EvaluacionProveedorCalidad.cliente → Cliente, EvaluacionProveedorCalidad.proveedor → Proveedor`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True, cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/proveedores_calidad_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/proveedores-externos/<int:proveedor_id>/evaluacion/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.proveedores_calidad_nueva_evaluacion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvaluacionProveedorCalidad, ProcesoSGC, Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC, EvaluacionProveedorCalidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ProcesoSGC.cliente → Cliente, EvaluacionProveedorCalidad.cliente → Cliente, EvaluacionProveedorCalidad.proveedor → Proveedor, EvaluacionProveedorCalidad.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True, 
        proveedor_id=proveedor_id`
  - Plantillas HTML Parseadas: `calidad_sgc/proveedores_calidad_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/proveedores-externos/evaluacion/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.proveedores_calidad_eliminar_evaluacion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvaluacionProveedorCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, EvaluacionProveedorCalidad.cliente → Cliente, EvaluacionProveedorCalidad.proveedor → Proveedor`

#### 🔗 Ruta Servidor: `/calidad-sgc/requisitos`
- **Nombre de Función (Backend Handler):** `calidad_sgc.requisitos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** REQUISITOS DE PRODUCTOS/SERVICIOS (cláusula 8.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RequisitoProducto`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RequisitoProducto, ProcesoSGC`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RequisitoProducto.cliente → Cliente, RequisitoProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo=tipo_filtro, estado_cumplimiento=estado_filtro... (+2 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/requisitos_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/requisitos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.requisitos_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RequisitoProducto`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RequisitoProducto.cliente → Cliente, RequisitoProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=req.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/requisitos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/requisitos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.requisitos_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RequisitoProducto`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RequisitoProducto.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/requisitos/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.requisitos_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RequisitoProducto`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RequisitoProducto.cliente → Cliente, RequisitoProducto.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/requisitos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CLÁUSULA 9.1.2 · RETROALIMENTACIÓN DEL CLIENTE | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RetroalimentacionCliente`
    - Relaciones Navegadas (JOINS): `RetroalimentacionCliente.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo=tipo_filtro, estado=estado_filtro`
  - Plantillas HTML Parseadas: `calidad_sgc/retroalimentacion_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RetroalimentacionCliente.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/retroalimentacion_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion/<int:id>/cerrar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_cerrar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'retroalimentacion_cerrar' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RetroalimentacionCliente.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RetroalimentacionCliente.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/retroalimentacion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RetroalimentacionCliente.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/retroalimentacion/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.retroalimentacion_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, RetroalimentacionCliente.cliente → Cliente, RetroalimentacionCliente.accion_mejora → AccionMejora`
  - Plantillas HTML Parseadas: `calidad_sgc/retroalimentacion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/revision-direccion-sgc`
- **Nombre de Función (Backend Handler):** `calidad_sgc.revision_direccion_sgc_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** (reutiliza AuditoriaSST con tipo=revision_gerencial, tipo_sistema=calidad) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo='revision_gerencial', tipo_sistema='calidad'`
  - Plantillas HTML Parseadas: `calidad_sgc/revision_direccion_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/revision-direccion-sgc/<int:id>`
- **Nombre de Función (Backend Handler):** `calidad_sgc.revision_direccion_sgc_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaSST, IndicadorCalidadSGC, RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AuditoriaSST.cliente → Cliente, RetroalimentacionCliente.cliente → Cliente, IndicadorCalidadSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=rev.cliente_id, anio=anio, cliente_id=rev.cliente_id, cliente_id=rev.cliente_id, tipo_sistema='calidad', tipo='interna'... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/revision_direccion_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/revision-direccion-sgc/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.revision_direccion_sgc_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST, IndicadorCalidadSGC, RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente, RetroalimentacionCliente.cliente → Cliente, IndicadorCalidadSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=rev.cliente_id, anio=anio, cliente_id=rev.cliente_id, tipo_sistema='calidad', tipo='interna', cliente_id=rev.cliente_id`
  - Plantillas HTML Parseadas: `calidad_sgc/revision_direccion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/revision-direccion-sgc/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.revision_direccion_sgc_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/revision-direccion-sgc/nueva`
- **Nombre de Función (Backend Handler):** `calidad_sgc.revision_direccion_sgc_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AuditoriaSST, IndicadorCalidadSGC, RetroalimentacionCliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AuditoriaSST.cliente → Cliente, RetroalimentacionCliente.cliente → Cliente, IndicadorCalidadSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=cliente_id, tipo_sistema='calidad', tipo='interna', cliente_id=cliente_id... (+1 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/revision_direccion_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/riesgos-calidad`
- **Nombre de Función (Backend Handler):** `calidad_sgc.riesgos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** MATRIZ DE RIESGOS DE CALIDAD (cláusula 6.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RiesgoCalidad, ProcesoSGC`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RiesgoCalidad.cliente → Cliente, RiesgoCalidad.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, proceso_id=proceso_filtro, tratamiento=tratamiento_filtro... (+3 más)`
  - Plantillas HTML Parseadas: `calidad_sgc/riesgos_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/riesgos-calidad/<int:id>/detalle`
- **Nombre de Función (Backend Handler):** `calidad_sgc.riesgos_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RiesgoCalidad.cliente → Cliente`
  - Plantillas HTML Parseadas: `calidad_sgc/riesgos_detalle.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/riesgos-calidad/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.riesgos_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RiesgoCalidad.cliente → Cliente, RiesgoCalidad.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=riesgo.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/riesgos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/riesgos-calidad/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.riesgos_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RiesgoCalidad.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/riesgos-calidad/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.riesgos_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ProcesoSGC, RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ProcesoSGC`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ProcesoSGC.cliente → Cliente, RiesgoCalidad.cliente → Cliente, RiesgoCalidad.proceso → ProcesoSGC`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/riesgos_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/roles-sgc`
- **Nombre de Función (Backend Handler):** `calidad_sgc.roles_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ROLES Y RESPONSABILIDADES SGC (cláusula 5.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RolSGC`
    - Relaciones Navegadas (JOINS): `RolSGC.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `calidad_sgc/roles_lista.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/roles-sgc/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.roles_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RolSGC.cliente → Cliente, RolSGC.usuario → Usuario, RolSGC.empleado → Empleado`
  - Plantillas HTML Parseadas: `calidad_sgc/roles_form.html`

#### 🔗 Ruta Servidor: `/calidad-sgc/roles-sgc/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `calidad_sgc.roles_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RolSGC.cliente → Cliente`

#### 🔗 Ruta Servidor: `/calidad-sgc/roles-sgc/nuevo`
- **Nombre de Función (Backend Handler):** `calidad_sgc.roles_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Calidad_Sgc**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGC`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RolSGC.cliente → Cliente, RolSGC.usuario → Usuario, RolSGC.empleado → Empleado`
  - Plantillas HTML Parseadas: `calidad_sgc/roles_form.html`

---

### 📦 Blueprint / Módulo: `CAPACITACIONES`

#### 🔗 Ruta Servidor: `/capacitaciones/`
- **Nombre de Función (Backend Handler):** `capacitaciones.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** SESIONES DE CAPACITACIÓN | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ActividadPlan`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, cliente_id=cid_ctx, SesionCapacitacion.cliente_id.in_(cids... (+9 más)`
  - Plantillas HTML Parseadas: `capacitaciones/lista.html`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>`
- **Nombre de Función (Backend Handler):** `capacitaciones.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, SesionCapacitacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=sesion.cliente_id, activo=True
    `
  - Plantillas HTML Parseadas: `capacitaciones/detalle.html`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `capacitaciones.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, SesionCapacitacion, TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TemaCapacitacion, CentroTrabajo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.centro_trabajo → CentroTrabajo, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.formulario_evaluacion → FormularioPersonalizado, CentroTrabajo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=sesion.cliente_id, activo=True
    , cliente_id=sesion.cliente_id, activo=True, 
        FormularioPersonalizado.cliente_id == sesion.cliente_id,
        FormularioPersonalizado.activo == True,
        FormularioPersonalizado.tipo_formulario.in_(['evaluacion', 'encuesta']`
  - Plantillas HTML Parseadas: `capacitaciones/form.html`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `capacitaciones.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/enviar-firmas`
- **Nombre de Función (Backend Handler):** `capacitaciones.enviar_firmas_masivas`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Genera enlaces de firma externa para todos los asistentes de la sesión.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'enviar_firmas_masivas' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.asistentes → AsistenteCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/evaluacion`
- **Nombre de Función (Backend Handler):** `capacitaciones.evaluacion_admin`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── Admin: gestión de evaluación ───────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluacion_admin' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, AccesoEvaluacion.sesion → SesionCapacitacion`
    - Filtros Aplicados (WHERE): `sesion_id=id, 
        FormularioPersonalizado.cliente_id == sesion.cliente_id,
        FormularioPersonalizado.activo == True,
        FormularioPersonalizado.tipo_formulario.in_(['evaluacion', 'encuesta']`
  - Plantillas HTML Parseadas: `capacitaciones/evaluacion_admin.html`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/evaluacion/configurar`
- **Nombre de Función (Backend Handler):** `capacitaciones.evaluacion_configurar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Actualiza la configuración de contenido y formulario de evaluación.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluacion_configurar' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.formulario_evaluacion → FormularioPersonalizado`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/evaluacion/generar`
- **Nombre de Función (Backend Handler):** `capacitaciones.evaluacion_generar_accesos`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluacion_generar_accesos' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.asistentes → AsistenteCapacitacion, AccesoEvaluacion.sesion → SesionCapacitacion, AccesoEvaluacion.asistente → AsistenteCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/evaluacion/nuevo-acceso`
- **Nombre de Función (Backend Handler):** `capacitaciones.evaluacion_nuevo_acceso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Crea un token de acceso individual para persona externa.
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, AccesoEvaluacion.sesion → SesionCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/evaluacion/reset/<int:acceso_id>`
- **Nombre de Función (Backend Handler):** `capacitaciones.evaluacion_reset_acceso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Resetea completamente la evaluación de un participante para que pueda reintentar.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluacion_reset_acceso' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, AccesoEvaluacion.sesion → SesionCapacitacion, AccesoEvaluacion.asistente → AsistenteCapacitacion`
    - Filtros Aplicados (WHERE): `id=acceso_id, sesion_id=id`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `capacitaciones.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta el registro de la sesión de capacitación en Excel [todos los roles].
- **Comentarios Descriptivos:** EXPORTACIÓN EXCEL | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/importar-asistencia`
- **Nombre de Función (Backend Handler):** `capacitaciones.importar_asistencia`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Importa archivo Excel con asistentes y registra masivamente [solo asesores].
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_asistencia' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteCapacitacion, Empleado, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, AsistenteCapacitacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.asistentes → AsistenteCapacitacion, AsistenteCapacitacion.sesion → SesionCapacitacion, AsistenteCapacitacion.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=sesion.cliente_id, 
                        cliente_id=sesion.cliente_id
                    , 
                        sesion_id=sesion.id,
                        empleado_id=empleado.id
                    ... (+2 más)`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/informe-evaluacion`
- **Nombre de Función (Backend Handler):** `capacitaciones.informe_evaluacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'informe_evaluacion' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, AccesoEvaluacion.sesion → SesionCapacitacion`
    - Filtros Aplicados (WHERE): `sesion_id=id`
  - Plantillas HTML Parseadas: `capacitaciones/informe_evaluacion.html`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/plantilla-asistencia`
- **Nombre de Función (Backend Handler):** `capacitaciones.plantilla_asistencia`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta plantilla de asistencia con empleados activos pre-llenados para toma física [todos los roles].
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'plantilla_asistencia' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:id>/plantilla-empleados`
- **Nombre de Función (Backend Handler):** `capacitaciones.plantilla_empleados`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta plantilla con TODOS los empleados activos pre-llenados [solo asesores].
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'plantilla_empleados' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/<int:sesion_id>/asistentes/agregar`
- **Nombre de Función (Backend Handler):** `capacitaciones.agregar_asistente`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_asistente' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteCapacitacion, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AsistenteCapacitacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.asistentes → AsistenteCapacitacion, AsistenteCapacitacion.sesion → SesionCapacitacion, AsistenteCapacitacion.empleado → Empleado`
    - Filtros Aplicados (WHERE): `
            sesion_id=sesion_id, empleado_id=empleado_id`

#### 🔗 Ruta Servidor: `/capacitaciones/api/temas/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `capacitaciones.api_temas_cliente`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** API: temas por cliente (para AJAX en formulario) | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TemaCapacitacion`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, activo=True
    `
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/capacitaciones/asistentes/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `capacitaciones.eliminar_asistente`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AsistenteCapacitacion.sesion → SesionCapacitacion`

#### 🔗 Ruta Servidor: `/capacitaciones/eval/<token>`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_portada`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Página del participante: muestra contenido y formulario de evaluación en una sola pantalla.
- **Comentarios Descriptivos:** ── Portal participante (sin login requerido) ──────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_portada' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AccesoEvaluacion.sesion → SesionCapacitacion, AccesoEvaluacion.asistente → AsistenteCapacitacion`
    - Filtros Aplicados (WHERE): `token=token, 
                formulario_id=sesion.formulario_evaluacion_id, activa=True
            , 
            formulario_id=formulario.id, activa=True
        `
  - Plantillas HTML Parseadas: `capacitaciones/eval_portada.html`

#### 🔗 Ruta Servidor: `/capacitaciones/eval/<token>/completado`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_completado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Pantalla de finalización: muestra resultado de la evaluación.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_completado' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Relaciones Navegadas (JOINS): `AccesoEvaluacion.sesion → SesionCapacitacion`
    - Filtros Aplicados (WHERE): `token=token, 
            formulario_id=sesion.formulario_evaluacion_id, activa=True
        `
  - Plantillas HTML Parseadas: `capacitaciones/eval_completado.html`

#### 🔗 Ruta Servidor: `/capacitaciones/eval/<token>/marcar-contenido`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_marcar_contenido`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Marca el contenido como visto y redirige al quiz.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_marcar_contenido' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AccesoEvaluacion.sesion → SesionCapacitacion`
    - Filtros Aplicados (WHERE): `token=token`

#### 🔗 Ruta Servidor: `/capacitaciones/eval/<token>/quiz`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_quiz`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Redirige a la página unificada de evaluación (el quiz está integrado en eval_portada).
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_quiz' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/capacitaciones/eval/<token>/reintentar`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_reintentar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Permite al participante volver a intentar la evaluación si no aprobó y tiene intentos disponibles.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_reintentar' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccesoEvaluacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AccesoEvaluacion.sesion → SesionCapacitacion`
    - Filtros Aplicados (WHERE): `token=token`

#### 🔗 Ruta Servidor: `/capacitaciones/eval/u/<token_univ>`
- **Nombre de Función (Backend Handler):** `capacitaciones.eval_acceso_universal`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Enlace universal de evaluación (token opaco): el participante ingresa su documento y
    el sistema busca o crea su acceso automáticamente.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'eval_acceso_universal' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccesoEvaluacion, Empleado, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `SesionCapacitacion, AccesoEvaluacion, Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.asistentes → AsistenteCapacitacion, AccesoEvaluacion.sesion → SesionCapacitacion, AccesoEvaluacion.asistente → AsistenteCapacitacion`
    - Filtros Aplicados (WHERE): `token_universal=token_univ, 
                sesion_id=sesion.id, cedula=cedula
            , 
                    cliente_id=sesion.cliente_id,
                    documento=cedula,
                    activo=True
                `
  - Plantillas HTML Parseadas: `capacitaciones/eval_acceso_universal.html`

#### 🔗 Ruta Servidor: `/capacitaciones/nueva`
- **Nombre de Función (Backend Handler):** `capacitaciones.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan, CentroTrabajo, SesionCapacitacion, TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ActividadPlan, TemaCapacitacion, CentroTrabajo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.centro_trabajo → CentroTrabajo, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.formulario_evaluacion → FormularioPersonalizado, CentroTrabajo.cliente → Cliente, ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, activo=True, cliente_id=cid_ctx, activo=True, 
            FormularioPersonalizado.cliente_id == cid_ctx,
            FormularioPersonalizado.activo == True,
            FormularioPersonalizado.tipo_formulario.in_(['evaluacion', 'encuesta']`
  - Plantillas HTML Parseadas: `capacitaciones/form.html`

#### 🔗 Ruta Servidor: `/capacitaciones/temas`
- **Nombre de Función (Backend Handler):** `capacitaciones.temas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** TEMAS DE CAPACITACIÓN | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'temas' - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TemaCapacitacion`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, 
            TemaCapacitacion.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `capacitaciones/temas.html`

#### 🔗 Ruta Servidor: `/capacitaciones/temas/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `capacitaciones.editar_tema`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `capacitaciones/tema_form.html`

#### 🔗 Ruta Servidor: `/capacitaciones/temas/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `capacitaciones.toggle_tema`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/capacitaciones/temas/nuevo`
- **Nombre de Función (Backend Handler):** `capacitaciones.nuevo_tema`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Capacitaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `capacitaciones/tema_form.html`

---

### 📦 Blueprint / Módulo: `COMITES`

#### 🔗 Ruta Servidor: `/comites/`
- **Nombre de Función (Backend Handler):** `comites.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── INDEX ──────────────────────────────────────────────────────────────────── | Para asesores/superadmin: mostrar selector si no hay cliente seleccionado
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteSST`
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente, ComiteSST.miembros → MiembroComite`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `comites/index.html, comites/selector_empresa.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>`
- **Nombre de Función (Backend Handler):** `comites.detalle_comite`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadCopasst, ComiteSST, CompromisoComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CompromisoComite, ActividadCopasst`
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente, ComiteSST.miembros → MiembroComite, ComiteSST.reuniones → ReunionComite, CompromisoComite.reunion → ReunionComite, ActividadCopasst.comite → ComiteSST`
    - Filtros Aplicados (WHERE): `comite_id=comite_id, 
            CompromisoComite.reunion_id.in_(reuniones_ids`
  - Plantillas HTML Parseadas: `comites/detalle.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/actividades`
- **Nombre de Función (Backend Handler):** `comites.actividades`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── ACTIVIDADES DEL COPASST ─────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actividades' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `comites/actividades.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/actividades/nueva`
- **Nombre de Función (Backend Handler):** `comites.nueva_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, ActividadCopasst, ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, ComiteSST.cliente → Cliente, ActividadCopasst.comite → ComiteSST, ActividadCopasst.accion_mejora → AccionMejora`
    - Filtros Aplicados (WHERE): `cliente_id=comite.cliente_id`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/editar`
- **Nombre de Función (Backend Handler):** `comites.editar_comite`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `comites/comite_form.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `comites.exportar_pdf_informe`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** PDF con informe completo del comité (miembros, reuniones, compromisos, actividades).
- **Comentarios Descriptivos:** ─── EXPORTACIÓN PDF ACTA ─────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ComiteSST.miembros → MiembroComite, ComiteSST.reuniones → ReunionComite`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/miembros`
- **Nombre de Función (Backend Handler):** `comites.miembros`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── MIEMBROS ────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'miembros' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente, ComiteSST.miembros → MiembroComite`
  - Plantillas HTML Parseadas: `comites/miembros.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/miembros/agregar`
- **Nombre de Función (Backend Handler):** `comites.agregar_miembro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_miembro' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST, Empleado, MiembroComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteSST.miembros → MiembroComite, MiembroComite.comite → ComiteSST, MiembroComite.empleado → Empleado`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/reuniones`
- **Nombre de Función (Backend Handler):** `comites.reuniones`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── REUNIONES ────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reuniones' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ComiteSST.reuniones → ReunionComite`
  - Plantillas HTML Parseadas: `comites/reuniones_lista.html`

#### 🔗 Ruta Servidor: `/comites/<int:comite_id>/reuniones/nueva`
- **Nombre de Función (Backend Handler):** `comites.nueva_reunion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST, ReunionComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente, ReunionComite.comite → ComiteSST, ReunionComite.asistentes → AsistenteReunion`
  - Plantillas HTML Parseadas: `comites/reunion_form.html`

#### 🔗 Ruta Servidor: `/comites/actividad/<int:actividad_id>/eliminar`
- **Nombre de Función (Backend Handler):** `comites.eliminar_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadCopasst`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ActividadCopasst.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/api/compromiso/<int:compromiso_id>/estado`
- **Nombre de Función (Backend Handler):** `comites.api_actualizar_estado_compromiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─── API para actualización parcial de compromisos (AJAX) ────────────────────
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CompromisoComite.reunion → ReunionComite`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/comites/caso/<int:caso_id>/actualizar`
- **Nombre de Función (Backend Handler):** `comites.actualizar_caso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_caso' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoConvivencia.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/caso/<int:caso_id>/eliminar`
- **Nombre de Función (Backend Handler):** `comites.eliminar_caso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `CasoConvivencia.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/compromiso/<int:compromiso_id>/actualizar-estado`
- **Nombre de Función (Backend Handler):** `comites.actualizar_estado_compromiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_estado_compromiso' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CompromisoComite.reunion → ReunionComite`

#### 🔗 Ruta Servidor: `/comites/compromiso/<int:compromiso_id>/eliminar`
- **Nombre de Función (Backend Handler):** `comites.eliminar_compromiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `CompromisoComite.reunion → ReunionComite`

#### 🔗 Ruta Servidor: `/comites/convivencia`
- **Nombre de Función (Backend Handler):** `comites.convivencia_index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── COMITÉ DE CONVIVENCIA LABORAL ────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'convivencia_index' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoConvivencia, ComiteSST, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ComiteSST, Empleado, CasoConvivencia`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ComiteSST.cliente → Cliente, CasoConvivencia.comite → ComiteSST`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo='convivencia', activo=True
    , cliente_id=cliente_id, activo=True, 
                comite_id=comite.id
            `
  - Plantillas HTML Parseadas: `comites/convivencia.html`

#### 🔗 Ruta Servidor: `/comites/convivencia/<int:comite_id>/caso/nuevo`
- **Nombre de Función (Backend Handler):** `comites.nuevo_caso_convivencia`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoConvivencia, ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CasoConvivencia`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CasoConvivencia.comite → ComiteSST`
    - Filtros Aplicados (WHERE): `comite_id=comite_id`

#### 🔗 Ruta Servidor: `/comites/convivencia/guardar`
- **Nombre de Función (Backend Handler):** `comites.convivencia_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'convivencia_guardar' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/comites/miembro/<int:miembro_id>/editar`
- **Nombre de Función (Backend Handler):** `comites.editar_miembro`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MiembroComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `MiembroComite.comite → ComiteSST, MiembroComite.empleado → Empleado`
  - Plantillas HTML Parseadas: `comites/miembro_form.html`

#### 🔗 Ruta Servidor: `/comites/miembro/<int:miembro_id>/eliminar`
- **Nombre de Función (Backend Handler):** `comites.eliminar_miembro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MiembroComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `MiembroComite.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/nuevo`
- **Nombre de Función (Backend Handler):** `comites.nuevo_comite`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─── COMITÉ: CREAR / EDITAR ───────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteSST.cliente → Cliente, ComiteSST.miembros → MiembroComite`
  - Plantillas HTML Parseadas: `comites/comite_form.html`

#### 🔗 Ruta Servidor: `/comites/reunion/<int:reunion_id>`
- **Nombre de Función (Backend Handler):** `comites.detalle_reunion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionComite`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReunionComite.comite → ComiteSST`
  - Plantillas HTML Parseadas: `comites/reunion_detalle.html`

#### 🔗 Ruta Servidor: `/comites/reunion/<int:reunion_id>/compromisos/agregar`
- **Nombre de Función (Backend Handler):** `comites.agregar_compromiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─── COMPROMISOS DE REUNIÓN ───────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_compromiso' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoComite, ReunionComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReunionComite.comite → ComiteSST, CompromisoComite.reunion → ReunionComite`

#### 🔗 Ruta Servidor: `/comites/reunion/<int:reunion_id>/editar`
- **Nombre de Función (Backend Handler):** `comites.editar_reunion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteReunion, ReunionComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AsistenteReunion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ReunionComite.comite → ComiteSST, ReunionComite.asistentes → AsistenteReunion, AsistenteReunion.reunion → ReunionComite`
    - Filtros Aplicados (WHERE): `reunion_id=reunion_id`
  - Plantillas HTML Parseadas: `comites/reunion_form.html`

#### 🔗 Ruta Servidor: `/comites/reunion/<int:reunion_id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `comites.exportar_pdf_acta`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionComite`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReunionComite.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/reunion/<int:reunion_id>/exportar-word`
- **Nombre de Función (Backend Handler):** `comites.exportar_word_acta`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionComite`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReunionComite.comite → ComiteSST`

#### 🔗 Ruta Servidor: `/comites/vigia`
- **Nombre de Función (Backend Handler):** `comites.vigia_index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── VIGÍA SST ────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'vigia_index' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ComiteSST, Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ComiteSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo='vigia', activo=True
    , cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `comites/vigia.html`

#### 🔗 Ruta Servidor: `/comites/vigia/guardar`
- **Nombre de Función (Backend Handler):** `comites.vigia_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'vigia_guardar' - asociado al sistema maestro de **Comites**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteSST, Empleado, MiembroComite`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, MiembroComite`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ComiteSST.cliente → Cliente, MiembroComite.comite → ComiteSST, MiembroComite.empleado → Empleado`
    - Filtros Aplicados (WHERE): `comite_id=comite.id`

---

### 📦 Blueprint / Módulo: `CONFIGURACION`

#### 🔗 Ruta Servidor: `/configuracion/`
- **Nombre de Función (Backend Handler):** `configuracion.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Pantalla principal de configuración general de la app. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
  - Plantillas HTML Parseadas: `configuracion/index.html`

#### 🔗 Ruta Servidor: `/configuracion/empresa`
- **Nombre de Función (Backend Handler):** `configuracion.empresa`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Muestra el formulario de perfil SST de la empresa. [cliente | asesor | superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente`
  - Plantillas HTML Parseadas: `configuracion/empresa.html`

#### 🔗 Ruta Servidor: `/configuracion/empresa/guardar`
- **Nombre de Función (Backend Handler):** `configuracion.empresa_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda los datos básicos + perfil SST de la empresa. [cliente | asesor | superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa_guardar' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Cliente.perfil_sst → PerfilEmpresa, PerfilEmpresa.cliente → Cliente`

#### 🔗 Ruta Servidor: `/configuracion/empresa/logo`
- **Nombre de Función (Backend Handler):** `configuracion.empresa_logo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Sube el logo de la empresa cliente. [cliente | asesor | superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa_logo' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/guardar`
- **Nombre de Función (Backend Handler):** `configuracion.guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda los campos de texto/color del formulario de configuración. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'guardar' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave=clave`

#### 🔗 Ruta Servidor: `/configuracion/guardar-smtp`
- **Nombre de Función (Backend Handler):** `configuracion.guardar_smtp`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda los campos de configuración SMTP. La contraseña solo se
    actualiza si el usuario ingresó un nuevo valor. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'guardar_smtp' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave=clave`

#### 🔗 Ruta Servidor: `/configuracion/listas`
- **Nombre de Función (Backend Handler):** `configuracion.listas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Página principal de gestión de listas configurables.
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemListaConfigurable`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `lista=clave`
  - Plantillas HTML Parseadas: `configuracion/listas.html`

#### 🔗 Ruta Servidor: `/configuracion/listas/<lista>/<int:item_id>/editar`
- **Nombre de Función (Backend Handler):** `configuracion.lista_editar_item`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Edita la etiqueta y orden de un ítem. Solo asesor/superadmin.
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemListaConfigurable`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/listas/<lista>/<int:item_id>/eliminar`
- **Nombre de Función (Backend Handler):** `configuracion.lista_eliminar_item`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina un ítem permanentemente. Solo superadmin.
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemListaConfigurable`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/configuracion/listas/<lista>/<int:item_id>/toggle`
- **Nombre de Función (Backend Handler):** `configuracion.lista_toggle_item`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa o desactiva un ítem. Solo asesor/superadmin.
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemListaConfigurable`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/listas/<lista>/nuevo`
- **Nombre de Función (Backend Handler):** `configuracion.lista_nuevo_item`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Crea un nuevo ítem en la lista indicada. Solo asesor/superadmin.
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemListaConfigurable`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ItemListaConfigurable`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `lista=lista, valor=valor`

#### 🔗 Ruta Servidor: `/configuracion/login-page`
- **Nombre de Función (Backend Handler):** `configuracion.login_page`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Gestión del contenido de la pantalla de login. [superadmin]
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo, LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `LoginPageModulo, LoginPageStat`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.modulos → LoginPageModulo, LoginPageConfig.stats → LoginPageStat`
    - Filtros Aplicados (WHERE): `config_id=cfg.id, config_id=cfg.id`
  - Plantillas HTML Parseadas: `configuracion/login_page.html`

#### 🔗 Ruta Servidor: `/configuracion/login-page/guardar-tema`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_guardar_tema`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda el tema visual de la pantalla de login. [superadmin]
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave='login_tema'`

#### 🔗 Ruta Servidor: `/configuracion/login-page/guardar-texto`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_guardar_texto`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda hero_titulo, hero_intro y tamaños de logo. [superadmin]
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave=clave`

#### 🔗 Ruta Servidor: `/configuracion/login-page/modulo/<int:mid>/editar`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_modulo_editar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Edita una tarjeta de módulo. [superadmin]
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/login-page/modulo/<int:mid>/eliminar`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_modulo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una tarjeta de módulo. [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/configuracion/login-page/modulo/<int:mid>/toggle`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_modulo_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa o desactiva una tarjeta de módulo. [superadmin]
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/login-page/modulo/nuevo`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_modulo_nuevo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Agrega una tarjeta de módulo al login. [superadmin]
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageModulo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.modulos → LoginPageModulo`
    - Filtros Aplicados (WHERE): `config_id=cfg.id`

#### 🔗 Ruta Servidor: `/configuracion/login-page/stat/<int:sid>/editar`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_stat_editar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Edita una estadística del login. [superadmin]
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/login-page/stat/<int:sid>/eliminar`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_stat_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una estadística. [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/configuracion/login-page/stat/<int:sid>/toggle`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_stat_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa o desactiva una estadística. [superadmin]
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/configuracion/login-page/stat/nuevo`
- **Nombre de Función (Backend Handler):** `configuracion.login_page_stat_nuevo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Agrega una estadística al login. [superadmin]
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `LoginPageStat`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `LoginPageConfig.stats → LoginPageStat`
    - Filtros Aplicados (WHERE): `config_id=cfg.id`

#### 🔗 Ruta Servidor: `/configuracion/logo`
- **Nombre de Función (Backend Handler):** `configuracion.subir_logo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Sube el logo de la empresa principal. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'subir_logo' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave='logo_empresa'`

#### 🔗 Ruta Servidor: `/configuracion/logo/eliminar`
- **Nombre de Función (Backend Handler):** `configuracion.eliminar_logo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina el logo de la empresa principal y restaura el texto. [superadmin]
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `clave='logo_empresa'`

#### 🔗 Ruta Servidor: `/configuracion/resetear`
- **Nombre de Función (Backend Handler):** `configuracion.resetear`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Restaura todos los valores a sus defaults del sistema. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'resetear' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `clave=clave`

#### 🔗 Ruta Servidor: `/configuracion/test-email`
- **Nombre de Función (Backend Handler):** `configuracion.test_email`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Envía un correo de prueba al email del superadmin usando la config
    SMTP guardada en BD. Retorna JSON {ok, mensaje}. [superadmin]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'test_email' - asociado al sistema maestro de **Configuracion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`

---

### 📦 Blueprint / Módulo: `CONVIVENCIA`

#### 🔗 Ruta Servidor: `/convivencia/`
- **Nombre de Función (Backend Handler):** `convivencia.dashboard`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'dashboard' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
  - Plantillas HTML Parseadas: `convivencia/dashboard.html, convivencia/selector_empresa.html`

#### 🔗 Ruta Servidor: `/convivencia/casos`
- **Nombre de Función (Backend Handler):** `convivencia.casos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'casos' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral`
    - Relaciones Navegadas (JOINS): `ComiteConvivenciaLaboral.cliente → Cliente, ComiteConvivenciaLaboral.expedientes → ExpedienteConvivencia`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `
  - Plantillas HTML Parseadas: `convivencia/casos.html`

#### 🔗 Ruta Servidor: `/convivencia/casos/<int:caso_id>`
- **Nombre de Función (Backend Handler):** `convivencia.detalle_caso`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExpedienteConvivencia`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ExpedienteConvivencia.cliente → Cliente, ExpedienteConvivencia.actuaciones → ActuacionConvivencia, ExpedienteConvivencia.compromisos → CompromisoConvivenciaLaboral`
  - Plantillas HTML Parseadas: `convivencia/caso_detalle.html`

#### 🔗 Ruta Servidor: `/convivencia/casos/<int:caso_id>/actuacion`
- **Nombre de Función (Backend Handler):** `convivencia.agregar_actuacion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_actuacion' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActuacionConvivencia, ExpedienteConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ExpedienteConvivencia.cliente → Cliente, ActuacionConvivencia.expediente → ExpedienteConvivencia`

#### 🔗 Ruta Servidor: `/convivencia/casos/<int:caso_id>/estado`
- **Nombre de Función (Backend Handler):** `convivencia.cambiar_estado_caso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado_caso' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExpedienteConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ExpedienteConvivencia.cliente → Cliente`

#### 🔗 Ruta Servidor: `/convivencia/casos/nuevo`
- **Nombre de Función (Backend Handler):** `convivencia.nuevo_caso`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral, ExpedienteConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteConvivenciaLaboral.cliente → Cliente, ExpedienteConvivencia.cliente → Cliente, ExpedienteConvivencia.comite → ComiteConvivenciaLaboral`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `
  - Plantillas HTML Parseadas: `convivencia/caso_form.html`

#### 🔗 Ruta Servidor: `/convivencia/compromisos/<int:comp_id>/estado`
- **Nombre de Función (Backend Handler):** `convivencia.actualizar_compromiso`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** Verificar acceso
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_compromiso' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CompromisoConvivenciaLaboral.reunion → ReunionConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/configurar`
- **Nombre de Función (Backend Handler):** `convivencia.configurar_comite`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'configurar_comite' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteConvivenciaLaboral.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `
  - Plantillas HTML Parseadas: `convivencia/comite_form.html`

#### 🔗 Ruta Servidor: `/convivencia/integrantes`
- **Nombre de Función (Backend Handler):** `convivencia.integrantes`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** INTEGRANTES | ═══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'integrantes' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral, Empleado`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Empleado.cliente → Cliente, ComiteConvivenciaLaboral.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    , 
        cliente_id=cliente_id, activo=True
    `
  - Plantillas HTML Parseadas: `convivencia/integrantes.html`

#### 🔗 Ruta Servidor: `/convivencia/integrantes/<int:miembro_id>/eliminar`
- **Nombre de Función (Backend Handler):** `convivencia.eliminar_integrante`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MiembroConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `MiembroConvivenciaLaboral.comite → ComiteConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/integrantes/<int:miembro_id>/toggle`
- **Nombre de Función (Backend Handler):** `convivencia.toggle_integrante`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MiembroConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `MiembroConvivenciaLaboral.comite → ComiteConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/integrantes/agregar`
- **Nombre de Función (Backend Handler):** `convivencia.agregar_integrante`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_integrante' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComiteConvivenciaLaboral, Empleado, MiembroConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, ComiteConvivenciaLaboral`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ComiteConvivenciaLaboral.cliente → Cliente, MiembroConvivenciaLaboral.comite → ComiteConvivenciaLaboral, MiembroConvivenciaLaboral.empleado → Empleado`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `

#### 🔗 Ruta Servidor: `/convivencia/reuniones`
- **Nombre de Función (Backend Handler):** `convivencia.reuniones`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reuniones' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral`
    - Relaciones Navegadas (JOINS): `ComiteConvivenciaLaboral.cliente → Cliente, ComiteConvivenciaLaboral.reuniones → ReunionConvivenciaLaboral`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `
  - Plantillas HTML Parseadas: `convivencia/reuniones.html`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>`
- **Nombre de Función (Backend Handler):** `convivencia.detalle_reunion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral, ReunionConvivenciaLaboral.asistentes → AsistenteReunionConvivencia, ReunionConvivenciaLaboral.compromisos → CompromisoConvivenciaLaboral`
  - Plantillas HTML Parseadas: `convivencia/reunion_detalle.html`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>/asistente`
- **Nombre de Función (Backend Handler):** `convivencia.agregar_asistente`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_asistente' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteReunionConvivencia, ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AsistenteReunion.reunion → ReunionComite, ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral, AsistenteReunionConvivencia.reunion → ReunionConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>/cerrar`
- **Nombre de Función (Backend Handler):** `convivencia.cerrar_reunion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cerrar_reunion' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>/compromiso`
- **Nombre de Función (Backend Handler):** `convivencia.agregar_compromiso_reunion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_compromiso_reunion' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CompromisoConvivenciaLaboral, ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral, CompromisoConvivenciaLaboral.reunion → ReunionConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>/editar`
- **Nombre de Función (Backend Handler):** `convivencia.editar_reunion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral`
  - Plantillas HTML Parseadas: `convivencia/reunion_form.html`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/<int:reunion_id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `convivencia.exportar_pdf_reunion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/asistentes/<int:asistente_id>/eliminar`
- **Nombre de Función (Backend Handler):** `convivencia.eliminar_asistente`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AsistenteReunionConvivencia`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AsistenteReunion.reunion → ReunionComite, AsistenteReunionConvivencia.reunion → ReunionConvivenciaLaboral`

#### 🔗 Ruta Servidor: `/convivencia/reuniones/nueva`
- **Nombre de Función (Backend Handler):** `convivencia.nueva_reunion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Convivencia**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ComiteConvivenciaLaboral, ReunionConvivenciaLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ComiteConvivenciaLaboral`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComiteConvivenciaLaboral.cliente → Cliente, ReunionConvivenciaLaboral.comite → ComiteConvivenciaLaboral`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id
    `
  - Plantillas HTML Parseadas: `convivencia/reunion_form.html`

---

### 📦 Blueprint / Módulo: `CUMPLIMIENTO`

#### 🔗 Ruta Servidor: `/cumplimiento/`
- **Nombre de Función (Backend Handler):** `cumplimiento.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista principal de la matriz de cumplimiento.
    Asesores ven selector de empresa; clientes ven solo la suya.
    [asesor, cliente]
- **Comentarios Descriptivos:** MATRIZ DE CUMPLIMIENTO POR EMPRESA | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, NormaSST, PerfilEmpresa, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, NormaSST, RequisitoEmpresa, PerfilEmpresa`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, NormaSST.requisitos → RequisitoEmpresa, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `activo=True, activo=True, tipo=tipo_f... (+3 más)`
  - Plantillas HTML Parseadas: `cumplimiento/lista.html, cumplimiento/seleccionar_empresa.html`

#### 🔗 Ruta Servidor: `/cumplimiento/api/stats`
- **Nombre de Función (Backend Handler):** `cumplimiento.api_stats`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Retorna estadísticas de cumplimiento para una empresa en JSON.
- **Comentarios Descriptivos:** API JSON (para estadísticas en dashboard) | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/cumplimiento/evaluar/<int:norma_id>`
- **Nombre de Función (Backend Handler):** `cumplimiento.evaluar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Evalúa el cumplimiento de una norma para una empresa.
    Crea o actualiza el RequisitoEmpresa correspondiente.
    [asesor]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluar' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Cliente, NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RequisitoEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST, RequisitoEmpresa.accion_mejora → AccionMejora`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, norma_id=norma_id
    `
  - Plantillas HTML Parseadas: `cumplimiento/evaluar.html`

#### 🔗 Ruta Servidor: `/cumplimiento/exportar-excel`
- **Nombre de Función (Backend Handler):** `cumplimiento.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta la matriz de requisitos legales completa de una empresa en Excel — [todos].
- **Comentarios Descriptivos:** EXPORTACIÓN EXCEL | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/cumplimiento/importar-matriz/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `cumplimiento.importar_matriz`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Importa requisitos legales desde una plantilla Excel (.xlsx).
    Crea normas nuevas si no existen en el catálogo, y crea/actualiza RequisitoEmpresa.
    [asesor]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_matriz' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST, RequisitoEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `NormaSST.requisitos → RequisitoEmpresa, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `codigo=codigo, 
                cliente_id=cliente_id, norma_id=norma.id
            `

#### 🔗 Ruta Servidor: `/cumplimiento/inicializar/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `cumplimiento.inicializar_matriz`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Crea registros RequisitoEmpresa para todas las normas activas que aún
    no tienen evaluación en esta empresa (estado = pendiente).
    Acepta filtro opcional por sector_economico.
    [asesor]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'inicializar_matriz' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST, RequisitoEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `activo=True, cliente_id=cliente_id`

#### 🔗 Ruta Servidor: `/cumplimiento/masivo`
- **Nombre de Función (Backend Handler):** `cumplimiento.evaluar_masivo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda evaluaciones en bloque desde la vista de lista.
    Recibe JSON: [{norma_id, estado, aplica, responsable}]
    [asesor]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluar_masivo' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST, RequisitoEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `activo=True, 
            cliente_id=cliente_id, norma_id=norma.id
        `

#### 🔗 Ruta Servidor: `/cumplimiento/normas`
- **Nombre de Función (Backend Handler):** `cumplimiento.normas_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista el catálogo global de normas SST — [superadmin].
- **Comentarios Descriptivos:** CATÁLOGO DE NORMAS (Superadmin) | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `tipo=tipo_f`
  - Plantillas HTML Parseadas: `cumplimiento/normas_lista.html`

#### 🔗 Ruta Servidor: `/cumplimiento/normas/<int:norma_id>/editar`
- **Nombre de Función (Backend Handler):** `cumplimiento.norma_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Edita una norma del catálogo — [superadmin].
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `NormaSST.codigo == codigo,
                                       NormaSST.id != norma_id`
  - Plantillas HTML Parseadas: `cumplimiento/norma_form.html`

#### 🔗 Ruta Servidor: `/cumplimiento/normas/<int:norma_id>/eliminar`
- **Nombre de Función (Backend Handler):** `cumplimiento.norma_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una norma si no tiene requisitos asociados — [superadmin].
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `NormaSST.requisitos → RequisitoEmpresa`

#### 🔗 Ruta Servidor: `/cumplimiento/normas/<int:norma_id>/toggle`
- **Nombre de Función (Backend Handler):** `cumplimiento.norma_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa/desactiva una norma del catálogo — [superadmin].
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/cumplimiento/normas/nueva`
- **Nombre de Función (Backend Handler):** `cumplimiento.norma_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crea una nueva norma SST en el catálogo — [superadmin].
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Filtros Aplicados (WHERE): `codigo=codigo`
  - Plantillas HTML Parseadas: `cumplimiento/norma_form.html`

#### 🔗 Ruta Servidor: `/cumplimiento/plantilla-importacion`
- **Nombre de Función (Backend Handler):** `cumplimiento.plantilla_importacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga una plantilla Excel vacía para importar requisitos legales — [asesor].
- **Comentarios Descriptivos:** PLANTILLA E IMPORTACIÓN MASIVA | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'plantilla_importacion' - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/cumplimiento/requisito/<int:req_id>`
- **Nombre de Función (Backend Handler):** `cumplimiento.requisito_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Detalle de la evaluación de un requisito — [asesor, cliente].
- **Comentarios Descriptivos:** DETALLE DE UN REQUISITO (vista lectura) | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Cumplimiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RequisitoEmpresa.cliente → Cliente`
  - Plantillas HTML Parseadas: `cumplimiento/requisito_detalle.html`

---

### 📦 Blueprint / Módulo: `DASHBOARD`

#### 🔗 Ruta Servidor: `/`
- **Nombre de Función (Backend Handler):** `dashboard.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/comparativo`
- **Nombre de Función (Backend Handler):** `dashboard.comparativo`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Tabla comparativa de todas las empresas. Solo asesor / superadmin.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'comparativo' - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Filtros Aplicados (WHERE): `id=filtro_cliente, activo=True, activo=True`
  - Plantillas HTML Parseadas: `dashboard/comparativo.html`

#### 🔗 Ruta Servidor: `/empresa/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `dashboard.empresa`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Dashboard detallado de una empresa específica. Solo asesor / superadmin.
- **Comentarios Descriptivos:** RUTAS NUEVAS: EMPRESA, COMPARATIVO, PDF | ═══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa' - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `dashboard/empresa.html`

#### 🔗 Ruta Servidor: `/empresa/<int:cliente_id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `dashboard.exportar_pdf_empresa`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta el dashboard de una empresa específica como PDF ejecutivo.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/exportar-pdf`
- **Nombre de Función (Backend Handler):** `dashboard.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta el dashboard del asesor/empresa como PDF ejecutivo.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Filtros Aplicados (WHERE): `activo=True`

#### 🔗 Ruta Servidor: `/supervision/equipo`
- **Nombre de Función (Backend Handler):** `dashboard.equipo_coordinador`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'equipo_coordinador' - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `dashboard/equipo_coordinador.html`

#### 🔗 Ruta Servidor: `/supervision/exportar-pdf`
- **Nombre de Función (Backend Handler):** `dashboard.exportar_pdf_coordinador`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Dashboard**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

---

### 📦 Blueprint / Módulo: `DIAGNOSTICO`

#### 🔗 Ruta Servidor: `/diagnostico/`
- **Nombre de Función (Backend Handler):** `diagnostico.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.cliente → Cliente, AutoevaluacionSST.items → ItemAutoevaluacion`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, anio=int(anio_f, AutoevaluacionSST.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `diagnostico/lista.html`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>`
- **Nombre de Función (Backend Handler):** `diagnostico.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.cliente → Cliente, AutoevaluacionSST.items → ItemAutoevaluacion`
  - Plantillas HTML Parseadas: `diagnostico/detalle.html`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `diagnostico.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `diagnostico/form.html`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `diagnostico.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/evaluar`
- **Nombre de Función (Backend Handler):** `diagnostico.evaluar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'evaluar' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.items → ItemAutoevaluacion`
  - Plantillas HTML Parseadas: `diagnostico/evaluar.html`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `diagnostico.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR EXCEL | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `diagnostico.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR PDF | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutoevaluacionSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/finalizar`
- **Nombre de Función (Backend Handler):** `diagnostico.finalizar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'finalizar' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/generar-acciones`
- **Nombre de Función (Backend Handler):** `diagnostico.generar_acciones`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_acciones' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AccionMejora.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, AutoevaluacionSST.items → ItemAutoevaluacion`

#### 🔗 Ruta Servidor: `/diagnostico/<int:id>/reabrir`
- **Nombre de Función (Backend Handler):** `diagnostico.reabrir`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reabrir' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/diagnostico/documentos`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** LISTADO — Checklists de documentos | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ChecklistDocumentosSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=current_user.cliente_id, anio=int(anio_f`
  - Plantillas HTML Parseadas: `diagnostico/docs_lista.html`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ChecklistDocumentosSST.cliente → Cliente, ChecklistDocumentosSST.items → ItemChecklistDocumento`
  - Plantillas HTML Parseadas: `diagnostico/docs_detalle.html`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/evaluar`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_evaluar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'docs_evaluar' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente, ChecklistDocumentosSST.cliente → Cliente, ChecklistDocumentosSST.items → ItemChecklistDocumento`
    - Filtros Aplicados (WHERE): `cliente_id=ch.cliente_id, estado='vigente'`
  - Plantillas HTML Parseadas: `diagnostico/docs_evaluar.html`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR EXCEL (tabla tipo Res. 0312 con estilo oficial) | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ChecklistDocumentosSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR PDF | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ChecklistDocumentosSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/finalizar`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_finalizar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'docs_finalizar' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/<int:id>/reabrir`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_reabrir`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'docs_reabrir' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/diagnostico/documentos/nueva`
- **Nombre de Función (Backend Handler):** `diagnostico.docs_nueva`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ChecklistDocumentosSST, Empleado, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ChecklistDocumentosSST, PerfilEmpresa, Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PerfilEmpresa.cliente → Cliente, ChecklistDocumentosSST.cliente → Cliente, ChecklistDocumentosSST.items → ItemChecklistDocumento`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, anio=anio, cliente_id=cliente_id, cliente_id=cliente_id, activo=True`

#### 🔗 Ruta Servidor: `/diagnostico/items/<int:item_id>/plantilla`
- **Nombre de Función (Backend Handler):** `diagnostico.descargar_plantilla`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga plantilla Word pre-rellenada con datos de la empresa.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_plantilla' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ItemAutoevaluacion, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, ItemAutoevaluacion.autoevaluacion → AutoevaluacionSST`
    - Filtros Aplicados (WHERE): `cliente_id=cliente.id`

#### 🔗 Ruta Servidor: `/diagnostico/items/<int:item_id>/subir-evidencia`
- **Nombre de Función (Backend Handler):** `diagnostico.subir_evidencia`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Recibe un archivo de evidencia para el ítem y lo almacena con control de versiones.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'subir_evidencia' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvidenciaItemDiagnostico, ItemAutoevaluacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ItemAutoevaluacion.autoevaluacion → AutoevaluacionSST, EvidenciaItemDiagnostico.item → ItemAutoevaluacion, EvidenciaItemDiagnostico.autoevaluacion → AutoevaluacionSST, EvidenciaItemDiagnostico.cliente → Cliente, EvidenciaItemDiagnostico.subido_por → Usuario`
    - Filtros Aplicados (WHERE): `item_id=item_id`

#### 🔗 Ruta Servidor: `/diagnostico/items/evidencia/<int:ev_id>/descargar`
- **Nombre de Función (Backend Handler):** `diagnostico.descargar_evidencia`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga un archivo de evidencia ya subido.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_evidencia' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvidenciaItemDiagnostico`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `EvidenciaItemDiagnostico.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/items/evidencia/<int:ev_id>/eliminar`
- **Nombre de Función (Backend Handler):** `diagnostico.eliminar_evidencia`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina un archivo de evidencia. Solo asesores.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvidenciaItemDiagnostico`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `EvidenciaItemDiagnostico.item → ItemAutoevaluacion, EvidenciaItemDiagnostico.autoevaluacion → AutoevaluacionSST`

#### 🔗 Ruta Servidor: `/diagnostico/items/evidencia/<int:ev_id>/ver`
- **Nombre de Función (Backend Handler):** `diagnostico.ver_evidencia`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Muestra un archivo de evidencia inline en el navegador (PDFs e imágenes).
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_evidencia' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvidenciaItemDiagnostico`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `EvidenciaItemDiagnostico.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/nueva`
- **Nombre de Función (Backend Handler):** `diagnostico.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutoevaluacionSST, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AutoevaluacionSST, PerfilEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, AutoevaluacionSST.items → ItemAutoevaluacion`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id, anio=anio, cliente_id=cid_ctx`
  - Plantillas HTML Parseadas: `diagnostico/form.html, diagnostico/form.html`

#### 🔗 Ruta Servidor: `/diagnostico/objetivos`
- **Nombre de Función (Backend Handler):** `diagnostico.objetivos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** OBJETIVOS SST | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ObjetivoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_f, cliente_id=current_user.cliente_id, anio=anio_f`
  - Plantillas HTML Parseadas: `diagnostico/objetivos.html`

#### 🔗 Ruta Servidor: `/diagnostico/objetivos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `diagnostico.objetivo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ObjetivoSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/objetivos/<int:id>/seguimiento`
- **Nombre de Función (Backend Handler):** `diagnostico.objetivo_seguimiento`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Agrega o actualiza el seguimiento de un trimestre para un objetivo SST.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'objetivo_seguimiento' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ObjetivoSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/objetivos/nuevo`
- **Nombre de Función (Backend Handler):** `diagnostico.objetivo_nuevo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ObjetivoSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/diagnostico/politica`
- **Nombre de Función (Backend Handler):** `diagnostico.politica`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** POLÍTICA DE SST | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica' - asociado al sistema maestro de **Diagnostico**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaSST`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PoliticaSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cid_form`
  - Plantillas HTML Parseadas: `diagnostico/politica.html`

---

### 📦 Blueprint / Módulo: `DOCUMENTOS`

#### 🔗 Ruta Servidor: `/documentos/`
- **Nombre de Función (Backend Handler):** `documentos.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, tipo=tipo_f, estado=estado_f... (+8 más)`
  - Plantillas HTML Parseadas: `documentos/lista.html`

#### 🔗 Ruta Servidor: `/documentos/<int:familia_id>/versiones`
- **Nombre de Función (Backend Handler):** `documentos.versiones_familia`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista todas las versiones de una familia documental.
- **Comentarios Descriptivos:** VERSIONES DE FAMILIA | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'versiones_familia' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `familia_id=familia_id`
  - Plantillas HTML Parseadas: `documentos/versiones_familia.html`

#### 🔗 Ruta Servidor: `/documentos/<int:id>`
- **Nombre de Función (Backend Handler):** `documentos.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DistribucionDocumento, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DocumentoSST.descargas → DescargaDocumento, DocumentoSST.historial_estados → HistorialVersionDocumento, DocumentoSST.distribuciones → DistribucionDocumento`
    - Filtros Aplicados (WHERE): `familia_id=doc.familia_id, documento_id=doc.id`
  - Plantillas HTML Parseadas: `documentos/detalle.html`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/cambiar-estado`
- **Nombre de Función (Backend Handler):** `documentos.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Filtros Aplicados (WHERE): `familia_id=doc.familia_id, estado='vigente', DocumentoSST.id != doc.id`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/descargar`
- **Nombre de Función (Backend Handler):** `documentos.descargar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** DESCARGAR ARCHIVO | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DescargaDocumento, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente, DescargaDocumento.documento → DocumentoSST, DescargaDocumento.usuario → Usuario`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/distribucion`
- **Nombre de Función (Backend Handler):** `documentos.distribucion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'distribucion' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DistribucionDocumento, DocumentoSST, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, DocumentoSST.cliente → Cliente, DocumentoSST.distribuciones → DistribucionDocumento`
    - Filtros Aplicados (WHERE): `documento_id=id, cliente_id=doc.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `documentos/distribucion.html`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/distribucion/<int:dist_id>/eliminar`
- **Nombre de Función (Backend Handler):** `documentos.distribucion_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DistribucionDocumento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/distribucion/acuse-pdf`
- **Nombre de Función (Backend Handler):** `documentos.distribucion_acuse_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera el acuse de recibo de distribución controlada en PDF.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'distribucion_acuse_pdf' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DistribucionDocumento, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DocumentoSST.distribuciones → DistribucionDocumento`
    - Filtros Aplicados (WHERE): `documento_id=id`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `documentos.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `documentos/form.html, documentos/form.html`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `documentos.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/documentos/<int:id>/nueva-version`
- **Nombre de Función (Backend Handler):** `documentos.nueva_version`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crea una nueva versión de un documento existente.

    - Calcula version_num = MAX(version_num) + 1 para la familia.
    - El nuevo documento queda en estado 'borrador' (flujo auditado).
    - Al aprobarlo desde detalle, el sistema marcará la versión anterior como obsoleta.
    - Campo obligatorio: motivo_cambio.
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST, HistorialVersionDocumento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente, HistorialVersionDocumento.usuario → Usuario`
    - Filtros Aplicados (WHERE): `familia_id=familia_id, familia_id=familia_id, familia_id=familia_id`
  - Plantillas HTML Parseadas: `documentos/nueva_version.html, documentos/nueva_version.html`

#### 🔗 Ruta Servidor: `/documentos/generar-codigo`
- **Nombre de Función (Backend Handler):** `documentos.generar_codigo_ajax`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Devuelve el siguiente código sugerido para el tipo dado.
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_codigo_ajax' - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/documentos/listado-maestro`
- **Nombre de Función (Backend Handler):** `documentos.listado_maestro`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de documentos vigentes en formato oficial de auditoría.
- **Comentarios Descriptivos:** LISTADO MAESTRO | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, DocumentoSST.estado.in_(['vigente', 'por_revisar']`
  - Plantillas HTML Parseadas: `documentos/listado_maestro.html`

#### 🔗 Ruta Servidor: `/documentos/listado-maestro/exportar-excel`
- **Nombre de Función (Backend Handler):** `documentos.exportar_listado_maestro_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta el Listado Maestro en Excel para auditorías (solo vigentes).
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, DocumentoSST.estado.in_(['vigente', 'por_revisar']`

#### 🔗 Ruta Servidor: `/documentos/nuevo`
- **Nombre de Función (Backend Handler):** `documentos.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `documentos/form.html, documentos/form.html, documentos/form.html`

#### 🔗 Ruta Servidor: `/documentos/trd`
- **Nombre de Función (Backend Handler):** `documentos.trd_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** TABLA DE RETENCIÓN DOCUMENTAL (TRD) | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, DocumentoSST, TablaRetencionDocumental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente, TablaRetencionDocumental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, activo=True, cliente_id=cid_ctx, cliente_id=cid_ctx... (+2 más)`
  - Plantillas HTML Parseadas: `documentos/trd.html`

#### 🔗 Ruta Servidor: `/documentos/trd/<int:trd_id>/eliminar`
- **Nombre de Función (Backend Handler):** `documentos.trd_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TablaRetencionDocumental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/documentos/trd/nuevo`
- **Nombre de Función (Backend Handler):** `documentos.trd_nuevo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Documentos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TablaRetencionDocumental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TablaRetencionDocumental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente, TablaRetencionDocumental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo_documento=tipo`

---

### 📦 Blueprint / Módulo: `DOTACION_EPP`

#### 🔗 Ruta Servidor: `/dotacion-epp/`
- **Nombre de Función (Backend Handler):** `dotacion_epp.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Entregas de EPP | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, EntregaEPP, TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EntregaEPP, Empleado, TipoEPP`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, TipoEPP.cliente → Cliente, TipoEPP.entregas → EntregaEPP, EntregaEPP.cliente → Cliente, EntregaEPP.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, empleado_id=empleado_id, estado=estado... (+5 más)`
  - Plantillas HTML Parseadas: `dotacion_epp/lista.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/<int:id>`
- **Nombre de Función (Backend Handler):** `dotacion_epp.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntregaEPP`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `dotacion_epp/detalle.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `dotacion_epp.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, EntregaEPP, TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, TipoEPP`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, TipoEPP.cliente → Cliente, EntregaEPP.cliente → Cliente, EntregaEPP.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `dotacion_epp/form.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `dotacion_epp.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntregaEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/dotacion-epp/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `dotacion_epp.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntregaEPP`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `EntregaEPP.empleado → Empleado`

#### 🔗 Ruta Servidor: `/dotacion-epp/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `dotacion_epp.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Exportaciones | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EntregaEPP`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `EntregaEPP.empleado → Empleado`

#### 🔗 Ruta Servidor: `/dotacion-epp/nueva`
- **Nombre de Función (Backend Handler):** `dotacion_epp.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, EntregaEPP, TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, TipoEPP`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, TipoEPP.cliente → Cliente, EntregaEPP.cliente → Cliente, EntregaEPP.empleado → Empleado, EntregaEPP.creado_por → Usuario`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `dotacion_epp/form.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/tipos`
- **Nombre de Función (Backend Handler):** `dotacion_epp.lista_tipos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `dotacion_epp/tipos.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/tipos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `dotacion_epp.editar_tipo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `dotacion_epp/tipo_form.html`

#### 🔗 Ruta Servidor: `/dotacion-epp/tipos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `dotacion_epp.eliminar_tipo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `TipoEPP.entregas → EntregaEPP`

#### 🔗 Ruta Servidor: `/dotacion-epp/tipos/nuevo`
- **Nombre de Función (Backend Handler):** `dotacion_epp.nuevo_tipo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Dotacion_Epp**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoEPP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TipoEPP.cliente → Cliente`
  - Plantillas HTML Parseadas: `dotacion_epp/tipo_form.html`

---

### 📦 Blueprint / Módulo: `EMPLEADOS`

#### 🔗 Ruta Servidor: `/empleados/api/areas/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `empleados.api_areas_por_cliente`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area`
    - Relaciones Navegadas (JOINS): `Area.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/empleados/api/empleados/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `empleados.api_empleados_por_cliente`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** API endpoints para dropdowns dinámicos
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/empleados/areas`
- **Nombre de Función (Backend Handler):** `empleados.listar_areas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area, Cliente`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Cliente.areas → Area, Area.cliente → Cliente, Area.empleados → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cliente_id, activo=True... (+2 más)`
  - Plantillas HTML Parseadas: `empleados/areas.html`

#### 🔗 Ruta Servidor: `/empleados/areas/<int:area_id>/actualizar`
- **Nombre de Función (Backend Handler):** `empleados.actualizar_area`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_area' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Area.cliente → Cliente, Area.empleados → Empleado`
    - Filtros Aplicados (WHERE): `
        Area.cliente_id == area.cliente_id,
        Area.nombre == nombre,
        Area.id != area_id
    `

#### 🔗 Ruta Servidor: `/empleados/areas/<int:area_id>/editar`
- **Nombre de Función (Backend Handler):** `empleados.editar_area`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Area.cliente → Cliente, Area.empleados → Empleado`
  - Plantillas HTML Parseadas: `empleados/area_form.html`

#### 🔗 Ruta Servidor: `/empleados/areas/<int:area_id>/toggle`
- **Nombre de Función (Backend Handler):** `empleados.toggle_area`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Area.cliente → Cliente, Area.empleados → Empleado`

#### 🔗 Ruta Servidor: `/empleados/areas/crear`
- **Nombre de Función (Backend Handler):** `empleados.crear_area`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** Verificar permisos
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_area' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Area.cliente → Cliente, Area.empleados → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, nombre=nombre`

#### 🔗 Ruta Servidor: `/empleados/areas/nueva/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `empleados.nueva_area`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Verificar que el usuario puede acceder a esta empresa
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Cliente.areas → Area`
  - Plantillas HTML Parseadas: `empleados/area_form.html`

#### 🔗 Ruta Servidor: `/empleados/empleados`
- **Nombre de Función (Backend Handler):** `empleados.listar_empleados`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, Cliente`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Empleado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cliente_id, activo=True... (+2 más)`
  - Plantillas HTML Parseadas: `empleados/lista.html`

#### 🔗 Ruta Servidor: `/empleados/empleados/<int:id>/autorizacion-pdf`
- **Nombre de Función (Backend Handler):** `empleados.pdf_autorizacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga el PDF de Autorización de Tratamiento de Datos del empleado.
- **Comentarios Descriptivos:** ── PDF AUTORIZACIÓN TRATAMIENTO DE DATOS (MOD-35) ───────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'pdf_autorizacion' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente`

#### 🔗 Ruta Servidor: `/empleados/empleados/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `empleados.editar_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Muestra el formulario de edición de un empleado [asesor, cliente-propio]
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area, Area.cliente → Cliente, Area.empleados → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=empleado.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `empleados/form.html`

#### 🔗 Ruta Servidor: `/empleados/empleados/<int:id>/guardar`
- **Nombre de Función (Backend Handler):** `empleados.guardar_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Guarda los cambios de edición de un empleado [asesor, cliente-propio]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'guardar_empleado' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area`

#### 🔗 Ruta Servidor: `/empleados/empleados/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `empleados.toggle_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa/desactiva un empleado [asesor_required]
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente`

#### 🔗 Ruta Servidor: `/empleados/empleados/crear`
- **Nombre de Función (Backend Handler):** `empleados.crear_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** Verificar permisos
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_empleado' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Empleado.cliente → Cliente, Empleado.area → Area`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, documento=documento`

#### 🔗 Ruta Servidor: `/empleados/empleados/exportar-excel`
- **Nombre de Función (Backend Handler):** `empleados.exportar_empleados_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga la lista de empleados en formato Excel (.xlsx).
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Empleado`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Empleado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cliente_id, Empleado.cliente_id.in_(cids`

#### 🔗 Ruta Servidor: `/empleados/empleados/exportar-pdf`
- **Nombre de Función (Backend Handler):** `empleados.exportar_empleados_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga la lista de empleados en PDF (orientación apaisada, compacto).
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Empleado`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Empleado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cliente_id, Empleado.cliente_id.in_(cids`

#### 🔗 Ruta Servidor: `/empleados/empleados/importar`
- **Nombre de Función (Backend Handler):** `empleados.importar_empleados`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** GET:  muestra el formulario de carga.
    POST: procesa el archivo y renderiza la vista previa de validación.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_empleados' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado`
    - Filtros Aplicados (WHERE): `activo=True, Cliente.id.in_(cids`
  - Plantillas HTML Parseadas: `empleados/importar.html, empleados/importar.html, empleados/importar.html, empleados/importar.html, empleados/importar.html, empleados/importar.html, empleados/importar.html`

#### 🔗 Ruta Servidor: `/empleados/empleados/importar-confirmar`
- **Nombre de Función (Backend Handler):** `empleados.importar_empleados_confirmar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Recibe los datos validados (JSON en el form) y los persiste en la BD.
    Solo guarda filas con estado 'ok' o 'advertencia'.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_empleados_confirmar' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, documento=documento`

#### 🔗 Ruta Servidor: `/empleados/empleados/nuevo/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `empleados.nuevo_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Verificar que el usuario puede acceder a esta empresa
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Area, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Area`
    - Relaciones Navegadas (JOINS): `Cliente.empleados → Empleado, Cliente.areas → Area, Empleado.cliente → Cliente, Empleado.area → Area, Area.cliente → Cliente, Area.empleados → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `empleados/form.html`

#### 🔗 Ruta Servidor: `/empleados/empleados/plantilla-importacion`
- **Nombre de Función (Backend Handler):** `empleados.descargar_plantilla_importacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga la plantilla Excel para importación masiva de empleados.
- **Comentarios Descriptivos:** IMPORTACIÓN MASIVA DE EMPLEADOS | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_plantilla_importacion' - asociado al sistema maestro de **Empleados**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

---

### 📦 Blueprint / Módulo: `FIRMAS`

#### 🔗 Ruta Servidor: `/firmas/aprobar/<int:id>`
- **Nombre de Función (Backend Handler):** `firmas.aprobar_firma`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Aprobar una firma pendiente.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'aprobar_firma' - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FirmaDocumento, HistorialFirma, PasoAprobacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PasoAprobacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PasoAprobacion.flujo → FlujoAprobacionDoc, PasoAprobacion.usuario_aprobador → Usuario, FirmaDocumento.cliente → Cliente, FirmaDocumento.usuario → Usuario, FirmaDocumento.flujo → FlujoAprobacionDoc, HistorialFirma.firma_documento → FirmaDocumento, HistorialFirma.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            flujo_id=firma.flujo_id,
            nivel=firma.nivel_aprobacion
        `

#### 🔗 Ruta Servidor: `/firmas/auditoria`
- **Nombre de Función (Backend Handler):** `firmas.auditoria_firmas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista de todas las firmas realizadas (auditoría inmutable).
- **Comentarios Descriptivos:** HISTORIAL Y AUDITORÍA DE FIRMAS | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria_firmas' - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialFirma`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `HistorialFirma`
    - Relaciones Navegadas (JOINS): `HistorialFirma.cliente → Cliente`
    - Filtros Aplicados (WHERE): `HistorialFirma.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `firmas/auditoria.html`

#### 🔗 Ruta Servidor: `/firmas/auditoria/exportar-pdf`
- **Nombre de Función (Backend Handler):** `firmas.exportar_auditoria_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exportar auditoría de firmas a PDF.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, HistorialFirma`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, HistorialFirma`
    - Relaciones Navegadas (JOINS): `HistorialFirma.cliente → Cliente`
    - Filtros Aplicados (WHERE): `HistorialFirma.cliente_id.in_(cids`

#### 🔗 Ruta Servidor: `/firmas/firmar/<token>`
- **Nombre de Función (Backend Handler):** `firmas.firmar_externo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Enlace público temporal para firma externa (sin login).
    El firmante externo accede con el token único y firma el documento.
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════════════════════════════ | FIRMA EXTERNA — ENLACE PÚBLICO TEMPORAL | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'firmar_externo' - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FirmaDocumento, FirmaExterna, HistorialFirma`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `FirmaExterna`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `FirmaDocumento.cliente → Cliente, FirmaDocumento.firma_externa → FirmaExterna, FirmaExterna.cliente → Cliente, HistorialFirma.firma_documento → FirmaDocumento, HistorialFirma.cliente → Cliente`
    - Filtros Aplicados (WHERE): `token=token`
  - Plantillas HTML Parseadas: `firmas/firma_exitosa.html, firmas/firmar_externo.html, firmas/firmar_externo.html, firmas/firmar_externo.html`

#### 🔗 Ruta Servidor: `/firmas/flujos`
- **Nombre de Función (Backend Handler):** `firmas.listar_flujos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista de flujos de aprobación configurados.
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, FlujoAprobacionDoc`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `FlujoAprobacionDoc, Cliente`
    - Relaciones Navegadas (JOINS): `FlujoAprobacionDoc.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, FlujoAprobacionDoc.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `firmas/flujos_lista.html`

#### 🔗 Ruta Servidor: `/firmas/flujos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `firmas.editar_flujo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Editar flujo de aprobación existente.
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, FlujoAprobacionDoc, PasoAprobacion, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PasoAprobacion, Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, FlujoAprobacionDoc.cliente → Cliente, FlujoAprobacionDoc.pasos → PasoAprobacion, PasoAprobacion.flujo → FlujoAprobacionDoc, PasoAprobacion.usuario_aprobador → Usuario`
    - Filtros Aplicados (WHERE): `flujo_id=flujo.id, activo=True, activo=True... (+1 más)`
  - Plantillas HTML Parseadas: `firmas/flujo_form.html`

#### 🔗 Ruta Servidor: `/firmas/flujos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `firmas.eliminar_flujo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Eliminar flujo de aprobación.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FlujoAprobacionDoc`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `FlujoAprobacionDoc.cliente → Cliente`

#### 🔗 Ruta Servidor: `/firmas/flujos/nuevo`
- **Nombre de Función (Backend Handler):** `firmas.nuevo_flujo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crear nuevo flujo de aprobación.
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, FlujoAprobacionDoc, PasoAprobacion, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, FlujoAprobacionDoc.cliente → Cliente, PasoAprobacion.flujo → FlujoAprobacionDoc, PasoAprobacion.usuario_aprobador → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, activo=True, Cliente.id.in_(cids`
  - Plantillas HTML Parseadas: `firmas/flujo_form.html`

#### 🔗 Ruta Servidor: `/firmas/mis-aprobaciones`
- **Nombre de Función (Backend Handler):** `firmas.mis_aprobaciones`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Cola de documentos pendientes de aprobación por el usuario actual.
    Filtra por los flujos en los que el usuario es aprobador.
- **Comentarios Descriptivos:** COLA DE APROBACIONES PENDIENTES | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'mis_aprobaciones' - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FirmaDocumento, PasoAprobacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PasoAprobacion, FirmaDocumento`
    - Relaciones Navegadas (JOINS): `PasoAprobacion.flujo → FlujoAprobacionDoc, PasoAprobacion.usuario_aprobador → Usuario, FirmaDocumento.usuario → Usuario, FirmaDocumento.flujo → FlujoAprobacionDoc`
    - Filtros Aplicados (WHERE): `usuario_aprobador_id=current_user.id, 
        FirmaDocumento.flujo_id.in_(flujo_ids`
  - Plantillas HTML Parseadas: `firmas/mis_aprobaciones.html, firmas/mis_aprobaciones.html`

#### 🔗 Ruta Servidor: `/firmas/verificar/<token>`
- **Nombre de Función (Backend Handler):** `firmas.verificar_firma`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** URL pública de verificación de firma.
    Busca por token_verificacion y muestra los datos de la firma.
    No require autenticación.
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════════════════════════════ | VERIFICACIÓN PÚBLICA DE FIRMAS (SIN LOGIN) | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'verificar_firma' - asociado al sistema maestro de **Firmas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FirmaDocumento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `FirmaDocumento`
    - Filtros Aplicados (WHERE): `token_verificacion=token`
  - Plantillas HTML Parseadas: `firmas/verificar.html, firmas/verificar.html`

---

### 📦 Blueprint / Módulo: `FORMULARIOS_PERSONALIZADOS`

---

### 📦 Blueprint / Módulo: `GESTION_AMBIENTAL`

#### 🔗 Ruta Servidor: `/gestion-ambiental/`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Registros de Residuos | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS, RegistroResiduos`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroResiduos, OperadorPGIRS`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.cliente → Cliente, OperadorPGIRS.registros → RegistroResiduos, RegistroResiduos.cliente → Cliente, RegistroResiduo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, mes=mes, categoria=categoria... (+4 más)`
  - Plantillas HTML Parseadas: `gestion_ambiental/lista.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/<int:id>`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduos`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `gestion_ambiental/detalle.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS, RegistroResiduos`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `OperadorPGIRS`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.cliente → Cliente, RegistroResiduos.cliente → Cliente, RegistroResiduo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `gestion_ambiental/form.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduos`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/gestion-ambiental/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Exportaciones | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduos`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/gestion-ambiental/nuevo`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS, RegistroResiduos`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `OperadorPGIRS`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.cliente → Cliente, RegistroResiduos.cliente → Cliente, RegistroResiduo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `gestion_ambiental/form.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/operadores`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.lista_operadores`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `OperadorPGIRS`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid`
  - Plantillas HTML Parseadas: `gestion_ambiental/operadores.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/operadores/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.editar_operador`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `gestion_ambiental/operador_form.html`

#### 🔗 Ruta Servidor: `/gestion-ambiental/operadores/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.eliminar_operador`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.registros → RegistroResiduos`

#### 🔗 Ruta Servidor: `/gestion-ambiental/operadores/nuevo`
- **Nombre de Función (Backend Handler):** `gestion_ambiental.nuevo_operador`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Gestion_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `OperadorPGIRS`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `OperadorPGIRS.cliente → Cliente`
  - Plantillas HTML Parseadas: `gestion_ambiental/operador_form.html`

---

### 📦 Blueprint / Módulo: `INDICADORES`

#### 🔗 Ruta Servidor: `/indicadores/`
- **Nombre de Función (Backend Handler):** `indicadores.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** LISTA  [todos los roles con acceso] | ───────────────────────────────────────────────────────────────────────────── | Determinar qué clientes puede ver el usuario
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, DatoBaseIndicador`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DatoBaseIndicador`
    - Relaciones Navegadas (JOINS): `DatoBaseIndicador.cliente → Cliente`
    - Filtros Aplicados (WHERE): `anio=anio, DatoBaseIndicador.cliente_id.in_(ids`
  - Plantillas HTML Parseadas: `indicadores/lista.html`

#### 🔗 Ruta Servidor: `/indicadores/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `indicadores.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** EDITAR  [asesor] | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DatoBaseIndicador`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `DatoBaseIndicador.cliente → Cliente`
  - Plantillas HTML Parseadas: `indicadores/form.html`

#### 🔗 Ruta Servidor: `/indicadores/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `indicadores.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ELIMINAR  [asesor] | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DatoBaseIndicador`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `DatoBaseIndicador.cliente → Cliente`

#### 🔗 Ruta Servidor: `/indicadores/exportar-excel`
- **Nombre de Función (Backend Handler):** `indicadores.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR EXCEL  [todos los roles con acceso] | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, DatoBaseIndicador`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `DatoBaseIndicador.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio`

#### 🔗 Ruta Servidor: `/indicadores/historial`
- **Nombre de Función (Backend Handler):** `indicadores.historial`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** HISTORIAL — tabla + gráficas  [todos los roles con acceso] | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'historial' - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Cliente, DatoBaseIndicador, SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora, SesionCapacitacion`
    - Relaciones Navegadas (JOINS): `Cliente.perfil_sst → PerfilEmpresa, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, SesionCapacitacion.cliente → Cliente, DatoBaseIndicador.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=cliente_id, 
        cliente_id=cliente_id, estado='cerrada'... (+6 más)`
  - Plantillas HTML Parseadas: `indicadores/historial.html`

#### 🔗 Ruta Servidor: `/indicadores/nuevo`
- **Nombre de Función (Backend Handler):** `indicadores.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** NUEVO  [asesor] | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Indicadores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DatoBaseIndicador`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DatoBaseIndicador`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DatoBaseIndicador.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id, anio=anio, mes=mes`
  - Plantillas HTML Parseadas: `indicadores/form.html, indicadores/form.html, indicadores/form.html`

---

### 📦 Blueprint / Módulo: `INFORME_GESTION`

#### 🔗 Ruta Servidor: `/informe-gestion/`
- **Nombre de Función (Backend Handler):** `informe_gestion.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Formulario de selección de período para generar el informe.
- **Comentarios Descriptivos:** ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, InformeGestion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, modulo_key='informe_gestion'`
  - Plantillas HTML Parseadas: `informe_gestion/generar.html`

#### 🔗 Ruta Servidor: `/informe-gestion/<int:informe_id>/eliminar`
- **Nombre de Función (Backend Handler):** `informe_gestion.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina un informe guardado (solo asesores).
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeGestion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/informe-gestion/<int:informe_id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `informe_gestion.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga un Excel consolidado del informe de gestión.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeGestion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.inspecciones → Inspeccion, Inspeccion.visita → Visita, Proveedor.cliente → Cliente, InformeGestion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/informe-gestion/<int:informe_id>/exportar-prompt`
- **Nombre de Función (Backend Handler):** `informe_gestion.exportar_prompt`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga un .txt con el prompt completo + datos JSON listos para pegar en cualquier IA.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeGestion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/informe-gestion/<int:informe_id>/prompt-json`
- **Nombre de Función (Backend Handler):** `informe_gestion.prompt_json`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Devuelve el prompt completo como JSON para uso con NotebookLM u otras herramientas.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'prompt_json' - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeGestion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/informe-gestion/<int:informe_id>/ver`
- **Nombre de Función (Backend Handler):** `informe_gestion.ver`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Muestra un informe guardado sin regenerar nada.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver' - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeGestion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente`
  - Plantillas HTML Parseadas: `informe_gestion/resultado.html`

#### 🔗 Ruta Servidor: `/informe-gestion/generar`
- **Nombre de Función (Backend Handler):** `informe_gestion.generar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Ejecuta los 3 pasos: recopilación → IA → PDF y muestra el resultado.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar' - asociado al sistema maestro de **Informe_Gestion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, InformeGestion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `InformeGestion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InformeGestion.cliente → Cliente, InformeGestion.generado_por → Usuario`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id,
        modulo_key=modulo_key,
        periodo_hash=periodo_hash,
        anio=anio,
        mes_ini=mes_ini,
        mes_fin=mes_fin,
    `
  - Plantillas HTML Parseadas: `informe_gestion/resultado.html`

---

### 📦 Blueprint / Módulo: `INFORMES_ENTIDADES`

#### 🔗 Ruta Servidor: `/informes-entidades/`
- **Nombre de Función (Backend Handler):** `informes_entidades.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─── Rutas ────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeEntidadControl.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=current_user.cliente_id, tipo_informe=ftype... (+3 más)`
  - Plantillas HTML Parseadas: `informes_entidades/lista.html`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>`
- **Nombre de Función (Backend Handler):** `informes_entidades.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfigNotificacion, FuratDetalle, InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa, FuratDetalle, ConfigNotificacion`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.furat → FuratDetalle, FuratDetalle.reporte → ReporteIncidente, FuratDetalle.cliente → Cliente, ConfigNotificacion.cliente → Cliente, PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=informe.cliente_id, cliente_id=informe.cliente_id, 
            cliente_id=informe.cliente_id, tipo_evento='informe_entidad', activo=True
        ... (+1 más)`
  - Plantillas HTML Parseadas: `informes_entidades/detalle.html`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `informes_entidades.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/enviar-email`
- **Nombre de Función (Backend Handler):** `informes_entidades.enviar_email`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Envía el informe ejecutivo mensual por email al representante legal y asesores.
- **Comentarios Descriptivos:** ── Ruta: Enviar informe ejecutivo por email ─────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'enviar_email' - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfigNotificacion, InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa, ConfigNotificacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ConfigNotificacion.cliente → Cliente, PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=informe.cliente_id, 
            cliente_id=informe.cliente_id, tipo_evento='informe_entidad', activo=True
        `

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/estado`
- **Nombre de Función (Backend Handler):** `informes_entidades.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `informes_entidades.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente, InformeEntidadControl.generado_por → Usuario`
    - Filtros Aplicados (WHERE): `cliente_id=informe.cliente_id`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `informes_entidades.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente, InformeEntidadControl.generado_por → Usuario`
    - Filtros Aplicados (WHERE): `cliente_id=informe.cliente_id`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/exportar-pdf-ejecutivo`
- **Nombre de Función (Backend Handler):** `informes_entidades.exportar_pdf_ejecutivo`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Descarga el PDF del informe ejecutivo mensual.
- **Comentarios Descriptivos:** EXPORTAR PDF — actualizado para manejar tipo ejecutivo | ═════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InformeEntidadControl.cliente → Cliente`

#### 🔗 Ruta Servidor: `/informes-entidades/<int:id>/generar-prompt-ia`
- **Nombre de Función (Backend Handler):** `informes_entidades.generar_prompt_ia`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera un prompt estructurado para IA con los datos del informe ejecutivo.
    Similar al sistema de Informe de Gestión IA, pero devuelve JSON 
    para copiar al portapapeles y usar en NotebookLM.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_prompt_ia' - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=informe.cliente_id`

#### 🔗 Ruta Servidor: `/informes-entidades/ejecutivo`
- **Nombre de Función (Backend Handler):** `informes_entidades.generar_ejecutivo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Formulario + generación del informe ejecutivo mensual SG-SST.
- **Comentarios Descriptivos:** ── Ruta: Generar informe ejecutivo mensual ──────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_ejecutivo' - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, InformeEntidadControl, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente, InformeEntidadControl.cliente → Cliente, InformeEntidadControl.generado_por → Usuario`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `informes_entidades/ejecutivo.html`

#### 🔗 Ruta Servidor: `/informes-entidades/generar`
- **Nombre de Función (Backend Handler):** `informes_entidades.generar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar' - asociado al sistema maestro de **Informes_Entidades**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, InformeEntidadControl`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InformeEntidadControl.cliente → Cliente, InformeEntidadControl.generado_por → Usuario`
  - Plantillas HTML Parseadas: `informes_entidades/generar.html`

---

### 📦 Blueprint / Módulo: `INSPECCIONES`

#### 🔗 Ruta Servidor: `/inspecciones/`
- **Nombre de Función (Backend Handler):** `inspecciones.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════ | PANEL PRINCIPAL INSPECCIONES | ═══════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente`
    - Filtros Aplicados (WHERE): `model.cliente_id.in_(cids_filter`
  - Plantillas HTML Parseadas: `inspecciones/index.html`

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════ | ═══════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionBotiquin.cliente → Cliente`
    - Filtros Aplicados (WHERE): `extract('year', InspeccionBotiquin.fecha, InspeccionBotiquin.cliente_id == cliente_fid`
  - Plantillas HTML Parseadas: `inspecciones/botiquin_lista.html`

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/<int:id>`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `inspecciones/botiquin_detalle.html`

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/<int:id>/crear-accion`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_crear_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'botiquin_crear_accion' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionBotiquin.cliente → Cliente`

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/<int:id>/exportar`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/inspecciones/botiquin/nueva`
- **Nombre de Función (Backend Handler):** `inspecciones.botiquin_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionBotiquin`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InspeccionBotiquin.cliente → Cliente`
  - Plantillas HTML Parseadas: `inspecciones/botiquin_form.html`

#### 🔗 Ruta Servidor: `/inspecciones/camillas/`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════ | ═══════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionCamilla.cliente → Cliente`
    - Filtros Aplicados (WHERE): `extract('year', InspeccionCamilla.fecha, InspeccionCamilla.cliente_id == cliente_fid`
  - Plantillas HTML Parseadas: `inspecciones/camillas_lista.html`

#### 🔗 Ruta Servidor: `/inspecciones/camillas/<int:id>`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `inspecciones/camillas_detalle.html`

#### 🔗 Ruta Servidor: `/inspecciones/camillas/<int:id>/crear-accion`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_crear_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'camillas_crear_accion' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionCamilla.cliente → Cliente`

#### 🔗 Ruta Servidor: `/inspecciones/camillas/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/inspecciones/camillas/<int:id>/exportar`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/inspecciones/camillas/nueva`
- **Nombre de Función (Backend Handler):** `inspecciones.camillas_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionCamilla`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InspeccionCamilla.cliente → Cliente`
  - Plantillas HTML Parseadas: `inspecciones/camillas_form.html`

#### 🔗 Ruta Servidor: `/inspecciones/extintores/`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════ | ═══════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionExtintor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `extract('year', InspeccionExtintor.fecha_inspeccion, InspeccionExtintor.cliente_id == cliente_fid`
  - Plantillas HTML Parseadas: `inspecciones/extintores_lista.html`

#### 🔗 Ruta Servidor: `/inspecciones/extintores/<int:id>`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `inspecciones/extintores_detalle.html`

#### 🔗 Ruta Servidor: `/inspecciones/extintores/<int:id>/crear-accion`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_crear_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'extintores_crear_accion' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionExtintor.cliente → Cliente`

#### 🔗 Ruta Servidor: `/inspecciones/extintores/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/inspecciones/extintores/<int:id>/exportar`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/inspecciones/extintores/nueva`
- **Nombre de Función (Backend Handler):** `inspecciones.extintores_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionExtintor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InspeccionExtintor.cliente → Cliente`
  - Plantillas HTML Parseadas: `inspecciones/extintores_form.html`

#### 🔗 Ruta Servidor: `/inspecciones/general/`
- **Nombre de Función (Backend Handler):** `inspecciones.general_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════════ | INSPECCIÓN GENERAL | ═══════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionGeneral`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionGeneral.cliente → Cliente`
    - Filtros Aplicados (WHERE): `extract('year', InspeccionGeneral.fecha_diligenciamiento, InspeccionGeneral.cliente_id == cliente_fid`
  - Plantillas HTML Parseadas: `inspecciones/general_lista.html`

#### 🔗 Ruta Servidor: `/inspecciones/general/<int:id>`
- **Nombre de Función (Backend Handler):** `inspecciones.general_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionGeneral`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `inspecciones/general_detalle.html`

#### 🔗 Ruta Servidor: `/inspecciones/general/<int:id>/crear-accion`
- **Nombre de Función (Backend Handler):** `inspecciones.general_crear_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'general_crear_accion' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionGeneral`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InspeccionGeneral.cliente → Cliente`

#### 🔗 Ruta Servidor: `/inspecciones/general/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `inspecciones.general_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionGeneral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/inspecciones/general/<int:id>/exportar`
- **Nombre de Función (Backend Handler):** `inspecciones.general_exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InspeccionGeneral`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/inspecciones/general/nueva`
- **Nombre de Función (Backend Handler):** `inspecciones.general_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Inspecciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, InspeccionGeneral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `InspeccionGeneral.cliente → Cliente, InspeccionGeneral.centro_trabajo → CentroTrabajo, CentroTrabajo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `inspecciones/general_form.html`

---

### 📦 Blueprint / Módulo: `MEDICINA_LABORAL`

#### 🔗 Ruta Servidor: `/medicina-laboral/`
- **Nombre de Función (Backend Handler):** `medicina_laboral.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** LISTA / DASHBOARD | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExamenMedico, Incapacidad, ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ExamenMedico, Incapacidad, ReintegroLaboral`
    - Relaciones Navegadas (JOINS): `ExamenMedico.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.reintegro → ReintegroLaboral, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cid, cliente_id=cid... (+3 más)`
  - Plantillas HTML Parseadas: `medicina_laboral/lista.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/alertas`
- **Nombre de Función (Backend Handler):** `medicina_laboral.alertas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ALERTAS (endpoint JSON para MOD-19 + vista interna) | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'alertas' - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `medicina_laboral/alertas.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/empleado/<int:empleado_id>`
- **Nombre de Función (Backend Handler):** `medicina_laboral.perfil_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PERFIL MÉDICO POR EMPLEADO | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'perfil_empleado' - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, ExamenMedico, Incapacidad, ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ExamenMedico, Incapacidad, ReintegroLaboral`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, Incapacidad.reintegro → ReintegroLaboral, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `empleado_id=empleado_id, empleado_id=empleado_id, empleado_id=empleado_id... (+1 más)`
  - Plantillas HTML Parseadas: `medicina_laboral/perfil_empleado.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/examenes/<int:id>`
- **Nombre de Función (Backend Handler):** `medicina_laboral.detalle_examen`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExamenMedico`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ExamenMedico.cliente → Cliente`
  - Plantillas HTML Parseadas: `medicina_laboral/detalle_examen.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/examenes/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.editar_examen`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, ExamenMedico`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `medicina_laboral/examen_form.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/examenes/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.eliminar_examen`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExamenMedico`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ExamenMedico.cliente → Cliente`

#### 🔗 Ruta Servidor: `/medicina-laboral/examenes/nuevo`
- **Nombre de Función (Backend Handler):** `medicina_laboral.nuevo_examen`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, ExamenMedico`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ExamenMedico.cliente → Cliente, ExamenMedico.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `medicina_laboral/examen_form.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/incapacidades/<int:id>`
- **Nombre de Función (Backend Handler):** `medicina_laboral.detalle_incapacidad`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Incapacidad`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Incapacidad.cliente → Cliente`
  - Plantillas HTML Parseadas: `medicina_laboral/detalle_incapacidad.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/incapacidades/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.editar_incapacidad`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `medicina_laboral/incapacidad_form.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/incapacidades/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.eliminar_incapacidad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Incapacidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `Incapacidad.cliente → Cliente`

#### 🔗 Ruta Servidor: `/medicina-laboral/incapacidades/nueva`
- **Nombre de Función (Backend Handler):** `medicina_laboral.nueva_incapacidad`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `medicina_laboral/incapacidad_form.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/informe/excel`
- **Nombre de Función (Backend Handler):** `medicina_laboral.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTACIÓN EXCEL | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/medicina-laboral/informe/pdf`
- **Nombre de Función (Backend Handler):** `medicina_laboral.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTACIÓN PDF | ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/medicina-laboral/reintegros/<int:id>`
- **Nombre de Función (Backend Handler):** `medicina_laboral.detalle_reintegro`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReintegroLaboral.cliente → Cliente`
  - Plantillas HTML Parseadas: `medicina_laboral/detalle_reintegro.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/reintegros/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.editar_reintegro`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad, ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, Incapacidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, Incapacidad.reintegro → ReintegroLaboral, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid`
  - Plantillas HTML Parseadas: `medicina_laboral/reintegro_form.html`

#### 🔗 Ruta Servidor: `/medicina-laboral/reintegros/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `medicina_laboral.eliminar_reintegro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ReintegroLaboral.cliente → Cliente`

#### 🔗 Ruta Servidor: `/medicina-laboral/reintegros/nuevo`
- **Nombre de Función (Backend Handler):** `medicina_laboral.nuevo_reintegro`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Medicina_Laboral**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, Incapacidad, ReintegroLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado, Incapacidad`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Incapacidad.cliente → Cliente, Incapacidad.empleado → Empleado, Incapacidad.reintegro → ReintegroLaboral, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.empleado → Empleado, ReintegroLaboral.incapacidad → Incapacidad`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True, cliente_id=cid`
  - Plantillas HTML Parseadas: `medicina_laboral/reintegro_form.html`

---

### 📦 Blueprint / Módulo: `NOTIFICACIONES`

#### 🔗 Ruta Servidor: `/notificaciones/`
- **Nombre de Función (Backend Handler):** `notificaciones.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista/hub de configuración de notificaciones.
- **Comentarios Descriptivos:** ---------------------------------------------------------------------------
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Notificaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ConfigNotificacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, ConfigNotificacion`
    - Relaciones Navegadas (JOINS): `ConfigNotificacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, cliente_id=c.id, cliente_id=c.id, activo=True... (+1 más)`
  - Plantillas HTML Parseadas: `notificaciones/lista.html`

#### 🔗 Ruta Servidor: `/notificaciones/configurar/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `notificaciones.configurar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Gestiona configuración de notificaciones para un cliente.
- **Comentarios Descriptivos:** Aislamiento
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'configurar' - asociado al sistema maestro de **Notificaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ConfigNotificacion, PerfilEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfigNotificacion, PerfilEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ConfigNotificacion.cliente → Cliente, PerfilEmpresa.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
                cliente_id=cliente_id, tipo_evento=tipo
            , cliente_id=cliente_id, cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `notificaciones/configurar.html`

#### 🔗 Ruta Servidor: `/notificaciones/historial`
- **Nombre de Función (Backend Handler):** `notificaciones.historial`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Historial de notificaciones enviadas.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'historial' - asociado al sistema maestro de **Notificaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, HistorialNotificacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `HistorialNotificacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id_f, tipo_evento=tipo_f, estado=estado_f... (+3 más)`
  - Plantillas HTML Parseadas: `notificaciones/historial.html`

#### 🔗 Ruta Servidor: `/notificaciones/mis-alertas`
- **Nombre de Función (Backend Handler):** `notificaciones.mis_alertas`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Permite al cliente configurar sus propias preferencias de notificación.
    Acceso: cliente (propietario) o asesor en contexto de cliente.
- **Comentarios Descriptivos:** Ruta cliente: Mis Alertas | ---------------------------------------------------------------------------
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'mis_alertas' - asociado al sistema maestro de **Notificaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ConfigNotificacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfigNotificacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ConfigNotificacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
                cliente_id=cid, tipo_evento=tipo
            , cliente_id=cid`
  - Plantillas HTML Parseadas: `notificaciones/mis_alertas.html`

#### 🔗 Ruta Servidor: `/notificaciones/test/<int:cliente_id>/<tipo_evento>`
- **Nombre de Función (Backend Handler):** `notificaciones.test_envio`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Prueba de envío manual.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'test_envio' - asociado al sistema maestro de **Notificaciones**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ConfiguracionApp`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`

---

### 📦 Blueprint / Módulo: `PESV`

#### 🔗 Ruta Servidor: `/pesv/`
- **Nombre de Función (Backend Handler):** `pesv.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PLANES PESV | ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, DiagnosticoPESV, InspeccionPreOperacional, IntegranteComitePESV, PESV, SiniestroPESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `VehiculoPESV, ConductorPESV, SiniestroPESV, InspeccionPreOperacional`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, PESV.actividades → ActividadPESV, PESV.siniestros → SiniestroPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, VehiculoPESV.siniestros → SiniestroPESV, ConductorPESV.cliente → Cliente, InspeccionPreOperacional.cliente → Cliente, InspeccionPreOperacional.vehiculo → VehiculoPESV, InspeccionPreOperacional.conductor → ConductorPESV, SiniestroPESV.cliente → Cliente, SiniestroPESV.pesv → PESV, SiniestroPESV.vehiculo → VehiculoPESV, SiniestroPESV.conductor → ConductorPESV, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, estado='activo', cliente_id=cid_ctx, cliente_id=cid_ctx, activo=True... (+7 más)`
  - Plantillas HTML Parseadas: `pesv/index.html, pesv/lista.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>`
- **Nombre de Función (Backend Handler):** `pesv.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Dashboard principal del PESV con acceso a cada sección.
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPESV, ConductorPESV, DiagnosticoPESV, InspeccionPreOperacional, IntegranteComitePESV, PESV, SiniestroPESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `VehiculoPESV, ConductorPESV, ActividadPESV, SiniestroPESV, DiagnosticoPESV, IntegranteComitePESV, InspeccionPreOperacional`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, PESV.actividades → ActividadPESV, PESV.siniestros → SiniestroPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, VehiculoPESV.siniestros → SiniestroPESV, ConductorPESV.cliente → Cliente, InspeccionPreOperacional.cliente → Cliente, InspeccionPreOperacional.vehiculo → VehiculoPESV, InspeccionPreOperacional.conductor → ConductorPESV, ActividadPESV.pesv → PESV, ActividadPESV.cliente → Cliente, SiniestroPESV.cliente → Cliente, SiniestroPESV.pesv → PESV, SiniestroPESV.vehiculo → VehiculoPESV, SiniestroPESV.conductor → ConductorPESV, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True, cliente_id=p.cliente_id, activo=True, pesv_id=p.id... (+4 más)`
  - Plantillas HTML Parseadas: `pesv/detalle_dashboard.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/actividades`
- **Nombre de Función (Backend Handler):** `pesv.ver_actividades`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista del plan de acción / actividades.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_actividades' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPESV, Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ActividadPESV, Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, PESV.actividades → ActividadPESV, ActividadPESV.pesv → PESV, ActividadPESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `pesv_id=p.id, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_actividades.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/comite`
- **Nombre de Función (Backend Handler):** `pesv.ver_comite`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista del Comité de Seguridad Vial.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_comite' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, IntegranteComitePESV, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `IntegranteComitePESV, Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente, IntegranteComitePESV.empleado → Empleado`
    - Filtros Aplicados (WHERE): `pesv_id=p.id, activo=True, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_comite.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/comite/acta/pdf`
- **Nombre de Función (Backend Handler):** `pesv.pdf_acta_comite_pesv`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** T-12 · PDF Acta del Comité de Seguridad Vial | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'pdf_acta_comite_pesv' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IntegranteComitePESV, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `IntegranteComitePESV`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `pesv_id=p.id, activo=True`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/conductores`
- **Nombre de Función (Backend Handler):** `pesv.ver_conductores`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de gestión de conductores.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_conductores' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConductorPESV, Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_conductores.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/diagnostico`
- **Nombre de Función (Backend Handler):** `pesv.ver_diagnostico`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de diagnósticos (autoevaluaciones).
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_diagnostico' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DiagnosticoPESV, Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DiagnosticoPESV, Empleado`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `pesv_id=p.id, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_diagnostico.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `pesv.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, PESV.responsable_vial_emp → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/form.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `pesv.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `pesv.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/indicadores`
- **Nombre de Función (Backend Handler):** `pesv.ver_indicadores`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de indicadores de desempeño PESV.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_indicadores' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente`
  - Plantillas HTML Parseadas: `pesv/seccion_indicadores.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/informe-anual/pdf`
- **Nombre de Función (Backend Handler):** `pesv.pdf_informe_anual_pesv`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** T-15 · PDF Informe de Gestión Anual PESV | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'pdf_informe_anual_pesv' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/inspecciones`
- **Nombre de Función (Backend Handler):** `pesv.ver_inspecciones`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de inspecciones pre-operacionales.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_inspecciones' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, InspeccionPreOperacional, PESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `InspeccionPreOperacional, VehiculoPESV, ConductorPESV`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, ConductorPESV.cliente → Cliente, InspeccionPreOperacional.cliente → Cliente, InspeccionPreOperacional.vehiculo → VehiculoPESV, InspeccionPreOperacional.conductor → ConductorPESV`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, cliente_id=p.cliente_id, activo=True, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_inspecciones.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/siniestros`
- **Nombre de Función (Backend Handler):** `pesv.ver_siniestros`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de siniestros registrados.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_siniestros' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, PESV, SiniestroPESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `SiniestroPESV, VehiculoPESV, ConductorPESV`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, PESV.siniestros → SiniestroPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, VehiculoPESV.siniestros → SiniestroPESV, ConductorPESV.cliente → Cliente, SiniestroPESV.cliente → Cliente, SiniestroPESV.pesv → PESV, SiniestroPESV.vehiculo → VehiculoPESV, SiniestroPESV.conductor → ConductorPESV`
    - Filtros Aplicados (WHERE): `pesv_id=p.id, cliente_id=p.cliente_id, activo=True, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_siniestros.html`

#### 🔗 Ruta Servidor: `/pesv/<int:id>/vehiculos`
- **Nombre de Función (Backend Handler):** `pesv.ver_vehiculos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista de gestión de vehículos.
- **Comentarios Descriptivos:** VISTAS SEPARADAS POR SECCIÓN | ════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_vehiculos' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `VehiculoPESV`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, VehiculoPESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/seccion_vehiculos.html`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/actividades/nueva`
- **Nombre de Función (Backend Handler):** `pesv.nueva_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPESV, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, ActividadPESV.pesv → PESV, ActividadPESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/comite/integrante/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.agregar_integrante`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agregar_integrante' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IntegranteComitePESV, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente, IntegranteComitePESV.empleado → Empleado`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/conductores/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.nuevo_conductor`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/conductor_form.html`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/diagnostico/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.nuevo_diagnostico`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DiagnosticoPESV, DiagnosticoPESVItem, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente, DiagnosticoPESV.items → DiagnosticoPESVItem, DiagnosticoPESVItem.diagnostico → DiagnosticoPESV`
  - Plantillas HTML Parseadas: `pesv/diagnostico_form.html`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/preop/nueva`
- **Nombre de Función (Backend Handler):** `pesv.nueva_preop`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, InspeccionPreOperacional, PESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `VehiculoPESV, ConductorPESV`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, VehiculoPESV.cliente → Cliente, VehiculoPESV.conductores → ConductorPESV, ConductorPESV.cliente → Cliente, InspeccionPreOperacional.cliente → Cliente, InspeccionPreOperacional.vehiculo → VehiculoPESV, InspeccionPreOperacional.conductor → ConductorPESV`
    - Filtros Aplicados (WHERE): `cliente_id=p.cliente_id, activo=True, cliente_id=p.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `pesv/preop_form.html`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/siniestros/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.nuevo_siniestro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV, SiniestroPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, PESV.siniestros → SiniestroPESV, SiniestroPESV.cliente → Cliente, SiniestroPESV.pesv → PESV, SiniestroPESV.vehiculo → VehiculoPESV, SiniestroPESV.conductor → ConductorPESV`

#### 🔗 Ruta Servidor: `/pesv/<int:pesv_id>/vehiculos/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.nuevo_vehiculo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PESV, VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, VehiculoPESV.cliente → Cliente`
  - Plantillas HTML Parseadas: `pesv/vehiculo_form.html`

#### 🔗 Ruta Servidor: `/pesv/actividades/<int:act_id>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ActividadPESV.pesv → PESV`

#### 🔗 Ruta Servidor: `/pesv/actividades/<int:act_id>/estado`
- **Nombre de Función (Backend Handler):** `pesv.actualizar_estado_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'actualizar_estado_actividad' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ActividadPESV.pesv → PESV`

#### 🔗 Ruta Servidor: `/pesv/comite/integrante/<int:int_id>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar_integrante`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `IntegranteComitePESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, IntegranteComitePESV.pesv → PESV, IntegranteComitePESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/conductores/<int:cid_c>/editar`
- **Nombre de Función (Backend Handler):** `pesv.editar_conductor`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConductorPESV, Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PESV, Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, ConductorPESV.cliente → Cliente, ConductorPESV.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=c.cliente_id, cliente_id=c.cliente_id, activo=True, cliente_id=c.cliente_id`
  - Plantillas HTML Parseadas: `pesv/conductor_form.html`

#### 🔗 Ruta Servidor: `/pesv/diagnostico/<int:diag_id>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar_diagnostico`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DiagnosticoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/diagnostico/<int:diag_id>/pdf`
- **Nombre de Función (Backend Handler):** `pesv.pdf_diagnostico_pesv`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** T-09 · PDF Autoevaluación / Diagnóstico PESV | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'pdf_diagnostico_pesv' - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DiagnosticoPESV`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PESV.cliente → Cliente, DiagnosticoPESV.pesv → PESV, DiagnosticoPESV.cliente → Cliente`

#### 🔗 Ruta Servidor: `/pesv/nuevo`
- **Nombre de Función (Backend Handler):** `pesv.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, PESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PESV, Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, PESV.cliente → Cliente, PESV.vehiculos → VehiculoPESV, PESV.responsable_vial_emp → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=cid_ctx, activo=True`
  - Plantillas HTML Parseadas: `pesv/form.html, pesv/form.html`

#### 🔗 Ruta Servidor: `/pesv/siniestros/<int:sid>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar_siniestro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SiniestroPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `PESV.siniestros → SiniestroPESV, SiniestroPESV.pesv → PESV`

#### 🔗 Ruta Servidor: `/pesv/vehiculos/<int:vid>/editar`
- **Nombre de Función (Backend Handler):** `pesv.editar_vehiculo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `VehiculoPESV.pesv_rel → PESV`
  - Plantillas HTML Parseadas: `pesv/vehiculo_form.html`

#### 🔗 Ruta Servidor: `/pesv/vehiculos/<int:vid>/eliminar`
- **Nombre de Función (Backend Handler):** `pesv.eliminar_vehiculo`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Pesv**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `VehiculoPESV`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

---

### 📦 Blueprint / Módulo: `PLAN_TRABAJO`

#### 🔗 Ruta Servidor: `/plan-trabajo/`
- **Nombre de Función (Backend Handler):** `plan_trabajo.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PLANES — LISTADO Y CRUD | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, PlanAnualSST, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo, Usuario`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.cliente → Cliente, PlanAnualSST.centro_trabajo → CentroTrabajo, PlanAnualSST.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, PlanAnualSST.cliente_id == cid_ctx, PlanAnualSST.cliente_id.in_(cids... (+12 más)`
  - Plantillas HTML Parseadas: `plan_trabajo/lista.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>`
- **Nombre de Función (Backend Handler):** `plan_trabajo.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PlanAnualSST.actividades → ActividadPlan`
  - Plantillas HTML Parseadas: `plan_trabajo/detalle.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>/asignar-asesor`
- **Nombre de Función (Backend Handler):** `plan_trabajo.asignar_asesor`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'asignar_asesor' - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `PlanAnualSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `plan_trabajo.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo, PlanAnualSST`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.cliente → Cliente, PlanAnualSST.centro_trabajo → CentroTrabajo, PlanAnualSST.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `
                id=centro_trabajo_id,
                cliente_id=plan.cliente_id,
                activo=True,
            , 
            PlanAnualSST.id != plan.id,
            PlanAnualSST.cliente_id == plan.cliente_id,
            PlanAnualSST.anio == plan.anio,
        , PlanAnualSST.centro_trabajo_id == centro_trabajo_id... (+1 más)`
  - Plantillas HTML Parseadas: `plan_trabajo/form.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `plan_trabajo.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>/exportar-gantt`
- **Nombre de Función (Backend Handler):** `plan_trabajo.exportar_gantt`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PlanAnualSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:id>/gantt`
- **Nombre de Función (Backend Handler):** `plan_trabajo.gantt`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Vista Gantt del plan: filas = series de actividades, columnas = meses.
- **Comentarios Descriptivos:** GANTT — Cronograma visual del plan | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'gantt' - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `PlanAnualSST.actividades → ActividadPlan, ActividadPlan.plan → PlanAnualSST`
  - Plantillas HTML Parseadas: `plan_trabajo/gantt.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/<int:plan_id>/actividad/nueva`
- **Nombre de Función (Backend Handler):** `plan_trabajo.nueva_actividad`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan, PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PlanAnualSST.cliente → Cliente, PlanAnualSST.actividades → ActividadPlan, ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
  - Plantillas HTML Parseadas: `plan_trabajo/actividad_form.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/actividad/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `plan_trabajo.editar_actividad`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
  - Plantillas HTML Parseadas: `plan_trabajo/actividad_form.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/actividad/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `plan_trabajo.eliminar_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ActividadPlan.plan → PlanAnualSST`

#### 🔗 Ruta Servidor: `/plan-trabajo/actividad/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `plan_trabajo.toggle_actividad`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Marca/desmarca actividad como ejecutada (rápido desde la vista).
    Si la actividad es recurrente y se marca como ejecutada, genera automáticamente
    la próxima ocurrencia con estado 'pendiente'.
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ActividadPlan`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
                actividad_origen_id=actividad.id,
                estado='pendiente'
            `

#### 🔗 Ruta Servidor: `/plan-trabajo/centros`
- **Nombre de Función (Backend Handler):** `plan_trabajo.centros`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'centros' - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id,
            nombre=nombre,
        , CentroTrabajo.cliente_id == cliente_id, CentroTrabajo.cliente_id.in_(cids... (+1 más)`
  - Plantillas HTML Parseadas: `plan_trabajo/centros.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/centros/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `plan_trabajo.editar_centro`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `
        CentroTrabajo.id != centro.id,
        CentroTrabajo.cliente_id == centro.cliente_id,
        CentroTrabajo.nombre == nombre,
    `

#### 🔗 Ruta Servidor: `/plan-trabajo/consolidado`
- **Nombre de Función (Backend Handler):** `plan_trabajo.consolidado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'consolidado' - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, Cliente, InformeGestion, PlanAnualSST, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, CentroTrabajo, Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.cliente → Cliente, PlanAnualSST.centro_trabajo → CentroTrabajo, PlanAnualSST.asesor_asignado → Usuario, PlanAnualSST.coordinador_asignador → Usuario, PlanAnualSST.actividades → ActividadPlan, InformeGestion.cliente → Cliente, InformeGestion.generado_por → Usuario`
    - Filtros Aplicados (WHERE): `activo=True, 
                                 cliente_id=cliente_activo.id,
                                 modulo_key=modulo_key,
                                 periodo_hash=periodo_hash,
                                 anio=anio_sel,
                                 mes_ini=1,
                                 mes_fin=12,
                             , PlanAnualSST.cliente_id == cid_ctx... (+13 más)`
  - Plantillas HTML Parseadas: `plan_trabajo/consolidado.html`

#### 🔗 Ruta Servidor: `/plan-trabajo/nuevo`
- **Nombre de Función (Backend Handler):** `plan_trabajo.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Plan_Trabajo**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CentroTrabajo, PlanAnualSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CentroTrabajo, PlanAnualSST`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CentroTrabajo.cliente → Cliente, CentroTrabajo.asesor_asignado → Usuario, PlanAnualSST.cliente → Cliente, PlanAnualSST.centro_trabajo → CentroTrabajo, PlanAnualSST.asesor_asignado → Usuario`
    - Filtros Aplicados (WHERE): `
                id=centro_trabajo_id,
                cliente_id=cliente_id,
                activo=True,
            , 
            PlanAnualSST.cliente_id == cliente_id,
            PlanAnualSST.anio == anio,
        , PlanAnualSST.centro_trabajo_id == centro_trabajo_id... (+1 más)`
  - Plantillas HTML Parseadas: `plan_trabajo/form.html`

---

### 📦 Blueprint / Módulo: `PLATAFORMA`

#### 🔗 Ruta Servidor: `/`
- **Nombre de Función (Backend Handler):** `plataforma.dashboard`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'dashboard' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, Usuario`
    - Relaciones Navegadas (JOINS): `Cliente.usuarios → Usuario`
    - Filtros Aplicados (WHERE): `plataforma_id=plataforma.id, Cliente.es_plataforma == False, Cliente.es_plataforma == False, Cliente.activo == True`
  - Plantillas HTML Parseadas: `plataforma/dashboard.html`

#### 🔗 Ruta Servidor: `/configuracion`
- **Nombre de Función (Backend Handler):** `plataforma.configuracion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** Branding / Configuración de plataforma | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'configuracion' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
  - Plantillas HTML Parseadas: `plataforma/configuracion.html`

#### 🔗 Ruta Servidor: `/empresas`
- **Nombre de Función (Backend Handler):** `plataforma.lista_empresas`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Sub-empresas | ─────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Filtros Aplicados (WHERE): `padre_id=plataforma.id`
  - Plantillas HTML Parseadas: `plataforma/empresas_lista.html`

#### 🔗 Ruta Servidor: `/empresas/<int:empresa_id>/conteo-datos`
- **Nombre de Función (Backend Handler):** `plataforma.empresa_conteo_datos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Retorna JSON con resumen de datos de la empresa hija — para modal de confirmación.
- **Comentarios Descriptivos:** ─── ELIMINACIÓN DE SUB-EMPRESA ──────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'empresa_conteo_datos' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AuditoriaSST, AutoevaluacionSST, Cliente, DatoBaseIndicador, DocumentoSST, Empleado, ExamenMedico, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, PESV, PlanAnualSST, RegistroAusentismo, ReporteIncidente, TemaCapacitacion, Usuario, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario, Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AccionMejora, Empleado, Area, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, AuditoriaSST, DocumentoSST, AutoevaluacionSST, DatoBaseIndicador, PESV, ExamenMedico, RegistroAusentismo`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Cliente.usuarios → Usuario, Visita.cliente → Cliente, Empleado.cliente → Cliente, Area.cliente → Cliente, ReporteIncidente.cliente → Cliente, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, DocumentoSST.cliente → Cliente, AuditoriaSST.cliente → Cliente, PESV.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, ExamenMedico.cliente → Cliente, Incapacidad.cliente → Cliente, RegistroAusentismo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=empresa_id, cliente_id=empresa_id, cliente_id=empresa_id... (+17 más)`

#### 🔗 Ruta Servidor: `/empresas/<int:empresa_id>/editar`
- **Nombre de Función (Backend Handler):** `plataforma.editar_empresa`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/empresas/<int:empresa_id>/eliminar`
- **Nombre de Función (Backend Handler):** `plataforma.eliminar_empresa`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una sub-empresa y TODOS sus datos. Requiere confirmación con nombre exacto.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, Area, AsistenteCapacitacion, AuditoriaAcceso, AuditoriaSST, AutoevaluacionSST, BugReport, ChecklistDocumentosSST, Cliente, ClienteObjetivoVisita, ConductorPESV, ConfigNotificacion, DatoBaseIndicador, DescargaDocumento, DocumentoSST, Empleado, EvaluacionMedica, EvidenciaItemDiagnostico, ExamenMedico, HistorialNotificacion, IdeaRoadmap, Incapacidad, InspeccionBotiquin, InspeccionCamilla, InspeccionExtintor, InspeccionGeneral, MatrizRiesgo, ObjetivoSST, PESV, PerfilEmpresa, PlanAnualSST, PoliticaSST, RegistroActividad, RegistroAusentismo, ReintegroLaboral, ReporteIncidente, RequisitoEmpresa, RespuestaBateria, ResultadoPVE, SesionCapacitacion, TemaCapacitacion, TipoIncidente, Usuario, VehiculoPESV, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RequisitoEmpresa, RegistroAusentismo, PESV, VehiculoPESV, ConductorPESV, AuditoriaSST, AccionMejora, Visita, ReporteIncidente, InspeccionBotiquin, InspeccionExtintor, InspeccionCamilla, InspeccionGeneral, AsistenteCapacitacion, SesionCapacitacion, TemaCapacitacion, MatrizRiesgo, PlanAnualSST, DocumentoSST, ChecklistDocumentosSST, EvidenciaItemDiagnostico, AutoevaluacionSST, DatoBaseIndicador, PoliticaSST, ObjetivoSST, TipoIncidente, ConfigNotificacion, EvaluacionMedica, ReintegroLaboral, ExamenMedico, Incapacidad, ResultadoPVE, RespuestaBateria, HistorialNotificacion, Empleado, Area, PerfilEmpresa, ClienteObjetivoVisita, Usuario, DescargaDocumento, BugReport, IdeaRoadmap, RegistroActividad, AuditoriaAcceso`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(, db.session.delete(`
    - Relaciones Navegadas (JOINS): `ClienteObjetivoVisita.cliente → Cliente, Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente, Visita.cliente → Cliente, Visita.asesor → Usuario, Empleado.cliente → Cliente, Area.cliente → Cliente, TipoIncidente.cliente → Cliente, ReporteIncidente.cliente → Cliente, ReporteIncidente.reportado_por → Usuario, InspeccionBotiquin.cliente → Cliente, InspeccionExtintor.cliente → Cliente, InspeccionCamilla.cliente → Cliente, InspeccionGeneral.cliente → Cliente, AccionMejora.cliente → Cliente, TemaCapacitacion.cliente → Cliente, TemaCapacitacion.sesiones → SesionCapacitacion, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.asistentes → AsistenteCapacitacion, AsistenteCapacitacion.sesion → SesionCapacitacion, MatrizRiesgo.cliente → Cliente, PlanAnualSST.cliente → Cliente, ConfigNotificacion.cliente → Cliente, HistorialNotificacion.cliente → Cliente, DocumentoSST.cliente → Cliente, DescargaDocumento.usuario → Usuario, AuditoriaSST.cliente → Cliente, HallazgoAuditoria.auditoria → AuditoriaSST, HallazgoAuditoria.accion_mejora → AccionMejora, PESV.cliente → Cliente, VehiculoPESV.cliente → Cliente, ConductorPESV.cliente → Cliente, PerfilEmpresa.cliente → Cliente, AutoevaluacionSST.cliente → Cliente, EvidenciaItemDiagnostico.cliente → Cliente, EvidenciaItemDiagnostico.subido_por → Usuario, ChecklistDocumentosSST.cliente → Cliente, DatoBaseIndicador.cliente → Cliente, PoliticaSST.cliente → Cliente, ObjetivoSST.cliente → Cliente, RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.accion_mejora → AccionMejora, BugReport.usuario → Usuario, IdeaRoadmap.usuario → Usuario, ExamenMedico.cliente → Cliente, Incapacidad.cliente → Cliente, ReintegroLaboral.cliente → Cliente, ReintegroLaboral.incapacidad → Incapacidad, RegistroAusentismo.cliente → Cliente, RegistroAusentismo.incapacidad → Incapacidad, AuditoriaAcceso.usuario → Usuario, AuditoriaAcceso.cliente → Cliente, EvaluacionMedica.cliente → Cliente, ResultadoPVE.cliente → Cliente, RespuestaBateria.cliente → Cliente, RegistroActividad.usuario → Usuario, RegistroActividad.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=eid, cliente_id=eid, cliente_id=eid... (+48 más)`

#### 🔗 Ruta Servidor: `/empresas/<int:empresa_id>/toggle`
- **Nombre de Función (Backend Handler):** `plataforma.toggle_empresa`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/empresas/crear`
- **Nombre de Función (Backend Handler):** `plataforma.crear_empresa`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_empresa' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`

#### 🔗 Ruta Servidor: `/empresas/importar`
- **Nombre de Función (Backend Handler):** `plataforma.importar_empresas`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Importación masiva de sub-empresas desde archivo Excel o CSV.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_empresas' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PerfilEmpresa.cliente → Cliente`
    - Filtros Aplicados (WHERE): `padre_id=plataforma.id`
  - Plantillas HTML Parseadas: `plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html, plataforma/importar_empresas.html`

#### 🔗 Ruta Servidor: `/empresas/plantilla`
- **Nombre de Función (Backend Handler):** `plataforma.descargar_plantilla_importacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y devuelve un archivo Excel con la plantilla para importar empresas.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_plantilla_importacion' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/usuarios`
- **Nombre de Función (Backend Handler):** `plataforma.lista_usuarios`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Usuarios de la plataforma | ───────────────────────────────────────────── | Usuarios admin-plataforma + usuarios de empresas hijas
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            db.or_(
                Usuario.plataforma_id == plataforma.id,
                Usuario.cliente_id.in_(ids_hijas`
  - Plantillas HTML Parseadas: `plataforma/usuarios_lista.html`

#### 🔗 Ruta Servidor: `/usuarios/<int:usuario_id>/editar`
- **Nombre de Función (Backend Handler):** `plataforma.editar_usuario`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            Usuario.email == email, Usuario.id != usuario_id
        `

#### 🔗 Ruta Servidor: `/usuarios/<int:usuario_id>/reset-password`
- **Nombre de Función (Backend Handler):** `plataforma.reset_password_usuario`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'reset_password_usuario' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/usuarios/crear`
- **Nombre de Función (Backend Handler):** `plataforma.crear_usuario`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_usuario' - asociado al sistema maestro de **Plataforma**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente`
    - Filtros Aplicados (WHERE): `email=email`

---

### 📦 Blueprint / Módulo: `PORTAL_PROVEEDORES`

---

### 📦 Blueprint / Módulo: `PRIVACIDAD`

#### 🔗 Ruta Servidor: `/privacidad/`
- **Nombre de Función (Backend Handler):** `privacidad.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── LISTA ─────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, RegistroTratamiento`
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid`
  - Plantillas HTML Parseadas: `privacidad/lista.html`

#### 🔗 Ruta Servidor: `/privacidad/<int:id>`
- **Nombre de Función (Backend Handler):** `privacidad.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── DETALLE RegistroTratamiento ───────────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroTratamiento`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `privacidad/detalle.html`

#### 🔗 Ruta Servidor: `/privacidad/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `privacidad.editar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── EDITAR / GUARDAR ──────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente`
  - Plantillas HTML Parseadas: `privacidad/form.html`

#### 🔗 Ruta Servidor: `/privacidad/<int:id>/guardar`
- **Nombre de Función (Backend Handler):** `privacidad.guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'guardar' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/privacidad/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `privacidad.toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ── TOGGLE ACTIVO ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/privacidad/arcop`
- **Nombre de Función (Backend Handler):** `privacidad.arcop_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, SolicitudARCOP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `SolicitudARCOP.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=current_user.cliente_id, cliente_id=cid, estado=estado_f... (+1 más)`
  - Plantillas HTML Parseadas: `privacidad/arcop_lista.html`

#### 🔗 Ruta Servidor: `/privacidad/arcop/<int:id>`
- **Nombre de Función (Backend Handler):** `privacidad.arcop_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudARCOP`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SolicitudARCOP.cliente → Cliente`
  - Plantillas HTML Parseadas: `privacidad/arcop_detalle.html`

#### 🔗 Ruta Servidor: `/privacidad/arcop/<int:id>/responder`
- **Nombre de Función (Backend Handler):** `privacidad.arcop_responder`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'arcop_responder' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudARCOP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudARCOP.atendido_por → Usuario`

#### 🔗 Ruta Servidor: `/privacidad/arcop/crear`
- **Nombre de Función (Backend Handler):** `privacidad.arcop_crear`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'arcop_crear' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudARCOP`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudARCOP.cliente → Cliente, SolicitudARCOP.empleado → Empleado, SolicitudARCOP.creado_por → Usuario`

#### 🔗 Ruta Servidor: `/privacidad/arcop/nueva`
- **Nombre de Función (Backend Handler):** `privacidad.arcop_nuevo`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `SolicitudARCOP.cliente → Cliente, SolicitudARCOP.empleado → Empleado`
  - Plantillas HTML Parseadas: `privacidad/arcop_form.html`

#### 🔗 Ruta Servidor: `/privacidad/auditoria`
- **Nombre de Función (Backend Handler):** `privacidad.auditoria`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── AUDITORÍA DE ACCESOS ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'auditoria' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaAcceso, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Relaciones Navegadas (JOINS): `AuditoriaAcceso.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=current_user.cliente_id, cliente_id=filtro_cliente_id, activo=True... (+1 más)`
  - Plantillas HTML Parseadas: `privacidad/auditoria.html`

#### 🔗 Ruta Servidor: `/privacidad/crear`
- **Nombre de Función (Backend Handler):** `privacidad.crear`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** El cliente_id viene del form (asesor elige empresa) o del contexto
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente, RegistroTratamiento.creado_por → Usuario`

#### 🔗 Ruta Servidor: `/privacidad/exportar-pdf`
- **Nombre de Función (Backend Handler):** `privacidad.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── EXPORTAR PDF — POLÍTICA DE PRIVACIDAD ────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, RegistroTratamiento`
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`

#### 🔗 Ruta Servidor: `/privacidad/nuevo`
- **Nombre de Función (Backend Handler):** `privacidad.nuevo`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── NUEVO / CREAR ─────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente`
  - Plantillas HTML Parseadas: `privacidad/form.html`

#### 🔗 Ruta Servidor: `/privacidad/retencion`
- **Nombre de Función (Backend Handler):** `privacidad.retencion_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** RETENCIÓN Y SUPRESIÓN — vinculada a TRD / RegistroTratamiento (MOD-35/MOD-16) | ══════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, RegistroTratamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, RegistroTratamiento`
    - Relaciones Navegadas (JOINS): `RegistroTratamiento.cliente → Cliente, HistorialRetencion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `privacidad/retencion_lista.html`

#### 🔗 Ruta Servidor: `/privacidad/retencion/registrar`
- **Nombre de Función (Backend Handler):** `privacidad.retencion_registrar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'retencion_registrar' - asociado al sistema maestro de **Privacidad**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialRetencion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `HistorialRetencion.cliente → Cliente, HistorialRetencion.registro_tratamiento → RegistroTratamiento, HistorialRetencion.documento_sst → DocumentoSST, HistorialRetencion.responsable → Usuario`

---

### 📦 Blueprint / Módulo: `PROVEEDORES`

#### 🔗 Ruta Servidor: `/proveedores/`
- **Nombre de Función (Backend Handler):** `proveedores.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | LISTADO DE PROVEEDORES | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Proveedor`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id, activo=True
        , activo=True, Proveedor.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `proveedores/lista.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>`
- **Nombre de Función (Backend Handler):** `proveedores.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | DETALLE DE PROVEEDOR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocEmpleadoProveedor, ExigenciaDocProveedor, Proveedor, SoporteProveedor, TokenPortalProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ExigenciaDocProveedor`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocProveedor.cliente → Cliente, SoporteProveedor.proveedor → Proveedor, SoporteProveedor.exigencia → ExigenciaDocProveedor, AutorizacionProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente, TokenPortalProveedor.proveedor → Proveedor, TokenPortalProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=proveedor.cliente_id, activo=True
    , proveedor_id=proveedor.id, exigencia_id=ex.id, activo=True... (+3 más)`
  - Plantillas HTML Parseadas: `proveedores/detalle.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/autorizaciones/nueva`
- **Nombre de Función (Backend Handler):** `proveedores.nueva_autorizacion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** AUTORIZACIONES — NUEVA | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutorizacionProveedor, EmpleadoProveedor, ExigenciaDocEmpleadoProveedor, ExigenciaDocProveedor, Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EmpleadoProveedor`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocProveedor.cliente → Cliente, AutorizacionProveedor.proveedor → Proveedor, AutorizacionProveedor.personal → EmpleadoProveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=proveedor.cliente_id, activo=True, activo=True, cliente_id=proveedor.cliente_id, activo=True... (+2 más)`
  - Plantillas HTML Parseadas: `proveedores/form_autorizacion.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `proveedores.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** EDITAR PROVEEDOR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente`
  - Plantillas HTML Parseadas: `proveedores/form_proveedor.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `proveedores.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** ELIMINAR PROVEEDOR (soft delete) | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/empleados/<int:eid>/toggle`
- **Nombre de Función (Backend Handler):** `proveedores.toggle_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmpleadoProveedor, Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `EmpleadoProveedor.proveedor → Proveedor`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/empleados/nuevo`
- **Nombre de Función (Backend Handler):** `proveedores.nuevo_empleado`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** EMPLEADOS DEL PROVEEDOR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmpleadoProveedor, Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `EmpleadoProveedor.proveedor → Proveedor`
  - Plantillas HTML Parseadas: `proveedores/form_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/soportes/subir/<int:exigencia_id>`
- **Nombre de Función (Backend Handler):** `proveedores.subir_soporte`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | SOPORTES — SUBIR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'subir_soporte' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor, Proveedor, SoporteProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SoporteProveedor.proveedor → Proveedor, SoporteProveedor.exigencia → ExigenciaDocProveedor`
  - Plantillas HTML Parseadas: `proveedores/form_soporte.html`

#### 🔗 Ruta Servidor: `/proveedores/<int:id>/tokens/generar`
- **Nombre de Función (Backend Handler):** `proveedores.generar_token_portal`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Genera un nuevo token de acceso al portal de autogestión del proveedor.
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_token_portal' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Proveedor, TokenPortalProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, TokenPortalProveedor.proveedor → Proveedor, TokenPortalProveedor.cliente → Cliente`

#### 🔗 Ruta Servidor: `/proveedores/autorizaciones/<int:id>`
- **Nombre de Función (Backend Handler):** `proveedores.detalle_autorizacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | AUTORIZACIONES — DETALLE | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutorizacionProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutorizacionProveedor.proveedor → Proveedor`
  - Plantillas HTML Parseadas: `proveedores/detalle_autorizacion.html`

#### 🔗 Ruta Servidor: `/proveedores/autorizaciones/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `proveedores.exportar_pdf_autorizacion`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | AUTORIZACIONES — EXPORTAR PDF | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AutorizacionProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `AutorizacionProveedor.proveedor → Proveedor`

#### 🔗 Ruta Servidor: `/proveedores/empleados-soportes/<int:id>/descargar`
- **Nombre de Función (Backend Handler):** `proveedores.descargar_soporte_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_soporte_empleado' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SoporteEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `EmpleadoProveedor.proveedor → Proveedor, SoporteEmpleadoProveedor.empleado → EmpleadoProveedor`

#### 🔗 Ruta Servidor: `/proveedores/empleados-soportes/<int:id>/revisar`
- **Nombre de Función (Backend Handler):** `proveedores.revisar_soporte_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'revisar_soporte_empleado' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SoporteEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `EmpleadoProveedor.proveedor → Proveedor, SoporteEmpleadoProveedor.empleado → EmpleadoProveedor`

#### 🔗 Ruta Servidor: `/proveedores/empleados/<int:eid>`
- **Nombre de Función (Backend Handler):** `proveedores.detalle_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** DETALLE DE EMPLEADO (documentos) | ═══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmpleadoProveedor, ExigenciaDocEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=empleado.proveedor.cliente_id, activo=True`
  - Plantillas HTML Parseadas: `proveedores/detalle_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/empleados/<int:eid>/soportes/subir/<int:exigencia_id>`
- **Nombre de Función (Backend Handler):** `proveedores.subir_soporte_empleado`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ═══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'subir_soporte_empleado' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmpleadoProveedor, ExigenciaDocEmpleadoProveedor, SoporteEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `EmpleadoProveedor.proveedor → Proveedor, SoporteEmpleadoProveedor.empleado → EmpleadoProveedor, SoporteEmpleadoProveedor.exigencia → ExigenciaDocEmpleadoProveedor`
  - Plantillas HTML Parseadas: `proveedores/form_soporte_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias`
- **Nombre de Función (Backend Handler):** `proveedores.exigencias`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | CRUD DE EXIGENCIAS DOCUMENTALES | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'exigencias' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `proveedores/exigencias.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias-empleado`
- **Nombre de Función (Backend Handler):** `proveedores.exigencias_empleado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXIGENCIAS DOCUMENTALES DE EMPLEADOS — CRUD | ═══════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'exigencias_empleado' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `proveedores/exigencias_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias-empleado/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `proveedores.editar_exigencia_empleado`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`
  - Plantillas HTML Parseadas: `proveedores/form_exigencia_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias-empleado/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `proveedores.eliminar_exigencia_empleado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`

#### 🔗 Ruta Servidor: `/proveedores/exigencias-empleado/nueva`
- **Nombre de Función (Backend Handler):** `proveedores.nueva_exigencia_empleado`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocEmpleadoProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Proveedor.cliente → Cliente, EmpleadoProveedor.proveedor → Proveedor, ExigenciaDocEmpleadoProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, _C.id.in_(cids`
  - Plantillas HTML Parseadas: `proveedores/form_exigencia_empleado.html, proveedores/form_exigencia_empleado.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `proveedores.editar_exigencia`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`
  - Plantillas HTML Parseadas: `proveedores/form_exigencia.html`

#### 🔗 Ruta Servidor: `/proveedores/exigencias/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `proveedores.eliminar_exigencia`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`

#### 🔗 Ruta Servidor: `/proveedores/exigencias/nueva`
- **Nombre de Función (Backend Handler):** `proveedores.nueva_exigencia`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, _C.id.in_(cids`
  - Plantillas HTML Parseadas: `proveedores/form_exigencia.html, proveedores/form_exigencia.html`

#### 🔗 Ruta Servidor: `/proveedores/nuevo`
- **Nombre de Función (Backend Handler):** `proveedores.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** CREAR PROVEEDOR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Proveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, _C.id.in_(cids`
  - Plantillas HTML Parseadas: `proveedores/form_proveedor.html, proveedores/form_proveedor.html`

#### 🔗 Ruta Servidor: `/proveedores/panel`
- **Nombre de Función (Backend Handler):** `proveedores.panel_cumplimiento`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | PANEL DE CUMPLIMIENTO | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'panel_cumplimiento' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ExigenciaDocProveedor, Proveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True, cliente_id=cliente_id, activo=True`
  - Plantillas HTML Parseadas: `proveedores/panel_cumplimiento.html`

#### 🔗 Ruta Servidor: `/proveedores/panel/exportar-excel`
- **Nombre de Función (Backend Handler):** `proveedores.panel_exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | PANEL — EXPORTAR EXCEL | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, ExigenciaDocProveedor, Proveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, ExigenciaDocProveedor.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True, cliente_id=cliente_id, activo=True`

#### 🔗 Ruta Servidor: `/proveedores/soportes/<int:id>/descargar`
- **Nombre de Función (Backend Handler):** `proveedores.descargar_soporte`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ───────────────────────────────────────── | SOPORTES — DESCARGAR | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'descargar_soporte' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SoporteProveedor`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SoporteProveedor.proveedor → Proveedor`

#### 🔗 Ruta Servidor: `/proveedores/soportes/<int:id>/revisar`
- **Nombre de Función (Backend Handler):** `proveedores.revisar_soporte`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** SOPORTES — REVISAR (asesor) | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'revisar_soporte' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SoporteProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SoporteProveedor.proveedor → Proveedor`

#### 🔗 Ruta Servidor: `/proveedores/tokens/<int:tid>/desactivar`
- **Nombre de Función (Backend Handler):** `proveedores.desactivar_token_portal`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Desactiva (revoca) un token de portal.
- **Comentarios Descriptivos:** Verificar que el token pertenece a un proveedor del cliente en contexto
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'desactivar_token_portal' - asociado al sistema maestro de **Proveedores**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TokenPortalProveedor`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Proveedor.cliente → Cliente, TokenPortalProveedor.proveedor → Proveedor, TokenPortalProveedor.cliente → Cliente`

---

### 📦 Blueprint / Módulo: `REHABILITACION`

#### 🔗 Ruta Servidor: `/rehabilitacion/`
- **Nombre de Función (Backend Handler):** `rehabilitacion.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** INDEX — listado + dashboard | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CasoRehabilitacion, Cliente`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True, estado=f_estado, origen=f_origen... (+4 más)`
  - Plantillas HTML Parseadas: `rehabilitacion/index.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>`
- **Nombre de Función (Backend Handler):** `rehabilitacion.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.recomendaciones → RecomendacionMedReh, CasoRehabilitacion.planes → PlanAccionReh, CasoRehabilitacion.seguimientos → SeguimientoReh`
  - Plantillas HTML Parseadas: `rehabilitacion/detalle.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/cambiar-estado`
- **Nombre de Función (Backend Handler):** `rehabilitacion.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** CAMBIO DE ESTADO | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/cerrar`
- **Nombre de Función (Backend Handler):** `rehabilitacion.cerrar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Comentarios Descriptivos:** CIERRE DEL CASO | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cerrar' - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.recomendaciones → RecomendacionMedReh, CasoRehabilitacion.planes → PlanAccionReh, CasoRehabilitacion.seguimientos → SeguimientoReh`
    - Filtros Aplicados (WHERE): `estado_plan='activo'`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `rehabilitacion.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** EDITAR CASO | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `rehabilitacion/editar.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `rehabilitacion.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR PDF | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/plan/<int:pid>/editar`
- **Nombre de Función (Backend Handler):** `rehabilitacion.editar_plan`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, PlanAccionReh`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `rehabilitacion/plan_form.html, rehabilitacion/plan_form.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/plan/nuevo`
- **Nombre de Función (Backend Handler):** `rehabilitacion.nuevo_plan`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** PLANES DE ACCIÓN | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, PlanAccionReh`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.planes → PlanAccionReh`
  - Plantillas HTML Parseadas: `rehabilitacion/plan_form.html, rehabilitacion/plan_form.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/recomendacion/<int:rid>/editar`
- **Nombre de Función (Backend Handler):** `rehabilitacion.editar_recomendacion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, RecomendacionMedReh`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `rehabilitacion/recomendacion_form.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/recomendacion/nueva`
- **Nombre de Función (Backend Handler):** `rehabilitacion.nueva_recomendacion`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, RecomendacionMedReh`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.recomendaciones → RecomendacionMedReh`
  - Plantillas HTML Parseadas: `rehabilitacion/recomendacion_form.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/seguimiento/nuevo`
- **Nombre de Función (Backend Handler):** `rehabilitacion.nuevo_seguimiento`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** SEGUIMIENTOS | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, SeguimientoReh`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.seguimientos → SeguimientoReh`
  - Plantillas HTML Parseadas: `rehabilitacion/seguimiento_form.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/<int:id>/vista-jefe`
- **Nombre de Función (Backend Handler):** `rehabilitacion.vista_jefe`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** VISTA RESTRINGIDA — JEFE DE ÁREA | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'vista_jefe' - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion, RecomendacionMedReh`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.recomendaciones → RecomendacionMedReh`
    - Filtros Aplicados (WHERE): `caso_id=caso.id, 
            db.or_(
                RecomendacionMedReh.fecha_vencimiento == None,
                RecomendacionMedReh.fecha_vencimiento >= hoy,
            `
  - Plantillas HTML Parseadas: `rehabilitacion/vista_jefe.html`

#### 🔗 Ruta Servidor: `/rehabilitacion/exportar-excel`
- **Nombre de Función (Backend Handler):** `rehabilitacion.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EXPORTAR EXCEL | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/rehabilitacion/nuevo`
- **Nombre de Función (Backend Handler):** `rehabilitacion.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Rehabilitacion**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CasoRehabilitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CasoRehabilitacion.cliente → Cliente, CasoRehabilitacion.empleado → Empleado, CasoRehabilitacion.incapacidad → Incapacidad`
  - Plantillas HTML Parseadas: `rehabilitacion/nuevo.html, rehabilitacion/nuevo.html`

---

### 📦 Blueprint / Módulo: `REPORTES`

#### 🔗 Ruta Servidor: `/reportes/`
- **Nombre de Función (Backend Handler):** `reportes.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ReporteIncidente`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente`
    - Filtros Aplicados (WHERE): `ReporteIncidente.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `reportes/lista.html`

#### 🔗 Ruta Servidor: `/reportes/<int:id>`
- **Nombre de Función (Backend Handler):** `reportes.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.historial_estados → ReporteEstadoHistorial`
  - Plantillas HTML Parseadas: `reportes/detalle.html`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/crear-accion`
- **Nombre de Función (Backend Handler):** `reportes.crear_accion`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Crea una AccionMejora correctiva vinculada a este reporte de incidente. [asesor_required]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'crear_accion' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AccionMejora, ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AccionMejora`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, AccionMejora.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=reporte.cliente_id`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/encuesta`
- **Nombre de Función (Backend Handler):** `reportes.encuesta`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ── Encuesta de Investigación (FT-SST-059) ───────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'encuesta' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EncuestaInvestigacion, ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.encuesta → EncuestaInvestigacion, EncuestaInvestigacion.reporte → ReporteIncidente, EncuestaInvestigacion.cliente → Cliente`
  - Plantillas HTML Parseadas: `reportes/encuesta_form.html`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/encuesta/exportar`
- **Nombre de Función (Backend Handler):** `reportes.encuesta_exportar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.encuesta → EncuestaInvestigacion`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/estado`
- **Nombre de Función (Backend Handler):** `reportes.cambiar_estado`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'cambiar_estado' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp, ReporteEstadoHistorial, ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteEstadoHistorial.reporte → ReporteIncidente, ReporteEstadoHistorial.usuario → Usuario, ReporteEstadoHistorial.cliente → Cliente`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `reportes.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/furat`
- **Nombre de Función (Backend Handler):** `reportes.furat`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Diligenciar o editar el FURAT de un reporte. [asesor_required para escritura]
- **Comentarios Descriptivos:** ── FURAT — Formulario Único de Reporte de AT (Res. 156/2005) ────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'furat' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FuratDetalle, PerfilEmpresa, ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PerfilEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, Empleado.area → Area, ReporteIncidente.cliente → Cliente, ReporteIncidente.area → Area, ReporteIncidente.furat → FuratDetalle, FuratDetalle.reporte → ReporteIncidente, FuratDetalle.cliente → Cliente, PerfilEmpresa.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=reporte.cliente_id`
  - Plantillas HTML Parseadas: `reportes/furat_form.html`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/furat/pdf`
- **Nombre de Función (Backend Handler):** `reportes.furat_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exportar el FURAT en PDF. [todos con acceso al reporte]
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'furat_pdf' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.furat → FuratDetalle`

#### 🔗 Ruta Servidor: `/reportes/<int:id>/pdf`
- **Nombre de Función (Backend Handler):** `reportes.generar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_pdf' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/reportes/api/cie10`
- **Nombre de Función (Backend Handler):** `reportes.api_cie10`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cie10Diagnostico`
  - **Flujos de Datos Detectados:**
    - Filtros Aplicados (WHERE): `Cie10Diagnostico.activo == True,
                    db.or_(Cie10Diagnostico.codigo.ilike(f'{q}%'`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/reportes/api/sugerir-agente`
- **Nombre de Función (Backend Handler):** `reportes.api_sugerir_agente`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/reportes/enfermedades-laborales`
- **Nombre de Función (Backend Handler):** `reportes.enfermedades_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EnfermedadLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EnfermedadLaboral`
    - Relaciones Navegadas (JOINS): `EnfermedadLaboral.cliente → Cliente, EnfermedadLaboral.reporte → ReporteIncidente`
    - Filtros Aplicados (WHERE): `EnfermedadLaboral.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `reportes/enfermedades_lista.html`

#### 🔗 Ruta Servidor: `/reportes/enfermedades-laborales/<int:id>`
- **Nombre de Función (Backend Handler):** `reportes.enfermedad_detalle`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Cliente, EnfermedadLaboral`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Cliente.reportes → ReporteIncidente, Cliente.empleados → Empleado, EnfermedadLaboral.cliente → Cliente, EnfermedadLaboral.empleado → Empleado, EnfermedadLaboral.reporte → ReporteIncidente`
  - Plantillas HTML Parseadas: `reportes/enfermedad_form.html`

#### 🔗 Ruta Servidor: `/reportes/enfermedades-laborales/nuevo`
- **Nombre de Función (Backend Handler):** `reportes.enfermedad_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, EnfermedadLaboral, ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ReporteIncidente, Empleado, EnfermedadLaboral`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, ReporteIncidente.cliente → Cliente, EnfermedadLaboral.cliente → Cliente, EnfermedadLaboral.empleado → Empleado, EnfermedadLaboral.reporte → ReporteIncidente`
    - Filtros Aplicados (WHERE): `id=empleado_id, cliente_id=cliente_id, id=reporte_id, cliente_id=cliente_id, reporte_id=reporte_id`
  - Plantillas HTML Parseadas: `reportes/enfermedad_form.html`

#### 🔗 Ruta Servidor: `/reportes/nuevo`
- **Nombre de Función (Backend Handler):** `reportes.nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Reportes**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ReporteIncidente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ReporteIncidente.cliente → Cliente, ReporteIncidente.reportado_por → Usuario, ReporteIncidente.area → Area`
  - Plantillas HTML Parseadas: `reportes/form.html, reportes/form.html`

---

### 📦 Blueprint / Módulo: `RIESGOS`

#### 🔗 Ruta Servidor: `/riesgos/`
- **Nombre de Función (Backend Handler):** `riesgos.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** MATRICES — LISTADO Y CRUD | ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `MatrizRiesgo`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid_ctx, 
            MatrizRiesgo.cliente_id.in_(cids`
  - Plantillas HTML Parseadas: `riesgos/lista.html`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>`
- **Nombre de Función (Backend Handler):** `riesgos.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`
  - Plantillas HTML Parseadas: `riesgos/detalle.html`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `riesgos.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`
  - Plantillas HTML Parseadas: `riesgos/form.html`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `riesgos.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `riesgos.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Exporta la matriz de riesgos a Excel con logo del cliente. [asesor + cliente]
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>/importar-excel`
- **Nombre de Función (Backend Handler):** `riesgos.importar_excel`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Importación de peligros desde Excel con validación previa.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'importar_excel' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FilaMatrizRiesgo, MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, MatrizRiesgo.filas → FilaMatrizRiesgo, FilaMatrizRiesgo.matriz → MatrizRiesgo`
  - Plantillas HTML Parseadas: `riesgos/importar_excel.html, riesgos/importar_excel.html, riesgos/importar_excel.html, riesgos/importar_excel.html`

#### 🔗 Ruta Servidor: `/riesgos/<int:id>/plantilla-importar`
- **Nombre de Función (Backend Handler):** `riesgos.plantilla_importar`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga la plantilla Excel para importar peligros.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'plantilla_importar' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente`

#### 🔗 Ruta Servidor: `/riesgos/<int:matriz_id>/fila/nueva`
- **Nombre de Función (Backend Handler):** `riesgos.nueva_fila`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ──────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FilaMatrizRiesgo, MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, FilaMatrizRiesgo.matriz → MatrizRiesgo`
  - Plantillas HTML Parseadas: `riesgos/fila_form.html`

#### 🔗 Ruta Servidor: `/riesgos/categorias-peligro`
- **Nombre de Función (Backend Handler):** `riesgos.categorias_peligro`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista y gestión de categorías de peligro personalizables por empresa.
- **Comentarios Descriptivos:** ── GESTIÓN DE CATEGORÍAS DE PELIGRO ─────────────────────────────────────── | Asesores/superadmin pueden gestionar categorías de cualquier empresa
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'categorias_peligro' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CategoriaPeligro, Cliente`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, CategoriaPeligro`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, CategoriaPeligro.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, cliente_id=cid`
  - Plantillas HTML Parseadas: `riesgos/categorias_peligro.html`

#### 🔗 Ruta Servidor: `/riesgos/categorias-peligro/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `riesgos.categorias_peligro_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Elimina una categoría personalizada (no se puede eliminar las de defecto).
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CategoriaPeligro`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `CategoriaPeligro.cliente → Cliente`

#### 🔗 Ruta Servidor: `/riesgos/categorias-peligro/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `riesgos.categorias_peligro_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Activa o desactiva una categoría de peligro.
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CategoriaPeligro`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `CategoriaPeligro.cliente → Cliente`

#### 🔗 Ruta Servidor: `/riesgos/categorias-peligro/nueva`
- **Nombre de Función (Backend Handler):** `riesgos.categorias_peligro_nueva`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Agrega una categoría personalizada de peligro a la empresa.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CategoriaPeligro`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CategoriaPeligro`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `CategoriaPeligro.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, valor=valor, cliente_id=cid`

#### 🔗 Ruta Servidor: `/riesgos/fila/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `riesgos.editar_fila`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FilaMatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, FilaMatrizRiesgo.matriz → MatrizRiesgo`
  - Plantillas HTML Parseadas: `riesgos/fila_form.html`

#### 🔗 Ruta Servidor: `/riesgos/fila/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `riesgos.eliminar_fila`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FilaMatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, FilaMatrizRiesgo.matriz → MatrizRiesgo`

#### 🔗 Ruta Servidor: `/riesgos/nueva`
- **Nombre de Función (Backend Handler):** `riesgos.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Riesgos**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FilaMatrizRiesgo, MatrizRiesgo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, MatrizRiesgo.filas → FilaMatrizRiesgo, FilaMatrizRiesgo.matriz → MatrizRiesgo`
  - Plantillas HTML Parseadas: `riesgos/form.html`

---

### 📦 Blueprint / Módulo: `RLCPD`

#### 🔗 Ruta Servidor: `/rlcpd/`
- **Nombre de Función (Backend Handler):** `rlcpd.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista de solicitudes RLCPD del cliente activo.
- **Comentarios Descriptivos:** RUTAS PRINCIPALES | ═════════════════════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `SolicitudRLCPD`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, estado=estado`
  - Plantillas HTML Parseadas: `rlcpd/lista.html`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>`
- **Nombre de Función (Backend Handler):** `rlcpd.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Detalle completo de una solicitud RLCPD.
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CalificadorCIF, EvaluadorRLCPD, HistorialEstadoRLCPD, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `HistorialEstadoRLCPD, CalificadorCIF, EvaluadorRLCPD`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, SolicitudRLCPD.evaluadores → EvaluadorRLCPD, SolicitudRLCPD.calificadores → CalificadorCIF, SolicitudRLCPD.historial → HistorialEstadoRLCPD, EvaluadorRLCPD.solicitud → SolicitudRLCPD, CalificadorCIF.solicitud → SolicitudRLCPD, HistorialEstadoRLCPD.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `
        solicitud_id=id
    , solicitud_id=id, solicitud_id=id`
  - Plantillas HTML Parseadas: `rlcpd/detalle.html`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/cif`
- **Nombre de Función (Backend Handler):** `rlcpd.gestionar_cif`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Agregar o editar calificadores CIF de una solicitud.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'gestionar_cif' - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CalificadorCIF, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CalificadorCIF`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, SolicitudRLCPD.calificadores → CalificadorCIF, CalificadorCIF.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `solicitud_id=id, solicitud_id=id`
  - Plantillas HTML Parseadas: `rlcpd/cif.html`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/cif/<int:cif_id>/eliminar`
- **Nombre de Función (Backend Handler):** `rlcpd.eliminar_cif`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Eliminar un calificador CIF.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CalificadorCIF, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CalificadorCIF`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, CalificadorCIF.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `id=cif_id, solicitud_id=id, solicitud_id=id`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `rlcpd.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Editar una solicitud RLCPD existente.
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialEstadoRLCPD, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, HistorialEstadoRLCPD.solicitud → SolicitudRLCPD, HistorialEstadoRLCPD.usuario → Usuario`
  - Plantillas HTML Parseadas: `rlcpd/editar.html`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/evaluadores`
- **Nombre de Función (Backend Handler):** `rlcpd.gestionar_evaluadores`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Agregar o listar evaluadores de una solicitud.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'gestionar_evaluadores' - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvaluadorRLCPD, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EvaluadorRLCPD`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, SolicitudRLCPD.evaluadores → EvaluadorRLCPD, EvaluadorRLCPD.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `solicitud_id=id`
  - Plantillas HTML Parseadas: `rlcpd/evaluadores.html`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/evaluadores/<int:ev_id>/eliminar`
- **Nombre de Función (Backend Handler):** `rlcpd.eliminar_evaluador`
- **Transacciones HTTP Aceptadas:** `POST`
- **Documentación Funcional Original:** Eliminar un evaluador de la solicitud.
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EvaluadorRLCPD, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EvaluadorRLCPD`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, EvaluadorRLCPD.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `id=ev_id, solicitud_id=id`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/exportar-excel`
- **Nombre de Función (Backend Handler):** `rlcpd.exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga el reporte RLCPD en Excel.
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `rlcpd.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera y descarga el certificado RLCPD en PDF.
- **Comentarios Descriptivos:** ─── Exportación PDF ─────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente`

#### 🔗 Ruta Servidor: `/rlcpd/<int:id>/exportar-sispro`
- **Nombre de Función (Backend Handler):** `rlcpd.exportar_sispro`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Genera el archivo JSON de SISPRO para la solicitud RLCPD.
- **Comentarios Descriptivos:** ─── Exportación SISPRO (JSON) ────────────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `CalificadorCIF, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `CalificadorCIF`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `SolicitudRLCPD.cliente → Cliente, SolicitudRLCPD.paciente → PacienteRLCPD, SolicitudRLCPD.calificadores → CalificadorCIF, CalificadorCIF.solicitud → SolicitudRLCPD`
    - Filtros Aplicados (WHERE): `solicitud_id=id`

#### 🔗 Ruta Servidor: `/rlcpd/api/buscar-cif`
- **Nombre de Función (Backend Handler):** `rlcpd.api_buscar_cif`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** AJAX: devuelve lista de códigos CIF que coinciden con el término de búsqueda.
- **Comentarios Descriptivos:** ─── API AJAX — búsqueda de códigos CIF ──────────────────────────────────────
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/rlcpd/nueva`
- **Nombre de Función (Backend Handler):** `rlcpd.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Documentación Funcional Original:** Crear nueva solicitud RLCPD (con o sin paciente existente).
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `HistorialEstadoRLCPD, PacienteRLCPD, SolicitudRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PacienteRLCPD`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PacienteRLCPD.cliente → Cliente, SolicitudRLCPD.cliente → Cliente, SolicitudRLCPD.paciente → PacienteRLCPD, HistorialEstadoRLCPD.solicitud → SolicitudRLCPD, HistorialEstadoRLCPD.usuario → Usuario`
    - Filtros Aplicados (WHERE): `
            cliente_id=cliente_id,
            tipo_doc=tipo_doc,
            numero_doc=numero_doc,
        `
  - Plantillas HTML Parseadas: `rlcpd/nueva.html`

#### 🔗 Ruta Servidor: `/rlcpd/pacientes`
- **Nombre de Función (Backend Handler):** `rlcpd.pacientes`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista de pacientes RLCPD del cliente activo.
- **Comentarios Descriptivos:** ─── Pacientes ────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'pacientes' - asociado al sistema maestro de **Rlcpd**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PacienteRLCPD`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PacienteRLCPD`
    - Relaciones Navegadas (JOINS): `PacienteRLCPD.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, 
            db.or_(
                PacienteRLCPD.primer_nombre.ilike(like`
  - Plantillas HTML Parseadas: `rlcpd/pacientes.html`

---

### 📦 Blueprint / Módulo: `SANEAMIENTO`

#### 🔗 Ruta Servidor: `/saneamiento/`
- **Nombre de Función (Backend Handler):** `saneamiento.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ActividadSaneamiento`
    - Relaciones Navegadas (JOINS): `ActividadSaneamiento.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, tipo=tipo, estado=estado`
  - Plantillas HTML Parseadas: `saneamiento/lista.html`

#### 🔗 Ruta Servidor: `/saneamiento/<int:id>`
- **Nombre de Función (Backend Handler):** `saneamiento.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**
  - Plantillas HTML Parseadas: `saneamiento/detalle.html`

#### 🔗 Ruta Servidor: `/saneamiento/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `saneamiento.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ActividadSaneamiento.cliente → Cliente`
  - Plantillas HTML Parseadas: `saneamiento/form.html`

#### 🔗 Ruta Servidor: `/saneamiento/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `saneamiento.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/saneamiento/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `saneamiento.exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Exportaciones | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/saneamiento/nueva`
- **Nombre de Función (Backend Handler):** `saneamiento.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Saneamiento**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadSaneamiento`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ActividadSaneamiento.cliente → Cliente`
  - Plantillas HTML Parseadas: `saneamiento/form.html`

---

### 📦 Blueprint / Módulo: `SGA_AMBIENTAL`

#### 🔗 Ruta Servidor: `/sga-ambiental/`
- **Nombre de Función (Backend Handler):** `sga_ambiental.index`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PANEL PRINCIPAL | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGA, AspectoImpactoAmbiental, ComunicacionAmbiental, ConsumoRecursoAmbiental, ContextoAmbiental, ControlOperacionalAmbiental, DocumentoSST, EmisionAtmosferica, NormaSST, ObjetivoAmbiental, ParteInteresadaAmbiental, PermisoAmbiental, PoliticaAmbiental, RegistroResiduo, RolSGA, SesionCapacitacion, VertimientoAgua`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ContextoAmbiental, ParteInteresadaAmbiental, AlcanceSGA, PoliticaAmbiental, RolSGA, AspectoImpactoAmbiental, NormaSST, PermisoAmbiental, ObjetivoAmbiental, SesionCapacitacion, ComunicacionAmbiental, DocumentoSST, ControlOperacionalAmbiental, RegistroResiduo, ConsumoRecursoAmbiental, EmisionAtmosferica, VertimientoAgua`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, DocumentoSST.cliente → Cliente, ContextoAmbiental.cliente → Cliente, ParteInteresadaAmbiental.cliente → Cliente, AlcanceSGA.cliente → Cliente, PoliticaAmbiental.cliente → Cliente, RolSGA.cliente → Cliente, AspectoImpactoAmbiental.cliente → Cliente, PermisoAmbiental.cliente → Cliente, ObjetivoAmbiental.cliente → Cliente, ComunicacionAmbiental.cliente → Cliente, ControlOperacionalAmbiental.cliente → Cliente, RegistroResiduo.cliente → Cliente, ConsumoRecursoAmbiental.cliente → Cliente, EmisionAtmosferica.cliente → Cliente, EmisionAtmosferica.permiso → PermisoAmbiental, VertimientoAgua.cliente → Cliente, VertimientoAgua.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, anio=anio, estado='vigente', 
        cliente_id=cliente_id, activa=True, 
        cliente_id=cliente_id, vigente=True... (+15 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/index.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/aia`
- **Nombre de Función (Backend Handler):** `sga_ambiental.aia_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ASPECTOS E IMPACTOS AMBIENTALES (cláusula 6.1.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AspectoImpactoAmbiental`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo_impacto=tipo_filtro, medio_afectado=medio_filtro... (+3 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/aia_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/aia/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.aia_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/aia_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/aia/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.aia_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/aia/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.aia_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/aia_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/alcance`
- **Nombre de Función (Backend Handler):** `sga_ambiental.alcance_ver`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ALCANCE DEL SGA (cláusula 4.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'alcance_ver' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AlcanceSGA`
    - Relaciones Navegadas (JOINS): `AlcanceSGA.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, vigente=True, cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `sga_ambiental/alcance.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/alcance/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.alcance_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `AlcanceSGA.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/alcance/guardar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.alcance_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'alcance_guardar' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AlcanceSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AlcanceSGA`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AlcanceSGA.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, vigente=True`

#### 🔗 Ruta Servidor: `/sga-ambiental/capacitaciones-ambientales`
- **Nombre de Función (Backend Handler):** `sga_ambiental.capacitaciones_amb_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Reutiliza SesionCapacitacion con tipo_sistema='ambiental' | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion, TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `SesionCapacitacion, TemaCapacitacion`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente, TemaCapacitacion.sesiones → SesionCapacitacion, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo_sistema='ambiental'
    , cliente_id=None, 
        cliente_id=cliente_id, tipo_sistema='ambiental', activo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/capacitaciones_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/capacitaciones-ambientales/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.capacitacion_amb_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`

#### 🔗 Ruta Servidor: `/sga-ambiental/capacitaciones-ambientales/nueva`
- **Nombre de Función (Backend Handler):** `sga_ambiental.capacitacion_amb_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `SesionCapacitacion, TemaCapacitacion`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TemaCapacitacion`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TemaCapacitacion.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo_sistema='ambiental', activo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/capacitacion_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/comunicaciones-ambientales`
- **Nombre de Función (Backend Handler):** `sga_ambiental.comunicaciones_amb_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** COMUNICACIÓN AMBIENTAL (cláusula 7.4) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ComunicacionAmbiental`
    - Relaciones Navegadas (JOINS): `ComunicacionAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=None, tipo=tipo_f`
  - Plantillas HTML Parseadas: `sga_ambiental/comunicaciones_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/comunicaciones-ambientales/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.comunicacion_amb_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ComunicacionAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/comunicacion_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/comunicaciones-ambientales/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.comunicacion_amb_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ComunicacionAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/comunicaciones-ambientales/nueva`
- **Nombre de Función (Backend Handler):** `sga_ambiental.comunicacion_amb_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ComunicacionAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ComunicacionAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/comunicacion_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/consumos-recursos`
- **Nombre de Función (Backend Handler):** `sga_ambiental.consumos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** MONITOREO DE CONSUMOS (cláusula 8.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConsumoRecursoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConsumoRecursoAmbiental`
    - Relaciones Navegadas (JOINS): `ConsumoRecursoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=None, tipo=tipo_f... (+2 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/consumos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/consumos-recursos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.consumo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConsumoRecursoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ConsumoRecursoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/consumo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/consumos-recursos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.consumo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConsumoRecursoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ConsumoRecursoAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/consumos-recursos/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.consumo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConsumoRecursoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ConsumoRecursoAmbiental.cliente → Cliente, ConsumoRecursoAmbiental.creado_por → Usuario`
  - Plantillas HTML Parseadas: `sga_ambiental/consumo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/contexto`
- **Nombre de Función (Backend Handler):** `sga_ambiental.contexto_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CUESTIONES AMBIENTALES (cláusula 4.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ContextoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ContextoAmbiental`
    - Relaciones Navegadas (JOINS): `ContextoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `sga_ambiental/contexto_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/contexto/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.contexto_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ContextoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ContextoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/contexto_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/contexto/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.contexto_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ContextoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ContextoAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/contexto/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.contexto_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ContextoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ContextoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/contexto_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/controles-operacionales`
- **Nombre de Función (Backend Handler):** `sga_ambiental.controles_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** CONTROL OPERACIONAL DE ASPECTOS SIGNIFICATIVOS (cláusula 8.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, ControlOperacionalAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ControlOperacionalAmbiental, AspectoImpactoAmbiental`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente, ControlOperacionalAmbiental.aspecto → AspectoImpactoAmbiental, ControlOperacionalAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=None, tipo_control=tipo_f... (+3 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/controles_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/controles-operacionales/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.control_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, ControlOperacionalAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AspectoImpactoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente, ControlOperacionalAmbiental.aspecto → AspectoImpactoAmbiental, ControlOperacionalAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=ctrl.cliente_id, significativo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/control_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/controles-operacionales/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.control_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ControlOperacionalAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ControlOperacionalAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/controles-operacionales/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.control_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, ControlOperacionalAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AspectoImpactoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente, ControlOperacionalAmbiental.aspecto → AspectoImpactoAmbiental, ControlOperacionalAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, significativo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/control_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/documentos-sga`
- **Nombre de Función (Backend Handler):** `sga_ambiental.documentos_sga_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DocumentoSST`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo_sistema='ambiental', cliente_id=None, tipo=tipo_f... (+1 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/documentos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/documentos-sga/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.documento_sga_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/documento_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/documentos-sga/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.documento_sga_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/documentos-sga/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.documento_sga_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/documento_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/emisiones-atmosfericas`
- **Nombre de Función (Backend Handler):** `sga_ambiental.emisiones_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** EMISIONES ATMOSFÉRICAS (Res. 909/2008 — cláusula 8.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmisionAtmosferica, PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `EmisionAtmosferica, PermisoAmbiental`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, EmisionAtmosferica.cliente → Cliente, EmisionAtmosferica.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=None, tipo_fuente=tipo_f... (+3 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/emisiones_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/emisiones-atmosfericas/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.emision_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmisionAtmosferica, PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PermisoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, EmisionAtmosferica.cliente → Cliente, EmisionAtmosferica.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `
        cliente_id=em.cliente_id, tipo='permiso_emisiones', activo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/emision_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/emisiones-atmosfericas/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.emision_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmisionAtmosferica`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `EmisionAtmosferica.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/emisiones-atmosfericas/nueva`
- **Nombre de Función (Backend Handler):** `sga_ambiental.emision_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `EmisionAtmosferica, PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PermisoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, EmisionAtmosferica.cliente → Cliente, EmisionAtmosferica.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, tipo='permiso_emisiones', activo=True
    `
  - Plantillas HTML Parseadas: `sga_ambiental/emision_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/objetivos`
- **Nombre de Función (Backend Handler):** `sga_ambiental.objetivos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ObjetivoAmbiental`
    - Relaciones Navegadas (JOINS): `ObjetivoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=None, estado=estado_f`
  - Plantillas HTML Parseadas: `sga_ambiental/objetivos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/objetivos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.objetivo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, ObjetivoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AspectoImpactoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente, ObjetivoAmbiental.cliente → Cliente, ObjetivoAmbiental.aspecto → AspectoImpactoAmbiental`
    - Filtros Aplicados (WHERE): `
        cliente_id=obj.cliente_id, significativo=True`
  - Plantillas HTML Parseadas: `sga_ambiental/objetivo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/objetivos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.objetivo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ObjetivoAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/objetivos/<int:id>/seguimiento`
- **Nombre de Función (Backend Handler):** `sga_ambiental.objetivo_seguimiento`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'objetivo_seguimiento' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ObjetivoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ObjetivoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/objetivo_seguimiento.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/objetivos/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.objetivo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, ObjetivoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AspectoImpactoAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AspectoImpactoAmbiental.cliente → Cliente, ObjetivoAmbiental.cliente → Cliente, ObjetivoAmbiental.aspecto → AspectoImpactoAmbiental`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, significativo=True`
  - Plantillas HTML Parseadas: `sga_ambiental/objetivo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/partes`
- **Nombre de Función (Backend Handler):** `sga_ambiental.partes_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** PARTES INTERESADAS AMBIENTALES (cláusula 4.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ParteInteresadaAmbiental`
    - Relaciones Navegadas (JOINS): `ParteInteresadaAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, tipo=tipo_filtro`
  - Plantillas HTML Parseadas: `sga_ambiental/partes_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/partes/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.parte_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `ParteInteresadaAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/parte_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/partes/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.parte_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `ParteInteresadaAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/partes/nueva`
- **Nombre de Función (Backend Handler):** `sga_ambiental.parte_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ParteInteresadaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ParteInteresadaAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/parte_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/permisos`
- **Nombre de Función (Backend Handler):** `sga_ambiental.permisos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ── PERMISOS AMBIENTALES ─────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PermisoAmbiental`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=None, tipo=tipo_f`
  - Plantillas HTML Parseadas: `sga_ambiental/permisos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/permisos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.permiso_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/permiso_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/permisos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.permiso_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/permisos/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.permiso_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/permiso_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/politica`
- **Nombre de Función (Backend Handler):** `sga_ambiental.politica_ver`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** POLÍTICA AMBIENTAL (cláusula 5.2) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica_ver' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaAmbiental`
    - Relaciones Navegadas (JOINS): `PoliticaAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `sga_ambiental/politica.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/politica/guardar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.politica_guardar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica_guardar' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaAmbiental`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaAmbiental`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PoliticaAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`

#### 🔗 Ruta Servidor: `/sga-ambiental/requisitos-legales`
- **Nombre de Función (Backend Handler):** `sga_ambiental.req_legales_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Reutiliza NormaSST / RequisitoEmpresa con tipo_sistema='ambiental' | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `NormaSST, RequisitoEmpresa`
    - Relaciones Navegadas (JOINS): `RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `tipo_sistema='ambiental', activo=True, 
            RequisitoEmpresa.cliente_id == cliente_id,
            RequisitoEmpresa.norma_id.in_([n.id for n in normas_amb]`
  - Plantillas HTML Parseadas: `sga_ambiental/req_legales_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/requisitos-legales/<int:norma_id>/evaluar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.req_legal_evaluar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'req_legal_evaluar' - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `NormaSST, RequisitoEmpresa`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RequisitoEmpresa`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RequisitoEmpresa.cliente → Cliente, RequisitoEmpresa.norma → NormaSST`
    - Filtros Aplicados (WHERE): `
        cliente_id=cliente_id, norma_id=norma_id`

#### 🔗 Ruta Servidor: `/sga-ambiental/residuos-sga`
- **Nombre de Función (Backend Handler):** `sga_ambiental.residuos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** GESTIÓN DE RESIDUOS (Dec. 4741/2005 — cláusula 8.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RegistroResiduo`
    - Relaciones Navegadas (JOINS): `RegistroResiduo.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, anio=anio, cliente_id=None, mes=mes... (+2 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/residuos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/residuos-sga/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.residuo_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RegistroResiduo.cliente → Cliente`
  - Plantillas HTML Parseadas: `sga_ambiental/residuo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/residuos-sga/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.residuo_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RegistroResiduo.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/residuos-sga/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.residuo_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RegistroResiduo`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RegistroResiduo.cliente → Cliente, RegistroResiduo.creado_por → Usuario`
  - Plantillas HTML Parseadas: `sga_ambiental/residuo_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/roles`
- **Nombre de Función (Backend Handler):** `sga_ambiental.roles_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** ROLES SGA (cláusula 5.3) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RolSGA`
    - Relaciones Navegadas (JOINS): `RolSGA.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id`
  - Plantillas HTML Parseadas: `sga_ambiental/roles_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/roles/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.rol_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RolSGA.cliente → Cliente, RolSGA.usuario → Usuario, RolSGA.empleado → Empleado`
  - Plantillas HTML Parseadas: `sga_ambiental/rol_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/roles/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.rol_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `RolSGA.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/roles/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.rol_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RolSGA`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RolSGA.cliente → Cliente, RolSGA.usuario → Usuario, RolSGA.empleado → Empleado`
  - Plantillas HTML Parseadas: `sga_ambiental/rol_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/vertimientos`
- **Nombre de Función (Backend Handler):** `sga_ambiental.vertimientos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** VERTIMIENTOS (Res. 631/2015 / PSMV — cláusula 8.1) | ══════════════════════════════════════════════════════════════
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental, VertimientoAgua`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `VertimientoAgua, PermisoAmbiental`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, VertimientoAgua.cliente → Cliente, VertimientoAgua.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, cliente_id=None, tipo_cuerpo_receptor=tipo_f... (+4 más)`
  - Plantillas HTML Parseadas: `sga_ambiental/vertimientos_lista.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/vertimientos/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.vertimiento_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental, VertimientoAgua`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PermisoAmbiental, VertimientoAgua`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, VertimientoAgua.cliente → Cliente, VertimientoAgua.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `
                            cliente_id=vert.cliente_id, 
        PermisoAmbiental.cliente_id == vert.cliente_id,
        PermisoAmbiental.tipo.in_(['permiso_vertimiento', 'PSMV']`
  - Plantillas HTML Parseadas: `sga_ambiental/vertimiento_form.html`

#### 🔗 Ruta Servidor: `/sga-ambiental/vertimientos/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `sga_ambiental.vertimiento_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `VertimientoAgua`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `VertimientoAgua.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sga-ambiental/vertimientos/nuevo`
- **Nombre de Función (Backend Handler):** `sga_ambiental.vertimiento_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Sga_Ambiental**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PermisoAmbiental, VertimientoAgua`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PermisoAmbiental, VertimientoAgua`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PermisoAmbiental.cliente → Cliente, VertimientoAgua.cliente → Cliente, VertimientoAgua.permiso → PermisoAmbiental`
    - Filtros Aplicados (WHERE): `
                            cliente_id=cliente_id, 
        PermisoAmbiental.cliente_id == cliente_id,
        PermisoAmbiental.tipo.in_(['permiso_vertimiento', 'PSMV']`
  - Plantillas HTML Parseadas: `sga_ambiental/vertimiento_form.html`

---

### 📦 Blueprint / Módulo: `SIG`

#### 🔗 Ruta Servidor: `/sig/`
- **Nombre de Función (Backend Handler):** `sig.dashboard`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** 1. DASHBOARD — Mapa estratégico del SIG | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'dashboard' - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST, Cliente, PoliticaSIG, RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Cliente, PoliticaSIG, AuditoriaSST, RevisionDireccionSIG`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente, PoliticaSIG.cliente → Cliente, RevisionDireccionSIG.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid, 
            cliente_id=cid, tipo_sistema='integrado'
        , 
            cliente_id=cid
        `
  - Plantillas HTML Parseadas: `sig/dashboard.html`

#### 🔗 Ruta Servidor: `/sig/auditorias`
- **Nombre de Función (Backend Handler):** `sig.auditoria_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** 3. AUDITORÍAS INTEGRADAS (reutiliza AuditoriaSST con tipo_sistema='integrado') | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `AuditoriaSST`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cid, tipo_sistema='integrado'
    `
  - Plantillas HTML Parseadas: `sig/auditoria_lista.html`

#### 🔗 Ruta Servidor: `/sig/auditorias/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sig.auditoria_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `sig/auditoria_nueva.html`

#### 🔗 Ruta Servidor: `/sig/auditorias/nueva`
- **Nombre de Función (Backend Handler):** `sig.auditoria_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AuditoriaSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `AuditoriaSST.cliente → Cliente`
  - Plantillas HTML Parseadas: `sig/auditoria_nueva.html`

#### 🔗 Ruta Servidor: `/sig/documentos`
- **Nombre de Función (Backend Handler):** `sig.documentos_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** 5. LISTADO MAESTRO DE DOCUMENTOS SIG | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `DocumentoSST`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `DocumentoSST`
    - Relaciones Navegadas (JOINS): `DocumentoSST.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cid
    `
  - Plantillas HTML Parseadas: `sig/documentos_lista.html`

#### 🔗 Ruta Servidor: `/sig/documentos/exportar-excel`
- **Nombre de Función (Backend Handler):** `sig.documentos_exportar_excel`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/sig/politica`
- **Nombre de Función (Backend Handler):** `sig.politica_ver`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** 2. POLÍTICA INTEGRADA | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'politica_ver' - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `PoliticaSIG`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `PoliticaSIG`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `PoliticaSIG.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid`
  - Plantillas HTML Parseadas: `sig/politica_ver.html`

#### 🔗 Ruta Servidor: `/sig/revisiones`
- **Nombre de Función (Backend Handler):** `sig.revision_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** 4. REVISIÓN POR LA DIRECCIÓN INTEGRADA | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RevisionDireccionSIG`
    - Relaciones Navegadas (JOINS): `RevisionDireccionSIG.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cid
    `
  - Plantillas HTML Parseadas: `sig/revision_lista.html`

#### 🔗 Ruta Servidor: `/sig/revisiones/<int:id>`
- **Nombre de Función (Backend Handler):** `sig.revision_detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RevisionDireccionSIG.cliente → Cliente`
  - Plantillas HTML Parseadas: `sig/revision_detalle.html`

#### 🔗 Ruta Servidor: `/sig/revisiones/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `sig.revision_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `RevisionDireccionSIG.cliente → Cliente`
  - Plantillas HTML Parseadas: `sig/revision_form.html`

#### 🔗 Ruta Servidor: `/sig/revisiones/<int:id>/exportar-pdf`
- **Nombre de Función (Backend Handler):** `sig.revision_exportar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `RevisionDireccionSIG.cliente → Cliente`

#### 🔗 Ruta Servidor: `/sig/revisiones/nueva`
- **Nombre de Función (Backend Handler):** `sig.revision_nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `RevisionDireccionSIG`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `RevisionDireccionSIG.cliente → Cliente`
  - Plantillas HTML Parseadas: `sig/revision_form.html`

#### 🔗 Ruta Servidor: `/sig/riesgos`
- **Nombre de Función (Backend Handler):** `sig.riesgos_integrados`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** 6. GESTIÓN DE RIESGOS INTEGRADA | ─────────────────────────────────────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'riesgos_integrados' - asociado al sistema maestro de **Sig**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `AspectoImpactoAmbiental, FilaMatrizRiesgo, RiesgoCalidad`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `RiesgoCalidad, AspectoImpactoAmbiental`
    - Relaciones Navegadas (JOINS): `MatrizRiesgo.cliente → Cliente, MatrizRiesgo.filas → FilaMatrizRiesgo, FilaMatrizRiesgo.matriz → MatrizRiesgo, RiesgoCalidad.cliente → Cliente, AspectoImpactoAmbiental.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=cid
    , 
        cliente_id=cid, significativo=True
    , 
            MatrizRiesgo.cliente_id == cid
        `
  - Plantillas HTML Parseadas: `sig/riesgos_integrados.html`

---

### 📦 Blueprint / Módulo: `TENANT`

#### 🔗 Ruta Servidor: `/<slug>`
- **Nombre de Función (Backend Handler):** `tenant.index_tenant`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Home del tenant: redirige al subdominio si BASE_DOMAIN está configurado
    y el usuario está en el dominio base. Si ya en el subdominio, muestra el
    dashboard o redirige al login del tenant.
- **Comentarios Descriptivos:** ────────────────────────────────────────────── | ──────────────────────────────────────────────
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'index_tenant' - asociado al sistema maestro de **Tenant**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

#### 🔗 Ruta Servidor: `/<slug>/login`
- **Nombre de Función (Backend Handler):** `tenant.login_tenant`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** Si ya está autenticado en este tenant, ir al dashboard
- **Propósito Interno Inferido:** Control y enrutamiento relacionado explícitamente a la gestión de accesos, recuperación y permisos de sesión del usuario contra roles de base de datos - asociado al sistema maestro de **Tenant**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Usuario`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Usuario`
    - Relaciones Navegadas (JOINS): `Usuario.cliente → Cliente, Usuario.clientes_asignados → Cliente`
    - Filtros Aplicados (WHERE): `
            db.func.lower(Usuario.email`
  - Plantillas HTML Parseadas: `tenant/login.html, tenant/login.html`

#### 🔗 Ruta Servidor: `/<slug>/logout`
- **Nombre de Función (Backend Handler):** `tenant.logout_tenant`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Vuelve al login del tenant, no al login global
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'logout_tenant' - asociado al sistema maestro de **Tenant**.
- **Trazabilidad Funcional sobre el Código:**
  - **Flujos de Datos Detectados:**

---

### 📦 Blueprint / Módulo: `VIGILANCIA_EPI`

#### 🔗 Ruta Servidor: `/vigilancia-epi/`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Comentarios Descriptivos:** Inscripciones | ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InscripcionPVE`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `InscripcionPVE.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid`
  - Plantillas HTML Parseadas: `vigilancia_epi/lista.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/<int:id>`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InscripcionPVE, ResultadoPVE`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ResultadoPVE.cliente → Cliente, InscripcionPVE.cliente → Cliente, InscripcionPVE.resultados → ResultadoPVE`
    - Filtros Aplicados (WHERE): `inscripcion_id=id`
  - Plantillas HTML Parseadas: `vigilancia_epi/detalle.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, InscripcionPVE`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, InscripcionPVE.cliente → Cliente, InscripcionPVE.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `vigilancia_epi/inscripcion_form.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InscripcionPVE`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/vigilancia-epi/<int:inscripcion_id>/resultado/nuevo`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.nuevo_resultado`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InscripcionPVE, ResultadoPVE`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `ResultadoPVE.cliente → Cliente, ResultadoPVE.empleado → Empleado, InscripcionPVE.cliente → Cliente, InscripcionPVE.empleado → Empleado`
  - Plantillas HTML Parseadas: `vigilancia_epi/resultado_form.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/nueva`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Empleado, InscripcionPVE`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Empleado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `Empleado.cliente → Cliente, InscripcionPVE.cliente → Cliente, InscripcionPVE.empleado → Empleado`
    - Filtros Aplicados (WHERE): `cliente_id=cid, activo=True`
  - Plantillas HTML Parseadas: `vigilancia_epi/inscripcion_form.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/resultado/<int:id>/pdf`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.exportar_pdf_resultado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ResultadoPVE`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ResultadoPVE.empleado → Empleado`

#### 🔗 Ruta Servidor: `/vigilancia-epi/resultado/<int:id>/ver`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.ver_resultado`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'ver_resultado' - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ResultadoPVE`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `ResultadoPVE.cliente → Cliente`
  - Plantillas HTML Parseadas: `vigilancia_epi/resultado_detalle.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/tipos-pve`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.tipos_pve_lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Lista los tipos PVE base + personalizados del cliente activo.
- **Comentarios Descriptivos:** ─────────────────────────────────────────
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoPVEPersonalizado`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `TipoPVEPersonalizado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `cliente_id=cid`
  - Plantillas HTML Parseadas: `vigilancia_epi/tipos_pve.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/tipos-pve/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.tipos_pve_editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoPVEPersonalizado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `TipoPVEPersonalizado.cliente → Cliente`
  - Plantillas HTML Parseadas: `vigilancia_epi/tipo_pve_form.html, vigilancia_epi/tipo_pve_form.html`

#### 🔗 Ruta Servidor: `/vigilancia-epi/tipos-pve/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.tipos_pve_eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `InscripcionPVE, TipoPVEPersonalizado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `InscripcionPVE`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `InscripcionPVE.cliente → Cliente, TipoPVEPersonalizado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
        cliente_id=tipo.cliente_id, tipo_pve=tipo.codigo`

#### 🔗 Ruta Servidor: `/vigilancia-epi/tipos-pve/<int:id>/toggle`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.tipos_pve_toggle`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Función relámpago que permite alternar o switchear rápidamente ciertos estados (activo/inactivo, completado/pendiente) de un registro sin salir de pantalla - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoPVEPersonalizado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `TipoPVEPersonalizado.cliente → Cliente`

#### 🔗 Ruta Servidor: `/vigilancia-epi/tipos-pve/nuevo`
- **Nombre de Función (Backend Handler):** `vigilancia_epi.tipos_pve_nuevo`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Entrada al sistema que despliega el formulario en formato vacío y, si hay datos de entrada, procesa y valida su guardado en base de datos - asociado al sistema maestro de **Vigilancia_Epi**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `TipoPVEPersonalizado`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoPVEPersonalizado`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TipoPVEPersonalizado.cliente → Cliente`
    - Filtros Aplicados (WHERE): `
            cliente_id=cid, codigo=codigo`
  - Plantillas HTML Parseadas: `vigilancia_epi/tipo_pve_form.html, vigilancia_epi/tipo_pve_form.html, vigilancia_epi/tipo_pve_form.html, vigilancia_epi/tipo_pve_form.html`

---

### 📦 Blueprint / Módulo: `VISITAS`

#### 🔗 Ruta Servidor: `/visitas/`
- **Nombre de Función (Backend Handler):** `visitas.lista`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Despliega el listado principal de registros con la tabla de datos y los filtros aplicables - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan, Cliente, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `Visita, Cliente, ActividadPlan`
    - Relaciones Navegadas (JOINS): `Cliente.visitas → Visita, Visita.cliente → Cliente, Visita.asesor → Usuario, ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, Visita.cliente_id.in_(cids, Cliente.id.in_(cids... (+2 más)`
  - Plantillas HTML Parseadas: `visitas/lista.html`

#### 🔗 Ruta Servidor: `/visitas/<int:id>`
- **Nombre de Función (Backend Handler):** `visitas.detalle`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Muestra una vista en profundidad con la información y relaciones de un registro específico a través de su identificador - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Visita`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.asesor → Usuario`
  - Plantillas HTML Parseadas: `visitas/detalle.html`

#### 🔗 Ruta Servidor: `/visitas/<int:id>/acta-word`
- **Nombre de Función (Backend Handler):** `visitas.exportar_acta_word`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Visita`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.asesor → Usuario`

#### 🔗 Ruta Servidor: `/visitas/<int:id>/editar`
- **Nombre de Función (Backend Handler):** `visitas.editar`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Comentarios Descriptivos:** Verificar que el asesor tenga acceso a esta empresa
- **Propósito Interno Inferido:** Gestiona el ciclo de vida de edición, validando el formulario cargado y aplicando la actualización final a la base de datos - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `FotoVisita, TipoObjetivoVisita, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoObjetivoVisita`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`
    - Relaciones Navegadas (JOINS): `TipoObjetivoVisita.visitas → Visita, Visita.cliente → Cliente, Visita.asesor → Usuario, Visita.tipo_objetivo → TipoObjetivoVisita, Visita.inspecciones → Inspeccion, Visita.fotos → FotoVisita, Inspeccion.visita → Visita, FotoVisita.visita → Visita`
    - Filtros Aplicados (WHERE): `activo=True`
  - Plantillas HTML Parseadas: `visitas/editar.html`

#### 🔗 Ruta Servidor: `/visitas/<int:id>/eliminar`
- **Nombre de Función (Backend Handler):** `visitas.eliminar`
- **Transacciones HTTP Aceptadas:** `POST`
- **Propósito Interno Inferido:** Procesa de forma segura la eliminación (física o lógica en base de datos) del registro especificado y reevalúa dependencias - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Operaciones de Eliminación (DELETE): `db.session.delete(`

#### 🔗 Ruta Servidor: `/visitas/<int:id>/informe-word`
- **Nombre de Función (Backend Handler):** `visitas.exportar_informe_word`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Extrae y renderiza consolidados de datos para escupir un archivo reportes en formato interactivo (Excel/PDF/Word) - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Visita`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.asesor → Usuario`

#### 🔗 Ruta Servidor: `/visitas/<int:id>/pdf`
- **Nombre de Función (Backend Handler):** `visitas.generar_pdf`
- **Transacciones HTTP Aceptadas:** `GET`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'generar_pdf' - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ConfiguracionApp, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `ConfiguracionApp`
    - Operaciones de Escritura (INSERT/UPDATE): `session.commit, session.commit, session.commit, session.commit`
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.asesor → Usuario`

#### 🔗 Ruta Servidor: `/visitas/agenda`
- **Nombre de Función (Backend Handler):** `visitas.agenda`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Agenda del asesor: visitas pendientes (programadas) y ejecutadas.
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'agenda' - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `Visita`
  - **Flujos de Datos Detectados:**
    - Relaciones Navegadas (JOINS): `Visita.cliente → Cliente, Visita.asesor → Usuario`
    - Filtros Aplicados (WHERE): `Visita.cliente_id.in_(cids, Visita.fecha_programada.isnot(None, Visita.estado.in_(['finalizada', 'pdf_generado']`
  - Plantillas HTML Parseadas: `visitas/agenda.html`

#### 🔗 Ruta Servidor: `/visitas/api/objetivos/<int:cliente_id>`
- **Nombre de Función (Backend Handler):** `visitas.api_objetivos`
- **Transacciones HTTP Aceptadas:** `GET`
- **Documentación Funcional Original:** Devuelve los tipos de objetivo activos para un cliente. [asesor]
- **Propósito Interno Inferido:** Actúa puramente como API backend asíncrona sirviendo datos en formato nativo serializable y consumible por componentes externos o dinámicos del frontend - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ClienteObjetivoVisita, TipoObjetivoVisita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoObjetivoVisita`
    - Relaciones Navegadas (JOINS): `ClienteObjetivoVisita.cliente → Cliente, ClienteObjetivoVisita.objetivo → TipoObjetivoVisita, Visita.cliente → Cliente, Visita.asesor → Usuario, Visita.tipo_objetivo → TipoObjetivoVisita`
    - Filtros Aplicados (WHERE): `cliente_id=cliente_id, activo=True, activo=True`
  - Respuesta Técnica Devuelta: `JSON Payload / Dict Objects` respondiendo una solicitud API

#### 🔗 Ruta Servidor: `/visitas/nueva`
- **Nombre de Función (Backend Handler):** `visitas.nueva`
- **Transacciones HTTP Aceptadas:** `GET,POST`
- **Propósito Interno Inferido:** Maneja la acción algoritmica definida mediante el término 'nueva' - asociado al sistema maestro de **Visitas**.
- **Trazabilidad Funcional sobre el Código:**
  - Modelos ORM Interactuados Directamente: `ActividadPlan, AsistenteCapacitacion, Cliente, ClienteObjetivoVisita, FotoVisita, SesionCapacitacion, TipoObjetivoVisita, Visita`
  - **Flujos de Datos Detectados:**
    - Operaciones de Lectura (SELECT): `TipoObjetivoVisita, ActividadPlan, Cliente, ClienteObjetivoVisita, Visita`
    - Operaciones de Escritura (INSERT/UPDATE): `session.add, session.commit, session.add, session.commit, session.add, session.commit, session.add, session.commit`
    - Relaciones Navegadas (JOINS): `TipoObjetivoVisita.visitas → Visita, ClienteObjetivoVisita.cliente → Cliente, ClienteObjetivoVisita.objetivo → TipoObjetivoVisita, Cliente.visitas → Visita, Visita.cliente → Cliente, Visita.asesor → Usuario, Visita.tipo_objetivo → TipoObjetivoVisita, Visita.inspecciones → Inspeccion, Visita.fotos → FotoVisita, FotoVisita.visita → Visita, AccionMejora.cliente → Cliente, SesionCapacitacion.cliente → Cliente, SesionCapacitacion.tema → TemaCapacitacion, SesionCapacitacion.asistentes → AsistenteCapacitacion, AsistenteCapacitacion.sesion → SesionCapacitacion, ActividadPlan.plan → PlanAnualSST, ActividadPlan.cliente → Cliente, PESV.cliente → Cliente`
    - Filtros Aplicados (WHERE): `activo=True, 
            cliente_id=cliente_id_int,
            objetivo_id=tipo_objetivo.id,
            activo=True
        , cliente_id=cliente_id_int... (+2 más)`
  - Plantillas HTML Parseadas: `visitas/form.html, visitas/form.html, visitas/form.html`

---

## 2. Arquitectura de Base de Datos (SQLAlchemy ORM)

### 📊 Resumen de Arquitectura
- **Total de Modelos ORM:** 172
- **Total de Relaciones:** 364
- **Total de Foreign Keys:** 307

### 🏗️ Modelos Centrales (Por Número de Relaciones)
- **Cliente**: 8 relaciones
- **ReporteIncidente**: 8 relaciones
- **SolicitudRLCPD**: 7 relaciones
- **Usuario**: 6 relaciones
- **SesionCapacitacion**: 6 relaciones
- **CasoRehabilitacion**: 6 relaciones
- **Visita**: 5 relaciones
- **Inspeccion**: 5 relaciones
- **PlanAnualSST**: 5 relaciones
- **PESV**: 5 relaciones

### 🔗 Modelos con Múltiples Relaciones (>3)

#### `CasoRehabilitacion`
- **Columnas:** 18
- **Foreign Keys:** 4
- **Relaciones:** 6
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `empleado` → `Empleado`
  - `incapacidad` → `Incapacidad`
  - `recomendaciones` → `RecomendacionMedReh`
  - `planes` → `PlanAccionReh`
  - ... y 1 más

#### `Cliente`
- **Columnas:** 28
- **Foreign Keys:** 2
- **Relaciones:** 8
- **Relaciones Definidas:**
  - `usuarios` → `Usuario` ↔ cliente
  - `visitas` → `Visita` ↔ cliente
  - `reportes` → `ReporteIncidente` ↔ cliente
  - `enfermedades_laborales` → `EnfermedadLaboral` ↔ cliente
  - `empleados` → `Empleado` ↔ cliente
  - ... y 3 más

#### `ComiteConvivenciaLaboral`
- **Columnas:** 12
- **Foreign Keys:** 1
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `miembros` → `MiembroConvivenciaLaboral` ↔ comite
  - `reuniones` → `ReunionConvivenciaLaboral` ↔ comite
  - `expedientes` → `ExpedienteConvivencia` ↔ comite

#### `ConductorPESV`
- **Columnas:** 13
- **Foreign Keys:** 3
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `empleado` → `Empleado`
  - `vehiculo_principal` → `VehiculoPESV` ↔ conductores
  - `siniestros_como_conductor` → `SiniestroPESV` ↔ conductor

#### `DocumentoSST`
- **Columnas:** 22
- **Foreign Keys:** 1
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `descargas` → `DescargaDocumento` ↔ documento
  - `historial_estados` → `HistorialVersionDocumento`
  - `distribuciones` → `DistribucionDocumento`

#### `Empleado`
- **Columnas:** 31
- **Foreign Keys:** 2
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente` ↔ empleados
  - `area` → `Area` ↔ empleados
  - `reportes_como_afectado` → `ReporteIncidente` ↔ empleado_afectado
  - `enfermedades_laborales` → `EnfermedadLaboral` ↔ empleado

#### `EntregaEPP`
- **Columnas:** 13
- **Foreign Keys:** 5
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `empleado` → `Empleado`
  - `firma` → `FirmaDocumento`
  - `creado_por` → `Usuario`

#### `EvidenciaItemDiagnostico`
- **Columnas:** 11
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `item` → `ItemAutoevaluacion`
  - `autoevaluacion` → `AutoevaluacionSST`
  - `cliente` → `Cliente`
  - `subido_por` → `Usuario`

#### `ExpedienteConvivencia`
- **Columnas:** 20
- **Foreign Keys:** 2
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `comite` → `ComiteConvivenciaLaboral` ↔ expedientes
  - `actuaciones` → `ActuacionConvivencia` ↔ expediente
  - `compromisos` → `CompromisoConvivenciaLaboral` ↔ expediente

#### `FirmaDocumento`
- **Columnas:** 21
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `usuario` → `Usuario`
  - `firma_externa` → `FirmaExterna`
  - `flujo` → `FlujoAprobacionDoc`

#### `HistorialRetencion`
- **Columnas:** 10
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `registro_tratamiento` → `RegistroTratamiento`
  - `documento_sst` → `DocumentoSST`
  - `responsable` → `Usuario`

#### `InscripcionPVE`
- **Columnas:** 12
- **Foreign Keys:** 3
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `empleado` → `Empleado`
  - `resultados` → `ResultadoPVE`
  - `creado_por` → `Usuario`

#### `Inspeccion`
- **Columnas:** 15
- **Foreign Keys:** 1
- **Relaciones:** 5
- **Relaciones Definidas:**
  - `botiquines_generados` → `InspeccionBotiquin` ↔ inspeccion_origen
  - `extintores_generados` → `InspeccionExtintor` ↔ inspeccion_origen
  - `camillas_generadas` → `InspeccionCamilla` ↔ inspeccion_origen
  - `generales_generadas` → `InspeccionGeneral` ↔ inspeccion_origen
  - `visita` → `Visita` ↔ inspecciones

#### `PESV`
- **Columnas:** 24
- **Foreign Keys:** 2
- **Relaciones:** 5
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `vehiculos` → `VehiculoPESV` ↔ pesv_rel
  - `actividades` → `ActividadPESV` ↔ pesv
  - `siniestros` → `SiniestroPESV` ↔ pesv
  - `responsable_vial_emp` → `Empleado`

#### `PlanAnualSST`
- **Columnas:** 11
- **Foreign Keys:** 4
- **Relaciones:** 5
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `centro_trabajo` → `CentroTrabajo`
  - `asesor_asignado` → `Usuario`
  - `coordinador_asignador` → `Usuario`
  - `actividades` → `ActividadPlan` ↔ plan

#### `ReporteIncidente`
- **Columnas:** 43
- **Foreign Keys:** 4
- **Relaciones:** 8
- **Relaciones Definidas:**
  - `cliente` → `Cliente` ↔ reportes
  - `reportado_por` → `Usuario` ↔ reportes_enviados
  - `empleado_afectado` → `Empleado` ↔ reportes_como_afectado
  - `area` → `Area` ↔ reportes
  - `encuesta` → `EncuestaInvestigacion` ↔ reporte
  - ... y 3 más

#### `SesionCapacitacion`
- **Columnas:** 23
- **Foreign Keys:** 4
- **Relaciones:** 6
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `centro_trabajo` → `CentroTrabajo`
  - `tema` → `TemaCapacitacion` ↔ sesiones
  - `asistentes` → `AsistenteCapacitacion` ↔ sesion
  - `formulario_evaluacion` → `FormularioPersonalizado`
  - ... y 1 más

#### `SiniestroPESV`
- **Columnas:** 15
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `pesv` → `PESV` ↔ siniestros
  - `vehiculo` → `VehiculoPESV` ↔ siniestros
  - `conductor` → `ConductorPESV` ↔ siniestros_como_conductor

#### `SolicitudARCOP`
- **Columnas:** 15
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `empleado` → `Empleado`
  - `atendido_por` → `Usuario`
  - `creado_por` → `Usuario`

#### `SolicitudRLCPD`
- **Columnas:** 22
- **Foreign Keys:** 4
- **Relaciones:** 7
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `paciente` → `PacienteRLCPD` ↔ solicitudes
  - `evaluadores` → `EvaluadorRLCPD` ↔ solicitud
  - `calificadores` → `CalificadorCIF` ↔ solicitud
  - `historial` → `HistorialEstadoRLCPD` ↔ solicitud
  - ... y 2 más

#### `SoporteEmpleadoProveedor`
- **Columnas:** 17
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `empleado` → `EmpleadoProveedor`
  - `exigencia` → `ExigenciaDocEmpleadoProveedor`
  - `revisor` → `Usuario`
  - `subidor` → `Usuario`

#### `SoporteProveedor`
- **Columnas:** 17
- **Foreign Keys:** 4
- **Relaciones:** 4
- **Relaciones Definidas:**
  - `proveedor` → `Proveedor`
  - `exigencia` → `ExigenciaDocProveedor`
  - `revisor` → `Usuario`
  - `subidor` → `Usuario`

#### `Usuario`
- **Columnas:** 15
- **Foreign Keys:** 4
- **Relaciones:** 6
- **Relaciones Definidas:**
  - `cliente` → `Cliente` ↔ usuarios
  - `clientes_asignados` → `Cliente`
  - `creado_por` → `Usuario`
  - `coordinador_superior_ref` → `Usuario`
  - `visitas_realizadas` → `Visita` ↔ asesor
  - ... y 1 más

#### `VehiculoPESV`
- **Columnas:** 15
- **Foreign Keys:** 2
- **Relaciones:** 5
- **Relaciones Definidas:**
  - `cliente` → `Cliente`
  - `pesv_rel` → `PESV` ↔ vehiculos
  - `conductores` → `ConductorPESV` ↔ vehiculo_principal
  - `inspecciones_preop` → `InspeccionPreOperacional` ↔ vehiculo
  - `siniestros` → `SiniestroPESV` ↔ vehiculo

#### `Visita`
- **Columnas:** 21
- **Foreign Keys:** 3
- **Relaciones:** 5
- **Relaciones Definidas:**
  - `cliente` → `Cliente` ↔ visitas
  - `asesor` → `Usuario` ↔ visitas_realizadas
  - `tipo_objetivo` → `TipoObjetivoVisita` ↔ visitas
  - `inspecciones` → `Inspeccion` ↔ visita
  - `fotos` → `FotoVisita` ↔ visita

### 🌐 Diagrama de Relaciones Centrales

#### 🏗️ Arquitectura de Entidades Principales

```
                           ┌─────────────────────┐
                           │      CLIENTE        │
                           │   (núcleo central)  │
                           │      7 rels         │
                           └──────────┬──────────┘
                                      │
              ┌───────────────────────┼───────────────────────┐
              │                       │                       │
    ┌─────────▼─────────┐   ┌─────────▼─────────┐   ┌─────────▼─────────┐
    │     USUARIO       │   │      VISITA       │   │     EMPLEADO      │
    │   autenticación   │   │ inspecciones SST  │   │  recursos humanos │
    │      4 rels       │   │      5 rels       │   │      3 rels       │
    └───────────────────┘   └─────────┬─────────┘   └───────────────────┘
                                      │
                            ┌─────────▼─────────┐
                            │    INSPECCION     │
                            │ sistema auditoría │
                            │      5 rels       │
                            └─────────┬─────────┘
                                      │
              ┌───────────────────────┼───────────────────────┐
              │                       │                       │
    ┌─────────▼─────────┐   ┌─────────▼─────────┐   ┌─────────▼─────────┐
    │ INSPECCION_BOTIQ  │   │ INSPECCION_EXTINT │   │ INSPECCION_GRAL   │
    │   botiquines SST  │   │ extintores segur. │   │ infraestructura   │
    │      2 rels       │   │      2 rels       │   │      2 rels       │
    └───────────────────┘   └───────────────────┘   └───────────────────┘
```

#### 🚗 Ecosistema PESV (Plan Estratégico Seguridad Vial)

```
                    ┌────────────────┐
                    │      PESV      │◄──────────────┐
                    │ plan maestro   │               │
                    │    4 rels      │               │
                    └────────┬───────┘               │
                             │                       │
        ┌────────────────────┼────────────────────┐  │
        │                    │                    │  │
  ┌─────▼─────┐    ┌─────────▼─────────┐    ┌─────▼──▼───┐
  │ VEHICULO  │    │    ACTIVIDAD      │    │ CONDUCTOR  │
  │   PESV    │    │      PESV         │    │    PESV    │
  │  5 rels   │    │     1 rel         │    │   4 rels   │
  └─────┬─────┘    └───────────────────┘    └─────┬──────┘
        │                                          │
        └──────────┐              ┌────────────────┘
                   │              │
              ┌────▼──────────────▼────┐
              │    SINIESTRO_PESV      │
              │ gestión de accidentes  │
              │       4 rels           │
              └────────────────────────┘
```

#### 🛡️ Flujo de Gestión de Incidentes SST

```
  ┌─────────────────┐       ┌──────────────────┐       ┌─────────────────┐
  │ REPORTE_INCIDENTE│──────▶│ ENCUESTA_INVEST. │──────▶│ ACCION_MEJORA   │
  │  evento inicial  │       │  investigación   │       │   correctiva    │
  │     6 rels       │       │     1 rel        │       │    2 rels       │
  └─────────┬───────┘       └──────────────────┘       └─────────────────┘
            │
            ▼
  ┌─────────────────┐
  │   AUDITORIA     │
  │ seguimiento ISO │
  │     3 rels      │
  └─────────────────┘
```

**📍 Leyenda Expandida:**
- **Números en paréntesis:** Cantidad de relaciones ORM definidas
- **`◄──`:** Relación bidireccional (back_populates)
- **`▼`:** Relación de herencia/dependencia (hijo → padre)
- **`──────▶`:** Flujo de proceso de negocio
- **Texto descriptivo:** Propósito funcional del modelo

### 📋 Tabla de Modelos Principales

| **Modelo** | **Columnas** | **FK** | **Relaciones** | **Descripción** |
|------------|:------------:|:------:|:--------------:|----------------|
| **Cliente** | 28 | 2 | 8 | Entidad central - Empresa u organización |
| **Usuario** | 15 | 4 | 6 | Usuarios del sistema con roles específicos |
| **Empleado** | 31 | 2 | 4 | Personal de la empresa y datos laborales |
| **ReporteIncidente** | 43 | 4 | 8 | Eventos e incidentes de trabajo |
| **AccionMejora** | 24 | 1 | 1 | Acciones correctivas y preventivas |
| **Visita** | 21 | 3 | 5 | Visitas de inspección SST |
| **Inspeccion** | 15 | 1 | 5 | Sistema de auditoría e inspecciones |
| **PESV** | 24 | 2 | 5 | Plan Estratégico de Seguridad Vial |
| **VehiculoPESV** | 15 | 2 | 5 | Vehículos corporativos y control |
| **DocumentoSST** | 22 | 1 | 4 | Control documental del sistema |

### 🔗 Grupos de Modelos por Dominio

#### 🏢 **Gestión Empresarial**
- `Cliente`, `Usuario`, `Empleado`, `Area`, `PerfilEmpresa`

#### 🛡️ **Sistema SST**
- `ReporteIncidente`, `AccionMejora`, `MatrizRiesgo`, `PoliticaSST`, `ObjetivoSST`

#### 🔍 **Inspecciones y Auditorías**
- `Visita`, `Inspeccion`, `InspeccionBotiquin`, `InspeccionExtintor`, `InspeccionCamilla`, `InspeccionGeneral`, `AuditoriaSST`

#### 🚗 **PESV (Plan Estratégico Seguridad Vial)**
- `PESV`, `VehiculoPESV`, `ConductorPESV`, `SiniestroPESV`, `InspeccionPreOperacional`

#### 📋 **Control Documental**
- `DocumentoSST`, `FirmaDocumento`, `DescargaDocumento`, `HistorialVersionDocumento`

#### 🩺 **Medicina Laboral**
- `ExamenMedico`, `EvaluacionMedica`, `Incapacidad`, `ResultadoPVE`, `RespuestaBateria`

#### 🎓 **Capacitación**
- `TemaCapacitacion`, `SesionCapacitacion`, `AsistenteCapacitacion`

#### 🤝 **Gestión Social**
- `ComiteConvivenciaLaboral`, `ExpedienteConvivencia`

#### 🔒 **Privacidad y Datos**
- `RegistroTratamiento`, `HistorialRetencion`

#### 📊 **Calidad ISO 9001**
- Modelos con prefijo `*SGC`: ProcesoSGC, RiesgoCalidad, EvaluacionProveedorCalidad




---

## Metadatos de Generación

- **Timestamp de Generación:** 21/04/2026 20:48:31
- **Total de Blueprints Analizados:** 42
- **Total de Modelos ORM Detectados:** 172
- **Total de Plantillas HTML Inventariadas:** 395
- **Script Generador:** `scripts/actualizar_app_md.py`

> **Nota:** Este documento es generado automáticamente. No editar manualmente.
> Para actualizar, ejecutar: `python scripts/actualizar_app_md.py`

