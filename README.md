# Trabajo Final — DevOps Pipeline Challenge
## Universidad Nacional de Colombia — Sede Manizales
### DevOps & Automatización | 2026-1

**Docente:** Cesar Augusto Palacios Alarcón
**Repositorio:** https://github.com/cesarpalacios/devops-trabajo-final (Privado)

---

# Objetivo

Cada grupo debe montar un **pipeline DevOps completo** para una aplicación web sencilla, desde el código hasta el despliegue en **AWS** usando **GitHub Actions**.

> **La app es simple a propósito.** Lo que evaluamos es el pipeline, los contenedores, los tests, la observabilidad y el despliegue — no la complejidad del código.

---

# Retos Disponibles

Cada grupo elige **UN reto**. Todos incluyen código base para que se enfoquen en el ecosistema DevOps.

---

## Reto 1: API de Tareas (To-Do)

**App:** API REST con 4 endpoints para gestionar tareas.

```
GET    /tasks      → Listar tareas
POST   /tasks      → Crear tarea {titulo, descripcion}
PUT    /tasks/:id  → Actualizar tarea
DELETE /tasks/:id  → Eliminar tarea
GET    /health     → Estado del servicio
```

**Stack sugerido:** Python + FastAPI + SQLite
**Deploy:** EC2 con Docker o AWS App Runner

---

## Reto 2: API de Clima

**App:** API que consulta el clima de una ciudad usando Open-Meteo (gratuita, sin API key).

```
GET    /weather/:city     → Clima actual (temp, humedad, viento)
GET    /weather/:city/forecast → Pronóstico 5 días
GET    /history            → Últimas 10 consultas
GET    /health             → Estado del servicio
```

**API externa:** https://open-meteo.com (gratuita)
**Stack sugerido:** Python + Flask + SQLite
**Deploy:** EC2 con Docker

---

## Reto 3: Generador de Contraseñas

**App:** API que genera contraseñas seguras con diferentes opciones.

```
GET    /generate?length=16&numbers=true&symbols=true  → Generar contraseña
POST   /batch  → Generar múltiples contraseñas {cantidad, longitud}
GET    /stats   → Estadísticas de uso (total generadas, longitud promedio)
GET    /health  → Estado del servicio
```

**Stack sugerido:** Node.js + Express
**Deploy:** EC2 con Docker

---

## Reto 4: API de Libros

**App:** API REST para un catálogo de libros con búsqueda.

```
GET    /books           → Listar libros (con paginación)
GET    /books/:id       → Detalle de un libro
GET    /books/search?q= → Buscar por título o autor
POST   /books           → Agregar libro {titulo, autor, año, genero}
DELETE /books/:id       → Eliminar libro
GET    /health          → Estado del servicio
```

**Stack sugerido:** Python + FastAPI + SQLite
**Deploy:** EC2 con Docker o AWS App Runner

---

## Reto 5: API de Cotización de Monedas

**App:** API que consulta tasas de cambio usando una API gratuita.

```
GET    /rate/:from/:to  → Cotización actual (ej: /rate/USD/COP)
GET    /rates/:base     → Todas las tasas para una moneda base
GET    /history         → Últimas 20 consultas con timestamp
GET    /health          → Estado del servicio
```

**API externa:** https://open.er-api.com (gratuita, sin API key)
**Stack sugerido:** Node.js + Express + SQLite
**Deploy:** EC2 con Docker

---

## Reto 6: Mini Blog

**App:** API REST para publicar y consultar posts de un blog.

```
GET    /posts           → Listar posts (paginado)
GET    /posts/:id       → Ver un post
POST   /posts           → Crear post {titulo, contenido, autor}
PUT    /posts/:id       → Editar post
DELETE /posts/:id       → Eliminar post
GET    /health          → Estado del servicio
```

**Stack sugerido:** Python + Flask + SQLite
**Deploy:** EC2 con Docker

---

## Reto 7: Reto Libre

**App:** Propongan su propia idea. Debe tener mínimo 3 endpoints + `/health`.

**Requisitos:** Aprobación del docente antes de empezar.

---

# Requisitos del Pipeline

## 1. Repositorio (Git/GitHub)

| Requisito | Detalle |
|-----------|---------|
| Repo privado en GitHub | Agregar al docente (`cesarpalacios`) como collaborator |
| Estrategia de branching | Elegir y documentar (GitHub Flow recomendado para simplicidad) |
| Protección de main | Solo merge vía Pull Request |
| Commits semánticos | `feat:`, `fix:`, `docs:`, `ci:`, `test:` |
| `.gitignore` | `node_modules/`, `__pycache__/`, `.env`, `*.log`, `.venv/` |
| Mínimo 5 Pull Requests | Con review entre integrantes |

## 2. Contenerización (Docker)

| Requisito | Detalle |
|-----------|---------|
| `Dockerfile` | Imagen liviana. Los estudiantes pueden copiar uno base y adaptarlo |
| `docker-compose.yml` | `docker compose up` levanta todo |
| `.env.example` | Variables de entorno sin valores reales |
| Imagen publicada | Push a Docker Hub desde el pipeline |

## 3. Integración Continua (CI) — GitHub Actions

| Requisito | Detalle |
|-----------|---------|
| `.github/workflows/ci-cd.yml` | Pipeline automatizado |
| Trigger | Push y Pull Request a `main` |
| Build | Instalar dependencias automáticamente |
| Tests | **Mínimo 5 pruebas unitarias** |
| Linting | `flake8`/`ruff` (Python) o `eslint` (Node.js) |

## 4. Entrega Continua (CD) — Despliegue en AWS

| Requisito | Detalle |
|-----------|---------|
| Despliegue automático | Tras merge a `main`, desplegar en AWS sin intervención manual |
| AWS Academy | Usar el laboratorio de AWS Academy (cuenta gratuita del curso) |
| Opciones de despliegue | EC2 con Docker, AWS App Runner, o Elastic Beanstalk |
| Health check | Verificar que la app respondió después del despliegue |

**Flujo esperado:**
```
Push a main → GitHub Actions:
  1. Build (instalar deps)
  2. Test (5 tests mínimo)
  3. Lint (análisis estático)
  4. Docker build + push a Docker Hub
  5. Deploy a AWS (SSH a EC2 + docker pull + docker run)
```

## 5. Observabilidad

| Requisito | Detalle |
|-----------|---------|
| Logs | La aplicación genera logs legibles (pueden usar la librería estándar) |
| `/health` | Endpoint que retorne estado de la app + BD |
| Dashboard o métricas | Al menos UNO: CloudWatch, Prometheus + Grafana, o dashboard simple |

## 6. Seguridad (DevSecOps)

| Requisito | Detalle |
|-----------|---------|
| Sin secretos en código | `.env` para credenciales AWS, tokens, etc. |
| Escaneo de dependencias | `pip audit` o `npm audit` en el pipeline |
| Sin `sudo` en Dockerfile | Buena práctica de seguridad en contenedores |

## 7. Documentación

| Archivo | Contenido |
|---------|-----------|
| `README.md` | Descripción, cómo clonar, instalar, ejecutar, endpoints, cómo desplegar |
| `docs/pipeline.md` | Diagrama del CI/CD + explicación de cada stage |
| `docs/branching.md` | Estrategia elegida + por qué |

---

# Estructura del Repositorio

```
mi-proyecto-devops/
├── src/                        # Código fuente
├── tests/                      # Pruebas unitarias (mínimo 5)
├── .github/
│   └── workflows/
│       └── ci-cd.yml           # Pipeline GitHub Actions
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── .gitignore
├── docs/
│   ├── pipeline.md
│   └── branching.md
└── README.md
```

---

# Rúbrica de Evaluación

| Criterio | Peso | Lo que evaluamos |
|----------|------|-----------------|
| **Pipeline CI/CD funcional** | 25% | ¿GitHub Actions corre automáticamente? ¿Build + tests + lint pasan? |
| **Despliegue en AWS** | 20% | ¿La app está accesible en AWS? ¿Se despliega automáticamente tras merge? |
| **Contenerización (Docker)** | 15% | ¿`docker compose up` funciona? ¿Imagen publicada en Docker Hub? |
| **Pruebas Unitarias** | 10% | ¿Mínimo 5 tests? ¿Pasando en el pipeline? |
| **Observabilidad** | 10% | ¿Logs visibles? ¿`/health` funciona? ¿Dashboard o métricas? |
| **Seguridad** | 5% | ¿Sin secretos en código? ¿Escaneo de dependencias? |
| **Documentación y Branching** | 5% | ¿README completo? ¿PRs con review? ¿Docs? |
| **Presentación y Demo** | 10% | ¿Demo en vivo funciona? ¿Claridad? ¿Manejo del tiempo (15 min)? |

**Total: 100%**

---

# Formato de Presentación — Demo Day (15 min)

| Sección | Tiempo | Qué mostrar |
|---------|--------|-------------|
| **La App** | 2 min | ¿Qué hace? ¿Qué endpoints tiene? ¿Qué reto eligieron? |
| **El Pipeline** | 3 min | Mostrar GitHub Actions. Explicar cada stage del CI/CD |
| **Demo en vivo** | 5 min | Hacer un cambio → PR → Pipeline corre → App actualizada en AWS |
| **Observabilidad** | 2 min | Mostrar `/health`, logs, dashboard o métricas |
| **Preguntas** | 3 min | El docente pregunta sobre el pipeline y las decisiones |

---

# Stack Tecnológico Sugerido

| Componente | Python | Node.js |
|------------|--------|---------|
| Framework | FastAPI o Flask | Express |
| Base de datos | SQLite (viene con Python) | SQLite |
| Tests | pytest | Jest |
| Linter | flake8 / ruff | eslint |
| CI/CD | GitHub Actions | GitHub Actions |
| Contenedores | Docker + Compose | Docker + Compose |
| Deploy AWS | EC2 + Docker o App Runner | EC2 + Docker o App Runner |
| Observabilidad | CloudWatch o Prometheus | CloudWatch o Prometheus |

---

# Fechas Clave

| Fecha | Actividad |
|-------|-----------|
| **Mié 21 mayo** | Asignación de grupos + Explicación trabajo final |
| **Mar 27 mayo** | Trabajo en clase con acompañamiento |
| **Mié 28 mayo** | **Demo Day — Presentaciones finales** |

---

# Checklist de Entrega

- [ ] Repo privado con docente como collaborator
- [ ] `.github/workflows/ci-cd.yml` funcionando
- [ ] Mínimo 5 tests pasando en el pipeline
- [ ] `Dockerfile` + `docker-compose.yml`
- [ ] Imagen publicada en Docker Hub
- [ ] Despliegue automático en AWS
- [ ] App accesible desde internet (IP pública de EC2 o URL de App Runner)
- [ ] `GET /health` funcional
- [ ] Logs visibles
- [ ] Escaneo de seguridad en el pipeline
- [ ] Sin secretos en el código
- [ ] Mínimo 5 Pull Requests con review
- [ ] `README.md` completo con instrucciones
- [ ] `docs/pipeline.md` y `docs/branching.md`
- [ ] `.env.example` con variables necesarias
- [ ] Demo preparada (15 minutos)

---

*Trabajo Final — DevOps & Automatización — UNAL Sede Manizales — 2026-1*
