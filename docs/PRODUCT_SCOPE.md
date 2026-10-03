# Product Scope — MCP-local

## 1. Propósito

Desarrollar un plugin/MCP local para ChatGPT Desktop que permita a un agente trabajar sobre proyectos locales con **mínimo privilegio**, controles explícitos y evidencia auditable.

El proyecto prioriza seguridad y control por sobre acceso irrestricto al computador.

## 2. Arquitectura objetivo inicial

```text
ChatGPT Desktop
      │
      │ MCP / plugin
      ▼
MCP-local
      │
      ├─ validación de directorio autorizado
      ├─ herramientas de archivos de solo lectura
      ├─ Git de solo lectura
      ├─ ejecución controlada de tests
      └─ logging / errores
```

## 3. Fase inicial: read-only / diagnóstico

Capacidades candidatas:

- listar archivos dentro de directorios autorizados;
- leer archivos de texto dentro de directorios autorizados;
- buscar texto dentro del proyecto;
- consultar `git status`;
- consultar `git diff`;
- consultar `git log`;
- consultar branch/HEAD;
- ejecutar comandos de test previamente configurados o explícitamente permitidos.

Operaciones prohibidas por defecto en esta fase:

- borrar o mover archivos;
- escribir o reemplazar archivos;
- `git commit`;
- `git push`;
- `git reset --hard`;
- instalación arbitraria de paquetes;
- ejecución de shell arbitraria;
- acceso fuera de los directorios autorizados;
- lectura de secretos o credenciales.

## 4. Seguridad

Requisitos de diseño:

- deny-by-default;
- canonicalización y validación de rutas antes de acceder al filesystem;
- lista explícita de directorios autorizados;
- no seguir rutas que escapen del root autorizado;
- herramientas pequeñas y específicas en vez de un `execute_command` abierto;
- anotaciones MCP correctas para acciones de lectura;
- errores claros sin filtrar secretos;
- tests de path traversal y solicitudes fuera de scope;
- logs de operaciones suficientes para diagnóstico sin registrar secretos.

## 5. Tecnología

El Implementator debe utilizar APIs y SDKs oficiales vigentes de MCP/OpenAI cuando corresponda.

Documentación de referencia inicial:

- https://developers.openai.com/plugins/build/mcp-server
- https://developers.openai.com/plugins/build/plugins
- https://help.openai.com/en/articles/20001256-plugins-in-chatgpt

La documentación externa puede cambiar. El Implementator debe verificar la versión vigente antes de fijar una decisión incompatible.

## 6. Fuera de alcance inicial

No forma parte del primer Work Item:

- UI personalizada;
- publicación pública del plugin;
- control total del escritorio;
- acceso remoto;
- gestión de credenciales;
- operaciones Git de escritura;
- shell genérico;
- automatización autónoma destructiva.

Estas capacidades requieren Issues posteriores y revisión de seguridad separada.
