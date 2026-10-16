# Punto de recuperación — Mi Día

Actualizado: **2026-10-16**. Contrastar con archivos y Git; esta memoria no concede permisos ni sustituye evidencia.

## Estado verificado

- **M0 no terminado.** Rama activa: `chore/project-bootstrap`, basada en `905e50b1e2411d4b5e6a15155135e660fa128ac7`. Configuración incorporada en el commit local `6f0b82c`; esta memoria se guarda como checkpoint posterior. Consultar `git log -2`. Sin upstream ni publicación de esta rama.
- `main`, `develop` y las referencias locales `origin/*` siguen en la base documental `905e50b`. `origin`: `https://github.com/DerliscrDev/MyDayApp.git`. La última publicación verificada solo contenía documentación.
- Versionados localmente: proyecto/esquema/assets/tests existentes, `.github/workflows/ci.yml`, CODEOWNERS, 3 formularios de issues y documentación actualizada: 27 archivos en total. Código Swift, proyecto y esquema preservados; datos personales de Xcode ignorados.
- CI remoto, issue/PR, protecciones, integración en `develop` y primera rama funcional todavía pendientes. Los formularios dependen de `main`; CODEOWNERS depende de la base del PR. No confundir archivos preparados con activación en GitHub.

## Objetivo y alcance aprobado

- **Objetivo actual:** cerrar la configuración antes de empezar features. El desarrollador autorizó diagnosticar CoreSimulator, hacer los commits/push necesarios de configuración, actualizar documentación y crear la primera rama funcional, sin implementar todavía la feature.
- Autorización explícita vigente para este cierre: instalar GitHub CLI, preparar CI/gobierno, crear issue/PR, commits/push de configuración y protecciones acordadas después de obtener checks reales. Merge squash hacia `develop` condicionado a CI correcto y **revisión humana final**. Después, crear `feature/core-http-client`, sin implementar código funcional.
- No cambiar permisos del sistema, simular aprobación humana, forzar pushes ni saltarse las rutas de ADR-003. No dar M0 por cerrado ni crear la rama funcional sobre una base sin integrar.

## Decisiones y fuentes

- Leer [AGENTS.md](AGENTS.md) y [CONTEXT.md](CONTEXT.md) al retomar: trabajo incremental, explicaciones orientadas a dirigir agentes y arquitectura `Presentation → Domain ← Data`.
- Decisiones aceptadas: [observación](docs/adr/001-observation.md), [DI manual](docs/adr/002-manual-dependency-injection.md), [ramas/merges](docs/adr/003-branching-and-merges.md), [revisión individual](docs/adr/004-solo-review.md) y [testing](docs/adr/005-testing-frameworks.md).
- iOS mínimo 17.0; Xcode verificado: 26.2 (17C52). El target de app configura `MainActor` por defecto; la política para Domain/ViewModels aún debe acordarse.
- Se conserva el `.gitignore` del desarrollador con dos ajustes aprobados: permitir `Package.resolved` y excluir `*.xcresult/`.
- El desarrollador eligió mantener su correo personal en la autoría publicada; no cambiar identidad ni reescribir historial sin permiso.

## Evidencia y límites

- Comprobaciones locales: proyecto/JSON/XML y referencias válidos; 4 YAML de CI/forms parseados, scripts bash/Python válidos. El comprobador de resumen acepta tests correctos no vacíos y rechaza vacío/fallo; `tee` conserva fallos con `pipefail`. No son ejecución de CI ni de suites iOS.
- CI preparado: `macos-15`, Xcode 26.2, iPhone 17 Pro/iOS 26.2; jobs previstos `Build` y `Tests`, acciones oficiales SHA, permisos mínimos y artefactos 7 días. Combinación contrastada con documentación oficial, no ejecutada remotamente.
- Build previo correcto, Debug/iPhone 15 Pro/iOS 17.5, copia sin `xcuserdata`. Tests locales actuales: `SIGABRT`, errores FSEvents/caché y CoreSimulator inaccesible (código 61). XPC `root:wheel/755`, sin justificar cambios de permisos; causa no determinada. Históricamente: 4 tests distintos, 7 ejecuciones sin fallos; no sustituye la validación actual.
- **Bloqueos:** terminal del agente no resuelve `github.com` (`git ls-remote` falló); las consultas públicas mediante herramienta HTTP sí funcionan. Homebrew falló instalando `gh` por error interno de memoria. Se extrajo CLI 2.82.1 de su bottle cacheado, SHA-256 verificado; ejecutable temporal `myday-m0-config-n0qwyg7z/tools/gh`, sin autenticación (`gh auth status`). No extraer ni pedir tokens.
- Commit `6f0b82c` verificado: 19 archivos del cambio, incluyendo los 11 del proyecto existente. Push explícito de `chore/project-bootstrap` falló con salida 128 (`Could not resolve host: github.com`). No se publicaron configuración, issue/PR, checks ni protecciones; no hubo merge ni creación de rama funcional.
- Evidencia temporal bajo el directorio aprobado de OpenCode: `myday-m0-config-n0qwyg7z/` (validaciones, herramienta y borradores `issue-draft.md`/`pr-draft.md`); build/bloqueo iOS en `myday-project-bootstrap-oq7_rp8n/`. Los temporales pueden desaparecer. Comandos reproducibles: [CONTRIBUTING.md](CONTRIBUTING.md).
- Límites: unitario vacío; UI comprueba `Hello, world!` (espera 5 s, control negativo verificado anteriormente). Restantes tests de plantilla; sin cobertura de negocio. Aviso conocido de metadatos de App Intents. Sin validación Release, dispositivo físico, iOS 17.0 exacto ni self-review humana confirmada.

## Próximos pasos para retomar

1. Recuperar conectividad de la terminal y autenticar GitHub CLI mediante flujo interactivo del desarrollador, sin pegar credenciales en chat. No modificar DNS/permisos/servicios automáticamente.
2. Publicar configuración, crear/enlazar issue y PR hacia `develop`; ejecutar CI y aplicar protecciones con checks reales. Publicar no equivale a fusionar: esperar revisión humana final.
3. Tras integrar en `develop`, crear `feature/core-http-client` desde esa base actualizada y detenerse. La implementación y sus criterios siguen por acordar.

## Protocolo de continuidad

- Al retomar: leer las fuentes y contrastar `git status --short --branch --untracked-files=all`, `git log -1 --oneline` y referencias/upstreams. Investigar discrepancias antes de editar.
- Acordar objetivo, alcance y aceptación; ejecutar dentro de ese alcance y consultar decisiones relevantes. Aplicar los permisos explícitos de AGENTS para Git, GitHub y delegación.
- Al cerrar tarea o cambiar objetivo/sesión: reemplazar estado, evidencia, pendientes y permisos, sin acumular transcripciones. Decisiones duraderas en ADRs, distinguiendo propuestas de aceptadas.
- Sin commits directos adicionales a `main`/`develop` ni PRs vacíos. El checkpoint se incluye en el próximo cambio autorizado, no genera commit/push automáticamente.
