# Tessera — caja fuerte cifrada + limpieza de PC para Windows

[English](README.md) · [简体中文](README.zh-CN.md) · [Français](README.fr.md) · **Español** · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Descargar](https://img.shields.io/badge/descargar-%C3%BAltima%20versi%C3%B3n-brightgreen.svg)](../../releases/latest)
[![Plataforma](https://img.shields.io/badge/plataforma-Windows%2010%2F11%20x64-lightgrey.svg)]()
[![Android](https://img.shields.io/badge/android-aplicaci%C3%B3n%20complementaria-green.svg)](../../releases/latest)
[![Idiomas](https://img.shields.io/badge/idiomas-6-blue.svg)]()

**Tus archivos, contraseñas, portapapeles y notas tras un único desbloqueo — más un conjunto
de limpieza que mantiene en orden la máquina que hay debajo.** Nada se sube, nada «llama a
casa», y nada sale de tu ordenador salvo que tú lo configures explícitamente.

Gratis. Sin conexión primero. Windows 10/11 de 64 bits, con una aplicación complementaria de
Android para portapapeles y archivos.

## Descarga

**→ [Obtener la última versión](../../releases/latest)**

| Archivo | Para quién |
| --- | --- |
| `Tessera-Setup.exe` | Instalador de Windows — eliges la ubicación y queda entrada de desinstalación. **La mayoría quiere este.** |
| `Tessera.exe` | Portátil. Funciona desde cualquier sitio, incluida una memoria USB, y no toca el registro. |
| `Tessera-CrossDevice-<versión>.apk` | Móvil o tableta Android (portapapeles, archivos, sincronización). |

Una vez instalado no necesitas volver aquí: la aplicación encuentra las nuevas versiones sola.

## Qué obtienes

### Una caja fuerte cifrada

- **Archivos** — cifra cualquier cosa en un único `.ivault`. Poscuántico por defecto
  (AES-256-GCM para el contenido, con la clave de cada archivo envuelta además por
  ML-KEM-1024), o AES-256-GCM clásico / ChaCha20-Poly1305. Los nombres de archivo y las sumas
  de verificación viven *dentro* de la cabecera cifrada: un contenedor robado no revela
  ninguno de los dos.
- **Contraseñas** — gestor completo, importación desde un CSV de Chrome/Edge/Firefox, y el CSV
  en texto plano se elimina de forma segura después.
- **Historial del portapapeles** — clasificado automáticamente (URL / correo / teléfono /
  fórmula / código / texto), anclado, búsqueda, lista negra por aplicación y un panel global
  `Ctrl+Mayús+V`.
- **Notas** — Markdown con imágenes, categorías anidadas y búsqueda de texto completo. Cada
  nota te devuelve al punto donde dejaste de leer. Una nota de 4 MB muestra su primera
  pantalla en unos 40 ms.
- **Cinco formas de desbloquear, con una basta** — contraseña · código por correo ·
  autenticador (TOTP) · Windows Hello (rostro / huella / PIN) · clave de recuperación de un
  solo uso.

### Un conjunto de limpieza que dice la verdad

- Archivos basura, cachés de navegadores, rastros de privacidad, restos de controladores,
  archivos duplicados y grandes, adelgazamiento del disco C:, gestor de inicio, menú
  contextual, bloqueo de ventanas emergentes y 42 utilidades pequeñas.
- **Tres profundidades de análisis en la revisión de un clic** — *Análisis rápido* (las 4
  categorías más veloces, unos 2 s), *Revisión estándar* (10 categorías, ~15 s), *Análisis
  profundo* (las mismas 10, pero el adelgazamiento del disco consulta a DISM sobre el almacén
  de componentes: más lento y exacto en lugar de estimado). **21 escáneres en total**; los otros
  11 —duplicados, archivos grandes, espacio por carpeta, software instalado, menú contextual,
  datos del navegador, controladores sustituidos y demás— viven en sus propios paneles, así que
  una revisión nunca te obliga a esperar un barrido completo del disco que no pediste.
- **Nada irreversible se marca por ti.** Lo que la aplicación no puede recuperar *y* cuesta un
  esfuerzo real reconstruir aparece y se puede seleccionar, pero nunca viene preseleccionado:
  el camino de un clic tiene que ser el camino seguro.
- **Una simulación recorre la ruta de código real sin tocar un solo byte** e informa de lo que
  ganaría realmente cada unidad.
- **Cada categoría dice qué tipo de «vacío» encontró** — realmente limpio, bloqueado por
  permisos, o que nunca llegó a ejecutarse — y cuántas ubicaciones comprobó para saberlo. «No
  se encontró nada» y «nunca se ejecutó» no tienen permitido parecerse.
- **«Liberado» significa liberado.** Los archivos enviados a la papelera se cuentan *aparte*
  del espacio realmente recuperado, y la aplicación vuelve a medir la unidad después para que
  puedas contrastar su aritmética con lo que informa Windows.
- Cinco juicios sobre cada elemento — *Windows lo necesita · un controlador · algo que usas ·
  opcional · publicidad* — con el motivo escrito. Lo que Windows necesita queda bloqueado, y el
  backend lo rechaza aunque se lo pidan.
- Las claves del registro, entradas de inicio y controladores de menú se **desactivan con copia
  de seguridad**, nunca se borran. Si la exportación de la copia falla, no se cambia nada.

### Móvil ↔ PC, por tu propia red local

Copia en el móvil, pega en el PC, y al revés: texto, texto enriquecido, código, fórmulas
matemáticas e imágenes conservan su formato. Envía archivos o carpetas enteras, con reanudación
y cifrado de extremo a extremo. Emparejamiento con código de seis dígitos, código QR, enlace o
desde una lista de dispositivos cercanos. El tráfico se queda en tu red local.

### Interfaz

Un único lenguaje de diseño en todas las pantallas. Ocho colores de acento, claro/oscuro/sistema,
modo Simple o Profesional, y un modo de confort visual que desplaza la temperatura de color
**sin** tocar los colores de estado: la comodidad no debe costarte la capacidad de ver qué falló.

**Seis idiomas** — English, 简体中文, Français, Español, Русский, العربية — conmutables en
cualquier parte, incluida la pantalla de inicio de sesión.

## ¿Buscas una alternativa a…?

Tessera cubre en una sola aplicación sin conexión lo que normalmente requiere tres o cuatro programas:

- **CCleaner / BleachBit / Wise Disk Cleaner** — archivos basura, datos del navegador, restos
  del registro, gestor de inicio, duplicados, adelgazamiento del disco C:. Cada elemento explica
  qué es y qué pierdes al borrarlo, y una simulación te enseña antes lo que pasaría.
- **KeePass / Bitwarden / 1Password** — gestor de contraseñas local con TOTP incorporado.
  Sin cuenta, sin servidor, sin suscripción.
- **VeraCrypt / 7-Zip AES** — cifrado de archivos y carpetas, poscuántico por defecto.
- **Ditto / ClipClip** — historial del portapapeles con búsqueda, fijado y atajo global.
- **Obsidian / notas rápidas** — notas Markdown con imágenes y búsqueda de texto completo, cifradas en disco.
- **AirDroid / LocalSend** — sincronización de portapapeles y archivos solo por red local con Android.

Gratis y de código abierto (AGPL-3.0), Windows 10/11 de 64 bits.

## Al instalar: qué te preguntarán Windows y Android

**Windows.** Las compilaciones están firmadas con un certificado autofirmado, por lo que
SmartScreen puede avisar («Windows protegió su PC»). Pulsa **Más información → Ejecutar de todas
formas**. Ese aviso trata de que el certificado no se compró a una autoridad comercial, no de
que el archivo esté alterado.

Las actualizaciones dentro de la aplicación usan la misma clave de firma: tras descargar, la
aplicación comprueba que la firma sea *esta* clave y **borra el archivo en lugar de instalarlo**
si no lo es. No existe ninguna vía de «continuar igualmente».

**Android.** La primera actualización interna te pedirá permitir la instalación de aplicaciones
desconocidas para Tessera. Android nunca permite que una aplicación instalada de forma lateral
instale nada en silencio: esa confirmación final siempre es del sistema. Las actualizaciones
exigen una firma idéntica, impuesta por el propio Android, así que nadie más puede publicar una
compilación que suplante a esta aplicación.

## Verifica lo que has descargado

GitHub muestra un SHA-256 para cada archivo en la página de la versión. Las actualizaciones
internas lo comparan automáticamente; si has descargado a mano:

```powershell
Get-FileHash .\Tessera-Setup.exe -Algorithm SHA256
```

```bash
sha256sum Tessera-CrossDevice-*.apk
```

También puedes hacer clic derecho en el exe → **Propiedades → Firmas digitales**.

## Requisitos del sistema

Windows 10 u 11, 64 bits. Varias funciones dependen de API de Windows (Windows Hello,
integración con el shell, atajos globales). Aplicación complementaria de Android: Android 8.0 o
posterior.

## Preguntas

**¿El código fuente es público?** Por ahora no. Este repositorio solo contiene las compilaciones
publicadas.

**¿«Llama a casa»?** No. La única petición saliente que la aplicación hace por sí misma es la
comprobación de actualizaciones contra este repositorio, y puedes pasarla a manual.

**¿Dónde viven mis datos?** En una base de datos cifrada, en una carpeta que eliges durante la
configuración. Las actualizaciones nunca la tocan.

**He perdido mi contraseña.** Usa la clave de recuperación de un solo uso de la configuración.
Sin ella, y sin ninguno de los otros cuatro métodos de desbloqueo, la caja fuerte no puede
abrirse: para eso está.

## Licencia

AGPL-3.0, con licencia comercial también disponible. Los términos completos se distribuyen con
la aplicación y son visibles en su página Acerca de.
