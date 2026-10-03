# MCP-local

Plugin/MCP local orientado a ChatGPT Desktop para trabajar de forma controlada sobre repositorios y archivos autorizados del computador.

## Gobierno del desarrollo

Este repositorio usa un modelo de trabajo Supervisor → Implementator Agent IA → GitHub:

- **Humano / Product Owner**: define intención, prioridades, permisos y decisiones excepcionales.
- **ChatGPT Supervisor**: analiza arquitectura, delimita alcance, crea Issues, revisa PR/diff/evidencia y decide `SEMANTIC_ACCEPTED`, `REWORK`, `HOLD` o `ESCALATE`.
- **Implementator Agent IA**: ejecuta la implementación asignada (por ejemplo, Astra/Work u otro agente autorizado), crea o usa la branch indicada, modifica solamente el scope autorizado, ejecuta pruebas, hace commit/push y abre o actualiza el PR.
- **GitHub**: memoria persistente y fuente de evidencia del proyecto.

El Supervisor no implementa normalmente el mismo cambio que después revisará. El Implementator no se autoaprueba ni amplía el alcance silenciosamente.

Ver [docs/WORKFLOW_SUPERVISOR_IMPLEMENTATOR.md](docs/WORKFLOW_SUPERVISOR_IMPLEMENTATOR.md).

## Objetivo inicial

Construir un MCP local de mínimo privilegio para ChatGPT Desktop, priorizando:

1. acceso restringido a directorios explícitamente autorizados;
2. herramientas de lectura y diagnóstico del repositorio;
3. Git de solo lectura en la primera fase;
4. ejecución controlada de tests;
5. bloqueo de operaciones destructivas por defecto;
6. trazabilidad suficiente para auditoría y revisión.

Ver [docs/PRODUCT_SCOPE.md](docs/PRODUCT_SCOPE.md).

## Fuente de verdad

- El **Issue** define el objetivo, criterios de aceptación y scope autorizado.
- La **branch + commit + PR + diff + tests/CI** constituyen la evidencia.
- Los reportes de un agente no sustituyen el estado real del repositorio.
