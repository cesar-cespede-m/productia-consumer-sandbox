# ProductIA consumer sandbox

Repositorio de prueba para demostrar que una versión consumidora no puede integrar cambios de
`model-usage/` ni publicar tags `v*` sin evidencia ProductIA válida para el commit exacto.

No contiene datos productivos, modelos reales ni aprobaciones automatizadas. La evidencia llega
fuera del commit mediante la variable GitHub Actions `PRODUCTIA_RELEASE_EVIDENCE_B64`; ProductIA
la verifica antes de actualizar esa variable. El CI sólo verifica firma, sujeto, vigencia y
aprobaciones ya registradas por personas.

## Estructura inicial

- `model-usage/`: superficie protegida de ejemplo.
- `productia/public-key.pem`: clave pública Ed25519 del sandbox.
- `release-evidence.json`: **no se versiona**; se reconstruye temporalmente en CI.

El gate y la protección obligatoria de `main` pertenecen a `SDLC-02.2`. Hasta completarlos este
repositorio no debe presentarse como un release gate operativo.
