# Switch-SD-Manager

La herramienta todo-en-uno para preparar, actualizar y reparar la tarjeta SD
de tu Nintendo Switch. Todo desde una única ventana. Sin línea de comandos,
sin archivos sueltos, sin complicaciones.

![Plataforma](https://img.shields.io/badge/Windows-10%20%7C%2011-blue)
![Versión](https://img.shields.io/badge/versi%C3%B3n-v1.0-green)

---

## ¿Qué puedes hacer con esta herramienta?

- **Instalar o actualizar el pack de homebrew** desde GitHub o desde un ZIP
  que tengas en el PC.
- **Instalar o actualizar el firmware** de la Switch desde GitHub o desde
  un ZIP local.
- **Hacer una copia de seguridad completa** de tu SD antes de tocar nada.
- **Restaurar esa copia** si algo sale mal y quieres volver al estado
  anterior.
- **Activar o desactivar los módulos** de Atmosphere uno a uno.
- **Limpiar la SD** borrando selectivamente lo que tú elijas, protegiendo
  tus partidas guardadas.
- **Verificar la integridad de la SD** para detectar errores de disco.
- **Expulsar la SD de forma segura** cuando termines, sin riesgo de perder
  datos.

Todo con barra de progreso, registro detallado en pantalla y avisos claros
en cada paso. Pensada para que la uses sin miedo.

---

## Instalación

1. Descarga **`SwitchSDManager.exe`** desde la sección
   [**Releases**](../../releases) de este repositorio.
2. Ponlo en una carpeta normal del PC (por ejemplo, en el Escritorio).
   **No** lo pongas en la propia SD.
3. Doble clic para ejecutar.

> ⚠️ Si tu antivirus lo bloquea, añádelo a la lista blanca. Es un falso
> positivo habitual en este tipo de herramientas. El programa no hace nada
> fuera de la SD que le indiques.

---

## Cómo se usa

1. **Conecta la SD** de la Switch al PC con un lector.
2. **Abre el programa**. Detecta automáticamente las unidades que parecen
   una SD de Switch.
3. **Elige tu SD** en el desplegable de arriba (o pulsa "Examinar..." si no
   la detecta sola).
4. **Elige el pack**: la última versión de GitHub o un ZIP local.
5. **Marca si quieres instalar/actualizar el firmware**.
6. **Elige el modo**: actualización segura o limpieza selectiva.
7. **Pulsa INICIAR PROCESO** y espera.

La barra de progreso te dice en todo momento qué está haciendo. El registro
de la parte inferior muestra cada paso con detalle.

---

## Lo que hace la herramienta, al detalle

### Detección inteligente de la SD

Al abrir el programa, se escanean todas las unidades conectadas (menos `C:\`)
y se puntúan según las carpetas típicas de una SD de Switch: `atmosphere`,
`bootloader`, `switch`, `nintendo`, `emuMMC`, `emutendo`.

Si la puntuación es suficiente, la unidad aparece en la lista. Tú solo tienes
que elegirla.

También puedes seleccionar manualmente la carpeta raíz con el botón
**Examinar...** si tu SD no fue detectada automáticamente.

---

### Pack de homebrew (Atmosphere, bootloader, etc.)

Dos formas de conseguirlo:

**Desde GitHub:** la herramienta consulta los últimos releases del pack
kh4rly/Pack-Basico y te deja elegir la versión que quieras. La más reciente
aparece primero.

**Desde un ZIP local:** si ya tienes un ZIP del pack en el PC, lo puede usar
directamente. Detecta automáticamente si hay ZIPs compatibles en la carpeta
del programa.

**Qué hace con el pack:**

- Extrae todo el contenido en un área temporal primero.
- Ignora archivos basura que a veces vienen en los ZIPs (carpetas de macOS,
  archivos del sistema de Windows, etc.).
- Si el ZIP trae una carpeta envoltorio innecesaria, la salta
  automáticamente.
- Copia solo archivos nuevos o modificados a la SD.
- Preserva las fechas de los archivos.

---

### Firmware

Igual que con el pack, dos formas: **desde GitHub** (la última versión de
THZoria/NX_Firmware) o **desde un ZIP local**.

**Comportamiento especial del firmware:** la carpeta `1_Firmware/` de la SD
se borra al inicio del proceso y se extrae el firmware nuevo encima. Si algo
falla durante la extracción, la carpeta se limpia por completo, no se queda
a medias. Cuando el proceso termina con éxito, te muestra las instrucciones
para instalar el firmware en la consola con Daybreak.

---

### Modos de instalación

**🟢 Modo Actualización (seguro)** — solo añade y sobrescribe archivos. No
borra nada. Es el modo recomendado en el 99% de los casos.

**🔴 Modo Limpio** — antes de copiar, te muestra una ventana con todo el
contenido de la SD para que marques qué quieres borrar. Por defecto aparece
todo marcado **excepto** las carpetas protegidas, así no puedes borrar tus
partidas por accidente. Puedes marcar o desmarcar cada cosa individualmente.

---

### Copia de seguridad (backup)

Antes de tocar nada, la herramienta puede hacer una **copia completa de tu
SD** en el PC. Está **activada por defecto**.

- Se guarda con fecha y hora (por ejemplo `SD_Backup_20260415_143022`).
- Se organiza por tarjeta: cada SD tiene su propia carpeta según su número
  de serie, así no se mezclan backups de distintas SD.
- Si cancelas a mitad, el backup se marca como incompleto y no aparece
  luego en la lista de restauración.
- Si ya no tienes espacio en el PC, te avisa antes de empezar.
- Puedes desactivar esta opción en el checkbox correspondiente.

Los backups se guardan **junto al programa** (o en tu carpeta de usuario si
ejecutas el programa desde la propia SD).

---

### Restaurar una copia de seguridad

Si algo salió mal y quieres volver al estado anterior:

1. Conecta la SD al PC.
2. Pulsa el botón **📂 Restaurar**.
3. Elige el backup de la lista (ordenados del más reciente al más antiguo).
4. Decide si quieres incluir las carpetas protegidas.
5. Confirma.

**Qué hace exactamente:**

- Copia los archivos del backup de vuelta a la SD.
- **No borra** archivos de la SD que no estén en el backup.
- Si un archivo ya es idéntico, lo omite (no pierde tiempo copiándolo).
- Si dos archivos tienen el mismo tamaño pero contenido distinto, los
  vuelve a copiar sin dudar (nada de "ya estaba, me lo salto").
- Por defecto **no** sobrescribe `Nintendo`, `emuMMC` ni `emutendo`, para
  que no pierdas tus partidas guardadas si has avanzado desde el backup.

---

### Gestor de módulos de Atmosphere

Una ventana dedicada para activar o desactivar los sysmodules de Atmosphere.

- Lista todos los módulos detectados en `atmosphere/contents/`.
- Los muestra separados en **ACTIVADOS** y **DESACTIVADOS**.
- Marca o desmarca los que quieras, pulsa **Aplicar cambios** y listo.
- Los cambios se aplican creando o borrando el archivo `boot2.flag` que
  Atmosphere usa para saber si un módulo arranca o no.
- Requiere reiniciar la consola para que los cambios surtan efecto.

También tiene un botón **🔍 Diagnóstico** que te muestra exactamente qué
carpetas hay en `atmosphere/contents/` y por qué se aceptan o se descartan
como módulos. Útil si algo no aparece como esperabas.

---

### Verificación de integridad (CHKDSK)

Si ejecutas el programa **como administrador**, puede lanzar `chkdsk` sobre
la SD para verificar que no tiene errores de disco.

- Muestra el progreso en tiempo real.
- Vuelca toda la salida al registro.
- Si detecta anomalías, te pregunta si quieres continuar.
- Puedes cancelarlo en cualquier momento.

Si no ejecutas como admin, este paso se omite automáticamente (no es
crítico). Puedes activar el modo admin desde el propio programa con el
botón **🛡️ Reiniciar como Admin**.

---

### Expulsión segura

Al terminar, el programa intenta **expulsar la SD de forma segura** para
que puedas retirarla sin riesgo de perder datos.

- Está **activada por defecto** (checkbox en el paso 4).
- Solo se ejecuta si el proceso terminó correctamente.
- Usa dos métodos distintos por si uno falla.
- Si Windows tarda en liberar la unidad (antivirus, Explorador, etc.),
  espera unos segundos y reintenta automáticamente.
- Si aún así no puede, te lo dice y puedes usar el botón **🔌 Expulsar**
  manualmente cuando quieras.

También puedes expulsar la SD manualmente en cualquier momento con ese
mismo botón, sin necesidad de haber procesado nada.

---

### Verificación de cada archivo copiado

Opción **🔍 Verificar CRC** (desactivada por defecto).

Cuando está activada, después de copiar cada archivo lo vuelve a leer y
compara su contenido byte a byte con el original. Si algo no coincide, te
lo avisa y lo marca como fallido.

Es más lento (lee cada archivo dos veces), pero útil si sospechas que tu SD
tiene sectores defectuosos.

---

### Confirmación antes de cada operación

Antes de empezar cualquier proceso, el programa te muestra un resumen
completo con:

- La SD seleccionada (con su etiqueta y tamaño).
- El pack que se va a instalar.
- Si se instalará firmware o no.
- El modo elegido.
- Si se hará backup y si se expulsará al terminar.
- Si se verificará con CRC.

Tú confirmas, y solo entonces empieza. Nada se ejecuta a ciegas.

---

## Protecciones que te cuidan

Estas cosas no las tienes que pensar tú: el programa las aplica solo.

- **Tus partidas guardadas están a salvo.** Las carpetas `Nintendo`,
  `emuMMC` y `emutendo` nunca se tocan, salvo que tú lo autorices
  explícitamente en el diálogo de restauración.
- **La SD no se borra entera nunca.** Ni siquiera en modo limpio. Solo se
  borra lo que marques tú.
- **Backup automático antes de tocar nada.** Activado por defecto.
- **Un ZIP malicioso no puede hacerte daño.** Se rechazan ZIPs con rutas
  peligrosas o "bombas" que expandirían a tamaños absurdos.
- **No se puede apuntar al disco del sistema.** Si intentas seleccionar
  `C:\` o una carpeta del sistema, el programa lo bloquea.
- **Backups de cada SD por separado.** No se mezclan tus backups de la SD
  principal con los de una SD de repuesto.
- **Backups incompletos ignorados.** Si cancelas un backup a mitad, ese
  backup no aparece luego en la restauración. Nada de restaurar a medias.
- **Confirmación antes de tocar nada peligroso.** Nunca hay sorpresas.

---

## Preguntas frecuentes

**¿Modifica algo de mi PC?**
No. Solo escribe en la SD que le indiques y en la carpeta donde guarda los
backups. Nada más.

**¿Necesito permisos de administrador?**
No, salvo que quieras que se ejecute CHKDSK. Todo lo demás funciona
perfectamente sin admin.

**¿Funciona sin internet?**
Sí. Si usas ZIPs locales para el pack y el firmware, funciona 100% offline.

**¿Qué pasa si cancelo a mitad?**
Depende del momento:
- **Backup**: queda un backup marcado como incompleto, que se ignora en la
  restauración. Puedes borrarlo a mano sin miedo.
- **Pack**: los archivos ya copiados se quedan. Puedes volver a lanzar el
  proceso cuando quieras: solo copiará lo que falta.
- **Firmware**: la carpeta `1_Firmware/` se limpia. Se reintenta desde cero.

**¿Puedo usarla con varias SDs?**
Sí. Cada una tiene su propia carpeta de backups según su número de serie.
Cuando conectes una SD, el programa te mostrará solo sus backups, no los de
las otras.

**¿Qué pasa si reformateé la SD?**
Al reformatear, la SD recibe un número de serie nuevo. Los backups antiguos
quedan en la carpeta del número anterior. Basta con moverlos a la carpeta
nueva (el programa te dice cuál es en el registro).

**¿Por qué tarda unos segundos en abrir?**
Es normal la primera vez: el ejecutable se descomprime en un directorio
temporal. A partir de la segunda vez, abre mucho más rápido.

**¿Por qué el antivirus me lo bloquea?**
Falso positivo habitual en ejecutables generados así. Añade una exclusión
y listo.

**¿Es peligroso para mi Switch?**
No. La herramienta solo prepara la SD desde el PC. No toca la consola
directamente, no instala CFW ni modifica el firmware por sí sola. La
instalación del firmware se hace después, desde la propia Switch, siguiendo
las instrucciones que el programa te muestra al terminar.

---

## Créditos

- **Pack:** [kh4rly/Pack-Basico](https://github.com/kh4rly/Pack-Basico)
- **Firmware:** [THZoria/NX_Firmware](https://github.com/THZoria/NX_Firmware)
- **Autor:** N3DSwitchOS · [Canal @N3DSwitch](https://youtube.com/@N3DSwitch)
- **Tutoriales:**
  - [YouTube](https://youtu.be/fhc4cNYLzsw)
  - [TikTok](https://vm.tiktok.com/ZGJspQAPB/)

---

## Licencia

MIT.
