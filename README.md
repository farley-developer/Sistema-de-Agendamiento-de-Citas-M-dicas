# 🩺 MedAgenda - Backend API

**MedAgenda** es una API REST desarrollada en **Node.js con TypeScript** para gestionar un sistema de agendamiento de citas médicas. Está diseñada con una arquitectura modular, escalable y mantenible, aplicando buenas prácticas de desarrollo backend profesional.

---

## 📁 Estructura del Proyecto

```

medagenda-api/
├── src/
│   ├── config/               # Configuración global (DB, JWT, etc.)
│   ├── modules/              # Módulos organizados por dominio
│   │   ├── auth/             # Login, registro, JWT
│   │   ├── usuarios/         # CRUD de usuarios, roles
│   │   ├── medicos/          # Gestión de médicos
│   │   ├── especialidades/   # Catálogo de especialidades médicas
│   │   ├── disponibilidades/ # Horarios de atención médica
│   │   ├── citas/            # Agendamiento y control de citas
│   │   └── notificaciones/   # Envío de correos y recordatorios
│   ├── middlewares/          # Autenticación, validaciones, errores
│   ├── utils/                # Helpers y funciones compartidas
│   ├── routes/               # Indexador de rutas por módulo
│   ├── app.ts                # Configuración de la app Express
│   └── server.ts             # Punto de arranque del servidor
├── prisma/                   # ORM con Prisma (modelo y migraciones)
│   └── schema.prisma
├── tests/                    # Pruebas unitarias/integración
├── .env                      # Variables de entorno
├── package.json
├── tsconfig.json
└── README.md

```

---

## 🧩 Estructura de un Módulo

Cada módulo representa un dominio funcional y sigue la estructura:

```

módulo/
├── controller.ts     # Maneja solicitudes y respuestas HTTP
├── service.ts        # Contiene la lógica de negocio
├── repository.ts     # Interacción con la base de datos
├── dto.ts            # Esquemas de validación y tipado
├── routes.ts         # Define las rutas del módulo
└── módulo.test.ts    # Pruebas unitarias del módulo

```

---

## 🧰 Tecnologías Utilizadas

| Propósito                    | Herramienta                  |
|-----------------------------|------------------------------|
| Framework HTTP              | Express                      |
| Lenguaje principal          | TypeScript                   |
| ORM                         | Prisma                       |
| Base de datos               | PostgreSQL                   |
| Validación de datos         | Zod                          |
| Autenticación               | JWT + bcrypt                 |
| Pruebas                     | Jest                         |
| Variables de entorno        | dotenv                       |
| Documentación de API        | Swagger                      |
| Envío de correos            | Nodemailer                   |

---

## ✅ Buenas Prácticas

- Separación clara de responsabilidades: controller, service, repository.
- Uso estricto de TypeScript para tipado y seguridad.
- Validación robusta con Zod.
- Middleware centralizado de manejo de errores.
- Módulos organizados por dominio funcional.
- Código limpio y mantenible.

---

## 🚧 Estado del Proyecto

| Tarea                          | Estado     | Descripción                                                        |
|-------------------------------|------------|--------------------------------------------------------------------|
| Definición de estructura base | ✅ Completado | Arquitectura modular y escalable definida                          |
| Scaffold inicial del proyecto | 🔜 Pendiente | Crear carpetas y archivos base                                     |
| Módulo `usuarios`             | 🔜 Pendiente | CRUD básico de usuarios con autenticación                          |
| Diseño de la BD (Prisma)      | 🔜 Pendiente | Modelos iniciales: Usuario, Médico, Especialidad, Cita             |
| Configuración de Swagger      | 🔜 Pendiente | Documentar rutas públicas e internas                               |
| Pruebas iniciales (Jest)      | 🔜 Pendiente | Configuración básica y pruebas del módulo `usuarios`              |

---

## 📌 Cómo contribuir

1. Clona el repositorio
2. Instala dependencias con `npm install`
3. Crea un archivo `.env` a partir de `.env.example`
4. Corre la base de datos local con PostgreSQL
5. Usa `npx prisma migrate dev` para aplicar los modelos
6. Ejecuta el servidor con `npm run dev`

---

## 📫 Contacto

Este proyecto está en desarrollo como parte de un portafolio integral. Si deseas sugerir mejoras, colaborar o hacer una revisión del diseño, puedes abrir un issue o contactar directamente al autor.

---