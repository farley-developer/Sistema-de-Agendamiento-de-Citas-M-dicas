# 📝 Especificación de Requisitos del Software (SRS)

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
