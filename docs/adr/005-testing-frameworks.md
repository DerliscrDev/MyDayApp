# ADR-005 — Swift Testing y XCTest para las pruebas

- Estado: Aceptado por el desarrollador
- Fecha: 2026-10-15

## Contexto

Mi Día necesita pruebas de casos de uso, mappers, repositorios, ViewModels e integración entre componentes, además de unas pocas pruebas de flujos críticos de interfaz.

El desarrollador utiliza Xcode 26.2. La versión mínima de la app es iOS 17. Elegir herramientas de testing no cambia ese deployment target.

## Alternativas

### XCTest para todas las pruebas

- Framework consolidado que permite escribir pruebas unitarias y de integración.
- Con XCUIAutomation permite automatizar la interfaz de la app.
- Es una opción válida, especialmente al trabajar sobre suites existentes de XCTest.

### Swift Testing para pruebas unitarias y de integración; XCTest para UI

- Swift Testing ofrece `@Test`, `#expect` y pruebas parametrizadas, con soporte de concurrencia de Swift.
- XCTest con XCUIAutomation cubre las interacciones con la interfaz; Swift Testing no sustituye esa automatización.
- Requiere conocer dos herramientas, cada una con una responsabilidad definida.

## Decisión

Utilizar **Swift Testing para las pruebas unitarias y de integración** y **XCTest con XCUIAutomation para las pruebas de interfaz**.

Mantener las pruebas de UI en un target separado. No mezclar las APIs de aserción de ambos frameworks dentro de una misma prueba.

## Consecuencias

- Los tests unitarios cubrirán reglas, transformaciones y estados observables. Los de integración comprobarán la colaboración de componentes, sin red real.
- Las pruebas de UI utilizarán datos y condiciones controlados; no dependerán de APIs externas.
- No asumir que los tests se ejecutan secuencialmente: evitar estado mutable compartido y aislar los recursos de cada prueba.
- La cobertura numérica no reemplaza comprobar comportamientos, errores y casos límite.
- Configurar el esquema o plan de pruebas y el CI para ejecutar ambos targets. La mera existencia de tests no demuestra que el CI los ejecute.
- No añadir dependencias externas de testing al inicio.

## Verificación prevista

Al crear el proyecto, ejecutar al menos una prueba de Swift Testing y una prueba de UI con XCTest en un simulador disponible. Revisar el resultado para confirmar que se descubren y ejecutan ambas suites.

Posteriormente, verificar también su ejecución en GitHub Actions. Este paso solo documenta la decisión; todavía no hay proyecto ni tests que ejecutar.

## Participación de IA

El agente explicó las alternativas y recomendó la combinación. El desarrollador la aprobó explícitamente y el agente redactó este registro.
