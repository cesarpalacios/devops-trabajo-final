# Trabajo Final — DevOps Pipeline Challenge
## Universidad Nacional de Colombia — Sede Manizales
### DevOps & Automatización | 2026-1

**Docente:** Cesar Augusto Palacios Alarcón
**Repositorio:** https://github.com/cesarpalacios/devops-trabajo-final (Privado)

---

# Objetivo

Cada grupo debe diseñar, implementar y presentar un **pipeline DevOps completo** para una aplicación web, demostrando la aplicación práctica de **todos los temas vistos en el curso**.

> **La evaluación NO es la aplicación.** Es el ecosistema DevOps construido alrededor de ella.

---

# Temas Evaluados

| # | Tema del Curso | Cómo se evalúa en el proyecto |
|---|----------------|-------------------------------|
| 1 | Fundamentos DevOps (CALMS) | Documentación de decisiones, responsabilidad compartida en PRs |
| 2 | Automatización | Pipeline CI/CD 100% automatizado |
| 3 | Git & GitHub | Repo con estrategia de branching, PRs, protección de main |
| 4 | Branching & Pipelines | GitHub Actions con stages definidos |
| 5 | Docker | Dockerfile + Docker Compose + imagen publicada |
| 6 | Pruebas Unitarias | Mínimo 5 tests integrados al pipeline |
| 7 | Kubernetes | Despliegue con manifests YAML (bonus) |
| 8 | Artefactos | Build genera artefacto versionado (imagen Docker, .jar, .zip) |
| 9 | Observabilidad | Logs + métricas + health check |
| 10 | Seguridad (DevSecOps) | Sin secretos, escaneo de dependencias, SAST |

---

# Retos Disponibles

Cada grupo elige **UN reto**. El foco NO es la app, es el pipeline.

---

## Reto 1: API REST de Gestión (To-Do App)

**Descripción:** Construir una API REST para gestionar tareas personales con categorías y autenticación.

**Endpoints obligatorios:**
- `POST /auth/register` — Registro de usuario (nombre, email, contraseña)
- `POST /auth/login` — Login (retorna JWT)
- `GET /tasks` — Listar tareas del usuario autenticado
- `POST /tasks` — Crear tarea (título, descripción, categoría)
- `PUT /tasks/:id` — Actualizar tarea
- `DELETE /tasks/:id` — Eliminar tarea

**Reglas:**
- Cada tarea pertenece a un usuario y una categoría
- Contraseñas hasheadas (bcrypt)
- Validación de inputs en todos los endpoints
- Manejo centralizado de errores (400, 401, 403, 404, 500)

**Stack sugerido:**
- Python + FastAPI + SQLite + SQLAlchemy + python-jose + bcrypt
- Node.js + Express + SQLite + jsonwebtoken + bcrypt

---

## Reto 2: API de Clima en Tiempo Real

**Descripción:** Construir una API que reciba una ciudad y retorne el clima actual, con caché y múltiples endpoints.

**Endpoints obligatorios:**
- `GET /weather/:city` — Clima actual (temperatura, humedad, viento, descripción)
- `GET /weather/:city/forecast` — Pronóstico 5 días
- `GET /weather/:city/history` — Últimas 10 consultas (historial local)
- `GET /health` — Estado del servicio + API externa

**API externa:** Open-Meteo (gratuita, sin API key) — https://open-meteo.com

**Reglas:**
- Cache de resultados (no consultar la API externa más de 1 vez por ciudad cada 10 min)
- Historial local de consultas
- Manejo de errores si la API externa no responde

**Stack sugerido:**
- Python + Flask/FastAPI + SQLite (para historial)
- Node.js + Express + SQLite

---

## Reto 3: Microservicio de Procesamiento de Datos

**Descripción:** Construir un servicio que reciba archivos CSV/JSON, los procese y retorne estadísticas.

**Endpoints obligatorios:**
- `POST /upload` — Subir archivo CSV o JSON (máximo 10MB)
- `GET /process/:id` — Procesar archivo y retornar estadísticas (promedio, min, max, total registros)
- `GET /results/:id` — Obtener resultados procesados
- `GET /health` — Estado del servicio

**Reglas:**
- Soporte para CSV y JSON
- Validación de estructura del archivo
- Almacenamiento temporal de archivos
- Procesamiento asíncrono (retornar job ID, consultar resultado después)

**Stack sugerido:**
- Python + FastAPI + Redis (para jobs) + SQLite
- Node.js + Express + Bull queue + SQLite

---

# Requisitos Obligatorios del Pipeline

## 1. Repositorio y Control de Versiones

| Requisito | Detalle |
|-----------|---------|
| Repo en GitHub | **Privado.** Agregar al docente (`cesarpalacios`) como collaborator |
| Estrategia de branching | Elegir y documentar: GitHub Flow, Git Flow, o Trunk-Based |
| Protección de main | No push directo — solo vía Pull Request |
| Commits semánticos | `feat:`, `fix:`, `docs:`, `ci:`, `test:`, `chore:` |
| `.gitignore` correcto | `node_modules/`, `__pycache__/`, `.env`, `*.log`, `.venv/` |
| Mínimo 5 Pull Requests | Con review y aprobación entre integrantes del grupo |

## 2. Contenerización (Docker)

| Requisito | Detalle |
|-----------|---------|
| `Dockerfile` | Build optimizado (multi-stage recomendado para reducir tamaño) |
| `docker-compose.yml` | `docker compose up` levanta app + base de datos |
| `.env` + `.env.example` | Variables de entorno (`.env` en `.gitignore`, `.env.example` en repo) |
| Imagen publicada | Push automático a Docker Hub o GitHub Container Registry desde el pipeline |

## 3. Integración Continua (CI) — GitHub Actions

| Requisito | Detalle |
|-----------|---------|
| Pipeline CI | Archivo `.github/workflows/ci-cd.yml` |
| Trigger | Se ejecuta en cada Push y Pull Request a `main` |
| Build | Instalar dependencias y compilar/empaquetar automáticamente |
| Tests | Mínimo **5 pruebas unitarias** que pasen en el pipeline |
| Linting | Análisis estático: `flake8`/`ruff` (Python) o `eslint` (Node.js) |
| Artefacto | Imagen Docker con tag (ej: `v1.0-${{ github.sha }}`) |

## 4. Entrega Continua (CD)

| Requisito | Detalle |
|-----------|---------|
| Despliegue automático | Tras merge a `main`, despliegue sin intervención manual |
| Entorno | Docker Compose local, Minikube, o cloud (EC2, Render, Railway) |
| Kubernetes (bonus +5%) | Manifiestos YAML: `deployment.yml` + `service.yml` |

## 5. Observabilidad

| Requisito | Detalle |
|-----------|---------|
| Logs estructurados | La aplicación genera logs en formato JSON o estándar |
| Health check | `GET /health` retorna estado de la app + BD + APIs externas |
| Métricas/Dashboard | Al menos UNO: Prometheus + Grafana, dashboard de logs, o alerta básica |

## 6. Seguridad (DevSecOps)

| Requisito | Detalle |
|-----------|---------|
| Sin secretos en código | Verificado por el pipeline (grep o herramienta automática) |
| Escaneo de dependencias | `pip audit` / `npm audit` / Safety en el pipeline CI |
| Análisis estático (SAST) | Bandit (Python) / SonarCloud / CodeQL en el pipeline |

## 7. Documentación

| Archivo | Contenido obligatorio |
|---------|----------------------|
| `README.md` | Descripción, arquitectura, instrucciones de instalación, ejecución, endpoints |
| `docs/pipeline.md` | Diagrama del pipeline CI/CD + explicación de cada stage |
| `docs/branching.md` | Estrategia elegida + justificación + diagrama de flujo |
| `docs/observabilidad.md` | Qué se monitorea, cómo, y qué hacer si algo falla |
| `docs/seguridad.md` | Medidas de seguridad implementadas + herramientas usadas |

---

# Estructura del Repositorio

```
mi-proyecto-devops/
├── src/                        # Código fuente de la aplicación
├── tests/                      # Pruebas unitarias (mínimo 5)
├── k8s/                        # Manifiestos Kubernetes (bonus)
│   ├── deployment.yml
│   └── service.yml
├── .github/
│   └── workflows/
│       └── ci-cd.yml           # Pipeline GitHub Actions
├── docker-compose.yml          # App + servicios
├── Dockerfile                  # Build de la imagen
├── .env.example                # Variables sin valores reales
├── .gitignore
├── docs/
│   ├── pipeline.md
│   ├── branching.md
│   ├── observabilidad.md
│   └── seguridad.md
└── README.md
```

---

# Rúbrica de Evaluación

| Criterio | Peso | Lo que evaluamos |
|----------|------|-----------------|
| **Pipeline CI/CD funcional** | 20% | ¿Se ejecuta automáticamente? ¿Build + tests + lint pasan? ¿Despliegue funciona? |
| **Contenerización (Docker)** | 15% | ¿`docker compose up` levanta todo? ¿Imagen publicada? ¿Multi-stage? |
| **Pruebas Unitarias** | 10% | ¿Mínimo 5 tests? ¿Pasando en el pipeline? ¿Estructura AAA? |
| **Observabilidad** | 10% | ¿Logs estructurados? ¿`/health` funciona? ¿Dashboard o métricas? |
| **Seguridad (DevSecOps)** | 10% | ¿Sin secretos en código? ¿Escaneo automático? ¿SAST? |
| **Kubernetes / Despliegue** | 10% | ¿Despliegue funcional? K8s = bonus (+5%). Docker Compose = mínimo |
| **Documentación y Branching** | 10% | ¿README completo? ¿Docs? ¿PRs con review? ¿Commits semánticos? |
| **Presentación y Demo** | 15% | ¿Demo en vivo funciona? ¿Claridad? ¿Manejo del tiempo (15 min)? |

**Total: 100%** | K8s bonus: hasta +5% extra

---

# Formato de Presentación — Demo Day (15 min)

| Sección | Tiempo | Qué deben mostrar |
|---------|--------|-------------------|
| **1. La Aplicación** | 2 min | ¿Qué hace? ¿Qué endpoints tiene? ¿Qué reto eligieron? |
| **2. El Pipeline** | 4 min | Diagrama del CI/CD. Explicar cada stage: trigger → build → test → lint → deploy |
| **3. Demo en vivo** | 5 min | Hacer un cambio → Abrir PR → Ver pipeline corriendo → Ver despliegue actualizado |
| **4. Observabilidad y Seguridad** | 2 min | Mostrar logs, `/health`, dashboard, escaneo de seguridad |
| **5. Preguntas** | 2 min | El docente pregunta sobre el código y las decisiones tomadas |

**Regla de oro:**
> Si no pueden explicar su propio código, pierden puntos. La IA y Google ayudan, pero **ustedes son los arquitectos**.

---

# Stack Tecnológico Sugerido

| Componente | Python | Node.js |
|------------|--------|---------|
| Framework | FastAPI o Flask | Express |
| Base de datos | SQLite | SQLite |
| Tests | pytest | Jest |
| Linter | flake8 / ruff | eslint |
| CI/CD | GitHub Actions | GitHub Actions |
| Contenedores | Docker + Compose | Docker + Compose |
| Orquestación | Minikube (K8s) | Minikube (K8s) |
| Observabilidad | Prometheus + Grafana | Prometheus + Grafana |
| Seguridad SAST | Bandit | eslint security plugin |
| Seguridad deps | pip audit / Safety | npm audit |

---

# Fechas Clave

| Fecha | Actividad |
|-------|-----------|
| **Mié 21 mayo** | Asignación de grupos + Explicación del trabajo final |
| **Mar 27 mayo** | Trabajo en clase con acompañamiento del docente |
| **Mié 28 mayo** | **Demo Day — Presentaciones finales (15 min por grupo)** |

---

# Checklist de Entrega

Antes del Demo Day, verificar que todo esté listo:

- [ ] Repo privado con docente como collaborator
- [ ] `.github/workflows/ci-cd.yml` funcionando (build + test + lint)
- [ ] Mínimo 5 tests pasando en el pipeline
- [ ] `Dockerfile` + `docker-compose.yml` (todo levanta con un comando)
- [ ] Imagen publicada en Docker Hub o GHCR
- [ ] Despliegue automático tras merge a `main`
- [ ] `GET /health` funcional
- [ ] Logs estructurados
- [ ] Escaneo de seguridad en el pipeline
- [ ] Sin secretos en el código
- [ ] Mínimo 5 Pull Requests con review
- [ ] README.md completo
- [ ] `docs/` con pipeline.md, branching.md, observabilidad.md, seguridad.md
- [ ] `.env.example` con todas las variables necesarias
- [ ] `.gitignore` correcto
- [ ] Demo preparada (15 minutos)

---

*Trabajo Final — DevOps & Automatización — UNAL Sede Manizales — 2026-1*
