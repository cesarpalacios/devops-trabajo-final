# Reto 4: API de Libros

**Stack requerido:** Python + FastAPI + SQLite

## Problema

Catalogo de libros con paginacion y busqueda por titulo o autor.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/books` | Listar libros (paginado, ?page=1&limit=10) |
| GET | `/books/:id` | Detalle de un libro |
| GET | `/books/search?q=` | Buscar por titulo o autor |
| POST | `/books` | Agregar libro {titulo, autor, ano} |
| DELETE | `/books/:id` | Eliminar libro |
| GET | `/health` | Estado del servicio + BD |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- La paginacion devuelve total de registros y paginas en el cuerpo de respuesta.
- La busqueda debe ser insensible a mayusculas/minuscilas.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
