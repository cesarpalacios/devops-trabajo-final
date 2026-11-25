# Reto 10: API de Citas Medicas

**Stack requerido:** Python + FastAPI + SQLite

## Problema

Sistema de agendamiento de citas medicas: pacientes, medicos por especialidad y citas con validacion de no-solapamiento por medico.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| POST | /patients | Crear paciente {nombre, documento, telefono} |
| POST | /doctors | Crear medico {nombre, especialidad} |
| POST | /appointments | Crear cita {patient_id, doctor_id, fecha_hora, motivo} |
| GET | /appointments/:doctor_id | Citas de un medico ordenadas por fecha |
| DELETE | /appointments/:id | Cancelar cita |
| GET | /health | Estado del servicio + BD |
| GET | /metrics | Metricas en formato Prometheus |

## Reglas y consideraciones

- Crear cita para un medico que ya tiene otra en la misma fecha/hora responde 409 (no-solapamiento).
- Cita con paciente o medico inexistente responde 404.
- Fecha en el pasado responde 400.
- Cancelar una cita ya cancelada responde 409.
- RETO NUEVO 2026-2: no aparece en entregas de semestres anteriores.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
