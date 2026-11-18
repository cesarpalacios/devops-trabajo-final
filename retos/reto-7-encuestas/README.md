# Reto 7: API de Encuestas

**Stack requerido:** Python + FastAPI + SQLite

## Problema

Sistema de encuestas y votaciones: crear encuestas con opciones y calcular resultados en tiempo real.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| POST | `/surveys` | Crear encuesta {pregunta, opciones[]} |
| GET | `/surveys` | Listar encuestas activas |
| POST | `/surveys/:id/vote` | Votar {opcion} |
| GET | `/surveys/:id/results` | Resultados con conteo y porcentaje |
| GET | `/health` | Estado del servicio + BD |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- Votar por una opcion que no existe responde 400.
- Los resultados calculan porcentaje por opcion (incluyendo opcion con 0 votos).
- RETO NUEVO 2026-2: no aparece en entregas de semestres anteriores.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
