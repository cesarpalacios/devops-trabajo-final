# Reto 8: API de Reservas de Salas

**Stack requerido:** Node.js + Express + SQLite

## Problema

Sistema de reservas de salas de reunion con validacion de solapamiento de horarios.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| GET | `/rooms` | Listar salas disponibles |
| POST | `/reservations` | Reservar {sala, fecha, hora_inicio, hora_fin, responsable} |
| GET | `/reservations?date=` | Reservas de un dia |
| DELETE | `/reservations/:id` | Cancelar reserva |
| GET | `/health` | Estado del servicio + BD |
| GET | `/metrics` | Metricas en formato Prometheus |

## Reglas y consideraciones

- REGLA DE NEGOCIO clave: no se permiten reservas que se crucen en la misma sala (responde 409).
- hora_fin debe ser mayor a hora_inicio (400 si no).
- RETO NUEVO 2026-2: los tests de la regla de solapamiento valen parte de la nota.
- No aparece en entregas de semestres anteriores.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
