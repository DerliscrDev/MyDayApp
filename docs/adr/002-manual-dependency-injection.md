# ADR-002 — Inyección de dependencias manual

- Estado: Aceptado por el desarrollador
- Fecha: 2026-10-15

## Contexto

Las capas de Mi Día necesitan colaborar sin acoplar los ViewModels a implementaciones de repositorio ni los casos de uso a detalles de red o persistencia.

También necesitamos sustituir colaboradores en los tests sin utilizar red real. La app se construirá por features y todavía no tiene una complejidad que justifique infraestructura de resolución de dependencias.

## Alternativas

### Inyección manual

- Los objetos reciben sus dependencias explícitamente mediante sus inicializadores.
- La composición conecta las implementaciones con sus consumidores.
- Las dependencias requeridas son visibles y el compilador comprueba los tipos.
- Requiere escribir y mantener el código de composición.

### Contenedor propio

- Centraliza el registro y la resolución de dependencias.
- Puede simplificar la composición cuando existen muchos objetos y ciclos de vida diferentes.
- Añade infraestructura, reglas de resolución y gestión de ciclos de vida que debemos diseñar, probar y mantener.
- Según su diseño, puede ocultar dependencias y trasladar errores de configuración al tiempo de ejecución.

## Decisión

Empezar con **inyección manual mediante inicializadores**, componiendo las dependencias desde `App/`.

Los ViewModels recibirán casos de uso. Los casos de uso recibirán las abstracciones de repositorio que necesiten. Las implementaciones de Data recibirán sus colaboradores, como clientes de red o fuentes de datos.

No crear un contenedor genérico, service locator ni singletons globales para resolver dependencias. Tampoco añadir una librería externa de DI.

## Consecuencias

- La composición es el lugar que puede conocer las implementaciones concretas de las distintas capas para conectarlas; esto no autoriza a Domain a importar Data o Presentation.
- Los tests construirán los objetos con colaboradores controlados. La inyección facilita las pruebas, pero no las sustituye.
- Gestionar explícitamente qué instancias se comparten y cuánto viven; la inyección por sí sola no decide su ciclo de vida.
- No crear protocolos ni fábricas sin una necesidad concreta. La elección de DI manual no exige una abstracción para cada tipo.
- La carpeta prevista `Core/DI/` no obliga a crear infraestructura vacía ni un contenedor.
- Si la composición crece de forma problemática, evaluar el problema con ejemplos y registrar una nueva decisión antes de cambiar de estrategia.

## Verificación prevista

Al implementar la primera feature, comprobar que sus dependencias se reciben mediante inicializadores y que se puede probar sin red real utilizando colaboradores controlados.

Este paso es exclusivamente documental; todavía no hay implementación ni build que verificar.

## Participación de IA

El agente explicó ambas alternativas y recomendó inyección manual por simplicidad y claridad. El desarrollador aprobó explícitamente la propuesta y el agente redactó este registro.
