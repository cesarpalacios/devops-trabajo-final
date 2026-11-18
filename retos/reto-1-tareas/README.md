# Reto 1: API de Tareas (To-Do)

**Stack requerido:** Python + FastAPI + SQLite

## Problema

API CRUD de gestion de tareas personales. El caso clasico: modelar un recurso con ciclo de vida completo.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/tasks` | Listar tareas |
| POST | `/tasks` | Crear tarea {titulo, descripcion} |
| PUT | `/tasks/:id` | Actualizar tarea |
| DELETE | `/tasks/:id` | Eliminar tarea |
| GET | `/health` | Estado del servicio + BD |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- Las tareas deben persistir en SQLite (sobreviven a un reinicio del contenedor).
- POST responde 201 con la tarea creada; DELETE sobre id inexistente responde 404.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
