# Reto 2: API de Clima

**Stack requerido:** Python + Flask + SQLite

## Problema

Proxy de consultas de clima contra una API externa gratuita, con historial local de consultas.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/weather/:city` | Clima actual (temperatura, humedad, viento) |
| GET | `/weather/:city/forecast` | Pronostico 5 dias |
| GET | `/history` | Ultimas 10 consultas |
| GET | `/health` | Estado del servicio + API externa |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- API externa: Open-Meteo (https://open-meteo.com, gratuita y sin API key).
- Cachear respuestas por ciudad durante 10 minutos para no saturar la API externa.
- /health debe reportar si la API externa responde o no.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
