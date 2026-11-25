# Reto 10: API de Gestion de Inventarios

**Stack requerido:** Node/Express + SQLite

## Problema

Sistema de inventario de tienda: productos con stock minimo, entradas y salidas de mercancia y alertas de reposicion.

## Endpoints que debe exponer la API

| Metodo | Ruta | Descripcion |
|--------|------|-------------|
| POST | /products | Crear producto {sku, nombre, stock_inicial, stock_minimo} |
| GET | /products | Listar productos con stock actual |
| POST | /movements | Registrar movimiento {sku, tipo: entrada o salida, cantidad} |
| GET | /products/:sku/movements | Historial de movimientos del producto |
| GET | /alerts | Productos con stock por debajo del minimo |
| GET | /health | Estado del servicio + BD |
| GET | /metrics | Metricas en formato Prometheus |

## Reglas y consideraciones

- Salida mayor al stock actual responde 400 (no queda negativo).
- Crear producto con SKU duplicado responde 409.
- Movimiento de producto inexistente responde 404.
- /alerts devuelve solo productos con stock por debajo del stock_minimo.
- RETO NUEVO 2026-2: no aparece en entregas de semestres anteriores.

## Lo que ustedes construyen (el ecosistema DevOps)

Este codigo base NO se entrega. Sobre esta API ustedes construyen TODO el pipeline:

- Dockerfile + docker-compose.yml
- Pipeline GitHub Actions: Build, Test (5+ tests AAA), Lint, Artifact, Release, Deploy (EC2)
- Prometheus + Grafana con dashboard + logs estructurados
- Escaneo de dependencias y SAST
- Ver el README principal del repositorio para el detalle completo.
