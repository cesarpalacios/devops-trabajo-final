# Reto 9: Reto Libre

**Stack requerido:** El que prefieras (Python o Node.js)

## Problema

Propon tu propia idea de API. Minimo 3 endpoints utiles + /health + /metrics.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|

## Reglas y consideraciones

- Requiere aprobacion del docente por correo ANTES del registro en el formulario.
- La propuesta debe incluir: nombre, dominio del problema y los endpoints que expondra.
- Se valora creatividad y casos con reglas de negocio verificables con tests.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
