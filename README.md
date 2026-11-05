# AgendaFlow

Proyecto personal de aprendizaje para gestionar citas de una barbería desde un panel web y consultar la próxima cita con Alexa. Nació para explorar Oracle y PL/SQL, construir una API REST y conectar una aplicación web con un asistente de voz.

> **Proyecto educativo sin terminar.** Puede contener errores y funciones incompletas. No es un producto listo para producción. La base de datos necesaria no está incluida en estos repositorios.

## Componentes

Este repositorio organiza tres repositorios independientes mediante **submódulos Git**, cada uno con su propio historial.

| Componente | Responsabilidad | Tecnologías |
| --- | --- | --- |
| [Backend](https://github.com/juzer1711/agendaflow-backend) · [README](./agendaflow-backend/README.md) | API, autenticación y acceso a Oracle | Node.js, TypeScript, Express 4, oracledb, PL/SQL, JWT, bcrypt, Zod |
| [Frontend](https://github.com/juzer1711/agendaflow-frontend) · [README](./agendaflow-frontend/README.md) | Panel de administración | React 18, JavaScript, Vite 6, Tailwind CSS 3, React Router, Axios, Lucide |
| [Alexa](https://github.com/juzer1711/agenda-flow-alexa) · [README](./agenda-flow-alexa/README.md) | Consulta de citas por voz | Node.js, JavaScript, Alexa Skills Kit SDK, Express 5 |

## Funcionalidades implementadas

- Inicio de sesión de barberos. La API también implementa registro, aunque el panel no tiene esa pantalla.
- Consulta de citas, citas del día y próximas; creación y cancelación de reservas.
- Creación, edición y desactivación de servicios con precio y duración.
- Consulta y registro de clientes con datos de contacto y notas.
- Configuración de horarios semanales y registro de excepciones: días libres, vacaciones, bloqueos y horarios especiales.
- Consulta de disponibilidad y lectura o actualización del perfil del barbero desde la API.
- Consulta por voz de la próxima cita: Alexa anuncia cliente, servicio y fecha en la zona horaria de Colombia.

## Arquitectura

```text
Panel React ── HTTP + JWT ──┐
                          ├── API Express / TypeScript ── SQL y PL/SQL ── Oracle
Alexa ── Skill Node.js ────┘
```

El backend separa rutas, controladores, servicios y repositorios. Usa consultas parametrizadas y packages PL/SQL para las operaciones de negocio. El barbero se identifica mediante el JWT. Alexa consume la misma API con un token configurado manualmente y ofrece servidor HTTP local y un handler para AWS Lambda.

## Descargar y ejecutar

```bash
git clone --recurse-submodules https://github.com/juzer1711/agendaflow.git
cd agendaflow
```

Si ya clonaste el repositorio padre:

```bash
git submodule update --init --recursive
```

Se recomienda Node.js 22 LTS y npm. Cada submódulo se instala y configura por separado:

1. Prepara Oracle y configura el [backend](./agendaflow-backend/README.md), por defecto en el puerto `3000`.
2. Inicia el [panel web](./agendaflow-frontend/README.md), por defecto en `5173`.
3. Configura [Alexa](./agenda-flow-alexa/README.md) con la URL de la API y un JWT válido; su servidor local usa `3001`.

**No se incluyen scripts SQL de creación de tablas, packages ni datos iniciales.** Se necesita una instancia Oracle con el esquema esperado por el backend; instalar dependencias y configurar credenciales no basta para ejecutar el sistema desde cero.

## Estado y pendientes

- No hay edición o eliminación de clientes, edición de citas ni gestión completa de excepciones. Las citas se cancelan y los servicios se desactivan.
- El panel no incluye registro ni edición del perfil. Varios listados cargan hasta 100 registros y la vista de próximas citas filtra esa página en el navegador.
- La API delega disponibilidad en PL/SQL. Sin los scripts Oracle no se puede confirmar cómo valida horarios, excepciones, duración y concurrencia al reservar.
- Alexa solo consulta la próxima cita; no crea ni cancela reservas. No hay vinculación de cuentas ni renovación automática del JWT.
- WhatsApp y Gemini aparecen como ideas futuras en la documentación histórica, sin una integración implementada en estos repositorios.
- Existen pruebas del backend y una prueba local de Alexa con respuesta simulada; no equivalen a una validación completa con Oracle y un dispositivo Alexa.

AgendaFlow se conserva como muestra de aprendizaje y referencia de cómo conectar sus tres componentes.
