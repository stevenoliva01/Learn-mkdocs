---
title: Lab 18 — Workflow Templates
description: Adoptar un starter workflow y distinguirlo de reutilización en runtime.
tags: [GitHub Actions, Laboratorios]
---

# Lab 18 — Workflow Templates

## Objetivo

Usar una template proporcionada por GitHub como punto de partida y comprobar que el archivo resultante pertenece al repositorio. Lee [Workflow Templates](../reuse/workflow-templates.md) antes de empezar.

## Práctica

En un repositorio de pruebas abre **Actions → New workflow**, selecciona una template compatible, pulsa **Configure** y revisa cada trigger, job_id, Action e input antes de hacer commit. Ejecuta el workflow y observa que se creó `.github/workflows/<archivo>.yml`. Cambia un `name` visible y confirma que solo afecta a tu copia.

Una template de GitHub no actualiza automáticamente el workflow creado. Compara: un reusable workflow se llama como job, una Composite Action como step y una custom Action empaqueta lógica. La parte B, opcional, requiere una organización configurada: un repositorio especial `.github` puede ofrecer `workflow-templates/` y metadata. No crees organización ni cambies configuración corporativa para este lab.

## Rómpelo y corrígelo

Cambia temporalmente un input de una Action por un nombre inventado y observa el error o aviso de validación; restaura el input documentado. No copies una template sin revisar permisos, versiones y triggers. Limpia el workflow de prueba y continúa con [Lab 19](lab-19-javascript-custom-action.md).
