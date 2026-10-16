# Reglas para agentes — Mi Día

## Contexto

- Leer `CONTEXT.md` y `memory.md` antes de trabajar.
- El desarrollador ya es iOS developer. El objetivo de aprendizaje es dirigir agentes de IA: definir tareas, controlar su autonomía, revisar cambios y exigir evidencia. Responder en español; no explicar fundamentos de iOS ni el uso básico de Xcode salvo que lo solicite.
- Trabajar en pasos pequeños: no implementar un hito completo en una sola intervención.

## Ciclo de trabajo

1. Acordar el objetivo y el alcance del paso antes de modificar archivos.
2. Explicar la solución recomendada y sus alternativas cuando haya una decisión relevante. Esperar la elección del desarrollador antes de darla por aprobada.
3. Implementar solo el alcance acordado, sin refactors ni dependencias adicionales no solicitados.
4. Revisar los cambios y ejecutar las comprobaciones pertinentes disponibles.
5. Resumir qué cambió, por qué y cómo se verificó. Distinguir lo comprobado de lo pendiente.
6. Actualizar `memory.md` al cerrar un paso con decisiones, estado y próximo objetivo. Evitar transcripciones extensas.

## Arquitectura y calidad

- Respetar `Presentation → Domain ← Data`. Domain no debe depender de SwiftUI, SwiftData ni APIs de red.
- Mantener las Views sin lógica de negocio y los ViewModels dependientes de casos de uso, no de implementaciones de repositorio.
- Respetar iOS 17 como versión mínima; comprobar la disponibilidad antes de usar APIs posteriores.
- Añadir tests con cada caso de uso nuevo. Los tests automatizados no deben depender de red real.
- Preferir la solución más simple que cubra la necesidad actual; no crear abstracciones para features futuras.
- Registrar las decisiones arquitectónicas relevantes en `docs/adr/`, distinguiendo propuestas de decisiones aceptadas.

## Límites de autonomía

- No crear commits, hacer push, publicar PRs ni cambiar ajustes de GitHub sin autorización explícita para esa acción.
- No borrar ni sobrescribir trabajo ajeno ni ejecutar operaciones destructivas sin permiso.
- No añadir secretos al repositorio ni a la documentación.
- No delegar en otros agentes sin que el desarrollador lo solicite; si se propone delegación, explicar primero el objetivo y cómo se revisará su resultado.
- No afirmar que el build o los tests pasan si no se ejecutaron. Informar los bloqueos y las comprobaciones pendientes.
- No presentar una revisión de IA como una aprobación humana o una revisión independiente.
