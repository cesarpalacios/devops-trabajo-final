# Trabajo Final — DevOps Pipeline Challenge
## Universidad Nacional de Colombia — Sede Manizales
### DevOps & Automatización | 2026-2

**Docente:** Cesar Augusto Palacios Alarcón

---

# Objetivo

Cada grupo debe construir un **pipeline DevOps completo** desde cero sobre una API funcional: crear el Dockerfile, configurar GitHub Actions, generar artefactos, publicar releases, configurar observabilidad con Prometheus + Grafana, aplicar seguridad DevSecOps, y desplegar en **AWS EC2**.

> **Esto no es un ejercicio de programación.** Es un ejercicio de **flujo DevOps completo**: cada tema visto en clase debe reflejarse en el proyecto.

---

# 1. Formación de grupos

- Grupos de **2 a 3 personas** (individual solo con aprobación del docente).
- **Registro:** diligencia el formulario una sola vez por grupo:
  👉 **https://docs.google.com/forms/d/e/1FAIpQLSdAL4SWplE1eziIfx1FBG60kmCdbRlQKgA6uzB9Uvc4QR8lDQ/viewform**
- **Fecha límite de registro:** viernes 21 de noviembre de 2026.
- Cada grupo elige **UN reto** de la lista (sección 3). Los retos se asignan por orden de registro: si dos grupos eligen el mismo, lo repite el que registró después.

# 2. Repositorio del grupo

- Crear un repo **privado** llamado `devops-tf-<nombre-corto>` (ej: `devops-tf-clima`).
- Agregar al docente (`cesarpalacios`) como **collaborator** (Settings → Collaborators).
- El código base del reto lo construyen ustedes desde el enunciado; lo que NO se entrega es el ecosistema DevOps.

# 3. Retos disponibles

Cada grupo elige **UN reto**. Se entrega el enunciado de la API — construir el ecosistema DevOps es el trabajo.

## Reto 1: API de Tareas (To-Do)

```
GET     /tasks      → Listar tareas
POST    /tasks      → Crear tarea {titulo, descripcion}
PUT     /tasks/:id  → Actualizar tarea
DELETE  /tasks/:id  → Eliminar tarea
GET     /health     → Estado del servicio + BD
GET     /metrics    → Métricas en formato Prometheus
```

**Stack:** Python + FastAPI + SQLite

## Reto 2: API de Clima

```
GET     /weather/:city          → Clima actual (temperatura, humedad, viento)
GET     /weather/:city/forecast → Pronóstico 5 días
GET     /history                → Últimas 10 consultas
GET     /health                 → Estado del servicio + API externa
GET     /metrics                → Métricas en formato Prometheus
```

**API externa:** Open-Meteo (gratuita, sin API key) | **Stack:** Python + Flask + SQLite

## Reto 3: Generador de Contraseñas

```
GET     /generate?length=16&numbers=true&symbols=true → Generar contraseña
POST    /batch    → Generar múltiples {cantidad, longitud}
GET     /stats    → Estadísticas de uso
GET     /health   → Estado del servicio
GET     /metrics  → Métricas en formato Prometheus
```

**Stack:** Node.js + Express

## Reto 4: API de Libros

```
GET     /books           → Listar libros (paginado)
GET     /books/:id       → Detalle de un libro
GET     /books/search?q= → Buscar por título o autor
POST    /books           → Agregar libro {titulo, autor, año}
DELETE  /books/:id       → Eliminar libro
GET     /health          → Estado del servicio + BD
GET     /metrics         → Métricas en formato Prometheus
```

**Stack:** Python + FastAPI + SQLite

## Reto 5: API de Cotización de Monedas

```
GET     /rate/:from/:to  → Cotización (ej: /rate/USD/COP)
GET     /rates/:base     → Todas las tasas para una moneda
GET     /history         → Últimas 20 consultas
GET     /health          → Estado del servicio + API externa
GET     /metrics         → Métricas en formato Prometheus
```

**API externa:** https://open.er-api.com (gratuita) | **Stack:** Node.js + Express + SQLite

## Reto 6: Mini Blog

```
GET     /posts      → Listar posts
GET     /posts/:id  → Ver un post
POST    /posts      → Crear post {titulo, contenido, autor}
PUT     /posts/:id  → Editar post
DELETE  /posts/:id  → Eliminar post
GET     /health     → Estado del servicio + BD
GET     /metrics    → Métricas en formato Prometheus
```

**Stack:** Python + Flask + SQLite

## Reto 7: API de Encuestas 🆕

```
POST    /surveys             → Crear encuesta {pregunta, opciones[]}
GET     /surveys             → Listar encuestas activas
POST    /surveys/:id/vote    → Votar {opcion}
GET     /surveys/:id/results → Resultados con conteo y porcentaje
GET     /health              → Estado del servicio + BD
GET     /metrics             → Métricas en formato Prometheus
```

**Stack:** Python + FastAPI + SQLite

## Reto 8: API de Reservas de Salas 🆕

```
GET     /rooms               → Listar salas disponibles
POST    /reservations        → Reservar {sala, fecha, hora_inicio, hora_fin, responsable}
GET     /reservations?date=  → Reservas de un día
DELETE  /reservations/:id    → Cancelar reserva
GET     /health              → Estado del servicio + BD
GET     /metrics             → Métricas en formato Prometheus
```

**Regla de negocio:** no se permiten reservas que se crucen en la misma sala. **Stack:** Node.js + Express + SQLite

## Reto 9: Reto Libre

Propongan su propia idea. Mínimo 3 endpoints + `/health` + `/metrics`.
**Requiere aprobación del docente** (pídela por correo antes del registro).

---

# 4. Paralelismo: Temas del Curso → Trabajo Final

| # | Tema visto en clase | Dónde se aplica en el proyecto |
|---|---------------------|-------------------------------|
| 1 | Fundamentos DevOps (Cultura CALMS) | `docs/cultura.md` — Reflexión sobre decisiones DevOps y principios CALMS |
| 2 | Principios de Automatización | Pipeline GitHub Actions 100% automatizado (6 stages) |
| 3 | Git & GitHub | Repo con estrategia de branching, PRs con review, commits semánticos |
| 4 | Branching & Pipelines | GitHub Actions con stages definidos + estrategia documentada |
| 5 | Contenerización con Docker | `Dockerfile` creado desde cero + `docker-compose.yml` + imagen publicada |
| 6 | Pruebas Unitarias (AAA, pytest) | Mínimo 5 tests con estructura AAA, integrados al pipeline |
| 7 | Kubernetes | Manifiestos YAML (Deployment + Service) desplegados en EC2 con Minikube o k3s |
| 8 | Artefactos (ciclo de vida, versionado) | Imagen Docker versionada + GitHub Releases + publicación en Docker Hub/GHCR |
| 9 | Observabilidad (logs, métricas, trazas) | Prometheus + Grafana con dashboard + `/health` + logs estructurados |
| 10 | Seguridad DevSecOps (SAST, DAST, escaneo) | Escaneo de dependencias + SAST en pipeline + `.env` para secrets |

> **Todos los temas del curso se evalúan.** No hay tema suelto.

---

# 5. Requisitos del Pipeline

## 5.1 Repositorio y Control de Versiones

| Requisito | Detalle |
|-----------|---------|
| Repo privado en GitHub | Agregar al docente (`cesarpalacios`) como collaborator |
| Estrategia de branching | Elegir y documentar: GitHub Flow, Git Flow, o Trunk-Based |
| Protección de main | Solo merge vía Pull Request |
| Commits semánticos | `feat:`, `fix:`, `docs:`, `ci:`, `test:`, `chore:` |
| `.gitignore` | `node_modules/`, `__pycache__/`, `.env`, `*.log`, `.venv/` |
| Mínimo 5 Pull Requests | Con review y aprobación entre integrantes |

## 5.2 Contenerización — Dockerfile desde cero

**NO se proporciona Dockerfile.** Lo construyen desde cero. Deben resolver:
- Imagen base correcta (`python:3.12-slim`, `node:20-alpine`)
- Copiar código e instalar dependencias
- Exponer el puerto correcto
- Comando de inicio con `CMD`
- `docker-compose.yml` que levante todo con un comando

## 5.3 Pruebas Unitarias

| Requisito | Detalle |
|-----------|---------|
| Mínimo 5 tests | Estructura **AAA** (Arrange → Act → Assert) |
| Carpeta `tests/` | Separados del código fuente |
| Integrados al pipeline | Si un test falla, el build se detiene |
| Framework | `pytest` (Python) o `jest` (Node.js) |

## 5.4 GitHub Actions — Pipeline CI/CD (6 stages obligatorios)

```
Push/PR a main → 1.Build → 2.Test → 3.Lint → 4.Artifact → 5.Release → 6.Deploy (AWS EC2 + /health)
```

Si CUALQUIER stage falla → todo se detiene.

## 5.5 Despliegue

- EC2 encendida solo durante la demo y las verificaciones (costo).
- `/health` accesible públicamente al momento de la entrega.

---

# 6. Cronograma

| Hito | Fecha | Qué se verifica |
|------|-------|-----------------|
| Registro de grupos | **viernes 21 nov** | Form diligenciado + reto asignado |
| Hito 1 — Repo + Docker | **martes 25 nov** | Repo privado con collaborator, branching activo, app corre en Docker local |
| Hito 2 — Pipeline hasta Release | **martes 2 dic** | CI con Build/Test/Lint verde, imagen publicada en Docker Hub/GHCR, GitHub Release v0.1.0 |
| Entrega final + demos | **martes 9 dic** | Pipeline completo + despliegue EC2 + demo en vivo 15 min |

---

# 7. Entrega final

1. Repo privado con todo el pipeline y `docs/` completo.
2. Tag `v1.0.0` + **GitHub Release** con notas de la versión.
3. URL pública de `/health` en EC2 funcionando al momento de la demo.
4. **Demo en vivo (15 min por grupo):** pipeline corriendo en un push real, dashboard Grafana con tráfico, respuesta del docente sobre decisiones tomadas.

# 8. Calificación

| Componente | Peso |
|-----------|------|
| Pipeline CI/CD completo y funcional (6 stages) | 30% |
| Docker + Kubernetes en EC2 | 20% |
| Pruebas unitarias + calidad de código (lint, commits, PRs) | 15% |
| Observabilidad (Prometheus + Grafana + logs) | 15% |
| DevSecOps (escaneos, manejo de secrets) | 10% |
| Documentación (`docs/`, README, cultura.md) + demo | 10% |

**Reglas de honestidad:** el trabajo se defiende en la demo — cualquier integrante debe poder explicar cualquier parte del pipeline. Copia entre grupos = nota mínima para ambos.

# 9. Dudas

Correo del curso o pregunta en clase. ¡Éxitos!
