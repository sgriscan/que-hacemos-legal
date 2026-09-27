# ¿Qué hacemos? — textos legales

Política de Privacidad, Términos y Condiciones y Reglamento de Uso de la app ¿Qué hacemos?, en español (`es/`) e inglés (`en/`), publicados con GitHub Pages. La app los abre desde **Cuenta → Legal** con un navegador dentro de la app. También los enlaza desde el registro, desde el formulario de comentarios y desde el aviso de términos nuevos.

La versión en español es la que vale. Las páginas en inglés lo aclaran arriba.

## Direcciones

| Texto | Español | Inglés |
|---|---|---|
| Política de Privacidad | `/es/privacidad/` | `/en/privacidad/` |
| Términos y Condiciones | `/es/terminos/` | `/en/terminos/` |
| Reglamento de Uso | `/es/reglamento/` | `/en/reglamento/` |

La app arma estas direcciones desde `LEGAL_BASE_URL` (`project/src/config/legal.ts` del repo de la app). Si cambia el nombre de este repo o su dominio, hay que actualizar esa constante.

## Cómo publicar una versión nueva

El orden importa: **primero se publica el texto y después se registra la versión.** Así nunca se le pide a nadie que acepte un texto que todavía no puede leer.

1. **Editar los textos** en español y en inglés, y subir `version` y `vigente_desde` en el encabezado de **las seis páginas**. Las tres comparten número de versión.
2. **Publicar:** hacer push a `main` y esperar a que GitHub Pages termine de desplegar (pestaña *Actions*). Abrir las seis páginas y confirmar que muestran la versión nueva.
3. **Registrar la versión** en el SQL Editor de Supabase:

   ```sql
   insert into versiones_terminos (version, vigente_desde, requiere_aceptacion, resumen_es, resumen_en)
   values (2, '2026-12-01', true,
           'Resumen breve de lo que cambió, en español.',
           'Short summary of what changed, in English.');
   ```

   - `requiere_aceptacion = true` si el cambio es **relevante**. Las cuentas van a ver el aviso con el resumen y no van a poder guardar, comentar ni publicar hasta aceptar.
   - `requiere_aceptacion = false` si es una corrección menor, como una errata. Queda publicada con su fecha, sin aviso.

## Seguridad

`_layouts/default.html` declara una Content Security Policy por `<meta>` (`default-src 'self'`), porque GitHub Pages no permite configurar headers. Por eso el sitio **no usa tema de Jekyll** y los estilos van en `assets/estilo.css` y no inline: cualquier script, fuente o estilo externo quedaría bloqueado.

## Pendientes antes de dar los textos por finales

Los textos marcan en rojo (`<span class="pendiente">`) lo que falta completar o verificar:

- el responsable de los datos;
- la fecha de vigencia;
- la región de los servidores y la transferencia internacional;
- los plazos y la leyenda obligatoria de la Ley 25.326;
- la edad mínima;
- la jurisdicción;
- el plazo de borrado de las sesiones sin cuenta.

Antes de publicar la app en las tiendas, conviene que un abogado revise los textos finales.
