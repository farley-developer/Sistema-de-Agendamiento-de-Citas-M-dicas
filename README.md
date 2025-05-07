Perfecto, te entregaré el archivo con las correcciones propuestas, organizándolo por secciones separadas para facilitar su lectura y mantenimiento. Las secciones incluirán los textos corregidos y mejorados según mi feedback. 

Procedo a generar el archivo SRS corregido y organizado. El archivo será un documento Markdown (.md), pues es muy adecuado para documentación técnica clara y estructurada.

`MedAgenda_SRS_Corregido.md`
```md
# 📝 Especificación de Requisitos del Software (SRS) - MedAgenda

---

## 1. Información general del proyecto

- **Nombre del sistema**: MedAgenda  
- **Tipo de proyecto**: Web App (backend con Node.js + TypeScript, base de datos PostgreSQL)  
- **Cliente**: Clínica SaludVida (ficticia)  
- **Desarrollador**: Farley Piedrahita Orozco
- **Fecha de inicio**: 06/05/2025  
- **Objetivo general**: Crear una plataforma para agendamiento y gestión de citas médicas, accesible por pacientes y personal administrativo, que mejore la eficiencia operativa y la experiencia del paciente.

---

## 2. Requisitos funcionales (RF)

| Código | Descripción |
|--------|-------------|
| RF01 | El sistema debe permitir el registro y autenticación de usuarios (pacientes y personal administrativo). |
| RF02 | Los usuarios deben poder editar su perfil y ver su historial de citas. |
| RF03 | El personal administrativo debe poder registrar médicos, especialidades y horarios disponibles. |
| RF04 | Los pacientes deben poder agendar y cancelar citas médicas. |
| RF05 | El sistema debe enviar correos de confirmación y recordatorios automáticos. |
| RF06 | Debe existir un panel administrativo para visualizar y gestionar las citas programadas. |
| RF07 | El sistema debe soportar múltiples especialidades médicas. |
| RF08 | Los pacientes deben poder consultar su historial de citas pasadas. |
| RF09 | El sistema debe registrar inasistencias y permitir la reprogramación de citas. |

---

## 3. Requisitos no funcionales (RNF)

| Código | Descripción |
|--------|-------------|
| RNF01 | El backend debe desarrollarse con Node.js y TypeScript. |
| RNF02 | Se debe usar una base de datos relacional, preferiblemente PostgreSQL. |
| RNF03 | La arquitectura debe ser modular, limpia, escalable y preparada para crecimiento futuro. |
| RNF04 | Las contraseñas deben almacenarse con hashing seguro (ej. bcrypt). |
| RNF05 | Se debe implementar autenticación y autorización mediante JWT. |
| RNF06 | La API debe estar documentada (preferentemente con Swagger), incluyendo ejemplos de solicitudes y respuestas. |
| RNF07 | Se debe validar exhaustivamente la entrada de datos, incluyendo formatos, rangos y tipos. |
| RNF08 | Se deben incluir pruebas unitarias básicas para cada módulo, utilizando herramientas como Jest o Mocha. |
| RNF09 | El sistema debe protegerse contra ataques comunes (inyección SQL, CSRF, XSS). |
| RNF10 | El sistema debe poder desplegarse en servicios gratuitos (Render, Railway, etc.). |
| RNF11 | Se debe implementar manejo robusto de errores, con mensajes claros y adecuados para usuarios y logs para desarrolladores.|

---

## 4. Casos de uso principales

| ID | Actor | Caso de uso | Descripción | Flujos alternativos y excepciones |
|----|-------|-------------|-------------|----------------------------------|
| CU01 | Paciente | Registro de cuenta | El paciente se registra en el sistema con sus datos personales. | Validación de correo único, manejo de errores en caso de fallo. |
| CU02 | Paciente | Agendar cita | El paciente elige especialidad, médico y horario para agendar una cita. | Bloqueo de citas en horarios no disponibles, validación única por especialidad, errores por conflicto. |
| CU03 | Paciente | Cancelar o reprogramar cita | El paciente puede cancelar o reprogramar una cita existente. | Cancelación solo con 2 horas de anticipación, manejo de consecuencias en agenda. |
| CU04 | Administrativo | Crear disponibilidad médica | El personal crea la agenda de disponibilidad para médicos. | Validación de solapamientos, manejo de cancelaciones. |
| CU05 | Administrativo | Visualizar citas agendadas | Puede ver el cronograma de citas por médico y especialidad. | Actualización en tiempo real, manejo de visualización. |
| CU06 | Sistema | Enviar notificaciones | El sistema envía correos de confirmación y recordatorios automáticos. | Reintentos en envío fallido, log de envíos.|

---

## 5. Reglas de negocio

- Un paciente solo puede tener una cita activa por especialidad a la vez. (Ejemplo: un paciente no puede tener 2 citas activas para cardiología simultáneamente.)  
- Las citas solo se pueden cancelar con al menos 2 horas de anticipación. (Si se intenta cancelar después de este límite, el sistema debe denegar la acción y avisar.)  
- Los médicos solo pueden ser agendados dentro de sus horarios registrados. (La app validará si el horario solicitado está cubierto por alguna disponibilidad activa.)  
- Las inasistencias deben marcarse automáticamente si el paciente no confirma o no se presenta. (El sistema debe actualizar el estado y notificar al administrativo.)  
- Las citas no pueden tener estados "asistida" o "inasistencia" si aún no ha pasado su fecha/hora programada. (Ejemplo: no se puede marcar antes del momento de la cita.)  

---

## 6. Restricciones técnicas

- El sistema será una API REST sin frontend por ahora.  
- El sistema debe estar preparado para integrarse con un frontend en el futuro.  
- Se utilizarán herramientas open-source únicamente.  
- Los datos sensibles deben cifrarse adecuadamente.  
- Se debe proteger la API contra ataques de seguridad comunes (inyección SQL, CSRF, XSS).  
- El sistema debe ser escalable y modular para facilitar mantenimiento y extensiones.  

---

# 🗄️ Documentación del Esquema SQL - MedAgenda

---

## 📘 Tabla: `usuarios`

Contiene los datos de todos los usuarios del sistema (pacientes, médicos y administrativos).

| Campo             | Tipo           | Descripción                                     |
|------------------|----------------|-------------------------------------------------|
| id               | SERIAL         | Identificador único (PK)                        |
| nombre           | VARCHAR(100)   | Nombre completo del usuario                     |
| correo           | VARCHAR(100)   | Correo electrónico (único)                      |
| contraseña       | TEXT           | Contraseña (hasheada, e.g. bcrypt)             |
| rol              | VARCHAR(20)    | `'paciente'`, `'admin'` o `'medico'`            |
| fecha_nacimiento | DATE           | Fecha de nacimiento                             |
| telefono         | VARCHAR(20)    | Número de contacto                              |
| creado_en        | TIMESTAMP      | Fecha y hora de creación                        |

---

## 📘 Tabla: `especialidades`

Representa las distintas especialidades médicas disponibles en la clínica.

| Campo  | Tipo         | Descripción                     |
|--------|--------------|---------------------------------|
| id     | SERIAL       | Identificador único (PK)        |
| nombre | VARCHAR(100) | Nombre único de la especialidad |

---

## 📘 Tabla: `medicos`

Relaciona un usuario con una especialidad médica.

| Campo           | Tipo    | Descripción                                           |
|------------------|---------|-------------------------------------------------------|
| id               | SERIAL  | Identificador único (PK)                              |
| usuario_id       | INTEGER | Referencia a `usuarios(id)` (1:1, médico como usuario)|
| especialidad_id  | INTEGER | Referencia a `especialidades(id)`                    |

🔗 **Relaciones**:  
- `usuario_id` → `usuarios(id)` con restricción `UNIQUE`.  
- `especialidad_id` → `especialidades(id)`  

---

## 📘 Tabla: `disponibilidades`

Define los bloques horarios en que un médico está disponible.

| Campo         | Tipo    | Descripción                                       |
|---------------|---------|---------------------------------------------------|
| id            | SERIAL  | Identificador único (PK)                          |
| medico_id     | INTEGER | FK a `medicos(id)`                                |
| fecha         | DATE    | Día de disponibilidad                             |
| hora_inicio   | TIME    | Hora de inicio del bloque                         |
| hora_fin      | TIME    | Hora de fin del bloque                            |
| activo        | BOOLEAN | Indica si el bloque está activo o inactivo       |

🔐 **Restricciones**:  
- `hora_inicio < hora_fin`  
- `activo` para facilitar manejo y desactivación temporal.  
- `medico_id` → `medicos(id)`  

---

## 📘 Tabla: `citas`

Almacena la información de las citas médicas entre pacientes y médicos.

| Campo         | Tipo      | Descripción                                                  |
|---------------|-----------|--------------------------------------------------------------|
| id            | SERIAL    | Identificador único (PK)                                     |
| paciente_id   | INTEGER   | FK a `usuarios(id)` (debe tener rol `'paciente'`)            |
| medico_id     | INTEGER   | FK a `medicos(id)`                                           |
| fecha         | DATE      | Fecha de la cita                                             |
| hora          | TIME      | Hora de la cita                                              |
| estado        | VARCHAR   | `'pendiente'`, `'confirmada'`, `'cancelada'`, `'asistida'`, `'inasistencia'` |
| motivo        | TEXT      | Texto libre que indica el motivo de la cita                  |
| creada_en     | TIMESTAMP | Fecha de creación del registro                               |
| actualizada_en| TIMESTAMP | Fecha y hora de la última actualización (auditoría)         |

🔐 **Restricciones**:  
- `paciente_id` → `usuarios(id)`  
- `medico_id` → `medicos(id)`  
- `estado` restringido a valores posibles.  

---

## 🧭 Relaciones entre tablas

- `usuarios` se relaciona con:  
  - `medicos` (1:1 si el usuario es médico)  
  - `citas` como paciente  

- `especialidades` se relacionan con `medicos` (N:1)  

- `medicos` tienen múltiples `disponibilidades` y `citas`  

---

## 📌 Índices recomendados

| Índice                   | Propósito                                    |
|--------------------------|----------------------------------------------|
| `idx_citas_paciente`     | Agiliza búsquedas por paciente en citas      |
| `idx_citas_medico_fecha` | Mejora consultas por fecha para un médico    |
| `idx_disponibilidad_medico_fecha` | Permite encontrar horarios disponibles más rápido |

---

## 🔐 Reglas de integridad y negocio

- No se pueden registrar citas en horarios no disponibles (validar desde la app y BD).  
- Un médico no puede tener dos bloques de disponibilidad que se solapen.  
- Solo usuarios con rol `'paciente'` pueden agendar citas.  
- Citas no pueden tener estados "asistida" o "inasistencia" antes de la fecha/hora programada.  
- Cancelación de cita debe respetar la restricción de al menos 2 horas de anticipación.  

---

## ✏️ Notas adicionales

- Las contraseñas no se almacenan en texto plano, se utilizará hashing seguro (bcrypt).  
- Todos los campos de tipo `TIMESTAMP` usan la hora del servidor.  
- El modelo es extensible: se puede incorporar sistema pagos, reseñas, encuestas de satisfacción, etc.  

---