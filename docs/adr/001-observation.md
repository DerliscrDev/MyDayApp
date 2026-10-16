# ADR-001 — Observación de ViewModels con @Observable

- Estado: Aceptado por el desarrollador
- Fecha: 2026-10-15

## Contexto

Mi Día es una app nueva con SwiftUI y versión mínima iOS 17. Necesitamos que la interfaz responda a cambios del estado de presentación expuesto por los ViewModels, como carga, contenido y error.

La observación pertenece a Presentation. No debe introducir dependencias de UI ni de observación en Domain.

## Alternativas

### ObservableObject y @Published

- Enfoque válido y ampliamente utilizado, compatible con versiones anteriores a iOS 17.
- Requiere declarar las propiedades publicadas con `@Published`.
- Es útil al mantener código existente basado en este modelo o al necesitar compatibilidad con sistemas anteriores.

### @Observable

- Macro del framework Observation, disponible desde iOS 17.
- Reduce el código repetitivo: las propiedades observables no requieren `@Published`.
- Permite a SwiftUI seguir las propiedades observables que lee la vista al evaluar su contenido.
- No requiere añadir dependencias externas.

## Decisión

Utilizar `@Observable`, importando `Observation`, para los ViewModels que exponen estado observable a SwiftUI.

Se elige por ser una app nueva cuyo deployment target ya satisface el requisito de iOS 17. No existe código previo que necesite migración desde `ObservableObject`.

## Consecuencias

- Mantener la observación en Presentation; las entidades y los casos de uso de Domain no adoptarán `@Observable` por esta decisión.
- No utilizar `ObservableObject` y `@Published` como patrón por defecto para los nuevos ViewModels.
- Aprender y elegir correctamente la propiedad y el ciclo de vida del ViewModel en las Views. `@Observable` no sustituye por sí solo esa gestión.
- `@Observable` no proporciona aislamiento de concurrencia ni garantiza seguridad entre hilos. El uso de `@MainActor` se explicará y acordará al implementar los ViewModels.
- Si se baja la versión mínima de iOS, habrá que revisar esta decisión.

## Verificación prevista

Cuando exista el primer ViewModel, comprobar con tests sus cambios de estado y verificar que la vista refleja los cambios observados.

Este ADR documenta una elección; todavía no existe una implementación ni un build que la verifique en el proyecto.

## Participación de IA

El agente explicó las alternativas y recomendó `@Observable`. El desarrollador aprobó explícitamente la elección. El agente redactó este registro a partir de esa aprobación.
