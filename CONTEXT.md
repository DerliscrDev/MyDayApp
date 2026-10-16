# CONTEXT.md — Mi Día

> Documento de contexto del proyecto. Responde **qué** estamos construyendo y **por qué**.
> El **cómo** debe trabajar el agente vive en `AGENTS.md`; el estado vivo en `memory.md`.

---

## 1. Visión

**Mi Día** es una app iOS que centraliza en un solo dashboard lo que un desarrollador consulta a diario: clima, cambio de moneda (incluye guaraní, PYG), noticias tech, actividad de GitHub y tareas/hábitos.

### Objetivos del proyecto

1. Ser útil en el día a día.
2. Servir como **proyecto de portfolio** que demuestre:
   - Clean Architecture + MVVM en SwiftUI.
   - Testing serio (unitario, integración, UI mínimo).
   - Flujo de trabajo de equipo: ramas, PRs, code review, CI, branch protection.
   - Uso responsable de IA en el desarrollo (decisiones documentadas, no código ciego).
3. Construirse de forma **incremental**: un módulo (feature) por hito.
4. Aprender a dirigir agentes de IA y comprender las decisiones y el código del proyecto.

### No-objetivos (por ahora)

- Backend propio.
- Autenticación de usuarios.
- Publicación en App Store.

---

## 2. Stack

| Área | Decisión |
|---|---|
| Entorno de desarrollo | Xcode 26.2, build 17C52 (verificado localmente) |
| Versión mínima | iOS 17 (configurado como 17.0 en app y tests, Debug y Release) |
| Lenguaje | Swift (concurrencia moderna: async/await) |
| UI | SwiftUI |
| Patrón de presentación | MVVM con `@Observable` (aceptado en [ADR-001](docs/adr/001-observation.md)) |
| Arquitectura | Clean Architecture por feature |
| Inyección de dependencias | Manual mediante inicializadores, composición desde `App/` (aceptado en [ADR-002](docs/adr/002-manual-dependency-injection.md)) |
| Persistencia local | SwiftData |
| Red | `URLSession` + capa propia de networking (sin dependencias externas al inicio) |
| Tests | Swift Testing para pruebas unitarias y de integración; XCTest con XCUIAutomation para UI (aceptado en [ADR-005](docs/adr/005-testing-frameworks.md)) |
| CI | GitHub Actions con jobs `Build` y `Tests`; evidencia y estado por commit en `memory.md` |
| Gestión de dependencias | Swift Package Manager |

---

## 3. Arquitectura

### Regla de dependencias

```text
Presentation  →  Domain  ←  Data
```

- **Domain**: entidades, casos de uso, protocolos de repositorio. Swift puro: no importa SwiftUI, SwiftData ni nada de red.
- **Data**: DTOs, data sources (remote/local), implementaciones de repositorio, mappers DTO → Entity.
- **Presentation**: Views y ViewModels. Los ViewModels solo conocen casos de uso.

### Estructura de carpetas prevista

```text
MiDia/
├── App/                    (entry point, composición de dependencias)
├── Core/
│   ├── Networking/         (HTTPClient, Endpoint, errores)
│   ├── Persistence/
│   ├── DI/
│   └── DesignSystem/
├── Features/
│   ├── Weather/
│   │   ├── Domain/         (Entities, UseCases, RepositoryProtocol)
│   │   ├── Data/           (DTOs, RemoteDataSource, RepositoryImpl)
│   │   └── Presentation/   (View, ViewModel)
│   ├── Currency/
│   ├── TechNews/
│   ├── GitHubActivity/
│   └── Tasks/
└── Tests/
    ├── CoreTests/
    └── FeatureTests/
```

---

## 4. Módulos y APIs

> Verificar límites, términos de uso y atribución de cada API antes de integrarla.

| Módulo | API | Notas |
|---|---|---|
| Weather | Open-Meteo | Sin API key |
| Currency | open.er-api.com | Gratis, requiere atribución; incluye PYG |
| TechNews | Hacker News API (Firebase) | Sin key |
| GitHubActivity | GitHub REST API | Límite bajo sin autenticar |
| Tasks | Local (SwiftData) | Sin red |

---

## 5. Flujo de trabajo con Git (simulando un equipo)

### 5.1 Ramas

| Rama | Propósito | Protección objetivo |
|---|---|---|
| `main` | Siempre estable y "releasable". Solo recibe merges desde `release/*` o `hotfix/*`. | Sí |
| `develop` | Integración continua de features. | Sí |
| `feature/<modulo>-<descripcion>` | Trabajo de una feature | No |
| `fix/<descripcion>` | Corrección de bugs | No |
| `chore/<descripcion>` | Tareas de mantenimiento, CI, docs | No |
| `release/x.y.z` | Estabilización previa a release | Opcional |
| `hotfix/<descripcion>` | Arreglo urgente sobre `main` | No |

Ejemplo: `feature/weather-current-forecast`.

### 5.2 Convención de commits

[Conventional Commits](https://www.conventionalcommits.org/):

```text
feat(weather): add current forecast use case
fix(currency): handle PYG decimal formatting
test(tasks): cover CompleteTask use case
chore(ci): add GitHub Actions workflow
docs: update CONTEXT.md roadmap
```

### 5.3 Pull Requests

- Todo cambio posterior al arranque entra por PR, nunca con push directo a `develop` o `main`. La excepción única para establecer la base está documentada en ADR-003 y requiere autorización para ejecutarla.
- PR pequeño y enfocado (idealmente una capa o un caso de uso).
- Debe enlazar un issue (`Closes #N` cuando corresponda cerrar o `Refs #N` para referenciarlo). El cierre automático depende de la integración en la rama predeterminada.
- Plantilla [`.github/pull_request_template.md`](.github/pull_request_template.md), incluida en la base documental publicada en `main` y `develop`, con: contexto, cambios, cómo probar, checklist (tests, arquitectura, sin warnings nuevos), capturas si hay UI, self-review humana y asistencia de IA declarada.
- Estrategia aceptada en [ADR-003](docs/adr/003-branching-and-merges.md): ramas de trabajo hacia `develop` mediante **squash merge**; `release/*` y `hotfix/*` hacia `main` mediante **merge commit**, seguido de reintegración en `develop` mediante **merge commit** si hay cambios pendientes.
- Guía operativa: [CONTRIBUTING.md](CONTRIBUTING.md). Las rutas y el método de merge todavía no están automatizados.

### 5.4 Branch protection (configurar en GitHub → Settings → Branches)

Configuración verificada el 2026-10-16 en el ruleset activo [`protect-main-develop`](https://github.com/DerliscrDev/MyDayApp/rules/25219989), aplicado a `main` y `develop`. Los checks `Build` y `Tests` de GitHub Actions ya tuvieron ejecuciones reales correctas; cada cambio nuevo debe pasar su propia ejecución:

- [x] Require a pull request before merging
- [x] Require status checks to pass (`Build` y `Tests`, GitHub Actions)
- [x] Require branches to be up to date before merging
- [x] Require conversation resolution before merging
- [x] Do not allow force pushes / deletions
- [ ] Comprobar bypass de administradores: la API pública no expone esa lista

No exigir historial lineal en `main` ni `develop`: ambas reciben merge commits. No exigir aprobaciones formales ni revisión obligatoria de Code Owners en modo individual; el descarte de aprobaciones antiguas se reevaluará si se incorpora un reviewer humano.

> **Trabajo individual — decisión aceptada en [ADR-004](docs/adr/004-solo-review.md):** PR + CI + self-review documentada por el desarrollador, con revisión de IA adicional declarada. El ruleset exige cero aprobaciones y no exige review de Code Owners; esto no elimina la self-review acordada. GitHub no permite aprobar tu propio PR y una revisión de IA no satisface una aprobación humana requerida.

### 5.5 Archivos de gobierno del repo

Archivos publicados en `chore/project-bootstrap` (PR #2 hacia `develop`). La activación en cada rama y los resultados por commit deben comprobarse; no se infieren solo de su presencia:

- `.github/CODEOWNERS` — dueños por carpeta (por ejemplo `Core/` y `Features/*/Domain/`).
- `.github/pull_request_template.md`
- `.github/ISSUE_TEMPLATE/` — bug, feature, tech-debt.
- `.github/workflows/ci.yml` — build + tests en cada PR.
- `CONTRIBUTING.md` — flujo de ramas, commits, estilo de revisión.
- `docs/adr/` — Architecture Decision Records.

Los formularios de issues y la detección de CODEOWNERS dependen de la rama que GitHub consulta: los formularios usan la rama predeterminada (`main`), y CODEOWNERS la rama base del PR. Prepararlos en una rama de trabajo no los activa en esas ramas; no saltarse ADR-003 para publicarlos directamente.

### 5.6 Roles simulados (para el portfolio)

| Rol | Evidencia visible en el repo |
|---|---|
| Tech Lead | ADRs, CODEOWNERS, definición de arquitectura, roadmap en issues/milestones |
| Developer | Branches de feature y PRs con commits convencionales |
| Code Reviewer | Comentarios de review sustanciales en PRs (arquitectura, tests, naming, edge cases) |
| QA | Plan de pruebas, cobertura, bugs reportados con plantilla |
| DevOps | CI en GitHub Actions, protección de ramas |

---

## 6. Estrategia de testing

| Capa | Qué se prueba | Cómo |
|---|---|---|
| Domain | Casos de uso y reglas | Unit tests puros |
| Data | Mappers, repositorios | Unit tests con data sources falsos / `URLProtocol` mock |
| Presentation | ViewModels (estados, errores, loading) | Unit tests con casos de uso mockeados |
| UI | Flujos críticos | UI tests mínimos |

Reglas:
- Todo caso de uso nuevo nace con tests.
- Los tests no deben depender de red real.
- El CI debe fallar si fallan los tests.

---

## 7. Definition of Done (por PR)

- [ ] Compila sin warnings nuevos
- [ ] Tests nuevos o actualizados y en verde
- [ ] Respeta la regla de dependencias entre capas
- [ ] Sin lógica de negocio en Views
- [ ] PR descrito y con issue enlazado
- [ ] Revisado y aprobado según la política acordada en ADR-004
- [ ] Decisiones relevantes registradas (ADR si aplica)

---

## 8. Roadmap por hitos

| Hito | Contenido |
|---|---|
| **M0 — Fundaciones** | Repo, ramas, protección, CI básico, plantillas, `CONTEXT.md`, `AGENTS.md`, `memory.md`, proyecto Xcode vacío |
| **M1 — Core** | Capa de networking, manejo de errores, DI, design system mínimo |
| **M2 — Weather** | Primera feature completa de punta a punta (sirve de plantilla para las demás) |
| **M3 — Currency** | Segunda feature, incluye PYG y formato de moneda |
| **M4 — Tasks** | Persistencia local con SwiftData |
| **M5 — TechNews** | Lista paginada y detalle |
| **M6 — GitHubActivity** | Consumo con límite de rate y estados de error |
| **M7 — Dashboard** | Pantalla "Mi Día" que compone los módulos |
| **M8 — Pulido** | Accesibilidad, localización (es/en), README de portfolio, capturas |

---

## 9. Decisiones arquitectónicas (ADRs)

### Aceptadas

- [ADR-001: Observación de ViewModels con `@Observable`](docs/adr/001-observation.md).
- [ADR-002: Inyección de dependencias manual](docs/adr/002-manual-dependency-injection.md).
- [ADR-003: Ramas y estrategia de merge](docs/adr/003-branching-and-merges.md).
- [ADR-004: Revisión en trabajo individual](docs/adr/004-solo-review.md).
- [ADR-005: Swift Testing y XCTest para las pruebas](docs/adr/005-testing-frameworks.md).

No quedan decisiones pendientes de la lista inicial. Registrar nuevas decisiones cuando surjan necesidades concretas.

---

## 10. Uso de IA en el proyecto

- La IA asiste en diseño, implementación y revisión; **las decisiones las toma el desarrollador**.
- Todo código generado se revisa en PR como cualquier otro.
- Los prompts y decisiones relevantes se resumen en `memory.md` y/o ADRs.
- `AGENTS.md` define reglas para los agentes; `memory.md` guarda el estado vivo.
- El trabajo se realiza paso a paso, con explicaciones y verificación antes de avanzar.
