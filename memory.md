# Estado vivo — Mi Día

## Hito actual

**M0 — Fundaciones**, en curso. Existe un proyecto Xcode base con app SwiftUI y targets de tests unitarios/UI, todavía fuera de seguimiento. Git local en `main`; primer commit documental autorizado con el mensaje `chore: bootstrap repository documentation`. Sin remoto local ni push; CI y protecciones pendientes.

## Decisiones confirmadas por el desarrollador

- Construir paso a paso, con explicaciones orientadas a dirigir agentes de IA en el desarrollo de la app.
- El desarrollador aclaró que ya es iOS developer. Centrar las explicaciones en trabajo con agentes, no en fundamentos de iOS ni uso básico de Xcode.
- Versión mínima de la app: **iOS 17**.
- Reglas de trabajo de `AGENTS.md` revisadas y aprobadas por el desarrollador.
- ViewModels con **`@Observable`**, registrado en [ADR-001](docs/adr/001-observation.md).
- **Inyección de dependencias manual mediante inicializadores**, con composición desde `App/`, registrada en [ADR-002](docs/adr/002-manual-dependency-injection.md).
- **Swift Testing para pruebas unitarias y de integración; XCTest con XCUIAutomation para UI**, registrado en [ADR-005](docs/adr/005-testing-frameworks.md).
- **Ramas `main`/`develop`, squash de ramas de trabajo y merge commits de releases/hotfixes hacia `main` y de reintegración en `develop`**, registrado en [ADR-003](docs/adr/003-branching-and-merges.md).
- **PR + CI + self-review humana documentada, con revisión de IA adicional declarada**, sin aprobaciones formales obligatorias en modo individual, registrado en [ADR-004](docs/adr/004-solo-review.md).
- Entorno verificado con `xcodebuild -version`: **Xcode 26.2 (17C52)**.
- El desarrollador aprobó alinear la versión mínima, comprobar el entorno, ejecutar build y ambas suites, y actualizar documentación. Fuera de alcance: funcionalidades, refactors, cambios de concurrencia, Git, GitHub y CI.
- El desarrollador aprobó añadir una comprobación útil de la pantalla inicial mediante un test de UI, sin funcionalidades nuevas.
- El desarrollador aprobó preparar `.gitignore`, esquema compartido y plantilla local de PR, sin inicializar Git, crear commits ni configurar/publicar en GitHub o CI.
- El desarrollador autorizó después únicamente inicializar Git local en `main` y revisar archivos incluidos/ignorados, sin staging, commits ni conexión a GitHub.
- Después de actualizar su `.gitignore`, el desarrollador aprobó retirar la exclusión de `Package.resolved`, añadir `*.xcresult/`, hacer staging de los 11 archivos acordados y crear únicamente el primer commit local. No autorizó configurar remoto ni hacer push.
- URL de GitHub facilitada por el desarrollador: `https://github.com/DerliscrDev/MyDayApp.git`. No se comprobó el estado remoto ni se conectó el repositorio local.

## Último paso

- Se conserva el `.gitignore` del desarrollador con solo las dos modificaciones aprobadas; se actualiza `CONTRIBUTING.md` para no describir las reglas anteriores.
- Lista autorizada para el commit inicial (11 archivos): `.gitignore`, `.github/pull_request_template.md`, `AGENTS.md`, `CONTEXT.md`, `CONTRIBUTING.md`, `memory.md` y los cinco ADRs existentes en `docs/adr/`.
- Fuera del commit: `MyDayApp/`, `MyDayApp.xcodeproj/`, `MyDayAppTests/` y `MyDayAppUITests/`. No se modifican ni borran sus archivos.
- Git tiene identidad válida de autor y committer. El correo configurado no es un noreply de GitHub; no se muestra ni se cambia. Revisar la privacidad de esos metadatos antes de autorizar publicación.
- Esta memoria se prepara para incluirse en el propio commit inicial; al retomar, confirmar su hash y el estado final con `git log -1 --oneline` y `git status`, sin crear otro commit solo para registrar el hash.

## Pendientes

- Confirmar el commit inicial local y acordar la conexión/publicación en GitHub como una operación separada, incluyendo la revisión de privacidad de autoría.
- El test unitario sigue vacío; sustituirlo al incorporar comportamiento comprobable, sin inventar lógica solo para tener tests verdes.
- Crear `develop` desde el primer commit, incorporar el proyecto en una rama `chore/*`, publicar y configurar CI/protecciones con las autorizaciones correspondientes.
- Preparar plantillas de issues y CODEOWNERS cuando se conozca la cuenta responsable.
- Acordar el aislamiento de concurrencia al implementar Domain/ViewModels; la plantilla actual configura `SWIFT_DEFAULT_ACTOR_ISOLATION = MainActor` en el target de app.

Las recomendaciones previas del agente no equivalen a decisiones aprobadas.

## Verificación

- Antes de preparar el commit: rama `main`, cero commits, índice vacío y ningún remoto; identidades de autor/committer válidas comprobadas sin mostrar datos personales.
- Comprobaciones previas completadas: revisión del diff documental, 11 archivos autorizados en staging, `git diff --cached --check` correcto y 25 enlaces relativos contenidos en la propia base documental. `git check-ignore` permite `Package.resolved` y excluye bundles `.xcresult`. Sin coincidencias de los patrones de secretos revisados (no es una auditoría exhaustiva). Confirmar árbol y mensaje con `git show` después del commit; no hay ejecución de CI.
- Evidencia temporal de este paso: `/private/var/folders/2c/dhvztt5s5vlb4rjc5_t7vnc80000gn/T/opencode/myday-first-commit-oz3uw7wy/` (copias previas y hashes; sin datos de autoría).
- Build/tests no repetidos: cambios de documentación y exclusiones únicamente. Última ejecución: esquema compartido en copia sin `xcuserdata`, iPhone 15 Pro/iOS 17.5, 4 tests distintos y 7 ejecuciones sin fallos; evidencia en `/private/var/folders/2c/dhvztt5s5vlb4rjc5_t7vnc80000gn/T/opencode/myday-repo-prep-2eyrfooz/`.
- Límites: no hay autorización de remotos, publicación o CI ni self-review humana confirmada. El test unitario sigue vacío; la aserción UI no demuestra cobertura de reglas de negocio.

## Próximo paso propuesto

Revisar el resultado del primer commit local y la privacidad del correo de autoría antes de autorizar configurar `origin` y publicar. Crear `develop` desde esa base documental e incorporar el proyecto mediante `chore/*` requerirá acordar el siguiente alcance. No hacer push, cambiar identidad ni reescribir commits sin autorización explícita.
