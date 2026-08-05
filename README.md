# postmarketOS para Samsung Galaxy S6 y Galaxy J5 2017

Este repositorio construye automáticamente imágenes de **postmarketOS** para
estos móviles y las deja listas para descargar:

| Móvil | Modelo | Nombre en postmarketOS |
|---|---|---|
| Samsung Galaxy S6 | SM-G920F | `samsung-zeroflte` |
| Samsung Galaxy J5 (2017) | SM-J530F/DS | `samsung-j5y17lte` |

> **Xiaomi Redmi 9A / 9AT: no se puede.** postmarketOS no tiene soporte para
> ese móvil. Su nombre en clave (`dandelion`) no existe en la lista de modelos
> de postmarketOS, así que no hay nada que construir. Haría falta que alguien
> creara el soporte desde cero, con el móvil delante, y eso no es algo que se
> pueda generar automáticamente.

No hace falta que instales nada en tu ordenador para *construirlas*: la
construcción se hace sola en los servidores de GitHub.

---

## 1. Dónde se descargan las imágenes

Cuando una construcción termina, los ficheros aparecen en dos sitios:

- **Releases** → https://github.com/Marci9998/Postmarketoss6/releases
  (la forma más fácil: se descarga desde el navegador). Hay una release por
  móvil, con el nombre del móvil en el título.
- **Actions** → pestaña *Actions*, entras en la construcción y abajo del todo,
  en *Artifacts*, hay un `.zip` con todo dentro.

### Ficheros que vas a encontrar

| Fichero | Para qué sirve |
|---|---|
| `<movil>-boot.img.xz` | El arranque (kernel + initramfs) |
| `<movil>-root.img.xz` | El sistema completo de postmarketOS |
| `SHA256SUMS.txt` | Para comprobar que la descarga no está corrupta |

Los ficheros vienen comprimidos en `.xz`. **Descomprímelos antes de usarlos**
(en Windows con [7-Zip](https://www.7-zip.org/), en Linux/Mac con
`xz -d fichero.img.xz`). Al descomprimir te quedan los `.img` de verdad.

Si algún fichero fuese muy grande, aparecerá partido en trozos con nombre
`...part-00`, `...part-01`. En ese caso hay que juntarlos antes:

```bash
cat *-root.img.xz.part-* > root.img.xz
```

---

## 2. Antes de tocar nada

> ⚠️ **Aviso importante**: instalar esto borra TODO lo que haya en el teléfono
> (fotos, WhatsApp, cuentas, todo). Haz copia de seguridad antes.
> Además, activa el contador KNOX de forma **permanente** y anula la
> garantía.
>
> Hazlo solo si el móvil es viejo y no te importa perder lo que hay dentro.

**Datos para entrar en postmarketOS una vez instalado:**

- **Usuario:** `usuario`
- **Contraseña / PIN:** `147147`

---

## 3. Instalar en los Samsung (Galaxy S6 y Galaxy J5 2017)

Los dos Samsung se instalan igual, con un programa gratuito llamado
**Heimdall**: https://glassechidna.com.au/heimdall/

1. **Activa el desbloqueo OEM en el móvil**
   Ajustes → Información del teléfono → toca 7 veces en "Número de compilación"
   → vuelve atrás → *Opciones de desarrollador* → activa **Desbloqueo de OEM**.

2. **Apaga el móvil y entra en modo descarga**
   Con el móvil apagado, mantén pulsados a la vez:
   **Bajar volumen + Inicio (botón central) + Encendido**.
   Aparecerá una pantalla de aviso: pulsa **Subir volumen** para continuar.

3. **Conecta el móvil al ordenador con el cable USB.**

4. **Instálalo** (en la terminal, dentro de la carpeta donde tengas los `.img`
   ya descomprimidos). Para el Galaxy S6:

   ```bash
   heimdall flash --BOOT samsung-zeroflte-boot.img --USERDATA samsung-zeroflte-root.img
   ```

   Para el Galaxy J5 2017:

   ```bash
   heimdall flash --BOOT samsung-j5y17lte-boot.img --USERDATA samsung-j5y17lte-root.img
   ```

5. Cuando termine, el móvil se reinicia solo. El primer arranque tarda varios
   minutos. Ten paciencia.

---

## 4. Volver a Android

Se puede volver a Android instalando el firmware original:

Con Odin o Heimdall, usando el firmware original de tu modelo, que puedes
buscar en [SamMobile](https://www.sammobile.com/). El contador KNOX ya no se
puede volver a poner a cero.

---

## 5. Qué esperar de postmarketOS

- **Sistema:** postmarketOS (canal `edge`), basado en Alpine Linux
- **Escritorio:** Phosh (la interfaz táctil de GNOME para móviles)

Estos móviles están en la categoría *community* / *testing* de postmarketOS:
pantalla, táctil y batería suelen funcionar, pero **las llamadas telefónicas y
los datos móviles normalmente no funcionan**, y algunas cosas (cámara,
bluetooth, aceleración gráfica) pueden fallar según el modelo.

Es un Linux de escritorio metido en el móvil, no un sustituto de Android para
usarlo como teléfono normal. Lo que funciona en cada uno, actualizado:

- https://wiki.postmarketos.org/wiki/Samsung_Galaxy_S6_(samsung-zeroflte)
- https://wiki.postmarketos.org/wiki/Samsung_Galaxy_J5_2017_(samsung-j5y17lte)

---

## 6. Volver a construir las imágenes

Pestaña **Actions** → *Construir imagenes de postmarketOS* → botón
**Run workflow**. Construye los dos móviles a la vez.
