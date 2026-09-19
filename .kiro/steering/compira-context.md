# Contexto funcional — COMPIRA

## 1. Nombre del proyecto

COMPIRA — Plataforma web para centralizar flujos de trabajo en pymes.

---

# 2. Objetivo general

Desarrollar una plataforma web que centralice la información y facilite la gestión de tareas diarias en las pymes.

---

# 3. Problema que busca resolver

Actualmente muchas pymes gestionan sus tareas y actividades mediante múltiples herramientas y canales como:

- WhatsApp;
- correo electrónico;
- hojas de cálculo;
- herramientas de gestión de proyectos;
- aplicaciones de notas;
- documentos compartidos.

Esta dispersión genera problemas como:

- pérdida de trazabilidad;
- dificultad para conocer responsables;
- dificultad para conocer el estado real de las tareas;
- retrasos;
- duplicidad de información;
- pérdida de tiempo buscando información;
- dificultad para realizar seguimiento;
- fragmentación del ciclo de vida de las tareas.

COMPIRA busca centralizar este proceso en una única plataforma.

---

# 4. Proceso funcional principal

El proceso principal del sistema es la gestión centralizada del ciclo de vida de una tarea.

El flujo general comprende:

1. autenticación del usuario;
2. creación de la tarea;
3. definición de información de la tarea;
4. asignación de responsables;
5. notificación de la asignación;
6. ejecución de la actividad;
7. actualización de estado;
8. seguimiento;
9. reasignación cuando aplique;
10. aprobación o cierre;
11. registro del histórico;
12. consulta de indicadores.

---

# 5. Actores

## Administrador del sistema

Responsable de la configuración general de la organización.

Funciones conocidas:

- gestionar usuarios;
- asignar roles;
- remover usuarios;
- restablecer contraseñas;
- registrar usuarios;
- editar usuarios;
- consultar reportes de productividad;
- consultar porcentaje de cumplimiento;
- consultar tareas retrasadas;
- consultar carga de trabajo;
- consultar tiempos de cierre;
- consultar históricos de tareas;
- configurar notificaciones;
- configurar zona horaria;
- consultar panel de equipos.

---

## Coordinador de equipo

Responsable de supervisar las tareas de su equipo.

Funciones conocidas:

- crear tareas;
- asignar tareas;
- reasignar tareas;
- cancelar tareas;
- realizar seguimiento;
- monitorear avance;
- gestionar alertas de desempeño;
- consultar panel de seguimiento;
- aprobar actividades cuando aplique;
- cerrar actividades cuando aplique.

No tiene permisos para:

- gestionar usuarios;
- modificar configuraciones generales de la plataforma.

---

## Colaborador

Responsable de ejecutar las tareas asignadas.

Funciones conocidas:

- consultar tareas asignadas;
- recibir notificaciones;
- recibir recordatorios;
- actualizar estado de tareas;
- registrar observaciones;
- documentar la ejecución de la tarea.

No puede:

- crear tareas;
- modificar configuraciones generales.

---

# 6. Módulos funcionales

## M1. Autenticación y gestión de roles

Incluye:

- registro seguro de usuarios;
- inicio de sesión;
- asignación de permisos según rol;
- recuperación de contraseña;
- control de sesión;
- RBAC.

---

## M2. Gestión de tareas

Incluye:

- creación de tareas;
- título;
- descripción;
- fechas;
- asignación a uno o varios colaboradores;
- estados;
- actualización del estado;
- reasignación;
- cancelación;
- cierre;
- historial de cambios;
- trazabilidad.

---

## M3. Recordatorios automáticos

Incluye notificaciones cuando:

- una tarea es asignada;
- una tarea es reasignada;
- una tarea está próxima a vencer;
- una tarea está retrasada.

Las notificaciones se muestran dentro de la plataforma.

---

## M4. Panel de seguimiento

Debe permitir una vista consolidada del equipo.

Información conocida:

- estado de tareas;
- avance;
- tareas vencidas;
- tareas próximas a vencer;
- niveles de cumplimiento.

Filtros conocidos:

- usuario;
- estado;
- fecha.

---

## M5. Administración del sistema

Disponible para el Administrador.

Incluye:

- gestión de usuarios;
- gestión de roles;
- configuraciones generales;
- zona horaria;
- notificaciones;
- reportes de productividad.

---

# 7. Alcance explícitamente excluido

Actualmente se encuentran fuera del alcance:

- aplicación móvil nativa;
- implementación productiva con empresas reales;
- diseño UI/UX avanzado más allá de prototipos funcionales;
- estrategias comerciales;
- lanzamiento comercial.

Si durante el análisis aparece alguna funcionalidad que pudiera pertenecer a estos elementos, no debe agregarse automáticamente al backlog.

Debe registrarse como:

"Posible ampliación de alcance — requiere validación."

---

# 8. Principios del sistema

COMPIRA debe favorecer:

- simplicidad de uso;
- centralización;
- trazabilidad;
- control basado en roles;
- seguimiento del trabajo;
- reducción de dispersión de información.

---

# 9. Fuente de verdad

Este documento es un resumen funcional derivado del documento oficial:

`docs/proyecto/COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx`

Ante dudas, contradicciones o falta de información:

1. consultar el documento oficial;
2. no inventar una respuesta;
3. registrar la inconsistencia;
4. formular una pregunta pendiente.

---

# 10. Regla fundamental

No incorporar funcionalidad solamente porque parezca lógica para una plataforma de gestión de tareas.

Todo comportamiento debe provenir de:

1. documentación del proyecto;
2. decisión explícita del equipo;
3. una HU previamente aprobada.

En caso contrario debe tratarse como recomendación o pregunta pendiente.