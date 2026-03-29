---
title: "Mi primer post de prueba"
date: 2026-03-29
description: "Este es un post de prueba para ver cómo se visualiza el contenido en el blog."
tags: ["RedTeam", "Infraestructura", "prueba"]
draft: false
---

## Principio de separación funcional

La arquitectura implementa **aislamiento por función** — cada capacidad ofensiva (entrega de payloads, comando y control, phishing) opera en su propio canal independiente. Este patrón está documentado en operaciones de APT41/Winnti: si un nodo es "quemado" (detectado y atribuido), el blast radius se contiene a esa función específica. El C2 puede seguir operando aunque el servidor de phishing sea identificado.



## Capas de abstracción (de izquierda a derecha)

**Máquina Atacante (Kali)**: Es tu único punto de entrada a toda la infraestructura. Los firewalls de cada redirector están configurados para aceptar conexiones SSH _exclusivamente_ desde esta IP. Esto replica controles de perímetro empresarial aplicados de manera ofensiva.

**Backend Servers**: Nunca están expuestos directamente a Internet. PwnDrop escucha en `127.0.0.1`, Mythic en su puerto interno (7443), Evilginx en su configuración local. La clave es que _todo el tráfico público pasa primero por el redirector_.

**Redirectores (NGINX)**: Son la **única superficie pública**. Implementan filtrado en dos condiciones (User-Agent + IP whitelist), terminación SSL, y en el caso del C2, headers de WebSocket upgrade para mantener conexiones persistentes con los implantes.

**Firewalls (UFW)**: La última línea de defensa antes de Internet. Solo exponen puerto 443 al mundo, mientras que los puertos de administración (SSH, paneles de control) solo aceptan conexiones desde tu IP de operador.

## Imagen Descriptiva

![Estructura de Infraestructura](/images/Post_1/imagen_infra.png)


