# Workflow — ChatGPT Supervisor + Implementator Agent IA + GitHub

## 1. Objetivo

Este workflow organiza el trabajo colaborativo entre:

- **ChatGPT Supervisor**: planificación, arquitectura, definición de tareas y revisión.
- **Implementator Agent IA**: implementación y escritura técnica.
- **GitHub**: memoria persistente, tareas, código, evidencia y coordinación.
- **Humano / Product Owner**: intención del producto, prioridades, permisos y decisiones excepcionales.

Flujo base:

```text
Humano
  ↓ intención / prioridad
ChatGPT Supervisor
  ↓ define Work Item
GitHub Issue
  ↓
Implementator Agent IA
  ↓ implementa + verifica
Branch / Commit / PR
  ↓
ChatGPT Supervisor
  ↓ revisión independiente
SEMANTIC_ACCEPTED / REWORK / HOLD / ESCALATE
```

GitHub reemplaza el chat como memoria compartida.

## 2. Principio fundamental

Las conversaciones son temporales. GitHub y la documentación del proyecto son persistentes.

Una sesión nueva debe poder reconstruir el trabajo con:

```text
Repositorio
+ Issue
+ branch / PR
+ documentación técnica
+ evidencia de tests / CI
```

## 3. ChatGPT Supervisor

Responsabilidades:

- entender la intención del humano;
- estudiar el proyecto cuando sea necesario;
- razonar sobre arquitectura e impacto;
- dividir trabajo en tareas pequeñas;
- crear o definir el Work Item;
- delimitar Semantic Scope y Path Scope;
- indicar documentación relevante;
- revisar independientemente Issue, diff, PR, SHA y evidencia;
- detectar cambios fuera de alcance y sobreingeniería;
- solicitar REWORK;
- aceptar semánticamente el cambio para un SHA concreto;
- escalar decisiones de producto, permisos o trade-offs materiales.

El Supervisor no implementa normalmente el código de la tarea que posteriormente revisará.

## 4. Implementator Agent IA

El Implementator es el desarrollador ejecutor. Puede ser Astra/Work u otro agente autorizado por el humano.

Responsabilidades:

- leer el Issue;
- recuperar únicamente el contexto necesario;
- revisar documentación relevante;
- comprobar el estado del repositorio;
- crear o utilizar la branch indicada;
- implementar únicamente el alcance autorizado;
- ejecutar pruebas;
- revisar su propio diff;
- commit;
- push;
- abrir o actualizar PR;
- publicar evidencia y handoff breve.

No debe:

- redefinir arquitectura por iniciativa propia;
- ampliar scope silenciosamente;
- corregir problemas no relacionados;
- declarar su propio trabajo aprobado;
- hacer merge sólo porque los tests pasaron.

## 5. GitHub como centro del sistema

GitHub conserva:

```text
Issue       → intención concreta y alcance
Branch      → trabajo aislado
Commit      → estado exacto del código
PR          → propuesta de integración
Diff        → evidencia del cambio
Tests / CI  → evidencia automatizada
Review      → REWORK / aceptación / decisiones
Git history → historial
```

Regla: lo dicho por un modelo es un reporte; el repositorio es la evidencia.

## 6. Unidad de trabajo: GitHub Issue

Cada tarea suficientemente importante debe existir como Issue y contener:

```markdown
## Objective
Qué debe conseguir la tarea.

## Acceptance Criteria
Qué condiciones deben cumplirse.

## Authorized Scope
Qué archivos o módulos puede modificar.

## Relevant Sources
Qué documentación debe leer.

## Verification
Qué pruebas o comprobaciones debe ejecutar.

## Base
Branch o commit desde el que comienza.
```

Default recomendado:

```text
1 Issue → 1 objetivo → 1 branch → 1 PR
```

## 7. Scope

### Semantic Scope

Definido por Objective + Acceptance Criteria. Establece qué comportamiento está autorizado a cambiar.

### Path Scope

Definido por Authorized Scope. Establece qué archivos o módulos puede modificar.

Un cambio válido debe cumplir ambos límites.

## 8. Bootstrap del Implementator

Antes de modificar código debe poder responder:

1. ¿Cuál es el objetivo del Issue?
2. ¿Cuáles son los Acceptance Criteria?
3. ¿Qué comportamiento está autorizado a cambiar?
4. ¿Qué rutas están autorizadas?
5. ¿Qué documentos debe leer?
6. ¿Qué módulo es responsable?
7. ¿Qué interfaces podría afectar?
8. ¿Qué pruebas debe ejecutar?
9. ¿Cuál es la branch/base correcta?
10. ¿Existen cambios previos ajenos a la tarea?

Si una respuesta material falta, debe detener la implementación y pedir aclaración mediante el Issue/PR.

## 9. Política de lectura

Orden recomendado:

```text
Issue
↓
README si necesita orientación
↓
documentación pertinente
↓
código y tests afectados
```

Sólo ampliar contexto ante una dependencia real.

## 10. Implementación

- modificar únicamente lo necesario;
- preservar comportamiento no relacionado;
- evitar refactors oportunistas;
- evitar dependencias sin necesidad;
- evitar infraestructura futura sin requisito;
- respetar contratos existentes;
- mantener el cambio pequeño y revisable.

Para prototipos, preferir la solución más simple que cumpla correctamente el requisito.

## 11. Hallazgos fuera de alcance

```text
Relacionado y dentro del scope
→ puede corregirse.

No relacionado
→ reportar; no corregir.

Requiere ampliar scope
→ detenerse y solicitar decisión del Supervisor.
```

## 12. Verificación y publicación

Antes de publicar:

```text
implementar
↓
ejecutar tests relevantes
↓
corregir
↓
volver a ejecutar
↓
revisar diff completo
↓
commit
↓
push
↓
PR
```

El PR debe identificar Issue, branch, commit SHA, cambios, pruebas y limitaciones conocidas.

Handoff recomendado:

```text
WORK ITEM: #N
PR: #N
COMMIT: <sha>
VERIFICATION: PASS / FAIL
CI: PASS / FAIL / PENDING / NOT CONFIGURED
STATE: READY_FOR_REVIEW
UNEXPECTED FINDING: none / <detalle>
```

## 13. Revisión del Supervisor

El Supervisor revisa independientemente:

- Issue;
- Objective y Acceptance Criteria;
- Semantic Scope y Path Scope;
- diff y archivos cambiados;
- arquitectura, interfaces y dependencias;
- tests;
- documentación;
- PR;
- HEAD SHA;
- CI cuando exista.

Decisiones posibles:

- `SEMANTIC_ACCEPTED`: satisface el Work Item para el SHA revisado.
- `REWORK`: requiere corrección dentro del mismo objetivo.
- `HOLD`: impedimento objetivo temporal.
- `ESCALATE`: requiere decisión humana.

Un nuevo commit invalida la aceptación de un SHA anterior y exige nueva revisión.

## 14. Merge

`SEMANTIC_ACCEPTED` no concede automáticamente autoridad de merge.

Antes de integrar se verifica que:

- HEAD siga siendo el revisado;
- no exista conflicto relevante;
- CI requerido siga válido;
- no exista blocker nuevo.

La ejecución del merge depende de la política del repositorio y de la autorización del humano.

## 15. Reconstrucción de contexto

Una nueva sesión del Implementator recibe como mínimo:

```text
Repositorio
Issue
```

y reconstruye branch, HEAD, PR, últimos comentarios, documentación relevante, código y tests.

Una nueva sesión del Supervisor reconstruye qué producto se desarrolla, Work Item activo, objetivo, scope, HEAD, diff, evidencia y última decisión válida.

## 16. Reglas esenciales

1. GitHub es la memoria compartida.
2. El Issue define la tarea.
3. La documentación define el producto.
4. Implementator implementa; Supervisor revisa.
5. El Implementator no se autoaprueba.
6. Scope = comportamiento autorizado + rutas autorizadas.
7. Una decisión de review sólo vale para el SHA revisado.
8. Tests/CI son evidencia, no aprobación.
9. REWORK del mismo objetivo permanece en el mismo Issue/PR.
10. Una sesión nueva reconstruye desde GitHub, no desde el transcript.
