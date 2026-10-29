# Entregable — TP PHP2Shell / Node2Shell

**Grupo:** 02 · **Fork del grupo:** https://github.com/FrancoPetosa99/goys-2026-tp-vulnerabilidades · **Rama:** `grupo-02`
**Fecha:** jueves 29/10/2026

| Integrante | Usuario de GitHub | Rol en el TP |
|---|---|---|
| Eliseche, Martín | MartinEliseche | Inventario y arranque del laboratorio |
| Petosa Ayala, Franco | FrancoPetosa99 | Cambios y parche |
| Gutierrez Mora, Agustín | agugutierrezz | Detección |
| Bresciani, Isabella | IsabellaBresciani | Contención |
| Kalpin, Sofía | Sokalpin | Priorización y comunicación |

---

## 1. Inventario

| Activo | Versión | ¿Expuesto a Internet? | ¿Procesa contenido no confiable? | ¿Afectado? | Evidencia |
|--------|---------|:---------------------:|:--------------------------------:|:----------:|-----------|
| Portal PHP | PHP 8.3.35, imagen `php:8.3.35-apache` (Apache + mod_php) | Sí (supuesto: publicado detrás de un proxy) | Sí (requests HTTP y subida de archivos) | **Sí**: PHP2Shell (corregido en 8.3.36) | `inventario.sh`: `PHP 8.3.35` y `session.upload_progress.enabled=1`; `docker ps`: imagen `-apache` |
| API Node.js | Node v22.23.3, Brotli 1.1.0 embebido en el runtime, Express 5 | Sí (supuesto: publicada detrás de un proxy) | Sí (`POST /decode` acepta cuerpos `Content-Encoding: br`) | **Sí**: Node2Shell (corregido en 22.23.4) | `inventario.sh`: `v22.23.3 / brotli 1.1.0` y "sin paquete brotli externo: lo aporta el runtime" |
| Servicio Python | Python 3.11.18, paquete brotli 1.0.9 (PyPI) | No (solo red interna) | Indirectamente (cuerpos `br` que le envían otros servicios) | **Sí**: Node2Shell / Brotli (corregido en 1.0.10, 1.1.1 o 1.2.1) | `inventario.sh`: `python 3.11.18` y `brotli 1.0.9` |

Salidas completas en `capturas/01-compose-ps.txt` y `capturas/02-inventario.txt`.

**Notas sobre la evidencia:**

- El `(cli)` que muestra `php -v` corresponde a la línea de comandos dentro del
  contenedor, no al portal web. Que el portal corra con Apache + mod_php se infiere de
  la imagen `php:8.3.35-apache`; el árbol de procesos con `apache2` está en
  `capturas/03-detectar.txt`.
- Node trae Brotli **dentro del runtime** (`node:zlib`), por eso no aparece en
  `package.json` ni en `npm ls`. Un inventario basado solo en el manifiesto de
  dependencias habría concluido que Node no usa Brotli, y sería un error.

**Supuestos de exposición:** en el laboratorio ningún contenedor publica puertos al
host (`docker ps` solo muestra `80/tcp`, `3000/tcp` y `8000/tcp`) y la red `labnet` es
interna, así que literalmente nada está expuesto a Internet. Asumimos que en
producción el portal PHP y la API Node están publicados detrás de un reverse proxy, y
que el servicio Python solo es accesible desde la red interna. Si el proxy
descomprimiera Brotli antes de reenviar, el decoder vulnerable también estaría en el
proxy.

---

## 2. Priorización y decisión

| Activo | CVSS | Explotación real | Exposición | Criticidad | Decisión y plazo |
|--------|:----:|:----------------:|:----------:|:----------:|------------------|
| | | | | | |
| | | | | | |
| | | | | | |

**Justificación:** _¿por qué ese orden? ¿qué evidencia nueva cambiaría la decisión?_

---

## 3. Mitigación, parche y reversión

| Fecha/hora | Activo | Cambio | Antes | Después | Reversión |
|------------|--------|--------|-------|---------|-----------|
| | | | | | |
| | | | | | |

**Prueba funcional mínima:** _¿qué verificaron para saber que la app sigue viva?_

**Qué no se pudo probar:** _declararlo explícitamente._

---

## 4. Detección y caza

El análisis completo está en [`deteccion.md`](deteccion.md). Resumen:

**Hipótesis:** _una frase: proceso, padre, horario, evidencia esperada._

**Hallazgos en `audit.log`:**

| Línea / timestamp | Host | Padre → hijo | ¿Anómalo? | Por qué |
|-------------------|------|--------------|:---------:|---------|
| | | | | |
| | | | | |

**¿Hay compromiso?** _sí/no/indeterminado, con evidencia._

**Regla propuesta para Node.js:** _qué proceso observa, qué condición y qué falso
negativo podría tener._

**Limitaciones del log:** _usuario, IP de origen, retención, ruido, etc._

---

## 5. Contención

| Control | Configuración | Evidencia | ¿Funciona? |
|---------|---------------|-----------|:----------:|
| Red sin salida | `internal: true` | | |
| Usuario sin privilegios | | | |
| Filesystem read-only | | | |
| Capabilities mínimas | | | |
| Límites de memoria/PIDs | | | |
| Rechazo de Brotli innecesario | | | |

**Prueba de salida a Internet:** _resultado en los tres servicios._

---

## 6. Conclusión (5 líneas)

_¿Qué harían distinto en las primeras 2 horas?_

---

## 7. Checklist antes de entregar

- [x] El laboratorio arrancó y los 3 servicios quedaron `healthy`.
- [x] Inventario con evidencia (versión, SAPI, configuración).
- [ ] Matriz con decisión y plazo.
- [ ] Parche aplicado y verificado; reversión documentada.
- [ ] Al menos 3 controles de contención probados.
- [ ] `deteccion.md` completo.
- [ ] `informe.md` individual completo.
- [ ] Sin claves, tokens ni placeholders.
- [ ] Repo pusheado y PR abierto.

Checklist completa: [`../docs/checklist-entrega.md`](../docs/checklist-entrega.md).