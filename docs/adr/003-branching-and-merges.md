# ADR-003 — Ramas y estrategia de merge

- Estado: Aceptado por el desarrollador
- Fecha: 2026-10-15

## Contexto

Mi Día busca demostrar un flujo de equipo con PRs pequeños, integración de features y estabilización de releases. Se acepta el coste de mantener `main` y `develop` como parte de ese objetivo de portfolio.

## Alternativas

- Una rama principal y ramas cortas: menor mantenimiento y una opción razonable para un proyecto individual.
- `main`, `develop` y ramas de trabajo/release: más operaciones de integración, pero permite practicar el flujo propuesto en `CONTEXT.md`.

## Decisión

Mantener el modelo propuesto con las siguientes rutas:

| Origen | Se crea desde | Destino del PR | Método |
|---|---|---|---|
| `feature/*`, `fix/*`, `chore/*` | `develop` | `develop` | Squash merge |
| `release/x.y.z` | `develop` | `main` | Merge commit |
| La misma `release/x.y.z` | — | `develop`, después de integrar en `main` | Merge commit |
| `hotfix/*` | `main` | `main` | Merge commit |
| El mismo `hotfix/*` | — | `develop`, después de integrar en `main` | Merge commit |

- `main` representa las versiones estables; `develop` integra trabajo validado, no una garantía de release.
- Las releases admiten correcciones y estabilización, no features nuevas. Las features normales siguen entrando por `develop`.
- Mantener la rama release/hotfix hasta completar ambas integraciones. No recrearla desde otra base ni aplicar squash en su reintegración.
- Si no hay cambios que reintegrar, registrar ese hecho y omitir el PR vacío.
- Todo cambio posterior al arranque entra por PR. No hacer push directo a `main` ni `develop`.
- Usar Conventional Commits; el título de un PR con squash será el mensaje del commit resultante.

### Arranque del repositorio

Un repositorio vacío no permite abrir un PR sin una base. Se documenta una excepción única: crear un commit inicial mínimo de documentación en `main`, y crear `develop` desde ese mismo commit. El trabajo posterior irá en una rama `chore/*` hacia `develop`.

Ese primer commit establece la base documental, no representa una release de la app. Crear la base, publicar las ramas y activar protecciones son operaciones distintas que requieren autorización explícita; este ADR no las ejecuta ni las autoriza por sí mismo.

### Configuración objetivo de GitHub

- Habilitar squash y merge commits; elegir el método según la ruta del PR.
- No exigir historial lineal en `main` **ni en `develop`**: ambas reciben merge commits.
- Exigir PR, resolución de conversaciones y, cuando exista CI, sus checks reales de build y tests con la rama actualizada.
- Impedir force pushes y borrado de las ramas protegidas.
- Aplicar la política de revisión de [ADR-004](004-solo-review.md).
- Las rutas de origen/destino y el método de merge se revisan manualmente hasta disponer de automatización específica. Una protección genérica no garantiza esas rutas.

## Consecuencias

- El squash mantiene compacta la integración de ramas de trabajo; los merge commits preservan la ascendencia de releases y hotfixes entre ramas duraderas.
- Hay que verificar la reintegración para no perder correcciones en la siguiente release.
- No fusionar `develop` completo en `main` para publicar un hotfix: incluiría features aún no liberadas.
- La configuración admite errores de selección manual. Si se automatiza su validación, hacerlo en un cambio separado con tests.
- Este flujo tiene más coste que una única rama principal; revisarlo si deja de aportar aprendizaje o evidencia útil.

## Verificación y participación de IA

Se revisan las rutas y su compatibilidad con las protecciones documentadas. Git y GitHub todavía no están configurados; no se han probado merges reales.

El agente propuso concretar el modelo inicial y el desarrollador aprobó el bloque. El agente documentó las rutas y sus restricciones.
