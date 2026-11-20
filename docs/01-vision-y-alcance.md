# NexoReq: visión y alcance inicial

> - **Estado:** borrador para revisión
> - **Versión:** 0.1
> - **Fecha:** 20 de noviembre de 2026

## 1. Propósito del documento

Este documento establece una visión inicial compartida para NexoReq y delimita el producto mínimo viable (MVP). Su contenido servirá como punto de partida para refinar requisitos, priorizar el trabajo y tomar decisiones de diseño antes de iniciar la implementación.

Las definiciones aquí incluidas son propuestas sujetas a validación por las personas responsables del producto y del negocio.

## 2. Problema

Los requisitos de software suelen quedar distribuidos entre hojas de cálculo, documentos, mensajes y herramientas no integradas. Esta dispersión dificulta saber cuál es la versión vigente, quién solicitó un cambio, qué se aprobó y por qué se tomó una decisión.

Como consecuencia, los equipos enfrentan:

- requisitos ambiguos, incompletos, duplicados o desactualizados;
- pérdida de trazabilidad entre la solicitud, sus cambios y su validación;
- aprobaciones informales sin responsable, fecha ni evidencia;
- poca visibilidad del estado y la prioridad del alcance acordado;
- retrabajo, demoras y conflictos entre las personas interesadas y el equipo de desarrollo.

NexoReq busca resolver este problema mediante una fuente única y auditable para registrar, organizar, revisar y validar requisitos de software.

## 3. Visión del producto

Para equipos que necesitan definir y validar el alcance de un producto de software, **NexoReq** será un sistema centralizado de gestión de requisitos que permitirá mantener información estructurada, aplicar un flujo de revisión explícito y conservar el historial de las decisiones. A diferencia de los documentos aislados, NexoReq ofrecerá trazabilidad, control de acceso y una vista común del estado de cada requisito.

## 4. Objetivos

### 4.1. Objetivo general

Mejorar la calidad y la trazabilidad de los requisitos de software mediante un proceso centralizado de registro, revisión, validación y consulta.

### 4.2. Objetivos específicos

- Centralizar los requisitos de cada proyecto en un repositorio único.
- Estandarizar la información mínima necesaria para describir un requisito.
- Hacer visible el estado, la prioridad, la responsabilidad y el origen de cada requisito.
- Formalizar la aprobación o el rechazo con autor, fecha y observaciones.
- Conservar las versiones y los eventos relevantes para facilitar auditorías.
- Permitir búsquedas y filtros que apoyen el seguimiento del alcance.
- Establecer una base técnica sencilla de desplegar y preparada para crecer por módulos.

## 5. Alcance del MVP

### 5.1. Capacidades incluidas

1. **Acceso y autorización**
   - Autenticación de usuarios.
   - Alta, consulta, activación y desactivación básica de usuarios por un administrador.
   - Asignación de roles y permisos por proyecto.
   - Restricción de las operaciones según el rol asignado.

2. **Gestión de proyectos**
   - Creación, consulta, actualización y archivo de proyectos.
   - Asociación de integrantes y definición de su rol dentro del proyecto.

3. **Gestión de requisitos**
   - Creación, consulta, edición y archivo de requisitos.
   - Registro de código, título, descripción, tipo, prioridad, fuente, responsable y criterios de aceptación.
   - Clasificación inicial en requisitos funcionales y no funcionales.
   - Consulta del historial de versiones.

4. **Revisión y validación**
   - Envío de requisitos a revisión.
   - Aprobación o rechazo por una persona autorizada.
   - Registro de observaciones y de la evidencia básica de la decisión.

5. **Seguimiento y consulta**
   - Listado y detalle de requisitos por proyecto.
   - Búsqueda por texto y filtros por estado, tipo, prioridad y responsable.
   - Resumen básico de requisitos por estado.

6. **Trazabilidad y auditoría**
   - Registro del autor y la fecha de las acciones relevantes.
   - Conservación de cambios de estado, versiones y decisiones de validación.

### 5.2. Fuera del alcance del MVP

- Integraciones con Jira, Azure DevOps, GitHub, correo u otras plataformas.
- Generación de requisitos mediante inteligencia artificial.
- Notificaciones externas y flujos configurables por el usuario.
- Gestión completa de pruebas, incidencias, sprints o despliegues.
- Archivos adjuntos, firmas electrónicas y aprobación legal.
- Analítica avanzada, exportaciones personalizadas e informes ejecutivos.
- Aplicaciones móviles nativas y funcionamiento sin conexión.
- Inicio de sesión corporativo, multitenencia y administración entre organizaciones.

El repositorio `nexoreq-springboot` cubrirá inicialmente el backend y su API. Una interfaz de usuario y su tecnología deberán acordarse como un entregable separado.

### 5.3. Criterios de éxito del MVP

El MVP se considerará funcional cuando un equipo autorizado pueda:

- crear un proyecto y asignar sus participantes;
- registrar un requisito con la información mínima definida;
- enviarlo a revisión y obtener una decisión registrada;
- consultar su versión vigente, historial y responsables;
- localizar requisitos mediante búsqueda y filtros;
- verificar quién realizó cada acción relevante y cuándo la realizó.

## 6. Actores

| Actor | Responsabilidad principal en el MVP |
| --- | --- |
| **Administrador del sistema** | Gestionar usuarios, roles globales y parámetros básicos de acceso. |
| **Líder de proyecto / analista de requisitos** | Crear el proyecto, administrar participantes, registrar y mantener requisitos, y solicitar su revisión. |
| **Colaborador del proyecto** | Proponer o complementar requisitos de acuerdo con sus permisos. |
| **Validador / persona interesada** | Revisar requisitos y registrar su aprobación o rechazo con observaciones. |
| **Consultor / auditor** | Consultar requisitos, decisiones e historial sin modificar la información. |

Una misma persona podrá desempeñar más de un rol, siempre que los permisos se asignen de forma explícita dentro de cada proyecto.

## 7. Reglas de negocio iniciales

| Código | Regla |
| --- | --- |
| **RN-001** | Todo requisito debe pertenecer a un único proyecto. Solo los integrantes autorizados del proyecto, o un administrador del sistema, podrán consultarlo u operarlo. Al archivar un proyecto, sus requisitos se conservarán en modo de solo lectura. |
| **RN-002** | Al crear un requisito, el sistema debe asignarle un código único e inmutable dentro del proyecto. El código seguirá siendo válido aunque el requisito cambie de estado o sea archivado. |
| **RN-003** | Un borrador debe tener, como mínimo, título, descripción, tipo y prioridad. Para enviarlo a revisión también debe indicar fuente, responsable, criterios de aceptación y validador. |
| **RN-004** | El flujo ordinario será `Borrador` → `En revisión` → `Aprobado` o `Rechazado`. Un requisito rechazado podrá volver a `Borrador` para ser corregido y uno aprobado solo podrá volver a `Borrador` al generar una nueva versión según la RN-006. `Archivado` será una condición independiente del estado de validación y bloqueará nuevas transiciones. Solo se permitirán cambios ejecutados por roles autorizados. |
| **RN-005** | Solo un validador autorizado podrá aprobar o rechazar un requisito. Toda decisión debe registrar persona, fecha y hora; el rechazo debe incluir una observación obligatoria. |
| **RN-006** | Un cambio de contenido en un requisito aprobado debe generar una nueva versión, conservar la versión previamente aprobada y devolver el requisito a `Borrador` para una nueva validación. |
| **RN-007** | Los requisitos no se eliminarán físicamente en el MVP. Deberán archivarse y quedar en modo de solo lectura, conservando su código, versiones, cambios de estado y decisiones para fines de trazabilidad y auditoría. |

## 8. Arquitectura inicial

### 8.1. Enfoque

NexoReq se implementará como un **monolito modular**: una sola aplicación desplegable y una sola base de datos, con límites internos claros entre capacidades de negocio. Este enfoque reduce la complejidad operativa del MVP sin renunciar a una organización que permita evolucionar o extraer módulos si el crecimiento futuro lo justifica.

### 8.2. Tecnologías base

- **Java 21** como versión del lenguaje y de la plataforma.
- **Spring Boot** como base de la aplicación backend.
- **MySQL** como sistema de gestión de base de datos relacional.
- API HTTP con representación JSON para exponer las capacidades del backend.

Las versiones concretas de Spring Boot, sus dependencias y las herramientas de compilación se fijarán al iniciar la implementación.

### 8.3. Módulos lógicos previstos

| Módulo | Responsabilidad |
| --- | --- |
| **Identidad y acceso** | Usuarios, autenticación, roles y autorización. |
| **Proyectos** | Datos del proyecto, participantes y ciclo de vida del proyecto. |
| **Requisitos** | Registro, clasificación, versiones y consulta de requisitos. |
| **Validación** | Flujo de revisión, decisiones y observaciones. |
| **Trazabilidad y consulta** | Eventos de auditoría, historial y vistas resumidas. |

Cada módulo organizará sus responsabilidades en capas de entrada, aplicación, dominio e infraestructura. La comunicación entre módulos deberá ocurrir mediante contratos explícitos; un módulo no accederá directamente a las estructuras internas ni a las tablas propiedad de otro.

### 8.4. Criterios arquitectónicos

- Despliegue inicial como una única aplicación Spring Boot.
- Persistencia transaccional en una instancia MySQL y un esquema controlado por migraciones.
- Organización del código por capacidad de negocio, no únicamente por tipo técnico.
- Validación de datos y reglas tanto en los límites de entrada como en el dominio.
- Autorización aplicada en cada operación sensible.
- Registro auditable de cambios de estado, versiones y decisiones.
- Pruebas unitarias por módulo y pruebas de integración para persistencia, seguridad y flujos principales.
- Dependencias compartidas reducidas a aspectos transversales indispensables.

## 9. Entregables previstos

### 9.1. Definición y diseño

- Visión y alcance aprobados.
- Glosario y modelo de dominio inicial.
- Casos de uso o historias de usuario con criterios de aceptación.
- Especificación de requisitos funcionales y no funcionales priorizados.
- Modelo de datos y decisiones de arquitectura documentadas.
- Contrato inicial de la API.

### 9.2. Construcción del MVP

- Proyecto base con Java 21, Spring Boot y gestión de dependencias.
- Migraciones versionadas para MySQL.
- Implementación de los módulos definidos para el MVP.
- API documentada y mecanismos de autenticación y autorización.
- Datos de prueba o procedimiento reproducible para validar los flujos principales.

### 9.3. Calidad y entrega

- Pruebas unitarias, de integración y de aceptación de los flujos críticos.
- Evidencia de cumplimiento de las siete reglas de negocio iniciales.
- Guía de configuración, ejecución y despliegue.
- Registro de decisiones técnicas y limitaciones conocidas.
- Versión candidata del MVP y acta de validación funcional.

## 10. Decisiones pendientes de validación

Antes de iniciar el desarrollo se deberá confirmar:

- el modelo definitivo de roles y quién puede asignarlos;
- el formato del código único de los requisitos;
- la estrategia de autenticación y recuperación de acceso;
- los campos obligatorios y catálogos de tipo y prioridad;
- si un validador puede aprobar un requisito del que también es autor;
- el alcance y la tecnología de una futura interfaz de usuario;
- los objetivos medibles de rendimiento, disponibilidad y conservación de auditoría;
- el entorno objetivo de despliegue y sus restricciones operativas.
