# Workflow operativo — Supervisor ChatGPT + GitHub + Implementator (agente IA)

## 1. Objetivo

Este workflow organiza el trabajo colaborativo entre:

- **Supervisor (ChatGPT)**: planificación, arquitectura, definición de tareas, delegación y revisión independiente.
- **Implementator (agente IA)**: ejecución del desarrollo y gestión GitHub de cada tarea asignada (branch, commits, push, PR y evidencia).
- **GitHub**: memoria persistente, tareas, código, evidencia y coordinación.
- **Humano**: intención del producto, prioridades, permisos y decisiones excepcionales.

No utiliza GoFlow ni una mini aplicación auxiliar.

La idea principal es:

```text
Supervisor (ChatGPT)
    ↓ define trabajo
GitHub Issue
    ↓
Implementator (agente IA)
    ↓ implementa + verifica
GitHub Branch / Commit / PR
    ↓
Supervisor (ChatGPT)
    ↓ revisión
ACCEPT / REWORK / ESCALATE
```

GitHub reemplaza el chat como memoria compartida.

## Regla operativa de delegación

El **Supervisor (ChatGPT)** no toma a su cargo la implementación normal de las tareas que luego debe revisar. Su función es analizar, delimitar, crear o precisar el Work Item y **encargar formalmente su ejecución al Implementator (agente IA)**.

Una vez asignado un Work Item, el **Implementator** se hace cargo de su ciclo de ejecución en GitHub:

```text
leer Issue y documentación relevante
↓
comprobar base y estado del repositorio
↓
crear/usar branch de la tarea
↓
implementar
↓
probar y revisar diff
↓
commit + push
↓
abrir/actualizar PR
↓
publicar evidencia y handoff
```

El Supervisor vuelve a intervenir como revisor independiente sobre la evidencia persistida en GitHub. El Implementator **no se autoaprueba ni hace merge por iniciativa propia**, salvo que una política explícita del repositorio le otorgue esa autoridad.


---

# 2. Principio fundamental

> **Las conversaciones son temporales. GitHub y los documentos del proyecto son persistentes.**

Ni Supervisor (ChatGPT) ni Implementator (agente IA) deben depender de recordar conversaciones anteriores.

Una sesión nueva debe poder reconstruir el trabajo con:

```text
Repositorio
+ Issue
+ branch / PR
+ documentación técnica
```

No necesita recibir todo el transcript anterior.

---

# 3. Responsabilidades

## 3.1 Supervisor (ChatGPT)

Es el **Supervisor técnico**.

Responsabilidades:

- entender la intención del humano;
- estudiar el proyecto completo cuando sea necesario;
- razonar sobre arquitectura;
- identificar impacto;
- dividir trabajo en tareas pequeñas;
- crear o definir el Work Item;
- indicar documentación relevante;
- revisar PR, diff y evidencia;
- detectar cambios fuera de alcance;
- detectar sobreingeniería;
- solicitar REWORK;
- aceptar semánticamente el cambio.

No implementa normalmente el código de la tarea que posteriormente revisará.

Su foco es:

```text
pensar
planificar
delimitar
revisar
decidir
```

---

# 4. Implementator (agente IA)

Es el **desarrollador**.

Responsabilidades:

- leer el Issue;
- recuperar únicamente el contexto necesario;
- revisar la documentación relevante;
- comprobar el estado del repositorio;
- crear o utilizar la branch indicada;
- implementar únicamente el alcance autorizado;
- ejecutar las pruebas correspondientes;
- revisar su propio diff;
- commit;
- push;
- abrir o actualizar PR;
- informar resultados;
- mantener en GitHub el estado operativo de su tarea: branch, commits, PR, comentarios técnicos y evidencia de verificación;
- atender REWORK del Supervisor sobre el mismo Issue/PR cuando el objetivo no cambie.

Su foco es:

```text
leer contexto acotado
implementar
probar
revisar diff
publicar evidencia
```

No debe:

- redefinir arquitectura por iniciativa propia;
- ampliar el alcance silenciosamente;
- corregir problemas no relacionados;
- declarar su propio trabajo aprobado;
- hacer merge sólo porque los tests pasaron.

---

# 5. GitHub

GitHub es el centro del sistema.

Guarda:

```text
Issue
→ intención concreta y alcance

Branch
→ trabajo aislado

Commit
→ estado exacto del código

Pull Request
→ propuesta de integración

Diff
→ evidencia del cambio

CI
→ evidencia automatizada, cuando exista

Comentarios de review
→ REWORK / aceptación / decisiones

Git history
→ historial
```

Regla:

> **Lo dicho por un modelo es un reporte. El repositorio es la evidencia.**

---

# 6. Documentación del proyecto

El workflow simplificado utiliza la documentación normal del producto:

```text
README.md

docs/engineering/
├── SOFTWARE_REQUIREMENTS_SPECIFICATION.md
├── SOFTWARE_ARCHITECTURE.md
├── TECHNICAL_SPECIFICATION.md
├── VERIFICATION_PLAN.md
└── adr/
```

El workflow no se copia dentro de esos documentos.

La documentación describe la aplicación.

El workflow describe cómo trabajan las personas y modelos sobre ella.

---

# 7. Unidad de trabajo: GitHub Issue

Cada tarea suficientemente importante debe existir como un Issue.

Formato simplificado:

```markdown
## Objective

Qué debe conseguir esta tarea.

## Acceptance Criteria

Qué condiciones deben cumplirse.

## Authorized Scope

Qué archivos o módulos puede modificar.

## Relevant Sources

Qué documentación debe leer.

## Verification

Qué pruebas o comprobaciones debe realizar.

## Base

Branch o commit desde el que comienza el trabajo.
```

No necesitamos la gramática rígida utilizada por GoFlow.

Pero sí necesitamos que estas seis partes sean claras.

---

# 8. Dos tipos de scope

Aunque no exista GoFlow, seguimos utilizando dos límites.

## Semantic Scope

Proviene de:

```text
Objective
+
Acceptance Criteria
```

Define:

> qué comportamiento está autorizado a cambiar.

## Path Scope

Proviene de:

```text
Authorized Scope
```

Define:

> qué archivos o módulos puede modificar.

Para que un cambio sea válido debe cumplir ambos.

Ejemplo:

```text
Authorized Scope:
src/auth/**
```

no significa:

```text
puede cambiar cualquier regla de autenticación.
```

El comportamiento también debe estar autorizado por Objective y Acceptance Criteria.

---

# 9. Flujo completo

## Fase A — intención

El humano explica qué quiere conseguir.

```text
Humano
↓
Supervisor (ChatGPT)
```

Supervisor (ChatGPT) analiza:

- objetivo;
- arquitectura;
- módulos afectados;
- riesgos;
- alcance;
- verificación necesaria.

---

## Fase B — creación del Work Item

Supervisor (ChatGPT) prepara el Issue.

Debe ser suficientemente pequeño para que el Implementator pueda ejecutarlo sin reconstruir todo el proyecto.

Idealmente:

```text
1 Issue
→ 1 objetivo
→ 1 branch
→ 1 PR
```

Es un default, no una obligación absoluta.

---

# 10. Inicio del Implementator (agente IA)

El mensaje inicial puede ser extremadamente corto:

```text
Repositorio: <owner/repo>
Work Item: #123

Hazte cargo de la ejecución de esta tarea según el Issue, la documentación canónica y este workflow.
No amplíes el alcance.
Gestiona branch, commits, push y PR de la tarea; publica la evidencia cuando esté lista para revisión.
```

El prompt no transporta la especificación completa.

---

# 11. Bootstrap del Implementator (agente IA)

Antes de modificar código debe responder internamente estas preguntas:

1. ¿Cuál es el objetivo del Issue?
2. ¿Cuáles son los Acceptance Criteria?
3. ¿Qué comportamiento está autorizado a cambiar?
4. ¿Qué rutas están autorizadas?
5. ¿Qué documentos debo leer?
6. ¿Qué módulo es responsable de este comportamiento?
7. ¿Qué interfaces podría afectar?
8. ¿Qué pruebas debo ejecutar?
9. ¿Cuál es la branch/base correcta?
10. ¿Existen cambios previos que no pertenecen a esta tarea?

Si no puede responder una pregunta material, debe detenerse y pedir aclaración.

---

# 12. Política de lectura

Implementator (agente IA) no debe leer todo el repositorio automáticamente.

Orden recomendado:

```text
Issue
↓
README si necesita orientación
↓
secciones relevantes de SRS
↓
Architecture del módulo
↓
Technical Specification pertinente
↓
Verification Plan pertinente
↓
código y tests afectados
```

Sólo amplía contexto cuando encuentra una dependencia real.

---

# 13. Implementación

Durante la implementación:

- modificar únicamente lo necesario;
- mantener comportamiento no relacionado;
- no realizar refactors oportunistas;
- no introducir frameworks o dependencias sin necesidad;
- no crear infraestructura futura;
- respetar contratos existentes;
- mantener el cambio fácil de revisar.

Para prototipos:

> **La solución más simple que cumple correctamente el requisito normalmente es preferible.**

---

# 14. Problemas descubiertos durante el trabajo

Si el Implementator encuentra otro problema:

```text
Problema relacionado directamente
→ puede corregirse si está dentro del scope.

Problema no relacionado
→ se reporta.

Problema que requiere ampliar scope
→ se detiene y solicita decisión.
```

No debe existir:

```text
“Ya que estoy aquí también arreglé…”
```

---

# 15. Verificación local

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
```

Debe comprobar:

- tests;
- archivos cambiados;
- archivos nuevos;
- cambios accidentales;
- contratos afectados;
- documentación que realmente deba cambiar.

---

# 16. Publicación

Cuando el cambio esté listo:

```text
commit
↓
push
↓
PR
```

El PR debe permitir identificar:

```text
Issue
branch
commit SHA
qué cambió
qué pruebas se ejecutaron
limitaciones conocidas
```

---

# 17. Handoff de Implementator

El mensaje final debe ser breve.

Ejemplo:

```text
WORK ITEM: #123
PR: #145
COMMIT: abc1234
VERIFICATION: PASS
CI: PASS / NOT CONFIGURED / PENDING
STATE: READY_FOR_REVIEW
UNEXPECTED FINDING: none
```

No necesita escribir una explicación extensa de todo el desarrollo.

El Supervisor (ChatGPT) puede reconstruir el detalle desde GitHub.

---

# 18. Revisión de Supervisor (ChatGPT)

El Supervisor (ChatGPT) no debe aceptar simplemente porque Implementator diga que terminó.

Debe revisar independientemente:

- Issue;
- Objective;
- Acceptance Criteria;
- diff;
- archivos cambiados;
- arquitectura;
- interfaces;
- dependencias;
- tests;
- documentación;
- PR;
- SHA;
- CI cuando exista.

Y responder una de estas decisiones:

```text
SEMANTIC_ACCEPTED
REWORK
HOLD
ESCALATE
```

---

# 19. Significado de las decisiones

## SEMANTIC_ACCEPTED

El cambio satisface la intención del Work Item para el SHA revisado.

No significa automáticamente merge.

## REWORK

Existe una corrección necesaria dentro del mismo objetivo.

Normalmente continúa:

```text
mismo Issue
misma branch
mismo PR
```

## HOLD

Existe un impedimento objetivo que no puede resolverse actualmente.

Ejemplo:

```text
dependencia externa caída
credencial técnica no disponible
conflicto que impide continuar
```

## ESCALATE

Se requiere una decisión humana.

Ejemplos:

```text
cambio de alcance
decisión de producto
permiso
credencial
trade-off material
excepción importante
```

---

# 20. REWORK

El Supervisor (ChatGPT) debe escribir en el PR:

```text
Problema observado:
...

Resultado requerido:
...

Evidencia:
...

Scope:
permanece / cambia
```

Implementator:

```text
lee comentario
↓
corrige
↓
verifica
↓
nuevo commit
↓
push
↓
nuevo handoff
```

Un nuevo commit invalida cualquier aceptación anterior correspondiente a otro SHA.

---

# 21. Decisión vigente

Cuando existan varios comentarios antiguos:

> **La decisión vigente es la última decisión de Supervisor (ChatGPT) asociada explícitamente al HEAD actual.**

Ejemplo:

```text
SHA A → REWORK
SHA B → REWORK
SHA C → SEMANTIC_ACCEPTED
SHA D → nuevo commit
```

La aceptación de C no autoriza D.

D debe revisarse nuevamente.

---

# 22. CI

Cuando exista CI:

```text
PASS
FAIL
PENDING
NOT CONFIGURED
```

`PASS` es evidencia automática.

No significa aprobación.

Si el proyecto requiere CI y está:

```text
FAIL
PENDING
```

el cambio todavía no está listo para integración.

---

# 23. Integración

Secuencia conceptual:

```text
READY_FOR_REVIEW
↓
SEMANTIC_ACCEPTED
↓
verificar estado actual de target branch
↓
MERGE_ELIGIBLE
↓
MERGED
↓
CLOSED
```

Antes del merge se confirma que:

- HEAD sigue siendo el revisado;
- no apareció un conflicto relevante;
- CI requerido sigue válido;
- no existe un blocker nuevo.

---

# 24. Merge

Quién ejecuta el merge depende del proyecto.

Puede ser:

- humano;
- Supervisor (ChatGPT) mediante integración disponible;
- GitHub automático;
- otro actor autorizado.

La regla importante es:

> **SEMANTIC_ACCEPTED no concede por sí solo autoridad para hacer merge.**

Sólo se hace merge cuando el cambio está listo según la política del repositorio.

---

# 25. Cambio de sesión de Implementator

Si desaparece la sesión del Implementator:

La nueva sesión recibe solamente:

```text
Repositorio
Issue
```

y reconstruye:

```text
Issue
↓
branch
↓
HEAD
↓
PR
↓
últimos comentarios
↓
documentación relevante
↓
código
↓
tests
```

No necesita transcript anterior.

Debe responder nuevamente las diez preguntas de bootstrap antes de continuar.

---

# 26. Cambio de sesión de Supervisor (ChatGPT)

Una sesión nueva del Supervisor (ChatGPT) debe reconstruir:

1. ¿Qué producto se está desarrollando?
2. ¿Cuál es la arquitectura relevante?
3. ¿Qué Work Item está activo?
4. ¿Cuál es el objetivo?
5. ¿Qué scope fue autorizado?
6. ¿Cuál es el HEAD actual?
7. ¿Qué cambió realmente?
8. ¿Qué evidencia corresponde a ese SHA?
9. ¿Cuál es la última decisión válida para ese SHA?
10. ¿Qué decisión debe tomar ahora el Supervisor?

Tampoco necesita transcript anterior.

---

# 27. Rol del humano

El humano interviene principalmente para:

```text
intención
prioridades
decisiones de producto
cambio material de scope
credenciales
permisos
trade-offs importantes
```

No debería tener que copiar:

- diffs;
- código;
- contexto técnico;
- resultados extensos;

entre Supervisor (ChatGPT) y Implementator.

Un mensaje humano ideal puede ser simplemente:

```text
Implementator: trabaja Issue #123.
```

o:

```text
Supervisor (ChatGPT): revisa PR #145.
```

---

# 28. Reglas esenciales

El workflow simplificado puede resumirse en diez reglas:

1. GitHub es la memoria compartida.
2. El Issue define la tarea.
3. La documentación define el producto y su ingeniería.
4. Implementator implementa; Supervisor (ChatGPT) revisa.
5. Implementator no se autoaprueba.
6. Scope significa comportamiento autorizado + rutas autorizadas.
7. Una decisión de review sólo vale para el SHA revisado.
8. Tests/CI son evidencia, no aprobación.
9. REWORK del mismo objetivo permanece en el mismo Issue/PR.
10. Una sesión nueva reconstruye desde GitHub; no desde el transcript anterior.

---

# Resultado

Este workflow elimina completamente:

```text
GoFlow
.workflow/index.json
.workflow/project.json
wf status
wf begin
wf context
wf scope
wf verify
wf ci
wf review
```

y conserva la parte esencial del sistema:

```text
Humano
     ↓
Supervisor (ChatGPT)
     ↓
GitHub Issue
     ↓
Implementator (agente IA)
     ↓
Branch + code + tests + PR
     ↓
GitHub
     ↓
Supervisor (ChatGPT)
     ↓
SEMANTIC_ACCEPTED / REWORK / HOLD / ESCALATE
```

Es menos robusto mecánicamente que el workflow completo porque **scope, selección de contexto y verificación ya no están reforzados por software determinista**. Pero para operar la colaboración **Supervisor (ChatGPT) + Implementator (agente IA) + GitHub**, mantiene las partes más importantes sin introducir infraestructura adicional.