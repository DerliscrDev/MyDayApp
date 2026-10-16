# Punto de recuperación — Mi Día

Actualizado: **2026-10-16**. Contrastar con archivos y Git; esta memoria no concede permisos ni sustituye evidencia.

## Estado verificado

- **M0 no terminado.** Rama activa: `chore/project-bootstrap`, siguiendo `origin/chore/project-bootstrap`. Último head publicado: `ccb8bf4f189667be0b6697c7595c3b8480708839`; configuración `6f0b82c`. Se prepara otro checkpoint documental: consultar `git log -1` y publicar antes de la revisión final, sin asumir que está en GitHub.
- `main`, `develop` y sus referencias locales `origin/*` siguen en la base documental `905e50b`. `origin`: `https://github.com/DerliscrDev/MyDayApp.git`. La configuración está en la rama chore, no integrada en esas ramas.
- Versionados localmente: proyecto/esquema/assets/tests existentes, `.github/workflows/ci.yml`, CODEOWNERS, 3 formularios de issues y documentación actualizada: 27 archivos en total. Código Swift, proyecto y esquema preservados; datos personales de Xcode ignorados.
- [PR #2](https://github.com/DerliscrDev/MyDayApp/pull/2) abierto, base `develop`, head `chore/project-bootstrap`. Título y descripción corregidos; self-review humana pendiente. [Issue #1](https://github.com/DerliscrDev/MyDayApp/issues/1) enlazado, todavía sin descripción. CI de push y PR correcto en `ccb8bf4`; protecciones activas. Falta publicar el checkpoint, validar su nuevo head, revisión humana y merge. Rama funcional no creada.

## Objetivo y alcance aprobado

- **Objetivo:** cerrar M0 y detenerse en la primera rama funcional, sin implementar todavía la feature.
- Autorización explícita vigente para este cierre: instalar GitHub CLI, preparar CI/gobierno, crear issue/PR, commits/push de configuración y protecciones acordadas después de obtener checks reales. Merge squash hacia `develop` condicionado a CI correcto y **revisión humana final**. Después, crear `feature/core-http-client`, sin implementar código funcional.
- No cambiar permisos del sistema, simular aprobación humana, forzar pushes ni saltarse las rutas de ADR-003. No dar M0 por cerrado ni crear la rama funcional sobre una base sin integrar.

## Decisiones y fuentes

- Leer [AGENTS.md](AGENTS.md) y [CONTEXT.md](CONTEXT.md) al retomar: trabajo incremental, explicaciones orientadas a dirigir agentes y arquitectura `Presentation → Domain ← Data`.
- Decisiones aceptadas: [observación](docs/adr/001-observation.md), [DI manual](docs/adr/002-manual-dependency-injection.md), [ramas/merges](docs/adr/003-branching-and-merges.md), [revisión individual](docs/adr/004-solo-review.md) y [testing](docs/adr/005-testing-frameworks.md).
- iOS mínimo 17.0; Xcode verificado: 26.2 (17C52). El target de app configura `MainActor` por defecto; la política para Domain/ViewModels aún debe acordarse.
- Se conserva el `.gitignore` del desarrollador con dos ajustes aprobados: permitir `Package.resolved` y excluir `*.xcresult/`.
- El desarrollador eligió mantener su correo personal en la autoría publicada; no cambiar identidad ni reescribir historial sin permiso.

## Evidencia y límites

- Validación local de proyecto, JSON/XML, YAML y scripts correcta; comprobador de resumen y propagación de fallos verificados. No equivale a ejecución de CI ni de suites iOS.
- CI de push [38855332988](https://github.com/DerliscrDev/MyDayApp/actions/runs/38855332988) y PR [38856514344](https://github.com/DerliscrDev/MyDayApp/actions/runs/38856514344): `success` para `ccb8bf4`; `Build`/`Tests` correctos. No se han leído conteos remotos. Entorno macos-15/Xcode 26.2/iOS 26.2; no demuestra ejecución iOS 17. Cambios posteriores requieren CI propio.
- Build local previo correcto en iOS 17.5; tests locales actuales bloqueados por `SIGABRT`/CoreSimulator (código 61), causa desconocida. Históricamente: 4 tests distintos, 7 ejecuciones sin fallos; no sustituye la validación actual.
- **Bloqueos del agente:** terminal sin DNS a GitHub, CLI temporal `myday-m0-config-n0qwyg7z/tools/gh` sin autenticar. Herramienta HTTP pública funciona; el desarrollador publica desde su Terminal normal, con permiso `workflow` ya resuelto. No pedir ni extraer tokens.
- Ruleset [25219989](https://github.com/DerliscrDev/MyDayApp/rules/25219989), creado por el desarrollador, verificado por API: `active`, solo `main`/`develop`, ambas protegidas. PR obligatorio, 0 aprobaciones, sin Code Owners, conversaciones resueltas, checks `Build`/`Tests` de Actions (15368), rama actualizada, sin force/delete. Sin restricciones de updates ni historial lineal. Rebase también permitido por GitHub, pero no es el método acordado. Bypass no visible por API pública.
- Temporales de OpenCode: `myday-m0-config-n0qwyg7z/` (validaciones, CLI y borradores de issue/PR), `myday-project-bootstrap-oq7_rp8n/` (build/tests locales). Pueden desaparecer. Comandos: [CONTRIBUTING.md](CONTRIBUTING.md).
- Límites: unitario vacío; UI verifica saludo y el resto es plantilla, sin cobertura de negocio. Aviso conocido de App Intents. Sin validación Release, dispositivo físico, iOS 17.0 exacto ni self-review humana confirmada.

## Próximos pasos para retomar

1. Publicar el checkpoint documental desde la Terminal normal si persiste el bloqueo del agente; esperar CI verde en el nuevo head. No reutilizar resultados de `ccb8bf4` como validación de otro commit.
2. Completar objetivo/alcance/aceptación del issue #1 y documentar self-review humana real en el PR. Después, squash merge hacia `develop`, nunca hacia `main` en este PR.
3. Tras integrar en `develop`, crear `feature/core-http-client` desde esa base actualizada y detenerse. La implementación y sus criterios siguen por acordar.

## Protocolo de continuidad

- Al retomar: leer las fuentes y contrastar `git status --short --branch --untracked-files=all`, `git log -1 --oneline` y referencias/upstreams. Investigar discrepancias antes de editar.
- Acordar objetivo, alcance y aceptación; ejecutar dentro de ese alcance y consultar decisiones relevantes. Aplicar los permisos explícitos de AGENTS para Git, GitHub y delegación.
- Al cerrar tarea o cambiar objetivo/sesión: reemplazar estado, evidencia, pendientes y permisos, sin acumular transcripciones. Decisiones duraderas en ADRs, distinguiendo propuestas de aceptadas.
- Sin commits directos adicionales a `main`/`develop` ni PRs vacíos. El checkpoint se incluye en el próximo cambio autorizado, no genera commit/push automáticamente.
