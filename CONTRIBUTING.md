# Contribuir a Mi Día

## Referencias

- [Contexto y arquitectura](CONTEXT.md)
- [Reglas de los agentes](AGENTS.md)
- [Ramas y merges — ADR-003](docs/adr/003-branching-and-merges.md)
- [Revisión individual — ADR-004](docs/adr/004-solo-review.md)
- [Testing — ADR-005](docs/adr/005-testing-frameworks.md)

> Esta guía define el flujo acordado. Existen el proyecto Xcode, su esquema compartido, `.gitignore` y la plantilla local de PR. Git está inicializado en `main`; la base inicial se limita a documentación y `.gitignore`, sin incorporar aún el proyecto Xcode ni los tests. El remoto local no está configurado y la publicación, CI y las protecciones están pendientes.

## Unidad de trabajo

Cada cambio parte de un issue con objetivo, límites y criterios de aceptación verificables. Mantener un PR pequeño y enfocado; no mezclar una feature con refactors ajenos a su alcance.

Para delegar en un agente, especificar:

```text
Objetivo: resultado observable que debe entregar.
Alcance: archivos o componentes que puede modificar.
Fuera de alcance: cambios que no debe hacer.
Aceptación: comportamientos y comprobaciones exigidas.
Autonomía: qué puede ejecutar y cuándo debe consultar.
Entrega: resumen, archivos modificados, evidencia y pendientes.
```

Las decisiones relevantes se acuerdan antes de implementarlas. La autorización para modificar archivos no implica permiso para hacer commits, push, publicar PRs o fusionarlos.

## Ramas y merges

| Trabajo | Base | Destino | Método |
|---|---|---|---|
| `feature/<modulo>-<descripcion>` | `develop` | `develop` | Squash |
| `fix/<descripcion>` | `develop` | `develop` | Squash |
| `chore/<descripcion>` | `develop` | `develop` | Squash |
| `release/x.y.z` | `develop` | `main`, luego `develop` | Merge commit en ambos PRs |
| `hotfix/<descripcion>` | `main` | `main`, luego `develop` | Merge commit en ambos PRs |

- No hacer push directo a `main` ni `develop`, salvo el arranque único documentado en ADR-003.
- Conservar releases y hotfixes hasta completar su reintegración; si no hay diferencia pendiente, documentarlo en lugar de abrir un PR vacío.
- No exigir historial lineal en ninguna de las dos ramas duraderas.
- Antes de fusionar, comprobar base, origen y método: estas rutas no están automatizadas actualmente.

## Commits

Usar Conventional Commits con un scope cuando aporte contexto:

```text
feat(weather): add current forecast use case
test(weather): cover forecast failure
chore(ci): add build and test workflow
docs: document solo review policy
```

El título del PR debe seguir esta convención, especialmente cuando se convierta en el commit de squash.

## Pull Requests y revisión

1. Enlazar el issue y usar la [plantilla de PR](.github/pull_request_template.md), que solicita la evidencia de ADR-004. El archivo existe localmente; todavía no está publicado en GitHub.
2. Explicar qué cambia, por qué y cómo comprobarlo; añadir capturas si cambia la UI.
3. Revisar el diff completo, incluidos tests, configuración y archivos generados.
4. Revisar separación de capas, disponibilidad desde iOS 17, estados de error, cancelación/concurrencia cuando aplique y ausencia de secretos.
5. Comprobar que los tests verifican resultados relevantes y fallos, no solo llamadas internas ni una cifra de cobertura.
6. Registrar self-review humana y asistencia de IA sin atribuir independencia inexistente.
7. Resolver conversaciones y cumplir los checks configurados antes del merge. Fusionar es una decisión del desarrollador.

Usar `Closes #N` cuando corresponda cerrar un issue. GitHub aplica el cierre automático al integrar en la rama predeterminada; un PR hacia `develop` no garantiza que lo cierre. Usar `Refs #N` cuando solo se quiere enlazarlo o mantenerlo abierto.

En modo individual no se exigen aprobaciones formales ni review de Code Owners. El checklist no es una aprobación humana de GitHub. Si participa otra persona, identificar su revisión real y reevaluar ADR-004.

## Verificación

- Swift Testing: pruebas unitarias y de integración. XCTest con XCUIAutomation: flujos críticos de UI.
- Ningún test automatizado debe depender de red real. Aislar datos persistentes y estado compartido entre pruebas.
- Proyecto: `MyDayApp.xcodeproj`; esquema compartido: `MyDayApp`, en `MyDayApp.xcodeproj/xcshareddata/xcschemes/MyDayApp.xcscheme`; targets: `MyDayApp`, `MyDayAppTests` y `MyDayAppUITests`. Versión mínima efectiva: iOS 17.0 en Debug y Release para los tres targets.
- Para cada PR, registrar comando/pasos, entorno relevante, resultado y bloqueos. No incluir secretos ni logs sensibles.
- En cambios exclusivamente documentales, comprobar consistencia y enlaces relativos; justificar por qué build/tests no aplican.
- El CI futuro debe ejecutar las suites de pruebas y fallar ante errores. Una ejecución local no demuestra que el workflow exista o haya pasado.

### Validación local del proyecto base

Entorno comprobado: Xcode 26.2 (17C52). Destino elegido: iPhone 15 Pro con iOS 17.5, identificador local `CB7AB070-6533-4CE6-A313-AD1D20814BE5`. Consultar `xcodebuild -showdestinations -project MyDayApp.xcodeproj -scheme MyDayApp` si el destino no está disponible en otro equipo.

Comandos para reproducir la validación con artefactos fuera del proyecto:

```sh
ARTIFACTS="$(mktemp -d "${TMPDIR%/}/myday-validation.XXXXXX")"
DESTINATION='platform=iOS Simulator,id=CB7AB070-6533-4CE6-A313-AD1D20814BE5'

xcodebuild -project MyDayApp.xcodeproj -scheme MyDayApp \
  -configuration Debug -destination "$DESTINATION" \
  -derivedDataPath "$ARTIFACTS/DerivedData" build

xcodebuild -project MyDayApp.xcodeproj -scheme MyDayApp \
  -configuration Debug -destination "$DESTINATION" \
  -derivedDataPath "$ARTIFACTS/DerivedData" \
  -resultBundlePath "$ARTIFACTS/Tests-iOS17.xcresult" \
  -parallel-testing-enabled NO test
```

Estos comandos ejecutan ambas suites. El test de UI `MyDayAppUITests.testLaunchShowsInitialGreeting` abre la app y exige que exista el texto `Hello, world!` en un máximo de 5 segundos después de `app.launch()`. Es una comprobación mínima de la pantalla actual, no una prueba de reglas de negocio ni de su apariencia visual. Usa el texto existente sin añadir identificadores ni cambiar la app; si cambia el saludo, revisar también el test.

Los demás tests siguen siendo de plantilla: el unitario no contiene aserciones, y los de lanzamiento/rendimiento no verifican contenido. El resultado y las limitaciones de la última ejecución están en `memory.md`. El esquema compartido incluye ambos targets de tests sin omitir ninguno; su existencia no configura ni demuestra una ejecución de CI.

### Archivos compartidos y locales

El `.gitignore` actualizado por el desarrollador excluye estado personal de Xcode, productos de compilación y otros artefactos locales. Se conserva esa versión con dos ajustes acordados: excluir bundles `*.xcresult/` y permitir versionar `Package.resolved` para reproducir las versiones de dependencias de la app. No excluye el proyecto, `xcshareddata`, assets, código, tests ni documentación. Los archivos personales existentes se conservan en disco. Estar disponible para versionar no implica estar incluido en el primer commit: la base inicial excluye expresamente app, proyecto y tests.

## Definition of Done

- Criterios del issue satisfechos y alcance respetado.
- Build sin warnings nuevos y tests relevantes en verde, o no aplicabilidad justificada para cambios documentales.
- Arquitectura respetada, sin lógica de negocio en Views ni dependencias de infraestructura en Domain.
- PR con evidencia, self-review y conversaciones resueltas; checks obligatorios satisfechos cuando estén configurados.
- ADRs actualizados si hay una decisión relevante; `memory.md` actualizado si cambia el estado del proyecto.

## Estado del arranque

El commit base inicial es una excepción explícita, no un precedente para futuros pushes directos. Su ejecución, publicación y configuración de GitHub requieren autorización separada. Los documentos locales no habilitan protección alguna.
