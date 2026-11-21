# NexoReq: Especificación de Requerimientos de Software

> - **Identificación:** NEXOREQ-ERS-002
> - **Versión:** 0.1
> - **Estado:** borrador para revisión
> - **Fecha:** 20 de noviembre de 2026
> - **Fuente principal:** `docs/01-vision-y-alcance.md`, versión 0.1 y estado borrador para revisión

## 1. Propósito

Este documento especifica los requisitos funcionales y no funcionales del producto mínimo viable (MVP) de NexoReq. Su objetivo es convertir la visión y el alcance documentados —todavía en revisión— en comportamientos verificables que orienten el diseño, la implementación y la validación posterior del backend y su API.

Esta especificación no sustituye el Documento 01. Ambos documentos permanecen como borradores para revisión. Durante su refinamiento, `docs/01-vision-y-alcance.md` es la fuente principal para el alcance, los actores, las exclusiones y las reglas RN-001 a RN-007; cualquier ambigüedad debe resolverse mediante revisión explícita y no se interpretará como una aprobación formal.

## 2. Alcance de la especificación

El MVP de NexoReq comprende:

- autenticación, administración básica de usuarios y autorización por roles;
- creación, consulta, actualización y archivo de proyectos;
- gestión de participantes y roles por proyecto;
- creación, edición, consulta y archivo de requisitos;
- revisión, aprobación y rechazo de requisitos;
- versionamiento de requisitos aprobados;
- búsqueda, filtros y resumen por estado;
- trazabilidad y auditoría de las acciones relevantes.

El producto se especifica como un backend con API HTTP/JSON. La interfaz de usuario no forma parte del alcance de este borrador y permanece como una decisión separada.

### 2.1. Convenciones

- La palabra **debe** expresa una obligación del MVP.
- Los identificadores `RF`, `RNF`, `HU`, `EA`, `DP` y `CP` son únicos dentro de este documento.
- Una decisión marcada como **pendiente** no puede asumirse como regla definitiva durante la implementación.
- Los identificadores `CP-RF-*`, `CP-RNF-*` y `CP-EA-*` representan casos de prueba **previstos y no implementados**.
- Las prioridades de las historias son relativas dentro del MVP; ninguna historia aquí definida se traslada por ello a una fase posterior.

## 3. Glosario

| Término | Definición |
| --- | --- |
| **MVP** | Producto mínimo viable propuesto en el Documento 01, todavía en revisión. |
| **Proyecto** | Contenedor de participantes, permisos y requisitos relacionados con una iniciativa de software. |
| **Participante** | Usuario asociado explícitamente a un proyecto con uno o más roles. |
| **Requisito** | Necesidad o condición de software registrada y gestionada por NexoReq. |
| **Requisito funcional** | Requisito que describe un comportamiento o capacidad esperada del software. |
| **Requisito no funcional** | Requisito que establece una cualidad, restricción o condición operativa. |
| **Responsable** | Participante encargado de mantener o dar seguimiento a un requisito. |
| **Validador** | Participante autorizado para aprobar o rechazar un requisito. |
| **Estado de validación** | Uno de los estados `Borrador`, `En revisión`, `Aprobado` o `Rechazado`. |
| **Archivado** | Condición independiente del estado de validación que deja el recurso en solo lectura. |
| **Versión** | Representación identificada de un requisito. La versión vigente en `Borrador` puede editarse; una versión aprobada que haya sido reemplazada y toda versión histórica son inmutables. |
| **Versión vigente** | Versión actual sobre la que se consulta el estado y, cuando corresponde, se realizan cambios. |
| **Decisión de validación** | Aprobación o rechazo registrado con actor, fecha, hora y observaciones aplicables. |
| **Evento de auditoría** | Registro inmutable de una acción relevante para la trazabilidad. |
| **Regla de negocio (RN)** | Restricción de dominio documentada en el Documento 01. |
| **Requisito funcional (RF)** | Comportamiento observable que el sistema debe proporcionar. |
| **Requisito no funcional (RNF)** | Condición de calidad o restricción técnica verificable. |
| **Historia de usuario (HU)** | Necesidad expresada desde la perspectiva de un actor y acompañada de criterios de aceptación. |
| **Escenario de aceptación (EA)** | Ejemplo verificable en formato Dado/Cuando/Entonces. |
| **Caso de prueba previsto (CP)** | Referencia planificada para una prueba futura; no acredita una prueba implementada ni ejecutada. |

## 4. Actores y permisos propuestos

### 4.1. Actores

| Actor | Responsabilidad en el MVP |
| --- | --- |
| **Administrador del sistema** | Gestionar usuarios, roles globales y acceso; puede operar sobre proyectos conforme a RN-001. |
| **Líder de proyecto / analista de requisitos** | Crear y mantener proyectos, administrar participantes, gestionar requisitos y solicitar revisiones. |
| **Colaborador del proyecto** | Proponer y complementar requisitos según los permisos asignados. |
| **Validador / persona interesada** | Aprobar o rechazar requisitos del proyecto cuando esté autorizado. |
| **Consultor / auditor** | Consultar proyectos, requisitos, versiones, decisiones y auditoría sin modificar información. |

Una persona puede tener varios roles, pero cada asignación debe ser explícita por proyecto. El modelo definitivo de roles y la autoridad para asignarlos permanecen pendientes en DP-001.

### 4.2. Matriz de permisos propuesta

| Operación | Administrador | Líder / analista | Colaborador | Validador | Consultor / auditor |
| --- | --- | --- | --- | --- | --- |
| Administrar usuarios | Sí | No | No | No | No |
| Crear proyectos | Sí | Sí | No | No | No |
| Actualizar o archivar un proyecto | Sí | Sí, en sus proyectos | No | No | No |
| Gestionar participantes y roles | Sí | Sí, en sus proyectos | No | No | No |
| Consultar un proyecto autorizado | Sí | Sí | Sí | Sí | Sí |
| Crear requisitos | Sí | Sí | Según permiso explícito | No | No |
| Editar borradores | Sí | Sí | Según permiso explícito | No | No |
| Enviar a revisión | Sí | Sí | Según permiso explícito | No | No |
| Aprobar o rechazar | Solo si también es validador | Solo si también es validador | Solo si también es validador | Sí | No |
| Crear una nueva versión de un requisito aprobado | Sí | Sí | No | No | No |
| Archivar requisitos | Sí | Sí | Según permiso explícito | No | No |
| Consultar versiones y decisiones | Sí | Sí | Sí | Sí | Sí |
| Consultar auditoría | Sí | Sí, en sus proyectos | No | No | Sí, en sus proyectos |

La política sobre la aprobación de un requisito por la misma persona que lo creó o editó no se define en esta matriz; permanece pendiente en DP-005.

## 5. Reglas de negocio preservadas

| Código | Regla |
| --- | --- |
| **RN-001** | Todo requisito debe pertenecer a un único proyecto. Solo los integrantes autorizados del proyecto, o un administrador del sistema, podrán consultarlo u operarlo. Al archivar un proyecto, sus requisitos se conservarán en modo de solo lectura. |
| **RN-002** | Al crear un requisito, el sistema debe asignarle un código único e inmutable dentro del proyecto. El código seguirá siendo válido aunque el requisito cambie de estado o sea archivado. |
| **RN-003** | Un borrador debe tener, como mínimo, título, descripción, tipo y prioridad. Para enviarlo a revisión también debe indicar fuente, responsable, criterios de aceptación y validador. |
| **RN-004** | El flujo ordinario será `Borrador` → `En revisión` → `Aprobado` o `Rechazado`. Un requisito rechazado podrá volver a `Borrador` para ser corregido y uno aprobado solo podrá volver a `Borrador` al generar una nueva versión según la RN-006. `Archivado` será una condición independiente del estado de validación y bloqueará nuevas transiciones. Solo se permitirán cambios ejecutados por roles autorizados. |
| **RN-005** | Solo un validador autorizado podrá aprobar o rechazar un requisito. Toda decisión debe registrar persona, fecha y hora; el rechazo debe incluir una observación obligatoria. |
| **RN-006** | Un cambio de contenido en un requisito aprobado debe generar una nueva versión, conservar la versión previamente aprobada y devolver el requisito a `Borrador` para una nueva validación. |
| **RN-007** | Los requisitos no se eliminarán físicamente en el MVP. Deberán archivarse y quedar en modo de solo lectura, conservando su código, versiones, cambios de estado y decisiones para fines de trazabilidad y auditoría. |

## 6. Requisitos funcionales

### 6.1. Identidad y acceso

| ID | Requisito funcional |
| --- | --- |
| **RF-001** | El sistema debe autenticar a un usuario activo antes de permitirle acceder a cualquier operación protegida de la API. |
| **RF-002** | El sistema debe rechazar la autenticación con credenciales inválidas y la de usuarios inactivos sin conceder una sesión o identidad autenticada. |
| **RF-003** | El administrador debe poder registrar usuarios y consultar sus datos básicos y estado de activación. |
| **RF-004** | El administrador debe poder activar o desactivar un usuario; la desactivación debe bloquear nuevos accesos sin borrar su historial ni su autoría previa. |
| **RF-005** | El sistema debe permitir asignar explícitamente uno o más roles a un usuario dentro de cada proyecto y mantener independientes las asignaciones de proyectos distintos. |
| **RF-006** | El sistema debe autorizar cada operación protegida con base en el estado del usuario, su pertenencia al proyecto y los roles que tenga asignados. |

### 6.2. Proyectos

| ID | Requisito funcional |
| --- | --- |
| **RF-007** | Un actor autorizado debe poder crear un proyecto con un nombre identificable y una descripción opcional; el proyecto debe quedar activo al crearse. |
| **RF-008** | Un participante autorizado debe poder consultar los datos y la condición activa o archivada de un proyecto. |
| **RF-009** | El líder o administrador autorizado debe poder actualizar el nombre y la descripción de un proyecto activo. |
| **RF-010** | El líder o administrador autorizado debe poder archivar un proyecto sin eliminarlo físicamente. |
| **RF-011** | El líder o administrador autorizado debe poder asociar o retirar participantes y asignar o retirar sus roles dentro de un proyecto activo. |
| **RF-012** | Al archivarse un proyecto, el sistema debe conservar sus participantes, requisitos, versiones, decisiones y auditoría en modo de solo lectura, sin alterar por ese hecho el estado de validación ni la condición individual de archivo de sus requisitos. |

### 6.3. Requisitos

| ID | Requisito funcional |
| --- | --- |
| **RF-013** | Un actor autorizado debe poder crear un requisito dentro de un único proyecto activo; el requisito debe iniciar en estado `Borrador` y sin condición de archivo. |
| **RF-014** | Al crear un requisito, el sistema debe generar un código único dentro del proyecto y mantenerlo inmutable durante todo su ciclo de vida; su formato queda pendiente en DP-002. |
| **RF-015** | El sistema debe almacenar para cada requisito código, título, descripción, tipo, prioridad, fuente, responsable, criterios de aceptación, validador, estado de validación y condición de archivo. |
| **RF-016** | Para guardar un borrador, el sistema debe exigir título, descripción, tipo y prioridad e informar de forma identificable cualquier campo obligatorio ausente o inválido. |
| **RF-017** | El sistema debe permitir clasificar un requisito como funcional o no funcional; los catálogos definitivos permanecen pendientes en DP-004. |
| **RF-018** | Un actor autorizado debe poder editar el contenido de un requisito en estado `Borrador`, siempre que ni el requisito ni su proyecto estén archivados. Un requisito `Rechazado` debe volver primero a `Borrador` mediante RF-028. |
| **RF-019** | El sistema debe permitir asignar responsable y validador entre los participantes autorizados del proyecto y debe impedir asignaciones sin el rol o permiso explícito requerido. La correspondencia definitiva entre roles y permisos permanece pendiente en DP-001. |
| **RF-020** | Antes del envío a revisión, el sistema debe comprobar que el requisito contiene fuente, responsable, criterios de aceptación y validador, además de los campos mínimos del borrador. |
| **RF-021** | Un actor autorizado debe poder archivar un requisito sin eliminarlo físicamente y sin perder su código, estado, versiones, decisiones ni auditoría. |
| **RF-022** | Un requisito archivado debe permanecer consultable para actores autorizados y debe rechazar toda edición o transición de validación. |

### 6.4. Validación

| ID | Requisito funcional |
| --- | --- |
| **RF-023** | Un actor autorizado debe poder enviar un requisito completo de `Borrador` a `En revisión` cuando el requisito y su proyecto no estén archivados. |
| **RF-024** | El sistema debe aceptar únicamente las transiciones de estado definidas en RN-004 y rechazar cualquier otra sin modificar el estado vigente. |
| **RF-025** | Un validador autorizado debe poder aprobar un requisito que se encuentre `En revisión`; el nuevo estado debe ser `Aprobado`. |
| **RF-026** | Un validador autorizado debe poder rechazar un requisito que se encuentre `En revisión`; el nuevo estado debe ser `Rechazado` y la observación debe ser obligatoria. |
| **RF-027** | Toda aprobación o rechazo debe registrar el resultado, el validador, la fecha y hora y, cuando exista, la observación, asociándolos con la versión evaluada. |
| **RF-028** | Un actor autorizado debe poder devolver un requisito `Rechazado` a `Borrador` para corregirlo, conservando la decisión de rechazo previa. |
| **RF-029** | El sistema debe bloquear el envío, la aprobación, el rechazo y el retorno a borrador cuando el requisito o su proyecto estén archivados. |

### 6.5. Versiones

Versiones se considera una subcapacidad del módulo lógico de Requisitos; no implica un servicio desplegable separado.

| ID | Requisito funcional |
| --- | --- |
| **RF-030** | Cuando un actor autorizado solicite modificar el contenido de un requisito `Aprobado`, el sistema debe crear una nueva versión en lugar de alterar la versión aprobada. |
| **RF-031** | La versión previamente aprobada debe conservarse inmutable con su estado, contenido y decisión de validación. |
| **RF-032** | La nueva versión debe conservar el código del requisito, convertirse en la versión vigente y quedar en estado `Borrador` para una nueva validación. |
| **RF-033** | El sistema debe identificar cada versión, mantener su orden y registrar el actor y la fecha y hora de su creación sin fijar en este documento un formato de numeración. |
| **RF-034** | Un actor autorizado debe poder consultar la versión vigente y el historial de versiones anteriores con sus estados y decisiones asociadas. |

### 6.6. Consulta

Consulta se considera una subcapacidad del módulo lógico de Trazabilidad y consulta.

| ID | Requisito funcional |
| --- | --- |
| **RF-035** | Un actor autorizado debe poder listar los requisitos de un proyecto al que tenga acceso. |
| **RF-036** | Un actor autorizado debe poder consultar el detalle de un requisito, incluida su versión vigente, estado de validación y condición de archivo. |
| **RF-037** | El sistema debe permitir buscar requisitos por texto sobre, como mínimo, código, título y descripción dentro de los proyectos autorizados. |
| **RF-038** | El sistema debe permitir combinar filtros por estado, tipo, prioridad y responsable. |
| **RF-039** | El sistema debe presentar un resumen de cantidades por estado de validación y distinguir los requisitos archivados de los no archivados. |
| **RF-040** | Toda lista, búsqueda, detalle o resumen debe excluir información de proyectos para los que el usuario no tenga autorización. |

### 6.7. Auditoría

Auditoría se considera una subcapacidad del módulo lógico de Trazabilidad y consulta.

| ID | Requisito funcional |
| --- | --- |
| **RF-041** | El sistema debe generar eventos de auditoría para altas, cambios y archivos de proyectos o requisitos; cambios de participantes o roles; activación o desactivación de usuarios; transiciones de estado; decisiones de validación y creación de versiones. |
| **RF-042** | Cada evento de auditoría debe registrar al menos actor, fecha y hora, acción, tipo e identificador de entidad, proyecto relacionado y resultado. |
| **RF-043** | La escritura del evento de auditoría debe formar parte de la misma operación lógica que la acción auditada, de modo que una acción definida como auditable no se confirme sin su registro. |
| **RF-044** | Los usuarios del producto no deben poder editar ni eliminar eventos de auditoría mediante la API del MVP. |
| **RF-045** | Un administrador o auditor autorizado debe poder consultar eventos por proyecto o entidad y filtrarlos por acción, actor o intervalo de fechas. |
| **RF-046** | La auditoría debe conservar la atribución histórica cuando un usuario sea desactivado o retirado de un proyecto y cuando un proyecto o requisito sea archivado; el plazo de retención queda pendiente en DP-008. |

## 7. Requisitos no funcionales

### 7.1. Seguridad

| ID | Requisito no funcional |
| --- | --- |
| **RNF-001** | Toda operación protegida debe requerir una identidad autenticada y activa. |
| **RNF-002** | La autorización debe ejecutarse en el servidor para cada operación y denegar por defecto cualquier permiso no concedido expresamente. |
| **RNF-003** | Las credenciales, claves y secretos no deben almacenarse en el código fuente, el repositorio ni los registros de aplicación; deben suministrarse mediante configuración externa. |
| **RNF-004** | Si se adoptan contraseñas locales, nunca deben almacenarse en texto claro y deben protegerse con un mecanismo criptográfico vigente; la estrategia concreta permanece pendiente en DP-003. |
| **RNF-005** | Las respuestas y los registros no deben exponer credenciales, secretos ni detalles internos innecesarios para resolver un error. |

### 7.2. Integridad

| ID | Requisito no funcional |
| --- | --- |
| **RNF-006** | La persistencia debe impedir requisitos sin proyecto, códigos duplicados dentro de un proyecto y referencias a participantes inexistentes. |
| **RNF-007** | Las transiciones de estado, decisiones, versiones y eventos de auditoría relacionados deben persistirse de forma transaccional. |
| **RNF-008** | La inmutabilidad del código, las versiones históricas, las decisiones y la auditoría debe protegerse tanto en la lógica de aplicación como en la persistencia cuando sea aplicable. |
| **RNF-009** | Las actualizaciones concurrentes no deben sobrescribir silenciosamente cambios confirmados; el sistema debe detectar el conflicto y devolver un error identificable. |

### 7.3. Rendimiento

| ID | Requisito no funcional |
| --- | --- |
| **RNF-010** | Los listados de usuarios, proyectos, requisitos y eventos de auditoría deben ser paginados y no deben devolver colecciones sin un límite configurable. |
| **RNF-011** | El rendimiento debe medirse con un perfil de carga y volumen de datos reproducible, una vez se aprueben las métricas y umbrales pendientes en DP-007. |
| **RNF-012** | La aceptación de rendimiento no debe darse por cerrada hasta documentar y aprobar métricas, umbrales, datos de prueba y evidencia de medición; este requisito está pendiente de cuantificación. |

### 7.4. Mantenibilidad

| ID | Requisito no funcional |
| --- | --- |
| **RNF-013** | La solución debe implementarse como monolito modular con Java 21, Spring Boot y MySQL, y desplegarse inicialmente como una sola aplicación. |
| **RNF-014** | Los módulos Identidad y acceso, Proyectos, Requisitos, Validación y Trazabilidad y consulta deben tener límites explícitos y no deben acceder directamente a las tablas internas de otro módulo. |
| **RNF-015** | Cada módulo debe separar responsabilidades de entrada, aplicación, dominio e infraestructura. |
| **RNF-016** | Todo cambio de esquema debe gestionarse mediante migraciones versionadas y reproducibles. |
| **RNF-017** | Los registros técnicos deben usar información estructurada suficiente para correlacionar una solicitud y diagnosticar fallos sin incluir secretos. |

### 7.5. Usabilidad de la API

| ID | Requisito no funcional |
| --- | --- |
| **RNF-018** | La API debe usar HTTP y representaciones JSON en UTF-8 de forma consistente. |
| **RNF-019** | Los errores deben devolver una estructura uniforme con código estable, mensaje comprensible y detalles de validación por campo cuando correspondan. |
| **RNF-020** | El contrato OpenAPI debe documentar operaciones, esquemas, autenticación, permisos, filtros, paginación, respuestas y errores, y mantenerse sincronizado con la implementación. |
| **RNF-021** | Las fechas y horas expuestas por la API deben usar una representación inequívoca con zona horaria, documentada en el contrato. |

### 7.6. Pruebas

| ID | Requisito no funcional |
| --- | --- |
| **RNF-022** | Cada módulo debe contar con pruebas unitarias de sus reglas de dominio y casos de aplicación críticos. |
| **RNF-023** | Deben existir pruebas de integración para persistencia, autorización, transiciones, validación, versionamiento y auditoría. |
| **RNF-024** | Los escenarios de aceptación de este documento deben transformarse en pruebas reproducibles antes de aceptar el MVP; esto es una práctica de calidad y no incorpora al producto un módulo de gestión de pruebas. |

### 7.7. Trazabilidad

| ID | Requisito no funcional |
| --- | --- |
| **RNF-025** | Los identificadores de requisitos, reglas, historias, escenarios y casos de prueba previstos deben mantenerse únicos y estables durante la versión 0.1. |
| **RNF-026** | Todo cambio de alcance debe actualizar la matriz de trazabilidad y someterse a revisión antes de declararse parte del MVP. |
| **RNF-027** | La evidencia futura de pruebas debe identificar de forma explícita los requisitos y escenarios que verifica. |

### 7.8. Despliegue

| ID | Requisito no funcional |
| --- | --- |
| **RNF-028** | El backend debe producir un único artefacto desplegable que utilice una instancia MySQL y aplique migraciones de esquema controladas. |
| **RNF-029** | La configuración dependiente del entorno debe suministrarse externamente y separarse del artefacto desplegable. |
| **RNF-030** | La aplicación debe exponer un mecanismo de comprobación de salud que no revele secretos ni datos sensibles. |
| **RNF-031** | El procedimiento de configuración, migración, inicio y verificación del despliegue debe ser reproducible y estar documentado. |
| **RNF-032** | El entorno objetivo, las restricciones operativas y el objetivo cuantificado de disponibilidad permanecen pendientes en DP-007 y DP-009 y deben aprobarse antes de la aceptación operativa. |

## 8. Historias de usuario

### HU-001 — Autenticarse

- **Rol:** usuario registrado.
- **Objetivo:** autenticarme en NexoReq.
- **Beneficio:** acceder únicamente a las capacidades y proyectos autorizados.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-001-01:** un usuario activo con credenciales válidas obtiene una identidad autenticada.
  - **CA-HU-001-02:** credenciales inválidas no conceden acceso.
  - **CA-HU-001-03:** un usuario inactivo no puede autenticarse.

### HU-002 — Administrar usuarios

- **Rol:** administrador del sistema.
- **Objetivo:** registrar, consultar, activar y desactivar usuarios.
- **Beneficio:** controlar quién puede acceder sin perder atribución histórica.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-002-01:** el administrador puede registrar y consultar un usuario.
  - **CA-HU-002-02:** la desactivación impide nuevos accesos.
  - **CA-HU-002-03:** la desactivación no elimina acciones ni auditoría anteriores.

### HU-003 — Gestionar un proyecto

- **Rol:** líder de proyecto / analista de requisitos.
- **Objetivo:** crear, consultar y actualizar un proyecto.
- **Beneficio:** disponer de un espacio único para organizar el alcance.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-003-01:** un actor autorizado crea un proyecto activo con nombre.
  - **CA-HU-003-02:** un participante autorizado consulta el proyecto.
  - **CA-HU-003-03:** solo un actor autorizado actualiza un proyecto activo.

### HU-004 — Gestionar participantes

- **Rol:** líder de proyecto / analista de requisitos.
- **Objetivo:** asociar participantes y asignarles roles explícitos.
- **Beneficio:** delimitar responsabilidades y permisos por proyecto.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-004-01:** el líder puede asociar o retirar participantes en un proyecto activo.
  - **CA-HU-004-02:** cada rol queda asociado al usuario y al proyecto correspondiente.
  - **CA-HU-004-03:** los permisos de un proyecto no conceden acceso automático a otro.

### HU-005 — Registrar un requisito

- **Rol:** líder, analista o colaborador autorizado.
- **Objetivo:** registrar un requisito como borrador.
- **Beneficio:** centralizar una necesidad con información estructurada.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-005-01:** el requisito se crea en un único proyecto activo y en estado `Borrador`.
  - **CA-HU-005-02:** el código se genera y es único dentro del proyecto.
  - **CA-HU-005-03:** no se guarda el borrador si falta título, descripción, tipo o prioridad.

### HU-006 — Enviar a revisión

- **Rol:** líder, analista o colaborador autorizado.
- **Objetivo:** enviar un requisito completo a revisión.
- **Beneficio:** iniciar una decisión formal de validación.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-006-01:** un requisito completo pasa de `Borrador` a `En revisión`.
  - **CA-HU-006-02:** si falta fuente, responsable, criterios de aceptación o validador, el estado no cambia y se informan los campos faltantes.
  - **CA-HU-006-03:** un recurso archivado no puede enviarse a revisión.

### HU-007 — Aprobar un requisito

- **Rol:** validador autorizado.
- **Objetivo:** aprobar un requisito en revisión.
- **Beneficio:** dejar evidencia del alcance aceptado.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-007-01:** solo un validador autorizado puede aprobar.
  - **CA-HU-007-02:** la aprobación cambia `En revisión` a `Aprobado`.
  - **CA-HU-007-03:** la decisión registra validador, fecha, hora y versión evaluada.

### HU-008 — Rechazar un requisito

- **Rol:** validador autorizado.
- **Objetivo:** rechazar un requisito con una observación.
- **Beneficio:** comunicar de forma trazable qué debe corregirse.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-008-01:** solo un validador autorizado puede rechazar.
  - **CA-HU-008-02:** sin observación no se confirma el rechazo.
  - **CA-HU-008-03:** el rechazo cambia `En revisión` a `Rechazado` y conserva la decisión.

### HU-009 — Corregir un requisito rechazado

- **Rol:** líder, analista o colaborador autorizado.
- **Objetivo:** devolver un requisito rechazado a borrador y corregirlo.
- **Beneficio:** atender la observación sin perder la decisión previa.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-009-01:** un actor autorizado puede pasar de `Rechazado` a `Borrador`.
  - **CA-HU-009-02:** la decisión de rechazo permanece consultable.
  - **CA-HU-009-03:** después de corregirlo, debe cumplirse nuevamente la validación de envío.

### HU-010 — Versionar un requisito aprobado

- **Rol:** administrador del sistema o líder / analista autorizado.
- **Objetivo:** modificar un requisito aprobado mediante una nueva versión.
- **Beneficio:** evolucionar el alcance sin perder la aprobación anterior.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-010-01:** la versión aprobada anterior no se modifica.
  - **CA-HU-010-02:** la nueva versión conserva el código y queda vigente en `Borrador`.
  - **CA-HU-010-03:** ambas versiones y la decisión anterior permanecen consultables.

### HU-011 — Archivar un requisito

- **Rol:** líder, analista o actor autorizado.
- **Objetivo:** archivar un requisito sin eliminarlo.
- **Beneficio:** retirar contenido del trabajo activo conservando la trazabilidad.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-011-01:** el archivo no cambia el estado de validación.
  - **CA-HU-011-02:** el requisito archivado queda en solo lectura.
  - **CA-HU-011-03:** código, versiones, decisiones y auditoría permanecen consultables.

### HU-012 — Archivar un proyecto

- **Rol:** líder de proyecto o administrador autorizado.
- **Objetivo:** archivar un proyecto completo.
- **Beneficio:** cerrar su operación sin perder información histórica.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-012-01:** el proyecto y sus requisitos quedan en solo lectura.
  - **CA-HU-012-02:** los estados y marcas individuales de archivo de los requisitos no cambian por el archivo del proyecto.
  - **CA-HU-012-03:** participantes, versiones, decisiones y auditoría se conservan.

### HU-013 — Buscar y filtrar requisitos

- **Rol:** participante autorizado.
- **Objetivo:** buscar y filtrar requisitos de mis proyectos.
- **Beneficio:** localizar rápidamente información relevante.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-013-01:** la búsqueda considera como mínimo código, título y descripción.
  - **CA-HU-013-02:** los filtros de estado, tipo, prioridad y responsable se pueden combinar.
  - **CA-HU-013-03:** ningún resultado pertenece a un proyecto no autorizado.

### HU-014 — Consultar el resumen por estado

- **Rol:** líder, analista o participante autorizado.
- **Objetivo:** consultar cantidades de requisitos por estado.
- **Beneficio:** conocer la situación general del alcance.
- **Prioridad:** media, MVP.
- **Criterios de aceptación:**
  - **CA-HU-014-01:** el resumen distingue los cuatro estados de validación.
  - **CA-HU-014-02:** el resumen diferencia requisitos archivados y no archivados.
  - **CA-HU-014-03:** solo incluye proyectos autorizados.

### HU-015 — Consultar historial y auditoría

- **Rol:** consultor / auditor autorizado.
- **Objetivo:** consultar versiones, decisiones y eventos de auditoría.
- **Beneficio:** reconstruir quién cambió qué y cuándo.
- **Prioridad:** alta, MVP.
- **Criterios de aceptación:**
  - **CA-HU-015-01:** el historial muestra versiones y decisiones sin permitir su modificación.
  - **CA-HU-015-02:** cada evento informa actor, fecha y hora, acción, entidad, proyecto y resultado.
  - **CA-HU-015-03:** los filtros de auditoría no exponen proyectos no autorizados.

### HU-016 — Consultar en modo de solo lectura

- **Rol:** consultor / auditor.
- **Objetivo:** consultar proyectos y requisitos sin modificarlos.
- **Beneficio:** revisar la evidencia sin riesgo de alterar la información registrada.
- **Prioridad:** media, MVP.
- **Criterios de aceptación:**
  - **CA-HU-016-01:** el actor accede únicamente a proyectos autorizados.
  - **CA-HU-016-02:** puede consultar detalle, historial y decisiones.
  - **CA-HU-016-03:** cualquier intento de escritura es rechazado.

## 9. Escenarios de aceptación principales

### EA-001 — Autenticación de usuario activo

- **Dado** un usuario activo con credenciales válidas,
- **Cuando** solicita autenticarse,
- **Entonces** el sistema reconoce su identidad y limita el acceso a sus permisos.

### EA-002 — Acceso de usuario inactivo

- **Dado** un usuario desactivado,
- **Cuando** intenta autenticarse,
- **Entonces** el sistema rechaza el acceso y conserva intacta su atribución histórica.

### EA-003 — Aislamiento entre proyectos

- **Dado** un usuario que participa en el proyecto A pero no en el proyecto B,
- **Cuando** consulta un recurso del proyecto B,
- **Entonces** el sistema rechaza la operación y no devuelve información del recurso.

### EA-004 — Gestión de participantes

- **Dado** un proyecto activo y un líder autorizado,
- **Cuando** asocia un usuario y le asigna un rol,
- **Entonces** el sistema registra la membresía y aplica el rol solo en ese proyecto.

### EA-005 — Creación de un requisito

- **Dado** un proyecto activo y un actor autorizado,
- **Cuando** registra título, descripción, tipo y prioridad válidos,
- **Entonces** el sistema crea un requisito `Borrador` con un código único e inmutable dentro del proyecto.

### EA-006 — Envío a revisión completo e incompleto

- **Dado** un borrador al que le falta al menos fuente, responsable, criterios de aceptación o validador,
- **Cuando** un actor autorizado intenta enviarlo a revisión,
- **Entonces** el sistema rechaza el envío, informa los campos faltantes y conserva el estado `Borrador`;
- **Y dado** un borrador con todos los campos exigidos por RN-003,
- **Cuando** un actor autorizado lo envía a revisión,
- **Entonces** el sistema cambia su estado a `En revisión`.

### EA-007 — Aprobación

- **Dado** un requisito completo `En revisión` y un validador autorizado,
- **Cuando** el validador lo aprueba,
- **Entonces** el sistema lo deja `Aprobado` y registra validador, fecha, hora y versión evaluada.

### EA-008 — Rechazo con observación obligatoria

- **Dado** un requisito `En revisión` y un validador autorizado,
- **Cuando** intenta rechazarlo sin observación,
- **Entonces** el sistema no cambia el estado;
- **Y cuando** proporciona una observación y confirma el rechazo,
- **Entonces** el sistema deja el requisito `Rechazado` y conserva la decisión completa.

### EA-009 — Corrección posterior al rechazo

- **Dado** un requisito `Rechazado` no archivado,
- **Cuando** un actor autorizado solicita corregirlo,
- **Entonces** el sistema lo devuelve a `Borrador` sin eliminar la decisión de rechazo previa.

### EA-010 — Nueva versión de un requisito aprobado

- **Dado** un requisito `Aprobado` no archivado,
- **Cuando** un actor autorizado solicita modificar su contenido,
- **Entonces** el sistema conserva inmutable la versión aprobada y crea una nueva versión vigente en `Borrador` con el mismo código.

### EA-011 — Archivo de un requisito

- **Dado** un requisito no archivado,
- **Cuando** un actor autorizado lo archiva,
- **Entonces** el sistema conserva su estado de validación, lo deja en solo lectura y mantiene código, versiones, decisiones y auditoría.

### EA-012 — Archivo de un proyecto

- **Dado** un proyecto activo,
- **Cuando** un líder o administrador autorizado lo archiva,
- **Entonces** el proyecto y sus requisitos quedan en solo lectura sin cambiar el estado ni la condición individual de archivo de cada requisito.

### EA-013 — Búsqueda y filtros

- **Dado** un participante autorizado con requisitos en su proyecto,
- **Cuando** combina una búsqueda textual con filtros de estado y prioridad,
- **Entonces** recibe resultados paginados que satisfacen todos los criterios y no incluyen proyectos no autorizados.

### EA-014 — Consulta de auditoría

- **Dado** un auditor autorizado,
- **Cuando** consulta los eventos de un requisito,
- **Entonces** obtiene eventos inmutables con actor, fecha y hora, acción, entidad, proyecto y resultado, incluidos los asociados a usuarios desactivados.

## 10. Flujo de estados y archivo

### 10.1. Estados de validación

```text
Borrador ──enviar a revisión──> En revisión ──aprobar──> Aprobado
    ^                               │
    │                               └──rechazar──> Rechazado
    │                                                │
    └────────────────corregir────────────────────────┘

Aprobado ──crear nueva versión──> Borrador (nueva versión vigente)
```

| Origen | Destino | Condición mínima | Actor propuesto |
| --- | --- | --- | --- |
| `Borrador` | `En revisión` | Campos de RN-003 completos; requisito y proyecto no archivados. | Líder, analista o colaborador autorizado. |
| `En revisión` | `Aprobado` | Validador autorizado y recurso no archivado. | Validador. |
| `En revisión` | `Rechazado` | Validador autorizado, observación obligatoria y recurso no archivado. | Validador. |
| `Rechazado` | `Borrador` | Recurso no archivado; se conserva la decisión previa. | Líder, analista o colaborador autorizado. |
| `Aprobado` | `Borrador` de una nueva versión | Solicitud de cambio de contenido; se conserva inmutable la versión aprobada. | Administrador, líder o analista autorizado. |

No se permiten otras transiciones en la versión 0.1.

### 10.2. Condiciones de archivo

- `Archivado` es una condición ortogonal al estado de validación; no es un quinto estado.
- Archivar un requisito no cambia su estado ni elimina información.
- Un requisito archivado es de solo lectura y no admite transiciones.
- Archivar un proyecto deja el proyecto y todos sus requisitos en solo lectura, pero no cambia los estados ni las marcas individuales de archivo.
- El archivo conserva código, versiones, decisiones, participantes y auditoría.
- La eliminación física no está disponible en el MVP.
- La reactivación o el desarchivo no están definidos y permanecen pendientes en DP-011.

## 11. Casos de prueba previstos

Los siguientes identificadores son elementos de planificación. En la versión 0.1 no representan archivos de prueba existentes, ejecuciones ni resultados aprobados.

| Rango | Cobertura prevista | Estado |
| --- | --- | --- |
| `CP-RF-001` a `CP-RF-046` | Un caso funcional base por cada RF del mismo número. | Previsto, no implementado. |
| `CP-RNF-001` a `CP-RNF-032` | Una verificación no funcional base por cada RNF del mismo número. | Previsto, no implementado. |
| `CP-EA-001` a `CP-EA-014` | Un caso de aceptación por cada escenario EA del mismo número. | Previsto, no implementado. |

La especificación detallada de datos, precondiciones, pasos y resultados de cada CP será un entregable posterior de calidad y deberá mantener los vínculos de las matrices siguientes.

## 12. Matriz de trazabilidad funcional

| RF | Módulo lógico | HU relacionadas | RN relacionadas | EA relacionadas | Caso previsto |
| --- | --- | --- | --- | --- | --- |
| RF-001 | Identidad y acceso | HU-001 | — | EA-001 | CP-RF-001 |
| RF-002 | Identidad y acceso | HU-001 | — | EA-002 | CP-RF-002 |
| RF-003 | Identidad y acceso | HU-002 | — | — | CP-RF-003 |
| RF-004 | Identidad y acceso | HU-002 | — | EA-002 | CP-RF-004 |
| RF-005 | Identidad y acceso | HU-004 | RN-001 | EA-004 | CP-RF-005 |
| RF-006 | Identidad y acceso | HU-001, HU-004, HU-016 | RN-001 | EA-001, EA-003 | CP-RF-006 |
| RF-007 | Proyectos | HU-003 | — | — | CP-RF-007 |
| RF-008 | Proyectos | HU-003, HU-016 | RN-001 | EA-003 | CP-RF-008 |
| RF-009 | Proyectos | HU-003 | — | — | CP-RF-009 |
| RF-010 | Proyectos | HU-012 | RN-001 | EA-012 | CP-RF-010 |
| RF-011 | Proyectos | HU-004 | RN-001 | EA-004 | CP-RF-011 |
| RF-012 | Proyectos | HU-012, HU-016 | RN-001 | EA-012 | CP-RF-012 |
| RF-013 | Requisitos | HU-005 | RN-001, RN-004 | EA-005 | CP-RF-013 |
| RF-014 | Requisitos | HU-005 | RN-002 | EA-005 | CP-RF-014 |
| RF-015 | Requisitos | HU-005, HU-006 | RN-003 | EA-005, EA-006 | CP-RF-015 |
| RF-016 | Requisitos | HU-005 | RN-003 | EA-005 | CP-RF-016 |
| RF-017 | Requisitos | HU-005 | RN-003 | EA-005 | CP-RF-017 |
| RF-018 | Requisitos | HU-005, HU-009 | RN-004 | EA-009 | CP-RF-018 |
| RF-019 | Requisitos | HU-004, HU-006 | RN-003, RN-005 | EA-004, EA-006 | CP-RF-019 |
| RF-020 | Requisitos | HU-006 | RN-003 | EA-006 | CP-RF-020 |
| RF-021 | Requisitos | HU-011 | RN-007 | EA-011 | CP-RF-021 |
| RF-022 | Requisitos | HU-011, HU-016 | RN-004, RN-007 | EA-011 | CP-RF-022 |
| RF-023 | Validación | HU-006 | RN-003, RN-004 | EA-006 | CP-RF-023 |
| RF-024 | Validación | HU-006, HU-007, HU-008, HU-009 | RN-004 | EA-007, EA-008, EA-009 | CP-RF-024 |
| RF-025 | Validación | HU-007 | RN-004, RN-005 | EA-007 | CP-RF-025 |
| RF-026 | Validación | HU-008 | RN-004, RN-005 | EA-008 | CP-RF-026 |
| RF-027 | Validación | HU-007, HU-008, HU-015 | RN-005 | EA-007, EA-008, EA-014 | CP-RF-027 |
| RF-028 | Validación | HU-009 | RN-004 | EA-009 | CP-RF-028 |
| RF-029 | Validación | HU-006, HU-007, HU-008, HU-009, HU-011, HU-012 | RN-001, RN-004, RN-007 | EA-011, EA-012 | CP-RF-029 |
| RF-030 | Requisitos / Versiones | HU-010 | RN-006 | EA-010 | CP-RF-030 |
| RF-031 | Requisitos / Versiones | HU-010, HU-015 | RN-006 | EA-010 | CP-RF-031 |
| RF-032 | Requisitos / Versiones | HU-010 | RN-002, RN-004, RN-006 | EA-010 | CP-RF-032 |
| RF-033 | Requisitos / Versiones | HU-010, HU-015 | RN-006 | EA-010 | CP-RF-033 |
| RF-034 | Requisitos / Versiones | HU-010, HU-015, HU-016 | RN-006 | EA-010, EA-014 | CP-RF-034 |
| RF-035 | Trazabilidad y consulta / Consulta | HU-013, HU-016 | RN-001 | EA-003, EA-013 | CP-RF-035 |
| RF-036 | Trazabilidad y consulta / Consulta | HU-013, HU-016 | RN-001 | EA-003 | CP-RF-036 |
| RF-037 | Trazabilidad y consulta / Consulta | HU-013 | RN-001 | EA-013 | CP-RF-037 |
| RF-038 | Trazabilidad y consulta / Consulta | HU-013 | RN-001 | EA-013 | CP-RF-038 |
| RF-039 | Trazabilidad y consulta / Consulta | HU-014 | RN-001 | EA-013 | CP-RF-039 |
| RF-040 | Trazabilidad y consulta / Consulta | HU-013, HU-014, HU-016 | RN-001 | EA-003, EA-013 | CP-RF-040 |
| RF-041 | Trazabilidad y consulta / Auditoría | HU-015 | RN-005, RN-006, RN-007 | EA-014 | CP-RF-041 |
| RF-042 | Trazabilidad y consulta / Auditoría | HU-015 | RN-005 | EA-014 | CP-RF-042 |
| RF-043 | Trazabilidad y consulta / Auditoría | HU-015 | RN-005, RN-006, RN-007 | EA-014 | CP-RF-043 |
| RF-044 | Trazabilidad y consulta / Auditoría | HU-015, HU-016 | RN-007 | EA-014 | CP-RF-044 |
| RF-045 | Trazabilidad y consulta / Auditoría | HU-015 | RN-001 | EA-014 | CP-RF-045 |
| RF-046 | Trazabilidad y consulta / Auditoría | HU-002, HU-011, HU-012, HU-015 | RN-001, RN-007 | EA-002, EA-011, EA-012, EA-014 | CP-RF-046 |

## 13. Matriz de trazabilidad no funcional

| RNF | Categoría | Módulos afectados | HU relacionadas | RN relacionadas | Decisión relacionada | Caso previsto |
| --- | --- | --- | --- | --- | --- | --- |
| RNF-001 | Seguridad | Todos | HU-001 | RN-001 | DP-003 | CP-RNF-001 |
| RNF-002 | Seguridad | Todos | HU-001, HU-004, HU-007, HU-008, HU-016 | RN-001, RN-005 | DP-001 | CP-RNF-002 |
| RNF-003 | Seguridad | Identidad y acceso; Despliegue | HU-001, HU-002 | — | DP-003, DP-009 | CP-RNF-003 |
| RNF-004 | Seguridad | Identidad y acceso | HU-001 | — | DP-003 | CP-RNF-004 |
| RNF-005 | Seguridad | Todos | HU-001 a HU-016 | — | — | CP-RNF-005 |
| RNF-006 | Integridad | Identidad y acceso; Proyectos; Requisitos | HU-004, HU-005, HU-006 | RN-001, RN-002, RN-003, RN-005 | — | CP-RNF-006 |
| RNF-007 | Integridad | Validación; Requisitos / Versiones; Auditoría | HU-006 a HU-012, HU-015 | RN-004, RN-005, RN-006, RN-007 | — | CP-RNF-007 |
| RNF-008 | Integridad | Requisitos; Validación; Auditoría | HU-005, HU-007, HU-008, HU-010, HU-015 | RN-002, RN-005, RN-006, RN-007 | — | CP-RNF-008 |
| RNF-009 | Integridad | Proyectos; Requisitos; Validación | HU-003, HU-005 a HU-010 | RN-004, RN-006 | — | CP-RNF-009 |
| RNF-010 | Rendimiento | Identidad y acceso; Proyectos; Consulta; Auditoría | HU-002, HU-013, HU-015 | — | DP-007 | CP-RNF-010 |
| RNF-011 | Rendimiento | Todos | HU-001 a HU-016 | — | DP-007 | CP-RNF-011 |
| RNF-012 | Rendimiento | Todos | HU-001 a HU-016 | — | DP-007 | CP-RNF-012 |
| RNF-013 | Mantenibilidad | Todos | HU-001 a HU-016 | — | DP-010 | CP-RNF-013 |
| RNF-014 | Mantenibilidad | Todos | HU-001 a HU-016 | — | — | CP-RNF-014 |
| RNF-015 | Mantenibilidad | Todos | HU-001 a HU-016 | — | — | CP-RNF-015 |
| RNF-016 | Mantenibilidad | Persistencia de todos los módulos | HU-001 a HU-016 | — | DP-009 | CP-RNF-016 |
| RNF-017 | Mantenibilidad | Todos | HU-001 a HU-016 | — | — | CP-RNF-017 |
| RNF-018 | Usabilidad de la API | Todos | HU-001 a HU-016 | — | — | CP-RNF-018 |
| RNF-019 | Usabilidad de la API | Todos | HU-001 a HU-016 | RN-003, RN-005 | — | CP-RNF-019 |
| RNF-020 | Usabilidad de la API | Todos | HU-001 a HU-016 | — | — | CP-RNF-020 |
| RNF-021 | Usabilidad de la API | Validación; Requisitos / Versiones; Auditoría | HU-007, HU-008, HU-010, HU-015 | RN-005, RN-006 | — | CP-RNF-021 |
| RNF-022 | Pruebas | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-022 |
| RNF-023 | Pruebas | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-023 |
| RNF-024 | Pruebas | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-024 |
| RNF-025 | Trazabilidad | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-025 |
| RNF-026 | Trazabilidad | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-026 |
| RNF-027 | Trazabilidad | Todos | HU-001 a HU-016 | RN-001 a RN-007 | — | CP-RNF-027 |
| RNF-028 | Despliegue | Backend; Persistencia | HU-001 a HU-016 | — | DP-009, DP-010 | CP-RNF-028 |
| RNF-029 | Despliegue | Backend; Persistencia | HU-001 a HU-016 | — | DP-009 | CP-RNF-029 |
| RNF-030 | Despliegue | Backend | HU-001 a HU-016 | — | DP-009 | CP-RNF-030 |
| RNF-031 | Despliegue | Backend; Persistencia | HU-001 a HU-016 | — | DP-009 | CP-RNF-031 |
| RNF-032 | Despliegue | Todos | HU-001 a HU-016 | — | DP-007, DP-009 | CP-RNF-032 |

## 14. Cobertura de reglas de negocio

| Regla | RF que la especifican | HU principales | EA principales | CP previstos |
| --- | --- | --- | --- | --- |
| RN-001 | RF-005, RF-006, RF-008, RF-010 a RF-013, RF-029, RF-035 a RF-040, RF-045, RF-046 | HU-004, HU-005, HU-012, HU-013, HU-016 | EA-003, EA-004, EA-005, EA-012, EA-013 | CP-RF correspondientes, previstos |
| RN-002 | RF-014, RF-032 | HU-005, HU-010 | EA-005, EA-010 | CP-RF-014, CP-RF-032, previstos |
| RN-003 | RF-015 a RF-017, RF-019, RF-020, RF-023 | HU-005, HU-006 | EA-005, EA-006 | CP-RF correspondientes, previstos |
| RN-004 | RF-013, RF-018, RF-022 a RF-026, RF-028, RF-029, RF-032 | HU-006 a HU-011 | EA-006 a EA-011 | CP-RF correspondientes, previstos |
| RN-005 | RF-019, RF-025 a RF-027, RF-041 a RF-043 | HU-007, HU-008, HU-015 | EA-007, EA-008, EA-014 | CP-RF correspondientes, previstos |
| RN-006 | RF-030 a RF-034, RF-041, RF-043 | HU-010, HU-015 | EA-010, EA-014 | CP-RF-030 a CP-RF-034, CP-RF-041, CP-RF-043, previstos |
| RN-007 | RF-021, RF-022, RF-029, RF-041, RF-043, RF-044, RF-046 | HU-011, HU-012, HU-015, HU-016 | EA-011, EA-012, EA-014 | CP-RF correspondientes, previstos |

## 15. Exclusiones del MVP

Quedan fuera del MVP:

- integraciones con Jira, Azure DevOps, GitHub, correo u otras plataformas;
- generación de requisitos mediante inteligencia artificial;
- notificaciones externas y flujos configurables por el usuario;
- gestión completa de pruebas, incidencias, sprints o despliegues como capacidades del producto;
- archivos adjuntos, firmas electrónicas y aprobación legal;
- analítica avanzada, exportaciones personalizadas e informes ejecutivos;
- aplicaciones móviles nativas y funcionamiento sin conexión;
- inicio de sesión corporativo, multitenencia y administración entre organizaciones;
- una interfaz de usuario, hasta que se aprueben su alcance y tecnología.

Las pruebas unitarias, de integración y de aceptación previstas en este documento son actividades de ingeniería y calidad; no constituyen un módulo funcional de gestión de pruebas.

## 16. Decisiones pendientes

La clasificación expresa cuándo debe resolverse cada asunto, pero no anticipa su respuesta:

- **Bloquea diseño:** debe resolverse antes de cerrar el diseño detallado del área afectada.
- **Bloquea implementación:** debe resolverse antes de construir o configurar la base técnica afectada.
- **Bloquea aceptación/operación:** permite avanzar en el diseño funcional, pero debe resolverse antes de aceptar o poner en operación el MVP.
- **Puede resolverse posteriormente:** no bloquea el backend/API definido para el MVP; cualquier ampliación futura deberá revisar el alcance.

| ID | Decisión pendiente | Clasificación | Impacto y condición de resolución |
| --- | --- | --- | --- |
| **DP-001** | Modelo definitivo de roles y gobierno de permisos: quién recibe el rol inicial al crear un proyecto, quién puede asignar o retirar roles y qué controles impiden que una persona se autoasigne permisos superiores. | Bloquea diseño. | La matriz de permisos sigue siendo una propuesta. Debe resolverse antes de cerrar el modelo de autorización y los criterios de aceptación de RF-005, RF-006, RF-011 y RF-019. |
| **DP-002** | Formato del código único de los requisitos. | Bloquea implementación. | RF-014 exige unicidad e inmutabilidad, pero no fija patrón, prefijo ni longitud. Debe resolverse antes de cerrar esquema, validación y contrato de API. |
| **DP-003** | Estrategia de autenticación y recuperación de acceso. | Bloquea diseño. | No se define mecanismo de sesión, proveedor, credencial ni flujo de recuperación. Debe resolverse antes de cerrar la arquitectura de seguridad. |
| **DP-004** | Campos obligatorios definitivos y catálogos de tipo y prioridad. | Bloquea diseño. | Se preservan los mínimos de RN-003 sin cerrar valores de catálogo adicionales. Debe resolverse antes de cerrar dominio, persistencia y validación de API. |
| **DP-005** | Política de autoaprobación. | Bloquea diseño. | No se permite ni se prohíbe que el autor o último editor sea también el validador. Debe resolverse antes de cerrar permisos y flujo de validación. |
| **DP-006** | Alcance y tecnología de una futura interfaz de usuario. | Puede resolverse posteriormente. | La versión 0.1 especifica únicamente backend y API; una interfaz futura requerirá su propia revisión de alcance. |
| **DP-007** | Métricas y umbrales de rendimiento y disponibilidad. | Bloquea aceptación/operación. | RNF-011, RNF-012 y RNF-032 permanecen pendientes de cuantificación. Debe resolverse antes de cerrar la aceptación no funcional. |
| **DP-008** | Plazo y política de retención de auditoría. | Bloquea aceptación/operación. | RF-046 exige conservación, pero no fija duración ni política de purga. Debe resolverse antes del dimensionamiento y la operación productiva. |
| **DP-009** | Entorno objetivo y restricciones operativas de despliegue. | Bloquea aceptación/operación. | No se fijan infraestructura, topología ni objetivos operativos concretos. Debe resolverse antes de validar el despliegue y la preparación operativa. |
| **DP-010** | Versión concreta de Spring Boot, dependencias y herramienta de compilación. | Bloquea implementación. | Se mantiene Java 21, Spring Boot y MySQL sin seleccionar versiones o herramienta. Debe resolverse antes de crear la base técnica. |
| **DP-011** | Política de reactivación de proyectos o desarchivo de requisitos. | Puede resolverse posteriormente. | El MVP puede operar con archivo en solo lectura y sin operación inversa; añadirla en el futuro requerirá revisar el alcance. |

## 17. Criterio de preparación para aprobación

Esta especificación estará preparada para pasar de borrador a línea base aprobada cuando:

- no existan identificadores duplicados o referencias huérfanas;
- cada RF y RNF tenga al menos una referencia de verificación prevista;
- RN-001 a RN-007 mantengan cobertura explícita;
- las decisiones que bloquean diseño o implementación estén resueltas antes de iniciar la etapa afectada;
- las decisiones que bloquean aceptación u operación tengan responsable y fecha objetivo antes de la validación correspondiente;
- las decisiones que pueden resolverse posteriormente permanezcan registradas y cualquier ampliación resultante vuelva a revisión de alcance;
- las personas responsables del producto y de la arquitectura confirmen que el alcance coincide con el Documento 01.
