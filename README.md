# 📝 Especificación de Requisitos del Software (SRS)

## 1. Información general del proyecto

- **Nombre del sistema**: MedAgenda  
- **Tipo de proyecto**: Web App (backend con Node.js + TypeScript, base de datos PostgreSQL)  
- **Cliente**: Clínica SaludVida  
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
| RNF03 | La arquitectura debe ser modular, limpia y escalable. |
| RNF04 | Las contraseñas deben almacenarse con hashing seguro (ej. bcrypt). |
| RNF05 | Se debe implementar autenticación y autorización mediante JWT. |
| RNF06 | La API debe estar documentada (preferentemente con Swagger). |
| RNF07 | Se debe validar exhaustivamente la entrada de datos. |
| RNF08 | Se deben incluir pruebas unitarias básicas para cada módulo. |
| RNF09 | El sistema debe poder desplegarse en servicios gratuitos (Render, Railway, etc.). |

---

## 4. Casos de uso principales

| ID | Actor | Caso de uso | Descripción |
|----|-------|-------------|-------------|
| CU01 | Paciente | Registro de cuenta | El paciente se registra en el sistema con sus datos personales. |
| CU02 | Paciente | Agendar cita | El paciente elige especialidad, médico y horario para agendar una cita. |
| CU03 | Paciente | Cancelar o reprogramar cita | El paciente puede cancelar o reprogramar una cita existente. |
| CU04 | Administrativo | Crear disponibilidad médica | El personal crea la agenda de disponibilidad para médicos. |
| CU05 | Administrativo | Visualizar citas agendadas | Puede ver el cronograma de citas por médico y especialidad. |
| CU06 | Sistema | Enviar notificaciones | El sistema envía correos de confirmación y recordatorios automáticos. |

---

## 5. Reglas de negocio

- Un paciente solo puede tener una cita activa por especialidad a la vez.
- Las citas solo se pueden cancelar con al menos 2 horas de anticipación.
- Los médicos solo pueden ser agendados dentro de sus horarios registrados.
- Las inasistencias deben marcarse automáticamente si el paciente no confirma o no se presenta.

---

## 6. Restricciones técnicas

- El sistema será una API REST sin frontend por ahora.
- El sistema debe estar preparado para integrarse con un frontend en el futuro.
- Se utilizarán herramientas open-source únicamente.
- Los datos sensibles deben cifrarse adecuadamente.

# 🗄️ Documentación del Esquema SQL - MedAgenda

Este documento describe las tablas, campos, relaciones y reglas clave del modelo de datos del sistema de agendamiento de citas médicas **MedAgenda**.

---

## 📘 Tabla: `usuarios`

Contiene los datos de todos los usuarios del sistema (pacientes, médicos y administrativos).

| Campo             | Tipo           | Descripción                                     |
|------------------|----------------|-------------------------------------------------|
| id               | SERIAL         | Identificador único (PK)                        |
| nombre           | VARCHAR(100)   | Nombre completo del usuario                     |
| correo           | VARCHAR(100)   | Correo electrónico (único)                      |
| contraseña       | TEXT           | Contraseña (hasheada)                           |
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
- `usuario_id` → `usuarios(id)` con `UNIQUE`, para asegurar que un médico es un solo usuario.
- `especialidad_id` → `especialidades(id)`

---

## 📘 Tabla: `disponibilidades`

Define los bloques horarios en que un médico está disponible.

| Campo       | Tipo    | Descripción                                       |
|-------------|---------|---------------------------------------------------|
| id          | SERIAL  | Identificador único (PK)                          |
| medico_id   | INTEGER | FK a `medicos(id)`                                |
| fecha       | DATE    | Día de disponibilidad                             |
| hora_inicio | TIME    | Hora de inicio del bloque                         |
| hora_fin    | TIME    | Hora de fin del bloque                            |

🔐 **Restricciones**:
- `hora_inicio < hora_fin`
- `medico_id` → `medicos(id)`

---

## 📘 Tabla: `citas`

Almacena la información de las citas médicas entre pacientes y médicos.

| Campo        | Tipo     | Descripción                                                  |
|--------------|----------|--------------------------------------------------------------|
| id           | SERIAL   | Identificador único (PK)                                     |
| paciente_id  | INTEGER  | FK a `usuarios(id)` (debe tener rol 'paciente')              |
| medico_id    | INTEGER  | FK a `medicos(id)`                                           |
| fecha        | DATE     | Fecha de la cita                                             |
| hora         | TIME     | Hora de la cita                                              |
| estado       | VARCHAR  | `'pendiente'`, `'confirmada'`, `'cancelada'`, `'asistida'`, `'inasistencia'` |
| motivo       | TEXT     | Texto libre que indica el motivo de la cita                  |
| creada_en    | TIMESTAMP| Fecha de creación del registro                               |

🔐 **Restricciones**:
- `paciente_id` → `usuarios(id)`
- `medico_id` → `medicos(id)`
- `estado` restringido a los valores posibles.

---

## 🧭 Relaciones entre tablas

- `usuarios` se relaciona con:
  - `medicos` (1:1 si el usuario es médico)
  - `citas` como paciente

- `especialidades` se relacionan con `medicos` (N:1)

- `medicos` tienen múltiples `disponibilidades` y `citas`

---

## 📌 Índices recomendados

| Índice | Propósito |
|--------|-----------|
| `idx_citas_paciente` | Agiliza búsquedas por paciente en citas |
| `idx_citas_medico_fecha` | Mejora consultas por fecha para un médico |
| `idx_disponibilidad_medico_fecha` | Permite encontrar horarios disponibles más rápido |

---

## 🔐 Reglas de integridad

- No se pueden registrar citas en horarios no disponibles (debe validarse desde la app).
- Un médico no puede tener dos bloques de disponibilidad que se solapen.
- Solo usuarios con rol `'paciente'` pueden agendar citas.
- Citas no pueden tener estado "asistida" o "inasistencia" si aún no ha pasado su fecha/hora.

---

## ✏️ Notas adicionales

- Las contraseñas no se almacenan en texto plano.
- Todos los campos de tipo `TIMESTAMP` usan la hora del servidor.
- Este modelo es extensible: se puede incorporar un sistema de pagos, reseñas, encuestas de satisfacción, etc.

