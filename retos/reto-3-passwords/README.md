# Reto 3: Generador de Contrasenas

**Stack requerido:** Node.js + Express

## Problema

Servicio de generacion de contrasenas configurables (longitud, numeros, simbolos) con estadisticas de uso.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/generate?length=16&numbers=true&symbols=true` | Generar contrasena |
| POST | `/batch` | Generar multiples {cantidad, longitud} |
| GET | `/stats` | Estadisticas de uso (total generadas, promedio longitud) |
| GET | `/health` | Estado del servicio |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- Validar length entre 8 y 64; valores invalidos responden 400.
- Las stats sobreviven al reinicio: persistir en SQLite o archivo.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
