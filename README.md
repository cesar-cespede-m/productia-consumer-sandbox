# ProductIA consumer sandbox

Repositorio de prueba para demostrar que una versión consumidora no puede integrar cambios de
`model-usage/` ni publicar tags `v*` sin evidencia ProductIA válida para el commit exacto.

No contiene datos productivos, modelos reales ni aprobaciones automatizadas. La evidencia llega
fuera del commit mediante el secreto GitHub Actions `PRODUCTIA_RELEASE_EVIDENCE_B64`; ProductIA
la verifica antes de actualizar ese secreto. El CI sólo verifica firma, sujeto, vigencia y
aprobaciones ya registradas por personas.

## Estructura

- `model-usage/`: superficie protegida de ejemplo.
- `productia/public-key.pem`: clave pública Ed25519 del sandbox.
- `productia/verify_release_evidence.py`: verificador autónomo (copia del de ProductIA, con prueba
  de paridad en ProductIA); sólo depende de `productia/requirements-verifier.txt`.
- `.github/workflows/productia-gate.yml`: gate `productia-gate`.
- `release-evidence.json`: **no se versiona**; se reconstruye temporalmente en CI.

## Cómo funciona el gate

- **PR hacia `main` que modifica `model-usage/`:** verifica la evidencia contra el último commit
  del PR. Si el PR no toca `model-usage/`, el check pasa con «No aplica».
- **Tag `v*`:** verifica contra el commit del tag; el job `release-sandbox` sólo corre si el gate
  pasa.
- Usa `pull_request_target`: el workflow, el verificador y la clave pública salen de `main`; del
  PR sólo se lee la lista de archivos cambiados, nunca se ejecuta su código.
- Códigos: 0 válida, 10 esquema o secreto ausente, 11 firma, 12 otro repo o commit, 13 vencida,
  14 no aprobada/publicada, 15 contexto o estrategia no vigentes al emitir.

Limitación conocida: en un push de tag, GitHub usa el workflow del commit etiquetado. El gate sólo
es confiable si los tags se crean sobre commits que ya pasaron por `main` protegida.

Ensayo SDLC-02.2: PR que no toca model-usage/.
