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
- **Registro:** diligencia el formulario una sola vez por grupo con los integrantes y el **usuario de GitHub** de al menos un integrante:
  👉 **https://docs.google.com/forms/d/e/1FAIpQLSdAL4SWplE1eziIfx1FBG60kmCdbRlQKgA6uzB9Uvc4QR8lDQ/viewform**
- **Fecha límite de registro:** viernes 28 de noviembre de 2026. Nadie queda solo: quien no logre armarse en un grupo, el docente lo asigna a un grupo incompleto.
- **Asignación de retos:** el docente asigna **UN reto distinto** a cada grupo (sección 3) y comparte el acceso a este repositorio del enunciado con los usuarios de GitHub registrados. Cada grupo recibe por correo su reto asignado y las instrucciones de trabajo.

# 2. Repositorio del grupo

- Crear un repo **privado** llamado `devops-tf-<nombre-corto>` (ej: `devops-tf-clima`).
- Agregar al docente (`cesarpalacios`) como **collaborator** (Settings → Collaborators).
- El código base del reto lo construyen ustedes desde el enunciado; lo que NO se entrega es el ecosistema DevOps.

# 3. Retos disponibles

Los retos estan documentados en la carpeta **[retos/](retos/README.md)** — cada caso tiene su propio README con el problema, los endpoints que debe exponer la API y las reglas de negocio.

| Reto | Caso | Stack |
|------|------|-------|
| 1 | [API de Tareas](retos/reto-1-tareas/README.md) | FastAPI + SQLite |
| 2 | [API de Clima](retos/reto-2-clima/README.md) | Flask + Open-Meteo |
| 3 | [Generador de Contrasenas](retos/reto-3-passwords/README.md) | Node/Express |
| 4 | [API de Libros](retos/reto-4-libros/README.md) | FastAPI + SQLite |
| 5 | [API de Cotizacion de Monedas](retos/reto-5-monedas/README.md) | Node + SQLite |
| 6 | [Mini Blog](retos/reto-6-miniblog/README.md) | Flask + SQLite |
| 7 | [API de Encuestas](retos/reto-7-encuestas/README.md) 🆕 | FastAPI + SQLite |
| 8 | [API de Reservas de Salas](retos/reto-8-reservas/README.md) 🆕 | Node + SQLite |
| 9 | [Reto Libre](retos/reto-9-libre/README.md) | Libre |

Cada grupo elige **UN caso** y lo indica en el formulario de registro.

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

El entregable del trabajo final son **dos componentes obligatorios**: el **documento escrito** de documentación del proceso y el **repositorio** con el pipeline funcionando.

## 7.1 Documento escrito de documentación del proceso (PDF)

Informe de **10 a 15 páginas** que documente TODO el proceso seguido por el grupo. Estructura mínima:

1. **Portada** — curso, reto asignado, integrantes y correo.
2. **Introducción y objetivos** — qué construyeron y por qué.
3. **Arquitectura de la solución** — componentes, diagrama y decisiones de diseño.
4. **Gestión del repositorio** — estrategia de branching, commits semánticos, PRs y reviews.
5. **Pipeline CI/CD** — los 6 stages: qué hace cada uno, con evidencia de ejecución.
6. **Pruebas** — qué cubren los tests, resultados.
7. **Contenerización** — decisiones del Dockerfile y docker-compose.
8. **Despliegue en AWS EC2** — procedimiento y evidencia del `/health`.
9. **Observabilidad** — capturas del dashboard Grafana y análisis de métricas.
10. **Seguridad DevSecOps** — escaneos realizados y manejo de secrets.
11. **Dificultades y soluciones** — problemas reales del equipo y cómo los resolvieron.
12. **Conclusiones y aprendizajes** — qué se llevaron del curso.
13. **Anexos** — capturas adicionales.

> El documento se entrega en PDF por el canal que indique el docente antes de la fecha de entrega final.

## 7.2 Repositorio

1. Repo privado con todo el pipeline y `docs/` completo.
2. Tag `v1.0.0` + **GitHub Release** con notas de la versión.
3. URL pública de `/health` en EC2 funcionando al momento de la demo.
4. **Demo en vivo (15 min por grupo):** pipeline corriendo en un push real, dashboard Grafana con tráfico, respuesta del docente sobre decisiones tomadas.

# 8. Calificación

| Componente | Peso |
|-----------|------|
| Pipeline CI/CD completo y funcional (6 stages) | 25% |
| Docker + Kubernetes en EC2 | 20% |
| Pruebas unitarias + calidad de código (lint, commits, PRs) | 15% |
| Observabilidad (Prometheus + Grafana + logs) | 15% |
| DevSecOps (escaneos, manejo de secrets) | 10% |
| Documento escrito de documentación del proceso + `docs/`, README, cultura.md | 15% |

**Reglas de honestidad:** el trabajo se defiende en la demo — cualquier integrante debe poder explicar cualquier parte del pipeline. Copia entre grupos = nota mínima para ambos.

# 9. Dudas

Correo del curso o pregunta en clase. ¡Éxitos!
