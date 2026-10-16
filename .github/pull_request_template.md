## Objetivo y contexto

<!-- Qué problema resuelve, por qué y criterios de aceptación. -->

Issue: <!-- Refs #N o Closes #N; el cierre automático depende de la rama predeterminada. -->

## Alcance

- Cambios:
- Fuera de alcance:
- Decisiones/ADRs relacionados:

## Ramas e integración

- Origen → destino:
- Método de merge y motivo según ADR-003:
- Reintegración pendiente (release/hotfix; no aplica para otras ramas):

## Verificación

<!-- Evidencia real: comandos/pasos, entorno/destino, resultados y bloqueos.
Separar lo ejecutado localmente, lo verificado por CI y lo pendiente.
Si build/tests no aplican, justificarlo. No incluir secretos ni logs sensibles. -->

- Build y warnings nuevos respecto a la base:
- Tests y comportamiento comprobado (sin red real):
- CI (enlace real o indicar que no está configurado):
- Capturas si cambia la UI:
- Riesgos y comprobaciones pendientes:

## Self-review del desarrollador

<!-- Completar tras revisar personalmente el diff y la evidencia.
Anotar observaciones concretas sobre comportamiento, arquitectura, tests y riesgos.
Esta sección no es una aprobación formal de GitHub. -->

## Asistencia de IA

<!-- Indicar implementación/revisión asistida o declarar que no se usó IA.
Describir hallazgos y cuáles se corrigieron o descartaron, con motivo.
La autoevaluación del agente implementador no es una revisión independiente
ni una aprobación humana. -->

## Checklist

- [ ] Criterios de aceptación cubiertos y alcance respetado.
- [ ] Build sin warnings nuevos y tests relevantes correctos, o no aplicabilidad justificada.
- [ ] Sin dependencias inversas entre capas ni lógica de negocio en Views; compatible con iOS 17.
- [ ] Sin secretos ni datos personales innecesarios en cambios o evidencia.
- [ ] Documentación y ADRs actualizados cuando aplica.
- [ ] Self-review humana documentada y asistencia de IA declarada.
- [ ] Conversaciones resueltas y checks obligatorios satisfechos cuando estén configurados.
- [ ] Base, destino, método de merge y reintegración revisados antes de fusionar.
