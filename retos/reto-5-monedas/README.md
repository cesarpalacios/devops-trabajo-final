# Reto 5: API de Cotizacion de Monedas

**Stack requerido:** Node.js + Express + SQLite

## Problema

Consulta de tasas de cambio contra una API externa, con historial de las ultimas cotizaciones consultadas.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/rate/:from/:to` | Cotizacion (ej: /rate/USD/COP) |
| GET | `/rates/:base` | Todas las tasas para una moneda |
| GET | `/history` | Ultimas 20 consultas |
| GET | `/health` | Estado del servicio + API externa |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- API externa: https://open.er-api.com (gratuita).
- Validar codigos de moneda invalidos con 400.
- El historial guarda par, tasa y fecha de cada consulta.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
