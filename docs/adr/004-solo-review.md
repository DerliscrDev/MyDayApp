# ADR-004 — Revisión en trabajo individual

- Estado: Aceptado por el desarrollador
- Fecha: 2026-10-15

## Contexto

El proyecto se desarrolla de forma individual con asistencia de IA. GitHub no permite que el autor apruebe su propio PR. Necesitamos revisión visible y comprobaciones reproducibles sin simular una aprobación independiente inexistente.

## Alternativas

- Exigir aprobación de otra persona: aporta revisión independiente, pero depende de contar con un colaborador disponible.
- Exigir una aprobación sin disponer de otro reviewer: bloquea los PRs propios y fomenta excepciones a las reglas.
- PR obligatorio, CI y self-review documentada, con revisión de IA adicional: viable en trabajo individual, aunque no sustituye a otra persona.

## Decisión

Adoptar **PR + CI + self-review documentada por el desarrollador**, con revisión asistida por IA adicional identificada como tal.

- No exigir aprobaciones humanas ni revisión obligatoria de Code Owners mientras el proyecto sea individual.
- Cuando exista `CODEOWNERS`, usarlo para expresar responsabilidad; no considerarlo evidencia de revisión ni un reemplazo de la protección de ramas.
- El desarrollador revisa el diff, evalúa la evidencia y decide si fusionar. La recomendación del agente no toma esa decisión.
- No usar otra cuenta propia ni atribuir una revisión de IA a una persona para aparentar independencia.
- Si se incorpora un colaborador, revisar este ADR antes de activar aprobaciones obligatorias y descarte de aprobaciones antiguas.

## Evidencia mínima por PR

1. Issue enlazado, objetivo y alcance; indicar exclusiones relevantes.
2. Verificación: comandos o pasos ejecutados, resultado y limitaciones. Diferenciar pruebas locales, CI y comprobaciones no realizadas.
3. Self-review humana con observaciones concretas sobre arquitectura, comportamiento, errores, tests y riesgos; no solo casillas marcadas.
4. Si se usa IA: indicar en qué ayudó, hallazgos relevantes y cuáles se corrigieron o descartaron con motivo. Si no se usó para revisión, declararlo.
5. Resolver las conversaciones y registrar riesgos pendientes antes del merge.

La revisión del mismo agente que implementó es una autoevaluación asistida, no una revisión independiente. Una segunda sesión de IA puede aportar otra perspectiva, pero tampoco constituye una aprobación humana y debe respetar los permisos de delegación del proyecto.

## CI y excepciones

- El estado objetivo es exigir los checks reales de build y tests. Todavía no existe un workflow: no afirmar que el gate está activo ni inventar nombres de checks.
- Los PRs de documentación pueden justificar que build/tests locales no aplican. Eso no autoriza a saltarse checks obligatorios ya configurados.
- La excepción de arranque del repositorio se limita a la descrita en [ADR-003](003-branching-and-merges.md).

## Consecuencias

El flujo es transparente y practicable, pero no ofrece la independencia de una revisión por otra persona. Tests, CI y checklist reducen riesgos; no garantizan ausencia de defectos.

## Verificación y participación de IA

La guía de contribución y la plantilla de PR deben pedir esta evidencia sin presentar casillas marcadas como aprobaciones de GitHub. La aplicación de protecciones queda pendiente.

El agente recomendó el modelo y el desarrollador lo aprobó. Este registro no afirma que haya ocurrido una revisión humana del código de la app, que todavía no existe.
