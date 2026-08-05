# postmarketOS para el Samsung Galaxy S6

Este repositorio construye automáticamente una imagen de **postmarketOS** para el
**Samsung Galaxy S6** y la deja lista para descargar.

No hace falta que instales nada en tu ordenador para *construirla*: la construcción
se hace sola en los servidores de GitHub.

---

## 1. Dónde se descarga la imagen

Cada vez que la construcción termina, los ficheros aparecen en dos sitios:

- **Releases** del repositorio → https://github.com/Marci9998/Postmarketoss6/releases
  (esta es la forma más fácil, se descarga desde el navegador)
- **Actions** → pestaña *Actions*, entras en la construcción y abajo del todo,
  en *Artifacts*, hay un `.zip` con todo dentro.

### Ficheros que vas a encontrar

| Fichero | Para qué sirve |
|---|---|
| `samsung-zeroflte-boot.img.xz` | El arranque (kernel + initramfs) |
| `samsung-zeroflte-root.img.xz` | El sistema completo de postmarketOS |
| `SHA256SUMS.txt` | Sirve para comprobar que la descarga no está corrupta |

Los ficheros vienen comprimidos en `.xz`. **Descomprímelos antes de usarlos**
(en Windows con [7-Zip](https://www.7-zip.org/), en Linux/Mac con `xz -d fichero.img.xz`).
Al descomprimir te quedan los `.img` de verdad.

Si algún fichero fuese muy grande, aparecerá partido en trozos con nombre
`...part-00`, `...part-01`. En ese caso hay que juntarlos antes:

```bash
cat samsung-zeroflte-root.img.xz.part-* > samsung-zeroflte-root.img.xz
```

---

## 2. Cómo instalarlo en el móvil

> ⚠️ **Aviso importante**: esto borra TODO lo que haya en el teléfono (fotos,
> WhatsApp, cuentas, todo). Haz copia de seguridad antes. El proceso también
> anula la garantía y activa el contador KNOX de Samsung de forma permanente.
> Hazlo solo si el móvil es viejo y no te importa perder lo que hay dentro.

### Lo que necesitas

- El Samsung Galaxy S6 (modelo internacional **SM-G920F**)
- Un cable USB
- Un ordenador con **Heimdall** instalado (es un programa gratuito para
  instalar cosas en móviles Samsung): https://glassechidna.com.au/heimdall/

### Pasos

1. **Activa el desbloqueo OEM en el móvil**
   Ajustes → Información del teléfono → toca 7 veces en "Número de compilación"
   → vuelve atrás → *Opciones de desarrollador* → activa **Desbloqueo de OEM**.

2. **Apaga el móvil y entra en modo descarga**
   Con el móvil apagado, mantén pulsados a la vez:
   **Bajar volumen + Inicio (botón central) + Encendido**.
   Aparecerá una pantalla de aviso: pulsa **Subir volumen** para continuar.
   Ya estás en "modo descarga" (Download mode).

3. **Conecta el móvil al ordenador con el cable USB.**

4. **Instala postmarketOS** (en la terminal del ordenador, dentro de la carpeta
   donde tengas los `.img` ya descomprimidos):

   ```bash
   heimdall flash --BOOT samsung-zeroflte-boot.img --USERDATA samsung-zeroflte-root.img
   ```

5. Cuando termine, el móvil se reinicia solo y arranca postmarketOS.
   El primer arranque tarda un rato largo (varios minutos). Ten paciencia.

### Datos para entrar

- **Usuario:** `usuario`
- **Contraseña / PIN:** `147147`

---

## 3. Volver a Android

Si te arrepientes, se puede volver a Android instalando el firmware original
de Samsung con Odin o Heimdall (busca el firmware de tu modelo en
[SamMobile](https://www.sammobile.com/)). El contador KNOX, eso sí, ya no se
puede volver a poner a cero.

---

## 4. Qué lleva la imagen

- **Sistema:** postmarketOS (canal `edge`), basado en Alpine Linux
- **Escritorio:** Phosh (la interfaz táctil de GNOME para móviles)
- **Dispositivo:** `samsung-zeroflte` (Galaxy S6 SM-G920F, Exynos 7420)

El Galaxy S6 está en la categoría *community* de postmarketOS: pantalla, táctil,
wifi y batería funcionan, pero **la llamada telefónica y los datos móviles no
funcionan**. Es un Linux de escritorio en el móvil, no un sustituto de Android
para usar como teléfono normal.

Lista de lo que funciona y lo que no, actualizada:
https://wiki.postmarketos.org/wiki/Samsung_Galaxy_S6_(samsung-zeroflte)

---

## 5. Construir otra variante

En la pestaña **Actions** → *Construir imagen postmarketOS (Samsung Galaxy S6)* →
botón **Run workflow**, se puede elegir:

- `device`: `samsung-zeroflte` (S6 normal) o `samsung-zerolte` (S6 Edge)
- `ui`: `phosh`, `plasma-mobile`, `sxmo` o `none` (solo consola)
