# Trabajo Final — DevOps Pipeline Challenge
## Universidad Nacional de Colombia — Sede Manizales
### DevOps & Automatización | 2026-1

**Docente:** Cesar Augusto Palacios Alarcón
**Repositorio:** https://github.com/cesarpalacios/devops-trabajo-final (Privado)

---

# Objetivo

Cada grupo debe construir un **pipeline DevOps completo** desde cero: crear el Dockerfile, configurar GitHub Actions, generar artefactos, publicar releases, configurar observabilidad con Prometheus + Grafana, aplicar seguridad DevSecOps, y desplegar en **AWS EC2**.

> **Esto no es un ejercicio de programación.** Es un ejercicio de **flujo DevOps completo**: cada tema visto en clase debe reflejarse en el proyecto.

---

# Paralelismo: Temas del Curso → Trabajo Final

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

# Retos Disponibles

Cada grupo elige **UN reto**. Se les entrega código fuente funcional — lo que NO se les entrega es el ecosistema DevOps.

---

## Reto 1: API de Tareas (To-Do)

```
GET    /tasks      → Listar tareas
POST   /tasks      → Crear tarea {titulo, descripcion}
PUT    /tasks/:id  → Actualizar tarea
DELETE /tasks/:id  → Eliminar tarea
GET    /health     → Estado del servicio + BD
GET    /metrics    → Métricas en formato Prometheus
```

**Stack:** Python + FastAPI + SQLite

---

## Reto 2: API de Clima

```
GET    /weather/:city       → Clima actual (temperatura, humedad, viento)
GET    /weather/:city/forecast → Pronóstico 5 días
GET    /history             → Últimas 10 consultas
GET    /health              → Estado del servicio + API externa
GET    /metrics             → Métricas en formato Prometheus
```

**API externa:** Open-Meteo (gratuita, sin API key)
**Stack:** Python + Flask + SQLite

---

## Reto 3: Generador de Contraseñas

```
GET    /generate?length=16&numbers=true&symbols=true → Generar contraseña
POST   /batch    → Generar múltiples {cantidad, longitud}
GET    /stats    → Estadísticas de uso
GET    /health   → Estado del servicio
GET    /metrics  → Métricas en formato Prometheus
```

**Stack:** Node.js + Express

---

## Reto 4: API de Libros

```
GET    /books           → Listar libros (paginado)
GET    /books/:id       → Detalle de un libro
GET    /books/search?q= → Buscar por título o autor
POST   /books           → Agregar libro {titulo, autor, año}
DELETE /books/:id       → Eliminar libro
GET    /health          → Estado del servicio + BD
GET    /metrics         → Métricas en formato Prometheus
```

**Stack:** Python + FastAPI + SQLite

---

## Reto 5: API de Cotización de Monedas

```
GET    /rate/:from/:to  → Cotización (ej: /rate/USD/COP)
GET    /rates/:base     → Todas las tasas para una moneda
GET    /history         → Últimas 20 consultas
GET    /health          → Estado del servicio + API externa
GET    /metrics         → Métricas en formato Prometheus
```

**API externa:** https://open.er-api.com (gratuita)
**Stack:** Node.js + Express + SQLite

---

## Reto 6: Mini Blog

```
GET    /posts      → Listar posts
GET    /posts/:id  → Ver un post
POST   /posts      → Crear post {titulo, contenido, autor}
PUT    /posts/:id  → Editar post
DELETE /posts/:id  → Eliminar post
GET    /health     → Estado del servicio + BD
GET    /metrics    → Métricas en formato Prometheus
```

**Stack:** Python + Flask + SQLite

---

## Reto 7: Reto Libre

Propongan su propia idea. Mínimo 3 endpoints + `/health` + `/metrics`.
**Requiere aprobación del docente.**

---

# Requisitos del Pipeline

## 1. Repositorio y Control de Versiones (Sesiones 3-4)

| Requisito | Detalle |
|-----------|---------|
| Repo privado en GitHub | Agregar al docente (`cesarpalacios`) como collaborator |
| Estrategia de branching | Elegir y documentar: GitHub Flow, Git Flow, o Trunk-Based |
| Protección de main | Solo merge vía Pull Request |
| Commits semánticos | `feat:`, `fix:`, `docs:`, `ci:`, `test:`, `chore:` |
| `.gitignore` | `node_modules/`, `__pycache__/`, `.env`, `*.log`, `.venv/` |
| Mínimo 5 Pull Requests | Con review y aprobación entre integrantes |

## 2. Contenerización — Dockerfile desde cero (Sesión 5)

**NO se les proporciona Dockerfile.** Lo construyen desde cero.

**Lo que deben resolver:**
- Imagen base correcta (`python:3.12-slim`, `node:20-alpine`)
- Copiar código e instalar dependencias
- Exponer el puerto correcto
- Definir comando de inicio con `CMD`
- Crear `docker-compose.yml` que levante todo con un comando

**Errores que encontrarán (intencionales):**
- SQLite necesita permisos de escritura en el contenedor
- Las dependencias no se instalan sin el `requirements.txt` o `package.json` correcto
- El puerto del Dockerfile no coincide con la app
- Variables de entorno no definidas en el contenedor

## 3. Pruebas Unitarias (Sesión 6)

| Requisito | Detalle |
|-----------|---------|
| Mínimo 5 tests | Con estructura **AAA** (Arrange → Act → Assert) |
| En carpeta `tests/` | Separados del código fuente |
| Integrados al pipeline | Si un test falla, el build se detiene |
| Framework | `pytest` (Python) o `jest` (Node.js) |

**Ejemplo de test con estructura AAA:**
```python
def test_crear_tarea():
    # Arrange
    datos = {"titulo": "Estudiar DevOps", "descripcion": "Leer sobre CI/CD"}
    
    # Act
    respuesta = client.post("/tasks", json=datos)
    
    # Assert
    assert respuesta.status_code == 201
    assert respuesta.json()["titulo"] == "Estudiar DevOps"
```

## 4. GitHub Actions — Pipeline CI/CD (Sesiones 4-5)

Deben crear `.github/workflows/ci-cd.yml` con **6 stages obligatorios**:

```
Push/PR a main
      │
      ▼
┌─────────────┐
│  1. Build    │  → Instalar dependencias, verificar que compila
├─────────────┤
│  2. Test     │  → Correr 5+ pruebas unitarias (pytest / jest)
├─────────────┤
│  3. Lint     │  → Análisis estático (flake8/ruff o eslint)
├─────────────┤
│  4. Artifact │  → Construir imagen Docker con tag versionado
├─────────────┤
│  5. Release  │  → Push imagen a Docker Hub + crear GitHub Release
├─────────────┤
│  6. Deploy   │  → Desplegar en AWS EC2 + verificar /health
└─────────────┘

Si CUALQUIER stage falla → todo se detiene → notificación al equipo
```

**Detalle de cada stage:**

### Stage 1: Build
```yaml
- name: Build
  run: |
    pip install -r requirements.txt    # Python
    # O: npm install                   # Node.js
```

### Stage 2: Test
```yaml
- name: Test
  run: |
    pytest tests/ -v                    # Python
    # O: npm test                       # Node.js
```

### Stage 3: Lint
```yaml
- name: Lint
  run: |
    ruff check src/                     # Python
    # O: npx eslint src/                # Node.js
```

### Stage 4: Artifact (Docker Build)
```yaml
- name: Build Docker Image
  run: |
    docker build -t mi-app:v${{ github.run_number }} .
    docker tag mi-app:v${{ github.run_number }} mi-app:latest
```

### Stage 5: Release
```yaml
- name: Push to Docker Hub
  run: |
    docker push ${{ secrets.DOCKER_USER }}/mi-app:v${{ github.run_number }}
    docker push ${{ secrets.DOCKER_USER }}/mi-app:latest
```

### Stage 6: Deploy a EC2
```yaml
- name: Deploy to AWS EC2
  run: |
    ssh ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} "
      docker pull ${{ secrets.DOCKER_USER }}/mi-app:latest &&
      docker stop mi-app || true &&
      docker rm mi-app || true &&
      docker run -d --name mi-app -p 8080:8080 ${{ secrets.DOCKER_USER }}/mi-app:latest
    "
    curl http://${{ secrets.EC2_HOST }}:8080/health
```

## 5. Kubernetes (Sesión 7)

| Requisito | Detalle |
|-----------|---------|
| Manifiestos YAML | `k8s/deployment.yml` + `k8s/service.yml` |
| Deployment | Definir replicas, imagen, puertos, variables de entorno |
| Service | Exponer la app (NodePort o LoadBalancer) |
| Despliegue funcional | La app corre en K8s dentro de la EC2 (Minikube o k3s) |

**Ejemplo de Deployment (no copiar tal cual — adaptar):**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: mi-app
  template:
    metadata:
      labels:
        app: mi-app
    spec:
      containers:
      - name: mi-app
        image: ???         # Definir imagen correcta
        ports:
        - containerPort: ???  # Definir puerto correcto
```

## 6. Artefactos y Releases (Sesión 8)

| Requisito | Detalle |
|-----------|---------|
| Imagen Docker versionada | Tag con `v1.0`, `v2.0`, etc. (no solo `latest`) |
| Publicada en registry | Docker Hub o GitHub Container Registry |
| GitHub Release | Tag + release notes al hacer merge a main |
| Ciclo de vida documentado | `docs/artefactos.md` explicando: generación → almacenamiento → versionado → distribución |

**Ciclo de vida del artefacto (visto en clase):**
```
1. Generación    → Docker build produce la imagen
2. Almacenamiento → Push a Docker Hub / GHCR
3. Versionado     → Tag semántico (v1.0.0, v1.1.0)
4. Distribución   → EC2 hace pull y despliega
```

## 7. Observabilidad — Prometheus + Grafana (Sesiones 9-11)

| Requisito | Detalle |
|-----------|---------|
| `/metrics` | Endpoint con métricas en formato Prometheus (requests totales, latencia, errores) |
| Prometheus | Configurado para scrapear `/metrics` cada 5 segundos |
| Grafana | Dashboard con al menos 2 paneles (requests por segundo + tiempo de respuesta) |
| `/health` | Endpoint que verifique app + BD + APIs externas |
| Logs | La app genera logs estructurados visibles con `docker logs` |
| `docker-compose.yml` | Incluir app + Prometheus + Grafana |

**Configuración de Prometheus (`prometheus.yml`):**
```yaml
global:
  scrape_interval: 5s
scrape_configs:
  - job_name: 'mi-app'
    static_configs:
      - targets: ['app:8080']  # Ajustar puerto
```

**El docker-compose debe incluir:**
```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
  prometheus:
    image: prom/prometheus
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

## 8. Seguridad DevSecOps (Sesión 12)

| Requisito | Detalle |
|-----------|---------|
| Sin secretos en código | `.env` + GitHub Secrets para credenciales AWS y Docker Hub |
| Escaneo de dependencias | `pip audit` o `npm audit` en el pipeline (stage obligatorio) |
| SAST (análisis estático) | Bandit (Python) o CodeQL en el pipeline |
| Dockerfile seguro | Sin `sudo`, usuario no-root, imagen base slim/alpine |
| Sin vulnerabilidades conocidas | El pipeline falla si hay dependencias con vulnerabilidades críticas |

**Stage de seguridad en el pipeline:**
```yaml
- name: Security Scan
  run: |
    pip audit                          # Escaneo de dependencias Python
    # O: npm audit                     # Node.js
    bandit -r src/                     # SAST Python
```

## 9. Cultura DevOps — Reflexión CALMS (Sesiones 1-2)

| Requisito | Detalle |
|-----------|---------|
| `docs/cultura.md` | Reflexión escrita sobre cómo aplicaron cada pilar CALMS en el proyecto |

**Estructura del documento:**
```markdown
## Cultura DevOps en nuestro proyecto

### C — Culture (Cultura)
¿Cómo colaboramos? ¿Cómo nos comunicamos? ¿Hay responsabilidad compartida?

### A — Automation (Automatización)
¿Qué automatizamos? ¿Qué procesos eran manuales y los convertimos en automáticos?

### L — Lean (Optimización)
¿Qué desperdicios eliminamos? ¿Qué procesos simplificamos?

### M — Measurement (Medición)
¿Qué métricas monitoreamos? ¿Cómo sabemos que todo funciona bien?

### S — Sharing (Compartir)
¿Cómo compartimos conocimiento entre el equipo? ¿PRs, docs, code review?
```

## 10. Documentación Completa

| Archivo | Contenido | Tema del curso |
|---------|-----------|----------------|
| `README.md` | Descripción, arquitectura, cómo clonar, ejecutar, endpoints, desplegar en AWS | General |
| `docs/pipeline.md` | Diagrama CI/CD + explicación de cada stage + flujo de artefactos | Sesión 4-5 |
| `docs/branching.md` | Estrategia elegida + justificación + diagrama de flujo | Sesión 4 |
| `docs/artefactos.md` | Ciclo de vida del artefacto: generación → almacenamiento → versionado → distribución | Sesión 8 |
| `docs/observabilidad.md` | Qué métricas se monitorean, dashboard, qué hacer si algo falla | Sesión 9-11 |
| `docs/seguridad.md` | Medidas DevSecOps implementadas, herramientas, resultados del escaneo | Sesión 12 |
| `docs/cultura.md` | Reflexión CALMS aplicada al proyecto | Sesión 1-2 |
| `docs/k8s.md` | Diagrama de componentes K8s + manifiestos explicados | Sesión 7 |

---

# Estructura del Repositorio

```
mi-proyecto-devops/
├── src/                        # Código fuente (proporcionado)
├── tests/                      # Pruebas unitarias (mínimo 5, AAA)
├── k8s/                        # Manifiestos Kubernetes
│   ├── deployment.yml          # LO CONSTRUYEN ELLOS
│   └── service.yml             # LO CONSTRUYEN ELLOS
├── monitoring/                 # Configuración de observabilidad
│   └── prometheus.yml          # LO CONSTRUYEN ELLOS
├── .github/
│   └── workflows/
│       └── ci-cd.yml           # Pipeline completo (LO CONSTRUYEN ELLOS)
├── docker-compose.yml          # App + Prometheus + Grafana (LO CONSTRUYEN ELLOS)
├── Dockerfile                  # LO CONSTRUYEN ELLOS
├── .env.example                # LO CONSTRUYEN ELLOS
├── .gitignore                  # LO CONSTRUYEN ELLOS
├── docs/
│   ├── pipeline.md
│   ├── branching.md
│   ├── artefactos.md
│   ├── observabilidad.md
│   ├── seguridad.md
│   ├── cultura.md
│   └── k8s.md
└── README.md
```

> **Lo que se les da:** Código fuente funcional con tests de ejemplo
> **Lo que construyen:** TODO el ecosistema DevOps (Dockerfile, pipeline, K8s, Prometheus, Grafana, seguridad, documentación)

---

# Rúbrica de Evaluación

| Criterio | Peso | Tema del curso | Lo que evaluamos |
|----------|------|----------------|-----------------|
| **Pipeline CI/CD (GitHub Actions)** | 15% | Sesiones 4-5 | ¿Los 6 stages corren? ¿Falla si hay error? |
| **Artefactos y Releases** | 10% | Sesión 8 | ¿Imagen versionada? ¿GitHub Release? ¿Ciclo documentado? |
| **Despliegue en AWS EC2** | 15% | Sesión 5, 7 | ¿App accesible? ¿Deploy automático? ¿`/health` responde? |
| **Contenerización (Dockerfile)** | 10% | Sesión 5 | ¿Creado desde cero? ¿`docker compose up` funciona? |
| **Kubernetes** | 10% | Sesión 7 | ¿Manifiestos YAML? ¿Despliegue funcional en K8s? |
| **Pruebas Unitarias** | 10% | Sesión 6 | ¿Mínimo 5 tests AAA? ¿Pasando en pipeline? |
| **Observabilidad (Prometheus + Grafana)** | 10% | Sesiones 9-11 | ¿`/metrics`? ¿Dashboard con 2+ paneles? ¿Logs? |
| **Seguridad (DevSecOps)** | 5% | Sesión 12 | ¿Escaneo deps? ¿SAST? ¿Sin secrets? |
| **Documentación completa** | 5% | Todas | ¿7 docs? ¿README? ¿Diagramas? |
| **Cultura DevOps (CALMS)** | 5% | Sesiones 1-2 | ¿Reflexión escrita? ¿Responsabilidad compartida en PRs? |
| **Presentación y Demo** | 5% | — | ¿Demo en vivo? ¿Claridad? ¿15 min? |

**Total: 100%**

---

# Formato de Presentación — Demo Day (15 min)

| Sección | Tiempo | Qué mostrar |
|---------|--------|-------------|
| **La App + Cultura** | 2 min | ¿Qué hace? ¿Qué reto? ¿Cómo trabajaron en equipo (CALMS)? |
| **El Pipeline + Artefactos** | 3 min | Mostrar GitHub Actions. Explicar cada stage. Mostrar Docker Hub + GitHub Releases |
| **Kubernetes** | 2 min | Mostrar manifiestos. Explicar Deployment + Service |
| **Demo en vivo** | 4 min | Hacer cambio → PR → Pipeline → Deploy → App actualizada en AWS |
| **Observabilidad** | 2 min | Mostrar Grafana dashboard, `/metrics`, `/health`, logs |
| **Seguridad** | 1 min | Mostrar resultado del escaneo de deps + SAST |
| **Preguntas** | 1 min | El docente pregunta sobre el pipeline y las decisiones |

---

# Fechas Clave

| Fecha | Actividad |
|-------|-----------|
| **Mié 21 mayo** | Asignación de grupos + Explicación trabajo final |
| **Mar 27 mayo** | Trabajo en clase con acompañamiento |
| **Mié 28 mayo** | **Demo Day — Presentaciones finales** |

---

# Checklist de Entrega

**Pipeline:**
- [ ] `.github/workflows/ci-cd.yml` con 6 stages + escaneo de seguridad
- [ ] Build automático
- [ ] Mínimo 5 tests con estructura AAA pasando
- [ ] Lint pasando
- [ ] Artefacto Docker construido y versionado
- [ ] Release en Docker Hub + GitHub Release

**Docker:**
- [ ] `Dockerfile` creado desde cero
- [ ] `docker-compose.yml` con app + Prometheus + Grafana
- [ ] `.env.example` con todas las variables

**AWS + Kubernetes:**
- [ ] EC2 creada con Docker + K8s (Minikube/k3s)
- [ ] App accesible desde IP pública
- [ ] Manifiestos K8s: `deployment.yml` + `service.yml`
- [ ] Deploy automático desde GitHub Actions

**Observabilidad:**
- [ ] `/metrics` endpoint en formato Prometheus
- [ ] `prometheus.yml` configurado
- [ ] Grafana dashboard con mínimo 2 paneles
- [ ] `/health` funcional
- [ ] Logs visibles con `docker logs`

**Seguridad:**
- [ ] Sin secretos en código (GitHub Secrets)
- [ ] Escaneo de dependencias en pipeline
- [ ] SAST en pipeline
- [ ] Dockerfile sin sudo, usuario no-root

**Repositorio:**
- [ ] Repo privado con docente como collaborator
- [ ] Protección de main
- [ ] Mínimo 5 PRs con review
- [ ] Commits semánticos

**Documentación (7 archivos):**
- [ ] `README.md`
- [ ] `docs/pipeline.md`
- [ ] `docs/branching.md`
- [ ] `docs/artefactos.md`
- [ ] `docs/observabilidad.md`
- [ ] `docs/seguridad.md`
- [ ] `docs/cultura.md`
- [ ] `docs/k8s.md`

---

*Trabajo Final — DevOps & Automatización — UNAL Sede Manizales — 2026-1*
