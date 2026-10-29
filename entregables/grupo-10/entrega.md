# Entregable — TP PHP2Shell / Node2Shell

**Grupo:** 10 · **Fork del grupo:** benjaelizalde/goys-2026-tp-vulnerabilidades · **Rama:** `grupo-10`
**Fecha:** jueves 29/10/2026

| Integrante | Usuario de GitHub | Rol en el TP |
|---|---|---|
| Moscuzza, Vicente | VicenteMoscuzza | Inventario y Exposición |
| | | |
| | | |
| | | |

> Roles sugeridos: inventario, cambios/parche, detección, contención/comunicación.
> Todos tienen que poder explicar su parte y una parte ajena.

---

## 1. Inventario

| Activo | Versión | ¿Expuesto a Internet? | ¿Procesa contenido no confiable? | ¿Afectado? | Evidencia |
|--------|---------|:---------------------:|:--------------------------------:|:----------:|-----------|
| Portal PHP (`php-lab`) | PHP 8.3.35 NTS, Apache con mod_php (SAPI apache2handler) | No en el lab; sí asumido en producción | Sí (HTTP, puerto 80 interno) | Sí. PHP 8.3.35 es anterior al fix 8.3.36 (versions.env) y corre bajo Apache mod_php, el caso de mayor riesgo según recursos/contexto.md; no depende de la app. Explotación real confirmada, en CISA KEV desde el 26/10/2026 | `capturas/inventario.txt` (salida de `inventario.sh`), `capturas/inventario-php-apache.txt` (`php -v` y `apache2ctl -M` con `php_module (shared)`), `docker-compose.yml` (imagen `php:8.3.35-apache`) |
| API Node.js (`node-lab`) | Node v22.23.3 (bookworm-slim), Brotli 1.1.0 aportado por el runtime, sin paquete externo | No en el lab; sí asumido en producción | Sí (HTTP, puerto 3000 interno) | Sí. Node 22.23.3 es anterior al fix 22.23.4 y su Brotli 1.1.0 está en el rango afectado (1.0.3–1.2.0). El endpoint POST /decode (server.js, líneas 19-25) recibe un body con Content-Encoding br y lo pasa a zlib.brotliDecompressSync, o sea que el decoder procesa entrada no confiable. Explotación real sin confirmar al 27/10; RCE demostrado solo en laboratorio | capturas/inventario.txt, capturas/inventario-brotli.txt, laboratorio/versions.env, recursos/contexto.md, docker-compose.yml |
| Servicio Python (`py-lab`) | Python 3.11.18, paquete brotli 1.0.9 (fijado por `BROTLI_VERSION`) | No en el lab; sí asumido en producción | Sí (HTTP, puerto 8000 interno) | Sí. brotli 1.0.9 está en el rango afectado (1.0.3–1.2.0) y es anterior al fix 1.2.1. app.py (líneas 34-36) aplica brotli.decompress al body cuando Content-Encoding es br, o sea que el decoder procesa entrada no confiable. Explotación real sin confirmar al 27/10. | capturas/inventario.txt, capturas/inventario-brotli.txt, laboratorio/versions.env, recursos/contexto.md, docker-compose.yml |

**Supuestos de exposición:** en el laboratorio no hay `ports:` publicados y la red `labnet` es `internal: true`, por lo que ningún servicio es alcanzable desde Internet ni desde el host; todo se opera con `docker compose exec`. Para el análisis de riesgo se asume un despliegue real equivalente, con los tres servicios detrás de un proxy o balanceador que expone HTTP a Internet y reenvía contenido controlado por el usuario. El compose no incluye ningún proxy, así que ese supuesto no está verificado.

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

- [ ] El laboratorio arrancó y los 3 servicios quedaron `healthy`.
- [ ] Inventario con evidencia (versión, SAPI, configuración).
- [ ] Matriz con decisión y plazo.
- [ ] Parche aplicado y verificado; reversión documentada.
- [ ] Al menos 3 controles de contención probados.
- [ ] `deteccion.md` completo.
- [ ] `informe.md` individual completo.
- [ ] Sin claves, tokens ni placeholders.
- [ ] Repo pusheado y PR abierto.

Checklist completa: [`../docs/checklist-entrega.md`](../docs/checklist-entrega.md).
