# EveryProxy — Pedidos de proxy por tráfico con entrega controlada de credenciales

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/hero-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/hero-light.svg">
    <img alt="Un pedido de proxy residencial rotativo se aprueba y se aprovisiona; las credenciales se entregan una sola vez y no van a los registros compartidos." src="assets/hero-light.svg" width="100%">
  </picture>
</p>

<p align="center">
  <a href="https://cursor.com/en/install-mcp?name=everyinfra&config=eyJ1cmwiOiJodHRwczovL2FwaS5ldmVyeWluZnJhLmNvbS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgPEFQSSBLZXk%2BIn19"><img src="https://img.shields.io/badge/Instalar_en_Cursor-2F363D?style=for-the-badge&logo=cursor&logoColor=white" alt="Instalar en Cursor"></a>
  <a href="https://vscode.dev/redirect/mcp/install?name=everyinfra&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.everyinfra.com%2Fmcp%22%2C%22headers%22%3A%7B%22Authorization%22%3A%22Bearer%20%3CAPI%20Key%3E%22%7D%7D"><img src="https://img.shields.io/badge/Instalar_en_VS_Code-0065A9?style=for-the-badge&logo=githubcopilot&logoColor=white" alt="Instalar en VS Code"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.es.md#claude-code"><img src="https://img.shields.io/badge/Claude_Code-un_comando-B4532A?style=for-the-badge&logo=claude&logoColor=white" alt="Claude Code: un comando"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.es.md#codex"><img src="https://img.shields.io/badge/Codex-un_comando-2F363D?style=for-the-badge" alt="Codex: un comando"></a>
</p>

<p align="center">
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/stargazers"><img src="https://img.shields.io/github/stars/everyinfra/everyinfra-agent-plugins?style=social" alt="GitHub stars of everyinfra-agent-plugins"></a>
  &nbsp;<b>Dale una estrella al repo principal y recibe $1 de saldo.</b> <a href="https://everyinfra.com/es/docs?utm_source=github&utm_medium=readme&utm_campaign=everyproxy-plugin&utm_content=star_line#quickstart">Cómo reclamarlo →</a>
</p>

<p align="center"><a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <b>Español</b></p>

> [!TIP]
> **Este repositorio contiene una sola skill: EveryProxy.** El plugin completo de EveryInfra (10 skills, el servidor MCP remoto en `https://api.everyinfra.com/mcp` y la configuración para Claude Code, Codex, Cursor y VS Code) está en **[everyinfra/everyinfra-agent-plugins](https://github.com/everyinfra/everyinfra-agent-plugins)**. Los botones de arriba conectan el servidor MCP; la instalación del plugin en cada entorno está en el repo principal.

> [!NOTE]
> **Los pedidos de EveryProxy no están disponibles por ahora.** La skill sigue leyendo primero el catálogo en vivo e indica qué paquetes se pueden pedir antes de hacer ningún pedido.

[Configuración](docs/setup.md) · [Flujo de trabajo](docs/workflow.md) · [Prompts](examples/prompts.md) · [Referencia de capacidades](docs/reference.md) · [Documentación de la API](https://api.everyinfra.com/docs) <sub>(en inglés)</sub>

EveryProxy es una skill independiente de EveryInfra para pedir proxies facturados por tráfico a través de REST. Separa la elección del paquete, la consulta del estado del pedido y la entrega aprobada de credenciales, y mantiene las contraseñas de un solo uso fuera de los registros compartidos.

Úsala para flujos autorizados que necesitan tráfico a través de un proxy. Elegir un proxy no implementa por sí solo una búsqueda, un rastreo ni una sesión de navegador.

## Qué puedes hacer

- Revisar los paquetes de tráfico, las regiones y las opciones de sesión disponibles.
- Consultar el estado de un pedido sin provocar la entrega de credenciales.
- Entregar una contraseña de proxy de un solo uso solo a un destino seguro aprobado expresamente.

## Instalación

Es un paquete independiente con una sola skill y un flujo de trabajo **REST**; no es un servicio nuevo. Necesita un entorno de agente compatible y el acceso configurado que describe la [guía de configuración](docs/setup.md). Validar el paquete en local no demuestra que la API esté disponible.

Revisa el contenido del repositorio y luego instálalo en Codex. Estos comandos añaden un catálogo respaldado por GitHub, no un aval de ningún marketplace oficial:

```bash
codex plugin marketplace add everyinfra/everyproxy-plugin
codex plugin add everyproxy@everyproxy-plugin
```

Para una copia local, cambia la fuente del primer comando por `.`. La [guía de configuración](docs/setup.md) también cubre Claude Code y la conexión al servicio, que va aparte.

Con el acceso configurado, prueba:

> Revisa los paquetes de tráfico actuales para mi flujo autorizado y explica la unidad de facturación. No hagas ningún pedido.

Este primer prompt solo inspecciona o prepara. Revisa cualquier operación de pago o efecto externo antes de seguir.

## Cómo funciona

1. Lee `GET /api/v1/proxy/catalog` para ver tamaño del paquete, tipo, región y opciones de sesión fija.
2. Confirma el paquete, la región y el gasto antes de `POST /api/v1/proxy/order`.
3. Consulta `GET /api/v1/proxy/order/{order_id}` sin el parámetro de entrega cuando solo se pide el estado.
4. Usa `?deliver=1` solo cuando se quieran la entrega de credenciales y el cobro correspondiente.
5. Protege la contraseña de un solo uso e informa del estado sin imprimir credenciales. No crees un pedido de reemplazo automáticamente tras un reembolso.

**Un buen resultado:** una comparación del catálogo, un informe del estado del pedido o una entrega de credenciales aprobada. Una respuesta de estado correcta no prueba que se hayan entregado las credenciales ni que el sitio de destino sea accesible.

## Límites y seguridad

- Solo REST; no se anuncia ninguna herramienta MCP de proxy ni configuración automática del navegador.
- La facturación por tráfico no debe presentarse como alquiler por minuto.
- Nada de contraseñas en registros compartidos, pedidos de reemplazo de pago automáticos ni acceso garantizado al destino.

El paquete trae una sola skill: no concede permisos ni registra una segunda conexión MCP. No pongas credenciales en prompts, archivos del repositorio, capturas ni registros compartidos. Descubrir, ejecutar, cobrar y obtener un resultado externo final son estados distintos. Consulta la [política de seguridad](SECURITY.md).

## Preguntas frecuentes y validación

Las preguntas frecuentes, la comprobación sin conexión (`python3 scripts/validate.py`) y la guía para contribuir están en el [README en inglés](README.md#frequently-asked-questions).

Mantenido por [EveryInfra](https://everyinfra.com/es?utm_source=github&utm_medium=readme&utm_campaign=everyproxy-plugin&utm_content=footer). Licencia [Apache-2.0](LICENSE).
