# CLAUDE.md — Panel Contable JS-TECH (LÉEME PRIMERO)

> **IA nueva: lee este fichero ENTERO antes de hacer nada.** Es el contexto del proyecto.
> Al terminar algo importante, ACTUALÍZALO y avisa a Joan de commitear.

## Qué es este proyecto
**Frontend del panel contable de JS Technology Menorca SL**, la interfaz que usa **Raquel** (la contable).
Separado el 2026-06-09 del repo `averias-jstech` (donde estaba mezclado con la app de averías) para que
sea un producto independiente.

- Repo: `joanserramiret/panel-contable` (rama main). Git lo hace **Joan con GitHub Desktop**, NUNCA desde bash.
- Publicado en **GitHub Pages**: `https://joanserramiret.github.io/panel-contable/contable.html`
- Ficheros:
  - **`contable.html`** — el panel principal (≈3.000 líneas, 574 KB). Edítalo con Read/Edit puntual, no lo reescribas entero.
  - `portal_facturas.html` — portal de facturas.
  - `guia_contable.html` — guía de uso.
  - `plantilla_documento.html` — plantilla de presupuestos/facturas en PDF.
  - `cabecera_factura.png` — cabecera (se referencia por URL absoluta a github.io/panel-contable/).

## Backend (NO está aquí)
El panel habla con un **bridge Flask** que vive en **otro repo**: `C:\agora-bridge\bridge.py`
(repo `joanserramiret/agora-bridge`, Python 3.8 en el PC de Joan), expuesto por **ngrok**:
`https://uncooked-sprung-unpinned.ngrok-free.dev` → constante `BRIDGE_URL` en `contable.html`.
El bridge es **infraestructura compartida** (sirve contabilidad + averías + TPV); su briefing está en
`C:\agora-bridge\CLAUDE.md`. Si el cambio es de backend (endpoints, banco, proveedores, conciliación),
se toca ALLÍ, no aquí.

## Cómo trabajar (GOTCHAS)
- **El git lo hace Joan** con GitHub Desktop. Recuérdaselo tras editar. Nunca git/commit desde bash.
- **Cuidado con corrupción de mount** (bytes nulos / truncado al escribir desde el sandbox). Verifica
  SIEMPRE con Grep/Read del host tras escribir.
- **ngrok cachea**: añade `&nocache=<n>` al probar endpoints. Para recargar el HTML: Ctrl+Shift+R.
- Tras editar el bridge (otro repo), Joan debe **reiniciar el bridge** (Ctrl+C + `arrancar_bridge.bat`).

## ★ NORTE DEL PROYECTO (Joan, 2026-06-09)
1. **Empezar LIMPIOS el 2026.** Corte de periodo = 2026 + SOLO diciembre 2025 como frontera (para
   facturas de dic-25 cuyo cargo cae en ene-26). El panel **cuenta** dic-2025 en el cálculo para que
   los saldos cuadren, pero **NO lo lista** (Raquel solo ve 2026 limpio, sin filas de 2025). Nada de histórico.
2. **El panel = interface de uso directo para Raquel.** Debe DECIRLE qué falta y qué cuadra, no que ella
   (ni Joan, ni la IA) vaya proveedor por proveedor, banco por banco, cazando. Visión: vista **SEMÁFORO**
   por proveedor/cliente — 🟢 cuadrado (facturado=pagado), 🟡 falta factura (y dice CUÁNTO y de QUÉ cargo,
   ej. "CONTAMO: falta junio 115,63, cargo 01/06"), 🔴 descuadre real. Fontanería ya existe en el bridge
   (`/contabilidad/proveedores`, `/contabilidad/clientes`, `/banco/auditoria_prov`, tabla-puente
   `reclasif_manual`); falta la CAPA que lo presenta como semáforo accionable + fijar el corte de periodo.

## Quién es quién
- **Joan Serra Miret** (jsm@js-tech.es) — el usuario. Empresa JS TECHNOLOGY MENORCA SL (NIF B57841140), Maó.
  Trato directo y al grano. Sin ventanas de selección (AskUserQuestion) — preguntar por escrito en el chat.
- **Raquel** — la contable (Barcelona). Usuaria final del panel. Clasifica facturas en Dropbox y las mete
  en SisConta. NO tocar sus carpetas del Dropbox.

## Series conciliables (criterio del panel)
Factura de cliente = series **F, FD, APPF, APPFD** (APPF/APPFD = emitidas desde app/TPV). Las **T/TD**
(tickets y devoluciones de ticket) NO entran en conciliaciones. Pares factura+abono (FD↔F, APPFD↔APPF)
del mismo cliente, día e importe se **anulan** entre sí (rectificaciones que se matan) y no cuentan.
(Implementado en el bridge: helper `_anular_pares`, `_SERIES_CONCILIABLES`.)

## Despliegue (recordatorio para Joan)
1. Commit + push de los .html con GitHub Desktop (repo `joanserramiret/panel-contable`, rama main).
2. Activar **GitHub Pages** del repo (Settings → Pages → rama main) la primera vez.
3. En el navegador: Ctrl+Shift+R para saltar la caché.
4. URL para Raquel: `https://joanserramiret.github.io/panel-contable/contable.html`

## Pendiente / estado
- Traslado desde averias-jstech hecho 2026-06-09 (5 ficheros + URLs repuntadas a panel-contable).
- FALTA (Joan): commit/push del nuevo repo + activar GitHub Pages; luego BORRAR los 5 ficheros contables
  del repo `averias-jstech` para que quede solo de averías.
- FALTA (desarrollo): corte de periodo dic-2025 en el backend + vista semáforo para Raquel.
