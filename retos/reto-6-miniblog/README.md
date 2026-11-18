# Reto 6: Mini Blog

**Stack requerido:** Python + Flask + SQLite

## Problema

CRUD de publicaciones cortas: modelar contenido con autor y fechas.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/posts` | Listar posts (ordenados por fecha descendente) |
| GET | `/posts/:id` | Ver un post |
| POST | `/posts` | Crear post {titulo, contenido, autor} |
| PUT | `/posts/:id` | Editar post |
| DELETE | `/posts/:id` | Eliminar post |
| GET | `/health` | Estado del servicio + BD |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- Al editar un post se actualiza el campo fecha_edicion.
- Los listados incluyen extracto de los primeros 100 caracteres del contenido.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
