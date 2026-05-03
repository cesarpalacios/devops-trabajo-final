# Trabajo Final — DevOps Pipeline Challenge
## Universidad Nacional de Colombia — Sede Manizales
### DevOps & Automatización | 2026-1

**Docente:** Cesar Augusto Palacios Alarcón
**Repositorio:** https://github.com/cesarpalacios/devops-trabajo-final (Privado)

---

# Objetivo

Cada grupo debe construir un **pipeline DevOps completo** desde cero: crear el Dockerfile, configurar GitHub Actions, generar artefactos, publicar releases y desplegar en **AWS EC2**.

> **Esto no es un ejercicio de programación.** Es un ejercicio de **flujo DevOps**: entender cómo se conectan cada pieza del ciclo de vida del software — desde el código hasta producción.

---

# Lo que van a construir

```
Desarrollador hace un cambio
        │
        ▼
  Push a GitHub
        │
        ▼
GitHub Actions se ejecuta automáticamente:
  ┌─────────────────────────────────────┐
  │  1. Build     → Instalar dependencias │
  │  2. Test      → Correr pruebas         │
  │  3. Lint      → Análisis estático      │
  │  4. Artifact  → Construir imagen Docker│
  │  5. Release   → Publicar en Docker Hub │
  │  6. Deploy    → Desplegar en AWS EC2   │
  └─────────────────────────────────────┘
        │
        ▼
  App corriendo en AWS, accesible desde internet
```

---

# Retos Disponibles

Cada grupo elige **UN reto**. Se les entrega código fuente funcional — lo que NO se les entrega es el Dockerfile, el pipeline ni la configuración de despliegue.

---

## Reto 1: API de Tareas (To-Do)

```
GET    /tasks      → Listar tareas
POST   /tasks      → Crear tarea {titulo, descripcion}
PUT    /tasks/:id  → Actualizar tarea
DELETE /tasks/:id  → Eliminar tarea
GET    /health     → Estado del servicio
```

**Stack:** Python + FastAPI + SQLite
**Código base:** Se proporciona con endpoints funcionales y tests de ejemplo

---

## Reto 2: API de Clima

```
GET    /weather/:city       → Clima actual (temperatura, humedad, viento)
GET    /weather/:city/forecast → Pronóstico 5 días
GET    /history             → Últimas 10 consultas
GET    /health              → Estado del servicio
```

**API externa:** Open-Meteo (gratuita, sin API key) — https://open-meteo.com
**Stack:** Python + Flask + SQLite
**Código base:** Se proporciona con endpoints funcionales

---

## Reto 3: Generador de Contraseñas

```
GET    /generate?length=16&numbers=true&symbols=true → Generar contraseña
POST   /batch    → Generar múltiples {cantidad, longitud}
GET    /stats    → Estadísticas de uso
GET    /health   → Estado del servicio
```

**Stack:** Node.js + Express
**Código base:** Se proporciona con lógica funcional

---

## Reto 4: API de Libros

```
GET    /books           → Listar libros (paginado)
GET    /books/:id       → Detalle de un libro
GET    /books/search?q= → Buscar por título o autor
POST   /books           → Agregar libro {titulo, autor, año}
DELETE /books/:id       → Eliminar libro
GET    /health          → Estado del servicio
```

**Stack:** Python + FastAPI + SQLite
**Código base:** Se proporciona con CRUD funcional

---

## Reto 5: API de Cotización de Monedas

```
GET    /rate/:from/:to  → Cotización (ej: /rate/USD/COP)
GET    /rates/:base     → Todas las tasas para una moneda
GET    /history         → Últimas 20 consultas
GET    /health          → Estado del servicio
```

**API externa:** https://open.er-api.com (gratuita)
**Stack:** Node.js + Express + SQLite
**Código base:** Se proporciona con consulta a API funcional

---

## Reto 6: Mini Blog

```
GET    /posts      → Listar posts
GET    /posts/:id  → Ver un post
POST   /posts      → Crear post {titulo, contenido, autor}
PUT    /posts/:id  → Editar post
DELETE /posts/:id  → Eliminar post
GET    /health     → Estado del servicio
```

**Stack:** Python + Flask + SQLite
**Código base:** Se proporciona con CRUD completo

---

## Reto 7: Reto Libre

Propongan su propia idea. Mínimo 3 endpoints + `/health`.
**Requiere aprobación del docente.**

---

# Lo que DEBEN construir (sin código base)

Todo esto lo hacen los estudiantes desde cero:

## 1. Dockerfile

**NO se les proporciona.** Deben crearlo desde cero.

**Lo que deben resolver:**
- ¿Qué imagen base usar? (`python:3.12-slim`, `node:20-alpine`)
- ¿Cómo copiar el código e instalar dependencias?
- ¿Cómo exponer el puerto correcto?
- ¿Cómo definir el comando de inicio?

**Posibles errores que encontrarán:**
- La app usa SQLite pero el directorio no tiene permisos de escritura en el contenedor
- Las dependencias no se instalan correctamente
- El puerto no coincide entre Dockerfile y docker-compose
- Variables de entorno no definidas en el contenedor

> **Aprendizaje:** Entender cómo funciona la contenerización, qué necesita una app para correr en un contenedor, y cómo resolver errores de entorno.

## 2. docker-compose.yml

Deben crear un compose que levante la app con un solo comando.

```yaml
# Ejemplo de lo que deben construir (NO copiar esto tal cual):
services:
  app:
    build: .
    ports:
      - "?????:?????"  # Deben definir los puertos correctos
    environment:
      - ???              # Deben identificar qué variables necesita la app
```

## 3. GitHub Actions — Pipeline CI/CD

Deben crear `.github/workflows/ci-cd.yml` con los siguientes **stages obligatorios**:

### Stage 1: Build
- Instalar dependencias del proyecto
- Verificar que la app compila/arranca

### Stage 2: Test
- Ejecutar las pruebas unitarias
- **Mínimo 5 tests** (pueden venir con el código base o escribirlas ellos)
- Si un test falla, el pipeline se detiene

### Stage 3: Lint
- Análisis estático de código
- `flake8` o `ruff` (Python) / `eslint` (Node.js)

### Stage 4: Build de Artefacto (Docker)
- Construir la imagen Docker con el Dockerfile que crearon
- Taguear la imagen con el hash del commit o número de versión
- Esto es el **artefacto** — el producto del build que se va a desplegar

### Stage 5: Release / Publicación
- Push de la imagen a **Docker Hub** o **GitHub Container Registry**
- Crear un **GitHub Release** con tag de versión (ej: `v1.0.0`)
- El artefacto queda publicado y versionado

### Stage 6: Deploy a AWS EC2
- Conectarse por SSH a la instancia EC2 (usando secrets de GitHub)
- Hacer `docker pull` de la imagen publicada
- Levantar el contenedor en EC2
- Verificar con curl al `/health` que la app respondió

**Flujo visual del pipeline:**
```
Push/PR → Build → Test → Lint → Docker Build → Docker Push → Deploy EC2
           │        │       │         │              │            │
           └────────┴───────┴─────────┴──────────────┴────────────┘
                          Si cualquier paso falla, todo se detiene
```

## 4. Despliegue en AWS

| Requisito | Detalle |
|-----------|---------|
| AWS Academy | Usar la cuenta del laboratorio (gratuita) |
| EC2 | Levantar una instancia Ubuntu |
| Security Group | Abrir puerto 22 (SSH) + puerto de la app |
| GitHub Secrets | Guardar credenciales SSH y Docker Hub como secrets |
| Deploy automático | El pipeline despliega tras merge a `main` |
| App accesible | La API debe responder desde la IP pública de la EC2 |

**Proceso que deben seguir en AWS:**
1. Crear instancia EC2 (Ubuntu, t2.micro o t3.micro)
2. Instalar Docker en la EC2 (puede ser manual la primera vez)
3. Configurar Security Group (puertos)
4. Probar que funciona: `curl http://<IP-EC2>:<PUERTO>/health`

## 5. Pruebas Unitarias

| Requisito | Detalle |
|-----------|---------|
| Mínimo 5 tests | Pueden venir con el código base |
| Deben pasar en el pipeline | Si fallan, el build se detiene |
| Estructura AAA | Arrange → Act → Assert (visto en clase) |
| Tests en carpeta `tests/` | Separados del código fuente |

## 6. Observabilidad

| Requisito | Detalle |
|-----------|---------|
| Logs | La app genera logs visibles en el contenedor (`docker logs`) |
| `/health` | Endpoint que verifique app + BD + APIs externas |
| Métricas básicas | Al menos UNO: CloudWatch, dashboard simple, o contador de requests |

## 7. Seguridad

| Requisito | Detalle |
|-----------|---------|
| Sin secretos en código | `.env` + GitHub Secrets para credenciales |
| Escaneo de dependencias | `pip audit` o `npm audit` en el pipeline |
| Dockerfile sin `sudo` | Buena práctica de contenedores |

## 8. Artefactos y Releases

| Requisito | Detalle |
|-----------|---------|
| Imagen Docker versionada | Tag con versión o hash del commit |
| Publicada en Docker Hub o GHCR | Accesible para hacer pull desde EC2 |
| GitHub Release | Crear release con tag (ej: `v1.0.0`) al hacer merge a main |
| `.env.example` | Documentar todas las variables necesarias |

## 9. Documentación

| Archivo | Contenido |
|---------|-----------|
| `README.md` | Descripción, cómo clonar, instalar, ejecutar localmente, endpoints, cómo desplegar en AWS |
| `docs/pipeline.md` | Diagrama del CI/CD + explicación de cada stage + flujo de artefactos |
| `docs/branching.md` | Estrategia elegida + por qué + diagrama de flujo de ramas |

## 10. Control de Versiones

| Requisito | Detalle |
|-----------|---------|
| Repo privado | Agregar al docente (`cesarpalacios`) como collaborator |
| Branching | GitHub Flow recomendado: ramas por feature, PR a main |
| Protección de main | Solo merge vía Pull Request aprobado |
| Commits semánticos | `feat:`, `fix:`, `docs:`, `ci:`, `test:` |
| Mínimo 5 PRs | Con review entre integrantes |

---

# Estructura del Repositorio

```
mi-proyecto-devops/
├── src/                        # Código fuente (proporcionado)
├── tests/                      # Pruebas unitarias
├── .github/
│   └── workflows/
│       └── ci-cd.yml           # Pipeline (LO CONSTRUYEN ELLOS)
├── docker-compose.yml          # LO CONSTRUYEN ELLOS
├── Dockerfile                  # LO CONSTRUYEN ELLOS
├── .env.example                # LO CONSTRUYEN ELLOS
├── .gitignore                  # LO CONSTRUYEN ELLOS
├── docs/
│   ├── pipeline.md             # LO CONSTRUYEN ELLOS
│   └── branching.md            # LO CONSTRUYEN ELLOS
└── README.md                   # LO CONSTRUYEN ELLOS
```

> **Lo que se les da:** Código fuente funcional con tests
> **Lo que construyen:** Todo el ecosistema DevOps alrededor (Dockerfile, pipeline, deploy, docs)

---

# Rúbrica de Evaluación

| Criterio | Peso | Lo que evaluamos |
|----------|------|-----------------|
| **Pipeline CI/CD (GitHub Actions)** | 20% | ¿Los 6 stages corren automáticamente? ¿Si falla un test, se detiene? |
| **Artefactos y Releases** | 15% | ¿La imagen Docker se publica? ¿Hay releases versionados? ¿El flujo de artefactos está documentado? |
| **Despliegue en AWS EC2** | 20% | ¿La app está accesible? ¿Se despliega automáticamente tras merge? ¿`/health` responde? |
| **Contenerización (Dockerfile)** | 15% | ¿Crearon el Dockerfile desde cero? ¿`docker compose up` funciona? ¿Resolvieron los errores? |
| **Pruebas Unitarias** | 10% | ¿Mínimo 5 tests? ¿Pasando en el pipeline? |
| **Observabilidad** | 5% | ¿Logs visibles? ¿`/health` funciona? |
| **Seguridad** | 5% | ¿Sin secretos en código? ¿Escaneo de dependencias? |
| **Documentación y Branching** | 5% | ¿README, docs/, PRs con review, commits semánticos? |
| **Presentación y Demo** | 5% | ¿Demo en vivo funciona? ¿Claridad? ¿15 min? |

**Total: 100%**

---

# Formato de Presentación — Demo Day (15 min)

| Sección | Tiempo | Qué mostrar |
|---------|--------|-------------|
| **La App** | 2 min | ¿Qué hace? ¿Qué endpoints? ¿Qué reto? |
| **El Pipeline** | 4 min | Mostrar GitHub Actions corriendo. Explicar cada stage y el flujo de artefactos |
| **Demo en vivo** | 5 min | Hacer un cambio → Abrir PR → Pipeline corre → Imagen se publica → App se actualiza en AWS |
| **Artefactos** | 2 min | Mostrar Docker Hub con la imagen. Mostrar GitHub Releases |
| **Preguntas** | 2 min | El docente pregunta sobre el pipeline, el Dockerfile y las decisiones |

---

# Stack Tecnológico

| Componente | Python | Node.js |
|------------|--------|---------|
| Framework | FastAPI o Flask | Express |
| Base de datos | SQLite (incluido) | SQLite |
| Tests | pytest | Jest |
| Linter | flake8 / ruff | eslint |
| CI/CD | GitHub Actions | GitHub Actions |
| Contenedores | Docker | Docker |
| Artifact registry | Docker Hub | Docker Hub |
| Deploy | AWS EC2 + Docker | AWS EC2 + Docker |

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
- [ ] `.github/workflows/ci-cd.yml` con 6 stages
- [ ] Build automático
- [ ] Mínimo 5 tests pasando
- [ ] Lint pasando
- [ ] Artefacto Docker construido y publicado
- [ ] Release en GitHub con tag de versión

**Docker:**
- [ ] `Dockerfile` creado desde cero (no copiado de internet)
- [ ] `docker-compose.yml` funciona con `docker compose up`
- [ ] `.env.example` con todas las variables

**AWS:**
- [ ] EC2 creada con Docker instalado
- [ ] App accesible desde la IP pública
- [ ] `/health` responde correctamente
- [ ] Deploy automático desde GitHub Actions

**Repositorio:**
- [ ] Repo privado con docente como collaborator
- [ ] Protección de rama main
- [ ] Mínimo 5 PRs con review
- [ ] Commits semánticos
- [ ] `.gitignore` correcto
- [ ] Sin secretos en el código

**Documentación:**
- [ ] `README.md` completo
- [ ] `docs/pipeline.md` con diagrama del flujo de artefactos
- [ ] `docs/branching.md` con estrategia documentada

---

*Trabajo Final — DevOps & Automatización — UNAL Sede Manizales — 2026-1*
