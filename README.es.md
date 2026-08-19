# Tessera

[English](README.md) · [简体中文](README.zh-CN.md) · [Français](README.fr.md) · **Español** · [Русский](README.ru.md) · [العربية](README.ar.md)

[![Licence: AGPL-3.0](https://img.shields.io/badge/licence-AGPL--3.0-blue.svg)](LICENSE) [![Commercial licence available](https://img.shields.io/badge/commercial%20licence-available-brightgreen.svg)](LICENSE-COMMERCIAL.md) [![Platform: Windows](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-lightgrey.svg)]()

**Tessera** — una caja fuerte cifrada multifactor para Windows que además
incluye gestor de contraseñas, historial de portapapeles, notas, un asistente de IA y un
gestor de archivos rápido. Todo vive detrás de un único desbloqueo; nada sale de tu
equipo salvo que lo configures explícitamente.

> Estado: `0.1.18`, en desarrollo activo. Solo Windows 10/11 x64 — varias funciones
> (Windows Hello, integración con el explorador, atajos globales de archivos) dependen
> directamente de las API de Windows.

---

## Contenido

- [Qué incluye](#qué-incluye)
- [Primeros pasos](#primeros-pasos)
- [Primer inicio: el asistente de configuración](#primer-inicio-el-asistente-de-configuración)
- [Configurar el inicio de sesión por código de correo](#configurar-el-inicio-de-sesión-por-código-de-correo)
- [Configurar la aplicación de autenticación](#configurar-la-aplicación-de-autenticación)
- [Configurar el desbloqueo biométrico](#configurar-el-desbloqueo-biométrico)
- [Clave de recuperación — léelo bien](#clave-de-recuperación--léelo-bien)
- [Uso diario](#uso-diario)
- [Sincronización multidispositivo](#sincronización-multidispositivo)
- [Modelo de seguridad](#modelo-de-seguridad)
- [Dónde viven tus datos](#dónde-viven-tus-datos)
- [Desarrollo](#desarrollo)
- [Compilar una versión](#compilar-una-versión)
- [Licencia](#licencia)

---

## Qué incluye

| Función | Qué hace |
|---|---|
| **Cifrado de archivos** | Cifra/descifra cualquier archivo en un único archivo `.ivault`. Tres perfiles: **poscuántico** (por defecto: AES-256-GCM para el contenido, con la clave de cada archivo envuelta además por ML-KEM-1024, de modo que hay que romper ambas construcciones), AES-256-GCM clásico o ChaCha20-Poly1305. El perfil se aplica igual a una selección múltiple de archivos y a una carpeta completa. Selección por lotes de muchos archivos a la vez. Los nombres de archivo y las sumas de verificación viven en una cabecera *cifrada*, así que un `.ivault` robado no revela ninguno de los dos. El descifrado tiene su propia entrada para carpetas, junto a la de archivos, y abrir una carpeta cifrada muestra lo que contiene sin descifrar antes el contenedor: el listado solo lee los pocos cientos de KB donde está el índice del archivo comprimido, así que una carpeta de 10 GB se abre tan rápido como una pequeña, y la extracción escribe cada archivo una sola vez. |
| **Borrado seguro + búsqueda de restos** | Opcionalmente sobrescribe y luego borra el original tras cifrar, y busca restos en texto plano dejados por copias de seguridad de Office/WPS y copias con el mismo nombre en otras partes del disco. |
| **Gestor de contraseñas** | CRUD completo, oculto por defecto, importación desde un CSV de contraseñas de Chrome/Edge/Firefox (el CSV en texto plano se borra de forma segura tras la importación), exportación de vuelta al formato Chrome/Edge. |
| **Historial de portapapeles** | Historial cifrado con clasificación automática (URL / correo / teléfono / **fórmula** / código / texto), fijado, búsqueda, desplazamiento virtual, lista negra por aplicación, papelera, icono en la bandeja del sistema, y un panel global con `Ctrl+Shift+V`. Haz clic en una miniatura para abrir la imagen completa (pantalla completa o ventana flotante, a tu elección) con zoom, desplazamiento y clic para copiar. Exporta todo / lo fijado / lo oculto / solo lo marcado, con selección múltiple y seleccionar todo; importa varios archivos a la vez y los originales se eliminan de forma segura después. Importar nunca deja una segunda copia de algo que ya tienes: una entrada idéntica se combina en lugar de duplicarse, y conserva sus marcas de fijado y oculto. Para los duplicados que ya están en la bóveda, una limpieza enumera lo que ha encontrado y solo combina tras tu confirmación; lo que aparta va a la papelera. |
| **Notas** | Notas en Markdown con imágenes, categorías y búsqueda de texto completo, almacenadas en una base de datos cifrada. Las categorías se ordenan por nombre, por número de notas, por uso reciente — o en el orden que tú mismo dispongas. Sobre la lista hay cuatro contadores — Total, Activo, Oculto, Eliminado — y cada uno abre aquello que cuenta: pulsa Oculto para ver esas notas, Eliminado para abrir la papelera, y otra vez para volver. Los dos que pondrían notas ocultas en pantalla piden antes verificación de identidad, con el mismo desbloqueo que en el resto de la aplicación; una nota oculta que se ha enviado a la papelera sigue igualmente tras esa comprobación, y la papelera indica cuántos elementos está reteniendo, para que una lista corta nunca parezca datos perdidos. Cada nota recuerda dónde dejaste de leer o de editar y te devuelve allí al reabrirla, incluso en notas lo bastante grandes como para paginarse. Una nota grande aparece en milisegundos y sigue respondiendo mientras escribes: el renderizado se hace bloque a bloque en un hilo de fondo, de modo que la primera pantalla no espera al resto — una nota de 4 MB muestra su comienzo en unos 40 ms en lugar de un segundo y medio. |
| **Asistente de IA** | Chatea con cualquier endpoint compatible con OpenAI (DeepSeek por defecto). La lista de modelos se obtiene en vivo del proveedor y se muestra con la fecha de publicación de cada uno, del más reciente al más antiguo; los modelos que la aplicación no conoce siguen apareciendo y se pueden seleccionar, y los retirados quedan señalados en lugar de fallar en silencio. El modelo elegido se aplica a todas las funciones de IA. "Habilidades" importables usando el mismo formato `SKILL.md` que Claude Code. El historial de conversaciones se almacena cifrado. |
| **Gestor de archivos** | Copiar / cortar / pegar / eliminar a alta velocidad, interoperable con el propio portapapeles del Explorador, con una tarjeta de progreso en vivo. Atajos globales `Ctrl+V` / `Supr` / `Mayús+Supr`. |
| **Test de velocidad de red** | Ejecuta y grafica pruebas de conexión, con historial. El caudal se lee como lo leen los test de velocidad públicos: la conexión se muestrea en tramos de un cuarto de segundo y la cifra que se da es el percentil 90 del régimen estable, de modo que la rampa de arranque lento de TCP y los bajones puntuales no arrastran el número por debajo de lo que la línea entrega de verdad. La latencia mide una ida y vuelta TCP en lugar de una petición HTTPS completa, lo que deja fuera el tiempo de proceso del servidor, y la fluctuación es la variación media entre muestras consecutivas: la magnitud de la que depende que una llamada o una partida aguanten. Cada resultado trae un índice de estabilidad que indica cuánto se separaron los momentos rápidos y lentos de la prueba, y responde así a si un número distinto en la siguiente medición significa algo. |
| **Actualización automática firmada** | La release contiene **dos archivos: un instalador y un exe portable**, uno por modalidad. La confianza proviene de la firma Authenticode incrustada en su interior, comprobada contra un nombre de editor y una huella de clave pública fijados en la compilación: por eso basta con un certificado autofirmado gratuito; uno de pago solo evita el aviso de SmartScreen. Una descarga sin firmar, manipulada o firmada con otra clave se elimina, y una compilación sin firmante configurado rechaza toda actualización: nunca "permitir si no está configurado". Una release con más de un exe se rechaza en lugar de adivinar. La instalación es transaccional: la firma se vuelve a comprobar tras cerrarse la aplicación, el exe antiguo se conserva y una versión nueva fallida se revierte automáticamente. Tu bóveda, credenciales y ajustes viven fuera del exe y una actualización nunca los toca. |
| **Sincronización multidispositivo** | Copia en el móvil, pega en el PC — y al revés. Texto, texto enriquecido, código, fórmulas matemáticas y símbolos físicos conservan su formato; las imágenes también viajan. Envía cualquier archivo o carpeta a un dispositivo o a varios a la vez, con reanudación tras interrupción y cifrado de extremo a extremo. Cuatro formas de vincular — código QR, código de vinculación, eligiendo el dispositivo en la lista de cercanos o escribiendo una dirección a mano — unos diez segundos en cualquier caso. Cuando la lista de cercanos sale vacía, una exploración activa de la red encuentra dispositivos que la detección multicast no alcanza, y un diagnóstico de conexión señala el paso que falla. Compartir desde otra aplicación de Android pasa por el menú de compartir del sistema: mantén pulsada una foto o un archivo, elige Tessera y luego indica a qué dispositivos emparejados enviarlo. Los archivos van solo a los dispositivos elegidos —uno o varios, seleccionados explícitamente antes de cada envío—, mientras que el portapapeles siempre llega a todos los dispositivos emparejados. Una vez emparejado, un dispositivo que vuelve a aparecer en la red se reconecta solo en aproximadamente un segundo, y vincular solo hace falta una vez: los dispositivos ya vinculados se reencuentran en otra red o tras un cambio de dirección IP, sin depender del descubrimiento multidifusión que muchos routers descartan. |
| **Limpieza del PC** | Cinco paneles que se ocupan de la máquina en sí. Una revisión analiza de una vez los archivos inútiles, las cachés de los navegadores, los rastros de uso, los complementos, los programas de inicio, el origen de las ventanas emergentes y las referencias obsoletas del registro — y se va rellenando conforme termina cada parte, en lugar de dejarte mirando una pantalla quieta. El espacio en disco cubre el adelgazamiento del disco del sistema, los archivos grandes, el espacio por carpeta, los duplicados (comparados por tamaño, luego por una muestra de cada archivo y por último enteros) y una optimización que ejecuta TRIM en un SSD en vez de desfragmentarlo. Inicio y menús lista lo que se ejecuta al iniciar sesión — registro, carpeta de inicio, tareas programadas y servicios — además de cada entrada del menú contextual, con un bloqueador de ventanas emergentes que actúa en los dos frentes. Programas instalados recoge todo, y una caja de herramientas reúne cuarenta y dos pequeñas utilidades, con búsqueda y fijado. Antes de limpiar, una simulación recorre la ruta real del código sin tocar un byte e indica cuánto ganaría cada unidad y qué no podría recuperarse. Los datos del navegador se detallan por navegador y por perfil — cookies, historial, formularios, almacenamiento de sitios — sin listar nunca las contraseñas ni los marcadores, que tampoco pueden borrarse desde aquí. Los paquetes de controladores sustituidos por versiones más nuevas suelen ser el mayor peso muerto del disco del sistema, y la limpieza de disco de Windows no los toca. Carpetas vacías, accesos directos rotos y archivos de cero bytes se recogen del escritorio, documentos y descargas. El software instalado se ordena por tamaño *y* por tiempo sin usar, porque cuatro gigabytes solo se convierten en motivo para desinstalar cuando sabes que no lo abres desde hace un año. La acción de limpieza está arriba de la página, no abajo. Cada categoría dice qué tipo de «vacío» encontró — realmente nada que limpiar, bloqueado por permisos, o que nunca llegó a ejecutarse — y cuántas ubicaciones revisó. Los paneles se abren con el resultado anterior mientras corre un análisis nuevo. Puedes añadir reglas que apunten a tus propias carpetas — nunca a la raíz de una unidad, una carpeta del sistema o los datos de la caja fuerte — y dejar que una pasada programada mire sin borrar. |
| **Saber qué se puede quitar** | Cada elemento lleva uno de cinco juicios — Windows lo necesita, un controlador, algo que usas, opcional, publicidad — con el motivo escrito con palabras. Lo que Windows necesita queda bloqueado y no se puede seleccionar; el backend lo rechaza aunque se lo pidan. Todo lo que no se reconoce se llama opcional, nunca basura: perderse un archivo de caché cuesta megabytes, quitar un controlador cuesta una tarde. |
| **Nada que quites se pierde** | Las cachés y los archivos temporales se borran definitivamente: eso es lo que libera el espacio. Tus propios archivos van a la Papelera de reciclaje. Las claves del registro, los programas de inicio y los controladores del menú contextual se *desactivan con copia de seguridad*, nunca se borran. Se exporta una copia del registro antes de cualquier cambio, y si la exportación falla no se cambia nada. |
| **Interfaz y apariencia** | Un mismo lenguaje visual en todas las pantallas: la misma cabecera de página, las mismas tarjetas de sección y los mismos colores de estado en todas partes, sin nada que parezca añadido después. Ocho colores de acento, dos estilos visuales (Nativo mejorado / Lujo tecnológico oriental), sistema/claro/oscuro y un modo Simple o Profesional que oculta o despliega el detalle de diagnóstico. La app complementaria de Android usa la misma paleta y la misma organización por secciones. |
| **6 idiomas** | English, 简体中文, Français, Español, Русский, العربية — cambiable en cualquier momento, también en la pantalla de inicio de sesión. |

Cinco formas de desbloquear, **con que una sola baste**: contraseña · código de correo ·
aplicación de autenticación (TOTP) · Windows Hello · clave de
recuperación de un solo uso.

Aquí Windows Hello es el método que tengas registrado: **cara, huella o PIN**. La ventana es la
del propio sistema, así que un equipo con cámara Hello y sin lector de huellas se desbloquea con
la cara.

La pantalla de inicio de sesión **se abre en Windows Hello y lo solicita de inmediato**, sin hacer
clic. En un equipo donde Hello nunca se vinculó a esta bóveda, pasa al campo de contraseña en
silencio, sin ningún error: intentarlo no cuesta nada.

---

## Primeros pasos

### Opción A — descargar una versión publicada

Hay dos builds en la página de [Releases](../../releases). Son la misma aplicación;
elige el que encaje con tu forma de trabajar.

| | `Tessera.Setup.exe` | `Tessera.exe` |
|---|---|---|
| | **Instalador** | **Portable** |
| Ubicación de instalación | la eliges tú | donde pongas el archivo |
| Acceso directo en menú Inicio / escritorio | sí | no |
| Aparece en *Aplicaciones y características* | sí | no |
| Permisos de administrador | no hacen falta (instala por usuario) | no hacen falta |
| Se ejecuta desde un USB | no | sí |
| La actualización puede revertirse | no | sí |

Ninguno escribe nada fuera de tu propio perfil de usuario. **Desinstalar no borra tu
bóveda** — consulta [Dónde viven tus datos](#dónde-viven-tus-datos).

> En la primera ejecución Windows mostrará «Windows protegió su PC». Es SmartScreen
> reaccionando a un certificado autofirmado, no un aviso de virus. Pulsa **Más
> información → Ejecutar de todas formas**. Solo ocurre la primera vez; las
> actualizaciones dentro de la aplicación nunca lo provocan.

Ambos builds se actualizan solos, cada uno manteniéndose en su propia modalidad.

### Opción B — ejecutar desde el código fuente

```bash
git clone <this repo>
cd Tessera

# Lado Python — todas las dependencias de ejecución están declaradas en pyproject
pip install -e .

# Lado UI (node_modules no está versionado)
cd modules/file_vault/ui && npm install && cd ../../..

# Lanzar
python scripts/run.py run file_vault
```

Ese último comando ejecuta `npm run dev` dentro de `ui/`, que arranca Vite más una
ventana de Electron. El proceso principal de Electron lanza `backend_server.py` como un
proceso hijo de Python de larga duración y se comunica con él por NDJSON sobre
stdin/stdout; cerrar la ventana lo detiene.

Extra opcional — el desbloqueo por huella dactilar necesita los enlaces de Windows
Hello:

```bash
pip install winrt-Windows.Security.Credentials.UI winrt-Windows.Foundation
```

---

## Primer inicio: el asistente de configuración

En el primer lanzamiento, el asistente te guía, en orden:

1. **Establecer una contraseña** — esto envuelve tu clave maestra de identidad.
2. **Vincular una aplicación de autenticación** — escanea un código QR, confirma un
   código.
3. **Vincular el desbloqueo por huella dactilar** — *se omite automáticamente* si este
   PC no tiene Windows Hello.
4. **Configurar el correo** — opcional aquí, se puede añadir después en Ajustes. Ver
   abajo.
5. **Generar una clave de recuperación** — se muestra una sola vez. Anótala.

Después de eso, cada inicio abre la pantalla de inicio de sesión; pasa cualquier factor
y ya estás dentro.

---

## Configurar el inicio de sesión por código de correo

Este es el método de desbloqueo "envíame un código de 6 dígitos por correo". Funciona
enviando correo **desde tu propio buzón, a través de tu propia cuenta SMTP** — no hay
ningún servidor de Tessera en medio, por eso tienes que proporcionar credenciales SMTP.

### Paso 1 — obtener una contraseña específica de aplicación de tu proveedor de correo

Casi con toda seguridad no puedes usar tu contraseña de inicio de sesión normal. Los
proveedores exigen una contraseña de aplicación dedicada (Gmail) o un código de
autorización (QQ, 163) para clientes de terceros:

| Proveedor | Dónde obtener la credencial |
|---|---|
| **Gmail** | Activa la verificación en 2 pasos, luego Cuenta de Google → Seguridad → Contraseñas de aplicaciones. Obtienes un código de 16 caracteres; los espacios que muestra Google son solo para legibilidad y **no** forman parte de la contraseña. Google retiró el "acceso de apps poco seguras" en 2022, así que una contraseña de aplicación (u OAuth) es la única vía. |
| **QQ Mail** | Ajustes → Cuenta → activa el servicio POP3/IMAP/SMTP, luego genera un 授权码 (código de autorización). Usa ese, no tu contraseña de QQ. |
| **163 Mail** | Ajustes → POP3/SMTP/IMAP → activa el servicio y establece un 授权码. La misma idea. |
| **Outlook.com / Microsoft 365** | **No compatible.** Microsoft deshabilitó la autenticación básica para SMTP AUTH en 2026; estas cuentas ahora requieren OAuth 2.0 (XOAUTH2), que esta aplicación no implementa. Usa otro buzón para el código, o confía en los otros cuatro métodos de desbloqueo. |

### Paso 2 — rellenar los ajustes

En el paso *Configuración de correo y envío* del asistente, o más tarde vía
**Ajustes → Actualizar correo y envío**:

| Campo | Significado | Valor típico |
|---|---|---|
| Correo de recuperación | Adónde se envía el código (puede ser el mismo buzón) | `tu@ejemplo.com` |
| Servidor SMTP | El host de salida de tu proveedor | `smtp.gmail.com` · `smtp.qq.com` · `smtp.163.com` |
| Puerto SMTP | `465` para SSL, `587` para STARTTLS | `465` o `587` |
| Cuenta de envío | La dirección completa desde la que se envía el correo | `tu@gmail.com` |
| Contraseña específica de aplicación | La credencial del paso 1 — *no* tu contraseña de inicio de sesión | contraseña de aplicación de 16 caracteres / 授权码 |
| Usar SSL | Márcalo para el puerto **465**; déjalo sin marcar para **587** (STARTTLS) | — |

> En Ajustes, dejar el campo de contraseña en blanco conserva la ya guardada.

### Paso 3 — enviar un correo de prueba

Haz clic en **Enviar correo de prueba** y confirma que realmente llega. Si no llega, el
error se muestra literalmente — las causas habituales son la combinación
puerto/SSL incorrecta, usar la contraseña de inicio de sesión en vez de la de
aplicación, o que el SMTP aún no esté habilitado en la cuenta.

### Cómo se comporta al iniciar sesión

Elige la pestaña **Correo** en la pantalla de inicio de sesión y solicita un código. El
código tiene 6 dígitos, es válido durante **10 minutos**, se puede usar **una sola
vez**, y se guarda solo en la memoria del proceso backend — nunca se escribe en disco.

---

## Configurar la aplicación de autenticación

Escanea el código QR del asistente con Google Authenticator, Microsoft Authenticator,
Authy, o cualquier aplicación TOTP estándar, y luego escribe el código actual de 6
dígitos para confirmar la vinculación. La semilla también se puede escribir
manualmente si no puedes escanear. La semilla se guarda en el Administrador de
credenciales de Windows, no en el repositorio.

## Configurar el desbloqueo biométrico

Esto invoca al sensor real de Windows Hello. Necesita los dos paquetes `winrt-*` de arriba
más un PC con Windows Hello configurado. Si Hello no está disponible, el asistente omite
este paso y los otros cuatro métodos no se ven afectados.

Tessera le pregunta a Windows qué sensores biométricos tiene realmente este equipo y elige
el primero disponible en el orden **rostro → huella → iris**. La pestaña se rotula e ilustra
en consecuencia: un equipo con cámara de infrarrojos dice «Rostro»; uno que solo tiene lector
dice «Huella». Dibujar un rostro en un equipo sin cámara deja al usuario esperando un aviso
que no va a llegar.

**En todos los sitios donde se pide contraseña, el aviso biométrico se inicia solo**: la
pantalla de inicio de sesión, el diálogo de reautenticación que protege los Ajustes y las
comprobaciones a la entrada de cada módulo. No tienes que pulsar nada primero. Si este equipo
no puede usar biometría, o esta caja fuerte no tiene ninguna vinculación biométrica, no
aparece ningún aviso y se abre directamente la pestaña de contraseña: ni un error por una
función que nunca configuraste, ni un aviso de Hello condenado a fallar.

Windows Hello no permite que una aplicación elija *qué* modalidad usar: lo decide el sistema
según lo que hayas registrado. Esta cadena determina lo que Tessera te muestra, qué factor
inicia automáticamente y hacia dónde recurre.

## Clave de recuperación — léelo bien

La clave de recuperación se muestra **una sola vez**, durante la configuración.
Guárdala en un lugar seguro y *no* junto a tus archivos cifrados.

Si pierdes tu contraseña, pierdes el teléfono con tu autenticador, y muere la máquina
con tu huella vinculada — la clave de recuperación es la única vía de entrada que
queda. Si esa también se pierde, **los archivos no se pueden recuperar.** Ese es el
diseño, no un fallo.

---

## Uso diario

- **Cifrar** — elige uno o más archivos, elige un algoritmo, pulsa Cifrar. Un solo
  archivo abre un diálogo Guardar como; varios archivos piden una carpeta destino y
  escriben `<nombre>.ivault` para cada uno. Un fallo en un archivo no aborta el resto;
  obtienes un resumen al final.
- **Eliminar el original** — desactivado por defecto para que no pierdas datos por
  accidente. Pero cifrar no sirve de nada si el texto plano sigue junto al texto
  cifrado, así que marca **eliminar el original de forma segura tras cifrar** cuando el
  objetivo sea la confidencialidad. Esa eliminación sobrescribe primero con bytes
  aleatorios y evita la Papelera de reciclaje. (Mejor esfuerzo: el desgaste nivelado de
  los SSD significa que esto no es una garantía frente a la recuperación forense.)
- **Búsqueda de restos** — tras cifrar, la aplicación busca restos en texto plano:
  copias de seguridad de editores (`~$…`, `.bak`, `.wbk`, carpetas de autoguardado de
  WPS) y copias con el mismo nombre en otras partes del disco. Los elementos
  relacionados con el archivo de origen se marcan de antemano; las coincidencias con el
  mismo nombre en todo el disco **nunca** se marcan de antemano, ya que una de ellas
  podría ser un archivo que querías conservar. Puedes volver a ejecutar esto en
  cualquier momento sobre un archivo `.ivault`.
- **Bloquear** — cerrar la ventana la oculta en la bandeja del sistema. La clave
  maestra desbloqueada solo existe en la memoria del proceso backend y muere con él.

---

## Limpiar el ordenador

Cinco paneles bajo **Limpieza del PC** en la barra lateral. Funcionan con permisos de
usuario normal; solo unas pocas operaciones piden derechos de administrador, y entonces
aparece un pequeño proceso auxiliar que hace ese trabajo concreto y se cierra. Tessera
nunca se ejecuta como administrador: guarda material de clave descifrado, y un proceso
elevado de larga vida que lo tenga sería una presa mucho mayor que uno normal.

### Revisión

Un botón analiza nueve categorías a la vez y muestra cada una en cuanto llega. En una
máquina normal los primeros resultados están en pantalla en bastante menos de un segundo;
las tareas programadas tardan de cinco a nueve segundos y llegan las últimas, porque un
proceso sin elevar no puede ir más rápido — Windows ni siquiera le deja listar la carpeta.

Dos cosas quedan a propósito detrás de su propio botón en vez de dentro de ese análisis:
analizar el almacén de componentes y medir cada carpeta de primer nivel del disco del
sistema. Cada una tarda alrededor de un minuto, y meterlas en la revisión de un clic la
convertía en una espera de dos minutos.

En **Opciones de análisis** ajustas cuánto tiempo debe llevar un archivo sin tocarse para
contar como inútil. Un día por defecto, deliberadamente prudente: una instalación en curso
escribe en la carpeta temporal. En una máquina recién reiniciada, ponerlo en *Sin importar
la antigüedad* suele encontrar varias veces más.

### Espacio en disco

- **Disco del sistema** — archivo de hibernación, almacén de componentes, puntos de
  restauración, `Windows.old`, y lo que ocupa el archivo de paginación. Este último se
  muestra solo como información y no se puede seleccionar: desactivarlo hace que los
  programas fallen cuando falta memoria, en lugar de ir despacio.
- **Archivos grandes** — los mayores primero, en las carpetas y por encima del tamaño que
  elijas.
- **Espacio por carpeta** — qué carpetas ocupan el sitio, un nivel cada vez.
- **Duplicados** — comparados por tamaño, luego por una muestra tomada del principio y el
  final de cada archivo, y por último enteros. En cada grupo se conserva la copia con la
  ruta más corta y el resto queda preseleccionado.
- **Unidades** — una optimización que lee primero el tipo de medio. Tessera se niega a
  desfragmentar un SSD aunque se lo pidas: no hay nada que ganar y lo desgasta.

### Inicio y menús

Todo lo que se ejecuta al iniciar sesión, reunido desde cuatro sitios: el registro, la
carpeta de inicio, las tareas programadas y los servicios. Las herramientas que solo leen
el registro anuncian un puñado de entradas mientras la máquina sigue tardando un minuto en
arrancar.

Aquí no se borra nada. Las entradas del registro se mueven a una clave de copia de
seguridad, los accesos directos de inicio se renombran, las tareas programadas se
desactivan mediante el programador, y los servicios pasan a inicio manual en lugar de
desactivarse — desactivar uno hace que también falle todo lo que depende de él. Cada uno
vuelve con un clic.

El bloqueador de ventanas emergentes actúa en dos niveles: desactiva lo que las produce al
arrancar, y cierra con `WM_CLOSE` las ventanas que coinciden con una regla. Nunca termina
un proceso, nunca toca la ventana en la que estás trabajando, y registra todo lo que ha
cerrado.

### Programas instalados

Todo lo instalado, leído desde las tres ubicaciones del registro — 64 bits, 32 bits y por
usuario. Leer solo la primera deja fuera más de la mitad.

Los tamaños se miden desde la carpeta de instalación en vez de tomarse de lo que declaró
el instalador. La desinstalación ejecuta el desinstalador del propio programa; Tessera
nunca borra una carpeta de instalación por su cuenta, lo que dejaría atrás servicios,
controladores y entradas del registro.

La pestaña del registro dice claramente que limpiar referencias obsoletas **no hará tu PC
más rápido**. Vale la pena por orden. La única categoría que ayuda de verdad es la de
registros de componentes que apuntan a DLL que ya no existen.

### Herramientas

Dieciocho pequeñas utilidades, cada una con una línea que dice cuándo la querrías: vaciar
la caché DNS, ver qué programa ocupa un puerto, averiguar qué está bloqueando un archivo,
reiniciar el Explorador, reconstruir la caché de iconos, buscar carpetas vacías, encontrar
rutas demasiado largas para copiarse, renombrar en lote con vista previa antes.

El archivo hosts se muestra en solo lectura. Editarlo permite redirigir cualquier sitio, y
esa no es una capacidad que una caja fuerte cifrada deba tener en tu nombre.


## Sincronización multidispositivo

Tu móvil, tu tableta y este PC comparten un portapapeles y un punto de entrega de archivos.
Copia una fórmula en el móvil y pégala en Word en el PC. Suelta un vídeo de 4 GB en el PC y
recógelo en la tableta. Nada pasa por un servidor: los dispositivos hablan directamente por tu
red local, cifrados de extremo a extremo.

### Vincular un dispositivo

Abre **Multidispositivo → Añadir un dispositivo** y elige:

- **Mostrar mi código** — aparece un código QR. Escanéalo desde el otro dispositivo.
- **Introducir su código** — pega el código que muestra el otro dispositivo.
- **Dispositivos cercanos** — la lista de dispositivos sin vincular en esta red. Elige uno y
  vincula, sin intercambiar ningún código. Los nombres aparecen marcados como *sin verificar*: el
  nombre de un dispositivo es lo que escribió su dueño; lo que establece la identidad de verdad es
  la comprobación del código de seguridad de abajo.
- **Escribir una dirección** — la salida cuando nada más funciona, y la más fiable. La pestaña
  muestra la dirección de este equipo (`192.168.1.7:52140`) para dictarla, y un campo para la del
  otro. Puedes indicar `IP:puerto` o solo una IP: con solo la IP, Tessera consulta la baliza de
  descubrimiento del dispositivo por su puerto actual. Dos dispositivos que se alcancen por IP
  pueden vincularse así: no hace falta la misma subred y funciona donde el multicast está bloqueado.

**Si la lista de dispositivos cercanos sigue vacía, pulsa «Explorar la red» en vez de volver a
pulsar «Actualizar».** Actualizar solo relee lo que ya se ha oído, y muchos routers descartan los
paquetes multicast en los que se basa la detección automática: en esas redes, actualizar cien veces
sigue sin mostrar nada. Explorar pregunta directamente a cada dirección de la subred y esquiva el
problema.

Una franja amarilla en la parte superior significa que el Firewall de Windows está bloqueando el
servicio de sincronización, así que los demás dispositivos no ven este equipo. Pulsa
**Permitirlo**: una sola elevación, y la regla cubre todos los perfiles de red de Windows, pero
solo el programa de sincronización de Tessera: únicamente responde a sondeos de descubrimiento y
negociaciones QUIC cifradas, y sin vincular no se puede leer nada. Windows descarta esos paquetes
en silencio: sin error, sin registro, nada más que una lista vacía, y por eso conviene mostrarlo en
lugar de dejar que lo adivines. El **diagnóstico de conexión** que hay debajo de la lista recorre
cada paso —escucha, direcciones, anuncio, puerto de respuesta, detección, exploración activa— y
dice cuál falla y qué hacer.

Ambas pantallas mostrarán entonces los mismos seis dígitos y seis emojis. **Compáralos.** Si
coinciden, confirma en los dos; si difieren, hay alguien en medio — elige «No coinciden» y la
vinculación se abandona. Un código de vinculación sirve una sola vez y caduca al cabo de un
minuto, así que una captura de pantalla antigua no le sirve a nadie.

Al vincular eliges de qué tipo de dispositivo se trata:

| | Permanente | Temporal |
|---|---|---|
| Pensado para | Tus propios dispositivos | Los de otra persona — un amigo, un PC de aula |
| Reconexión automática | Sí | No |
| Sincronización del portapapeles | Sí | No |
| Caduca | Nunca, hasta que lo revoques | 10 min / 30 min / 1 hora / 1 día |
| Credenciales en disco | Guardadas, cifradas | No se escriben en ningún momento |

Revoca cualquier dispositivo cuando quieras. La revocación es inmediata y definitiva: las
credenciales antiguas dejan de funcionar en el siguiente intento de conexión, no en el próximo
reinicio.

### Copiar y pegar

Todo lo que copies va a tus dispositivos permanentes y aterriza en su portapapeles del sistema,
listo para pegar. Texto, texto enriquecido e imágenes viajan en la representación que pueda usar
cada aplicación receptora; las fórmulas y símbolos físicos se conservan intactos — `E = mc²`,
`m/s²`, `θ̇`, `∑∫√`. Solo se sincroniza el elemento más reciente del portapapeles, para que una
copia anterior no llegue tarde y sustituya lo que acabas de copiar. Windows comprueba cada 200 ms;
en Android 10 o posterior, el sistema solo permite a Tessera leer el portapapeles mientras está en
primer plano, así que después de copiar en otra aplicación Android basta con abrir Tessera una vez
para sincronizar. El contenido recibido sí se escribe en el portapapeles de Android.

Las imágenes grandes no se envían de golpe; el otro dispositivo solo las descarga si realmente
pegas.

### Enviar archivos

Selecciona uno o varios dispositivos en la lista y pulsa **Enviar archivos**. Puedes elegir
explícitamente uno o varios destinos; los archivos, carpetas, imágenes y paquetes de aplicación
se etiquetan en la lista de transferencias. Cada dispositivo tiene su propia transferencia
independiente: un móvil lento nunca frena a un PC rápido, y que uno rechace o falle no cancela a
los demás.

Cada transferencia activa muestra nombre, progreso, velocidad y tiempo restante estimado. Cualquiera
de los dos lados puede pausarla, reanudarla o cancelarla. La pausa conserva los bloques ya
verificados; tras una interrupción o reconexión continúa desde ahí, sin empezar de cero. Cada
archivo se verifica al llegar — una transferencia que no supera la verificación se informa como
fallida, nunca como «completada».

Los ejecutables recibidos (`.exe`, `.msi`, `.apk`) se guardan como archivos normales. Tessera
nunca ejecuta ni instala nada de lo que recibe.

### Mantenerse conectado

Los dispositivos que se caen — móvil bloqueado, portátil en suspensión, cambio de Wi-Fi, router
reiniciado — vuelven por sí solos. No hay botón de «reconectar» ni hace falta volver a escanear
nada.

La reconexión prueba todas las rutas a la vez (última dirección conocida, direcciones vistas
anteriormente y lo que el descubrimiento de red local informe en ese momento) y se queda con la
que responda primero. En una red local esto suele completarse en bastante menos de un segundo.
Como el descubrimiento está vivo, un dispositivo que haya cambiado de dirección IP — nuevo
Wi-Fi, nueva concesión DHCP, otra subred — se encuentra igualmente.

### Qué incluye esta versión

La parte de Windows está completa e incluida en esta publicación: el servicio de sincronización
va dentro del exe y arranca con la aplicación.

**La aplicación de Android se construye a partir del mismo núcleo de protocolo**, compilado
como biblioteca nativa (`tesseracore.aar`) con `gomobile bind`: la vinculación, el cifrado, la
máquina de estados de reconexión y la reanudación de transferencias son literalmente el mismo
código que corre en Windows, no una reimplementación que acabará divergiendo. El lado Android
solo añade lo que es realmente específico de la plataforma: un servicio en primer plano para
mantener vivo el proceso, un bloqueo multicast de Wi-Fi para que el descubrimiento mDNS siga
funcionando con la pantalla apagada, y la lectura/escritura del portapapeles del sistema.

Enumerar las interfaces de red es una de esas piezas propias de la plataforma. Android bloquea la
llamada al núcleo que la biblioteca estándar de Go usa para ello, así que el cliente Android lee la
lista mediante la API de Java y se la entrega al núcleo compartido antes de que arranque el nodo, y
de nuevo cada vez que cambia la red. Sin ese paso, todas las vías de descubrimiento —dispositivos
cercanos, barrido de la red local, la propia dirección de este dispositivo para la vinculación
manual— volverían vacías sin nada que lo explicara. El diagnóstico de conexión bajo **Dispositivos
cercanos** informa primero del número de interfaces por la misma razón: es lo primero que conviene
descartar.

La pantalla se compone de secciones plegables. Aquello que no está usando —el código de
vinculación, las actualizaciones, el bloqueo de la aplicación, el registro de incidencias— se
reduce a una línea de título y sigue plegado la próxima vez. La lista de dispositivos vinculados y
las acciones de envío no se pliegan: la pantalla existe para ellas. También hay métricas en vivo
—dispositivos en línea, latencia del enlace, velocidad actual, totales de la sesión, puerto de
escucha, direcciones locales, interfaces disponibles— leídas del flujo de eventos que ya está
llegando, sin sondeos adicionales en segundo plano, y el temporizador no corre mientras la sección
está cerrada.

El cliente de Android cubre la vinculación escaneando un código QR, la comprobación del código
de seguridad, la lista de dispositivos, el envío/recepción del portapapeles, la elección de uno o
varios destinos para archivos, los controles de transferencia y la elección de dónde se guardan
los archivos recibidos. También permite seleccionar y revocar dispositivos vinculados en lote.
Los dispositivos cercanos que ya están vinculados y en línea se muestran con ese estado, sin pedir
que se vinculen otra vez. El reconocimiento de códigos usa un modelo incorporado en lugar de uno
descargable: escanear para vincular ocurre casi siempre en un móvil recién instalado y aún sin red,
justo cuando un modelo que se descarga en el primer uso no está disponible.

La aplicación de Android también comprueba sus propias actualizaciones contra las releases de
este repositorio. La comprobación automática viene activada por defecto: mira una vez al abrir la
aplicación y solo avisa si de verdad existe una versión más nueva. Si la desactivas, no se
comprueba nada hasta que pulses **Buscar actualizaciones**. Solo la *comprobación* es automática:
Android no permite que una aplicación instalada fuera de la tienda instale un paquete en
silencio, así que el último paso siempre es el diálogo del propio sistema, que confirmas tú. La
descarga solo se acepta desde github.com, y es el sistema quien exige que el paquete nuevo lleve
la misma clave de firma que el instalado.

Se ha compilado y firmado pero **no** se ha ejecutado en hardware físico aquí; trata la primera
instalación como una prueba.

### Seguridad

Cada dispositivo tiene su propio par de claves de largo plazo, generado en el primer arranque y
guardado en la DPAPI de Windows — nunca un número de serie de hardware, un IMEI o una dirección
MAC, porque esos no se pueden revocar. Estar en la misma red no concede nada: cada conexión
vuelve a verificar la identidad contra la clave que confirmaste al vincular, y un dispositivo
cuya clave cambia de repente se rechaza a las claras en lugar de aceptarse en silencio.

Cada conexión negocia claves nuevas de un solo uso, así que grabar el tráfico de hoy y robar una
clave de dispositivo más adelante sigue sin permitir descifrarlo. Las rutas de los archivos
entrantes se comprueban antes de escribir nada — los escapes `../`, las rutas absolutas y los
nombres reservados de Windows se rechazan de plano.

---

### Conectar tus dispositivos

En el ordenador abre **Multidispositivo → Dispositivos y sincronización → Generar código de
vinculación**; en el móvil pulsa **Escanear para vincular** y apunta al código QR. Ambas
pantallas mostrarán los mismos seis dígitos y seis emojis: comprueba que coinciden, confirma
en los dos, y listo.

Los dos extremos pueden elegir dónde se guardan los archivos recibidos, y renombrar un
dispositivo se propaga a todos los vinculados en unos diez segundos, sin reconectar.

Si el escaneo parece quedarse colgado, lo habitual es que los dos dispositivos no estén en la
misma red, o que el Wi-Fi tenga aislamiento de AP (común en empresas, campus y hoteles). Una
zona Wi-Fi del móvil es la forma más rápida de saber cuál de las dos es. Guía completa:
[guía de conexión](modules/file_vault/crossdevice/docs/06-连接指南.md).

### Progreso de las transferencias

Aparece una tarjeta de progreso en cuanto empieza una transferencia: nombre del archivo,
dispositivo de destino, porcentaje, bytes enviados sobre el total, una fila por archivo. Se
refresca cada 250 ms en lugar de por bloque: un bloque son 4 MB, decenas por segundo en
gigabit, y repintar en cada bloque hace que la barra se entrecorte en vez de avanzar.

### Registro de diagnóstico

El registro de diagnóstico tiene su propia entrada en la barra de navegación, y el mismo
registro está también en los ajustes: un solo interruptor, un solo contenido, accesible desde
cualquiera de los dos sitios. Viene **desactivado por defecto**. Al activarlo
solo anota lo que falló: un atajo que no funcionó, un dispositivo que no conectó, un
servicio que no arrancó. Nunca anota lo que estabas haciendo: ni portapapeles, ni nombres
de archivo, ni rutas, ni claves.

Es texto plano a propósito, no cifrado: el caso que más necesita un registro es aquel en
que la caja fuerte no abre, y justo entonces un registro cifrado resulta ilegible. Por eso
mismo debe venir desactivado. Un botón lo borra todo.

### Guías

- [Guía de escritorio](docs/电脑端操作指南.md) — todos los atajos, vinculación, envío arrastrando, resolución de problemas
- [Guía de Android](docs/安卓端操作指南.md) — vinculación, menú de compartir, supervivencia en segundo plano, bloqueo de la app
- [Referencia de conexión](modules/file_vault/crossdevice/docs/06-连接指南.md) — qué requiere una conexión y cómo diagnosticarla

### Qué necesita la conexión

Ambos dispositivos deben estar en la misma red local: el mismo Wi-Fi, o un ordenador por
Ethernet detrás del mismo router. No hace falta nada más: ni cuenta, ni nube, ni redirección
de puertos.

El puerto de escucha no es fijo. Si el predeterminado ya está ocupado, el servicio recurre a uno
que asigna el sistema; tanto el código de vinculación como el registro mDNS anuncian el puerto
que realmente se está usando, así que nada más abajo depende de ello.

Tener una VPN o un proxy en el ordenador no es problema: al anunciar direcciones se omiten las
interfaces sin multicast (que es justo lo que es un túnel VPN), de modo que al móvil nunca se le
entrega una dirección de túnel a la que no puede llegar.

Lo que la aplicación no puede sortear es el **aislamiento de AP**: muchas redes de empresa,
campus y hoteles impiden que los dispositivos de un mismo Wi-Fi se comuniquen entre sí. Una zona
Wi-Fi del móvil es la forma más rápida de confirmarlo, y también de evitarlo.


## Modelo de seguridad

Cifrado en sobre de dos capas:

```
                    ┌─ contraseña ────────┐
                    ├─ código de correo ──┤
Clave Maestra de    ├─ TOTP ──────────────┤  cada uno envuelve la IMK de forma independiente
Identidad (IMK) ────┼─ Windows Hello ─────┤
                    └─ clave de recuperación┘
                          │
                          ▼
   clave por archivo (FK), nueva para cada archivo, cifra el contenido por bloques
   la FK se envuelve luego con la IMK y se guarda dentro de la cabecera del .ivault
```

- La capa de identidad (envoltura de la IMK, ranuras de claves) siempre es
  AES-256-GCM; tu elección de algoritmo solo afecta al contenido de los archivos.
- Las ranuras de la contraseña y de la clave de recuperación derivan su clave de
  envoltura con **scrypt con N=2¹⁷, r=8** (el mínimo que recomienda hoy OWASP): unos
  134 MB de RAM por intento, que es lo que hace impracticable una granja de GPU o
  ASIC — miles de núcleos en paralelo no sirven de nada si cada uno necesita sus
  propios 134 MB. Cada ranura registra el coste con el que fue sellada, así que subirlo
  más adelante nunca deja fuera a una bóveda existente; una ranura que siga con un
  coste antiguo se vuelve a envolver de forma silenciosa en el siguiente desbloqueo.
- Todas las sales y todas las claves se generan en la propia máquina del usuario
  (`os.urandom`) durante la configuración: **no hay ningún secreto incrustado en el
  exe**, así que la misma descarga en mil manos produce mil bóvedas sin relación.
- Las bases de datos de portapapeles / notas / historial de IA son SQLCipher, con
  clave derivada por HKDF de la IMK — sin una segunda contraseña que recordar, pero
  ilegibles mientras la caja fuerte está bloqueada.
- El proceso de renderizado no tiene acceso a Node ni al sistema de archivos
  (`contextIsolation: true`, `nodeIntegration: false`); solo puede llamar al backend a
  través de una API preload estrecha.
- **Con un solo factor basta para desbloquear**, así que la fortaleza global es la del
  factor más débil. Una contraseña filtrada derrota a los otros cuatro. Exigir varios
  factores simultáneamente no está soportado en esta versión.

---

## Dónde viven tus datos

| Qué | Dónde |
|---|---|
| Identidad, bases de datos de la caja fuerte, adjuntos | `%LOCALAPPDATA%\Tessera\data` *(configurable — ver abajo)* |
| Semilla TOTP, clave de correo, contraseña SMTP | Administrador de credenciales de Windows |
| Clave maestra desbloqueada | Solo memoria del proceso backend — nunca en disco |

> **¿Vienes de una versión llamada «Ideal1 File Vault»?** Tu bóveda se migra sola. En el primer arranque la aplicación busca la carpeta antigua `%LOCALAPPDATA%\Ideal1 File Vault\` y la migra con el mismo procedimiento transaccional que cualquier otro traslado: instantáneas SQLite coherentes, verificar y luego renombrar atómicamente, y **la carpeta antigua queda intacta**. También se conserva la ubicación personalizada que eligieras en el instalador anterior.

Nada se sube a ningún sitio. Las únicas conexiones salientes son las que tú configuras:
tu propio servidor SMTP para los códigos de inicio de sesión, el endpoint de IA que
elijas si usas el asistente, los destinos del test de velocidad, y GitHub Releases
para las comprobaciones de actualización.

Los datos de identidad están ligados a una sola máquina y no se transfieren
automáticamente. `data/` está en gitignore y nunca se versiona.


### Elegir dónde vive la caja fuerte

La ubicación por defecto le sirve a la mayoría. Puedes ponerla en otro sitio —una unidad
que realmente respaldes, un volumen cifrado, un segundo disco— de tres formas, por orden
de prioridad:

1. **La variable de entorno `IDEAL1_FILE_VAULT_DATA_DIR`.** Prevalece sobre todo, se
   aplica solo a esa ejecución y desactiva cualquier traslado automático: definirla
   significa que gestionas esa carpeta tú.
2. **El instalador.** `Tessera.Setup.exe` pide una carpeta de datos en su
   propia página, justo después de preguntar dónde instalar el programa. Son dos
   decisiones distintas: reinstalar en otra carpeta es trivial, mover una caja fuerte
   que llevas seis meses llenando no lo es.
3. **Ajustes → Acerca de → Cambiar ubicación.** Elige una carpeta y reinicia. El
   traslado ocurre en el siguiente arranque, *antes* de que nada abra la caja fuerte:
   copiar una base SQLite cifrada con sus conexiones abiertas perdería en silencio lo
   que aún esté en el registro de escritura anticipada. La carpeta antigua queda
   intacta, así que un traslado fallido no cuesta nada.

Desinstalar la aplicación nunca borra la carpeta de datos.

---

## Desarrollo

```
core/       infraestructura compartida: config, logging, registro de módulos, ayudantes de subprocesos
modules/    módulos de funciones conectables — una carpeta = un escenario autónomo
scripts/    punto de entrada CLI: run.py (list / run <module>)
docs/       notas de arquitectura, especificaciones de diseño, procedimiento de release
tests/      pruebas de humo
```

Reglas de la casa — la versión completa está en [`CLAUDE.md`](CLAUDE.md) y
[`modules/README.md`](modules/README.md):

- Python ≥ 3.9. Usa `python -X utf8 …` cuando la salida no sea ASCII.
- Los módulos nunca se importan entre sí; solo dependen de `core/`. La lógica
  compartida se promueve a `core/`.
- Los módulos pueden mezclar JavaScript o C, invocados mediante `run_command()` de
  `core/proc.py`.

Añadir un escenario: copia `modules/_template/` → `modules/<name>/`, edita
`MODULE_META`, impleméntalo, y luego confirma con `python scripts/run.py list`.

```bash
python -m pytest tests/                        # pruebas de humo
python -m pytest modules/file_vault/tests/     # suite de la caja fuerte
cd modules/file_vault/ui && npm run lint       # lint de la UI
```

Más detalles: [`docs/architecture.md`](docs/architecture.md) ·
[`modules/file_vault/README.md`](modules/file_vault/README.md)

---

## Compilar una versión

Las compilaciones deben ejecutarse en **Windows x64** — PyInstaller no es un compilador
cruzado.

```powershell
cd modules\file_vault\ui
npm install
npm run build:standalone
```

Eso compila los ayudantes C nativos, congela dos objetivos de PyInstaller, y luego
ejecuta `tsc` + `vite build` + `electron-builder`, produciendo
`ui/release/<version>/` — both `Tessera.exe` (portable) and
`Tessera.Setup.exe` (installer).

La firma la gestiona la propia compilación: crea el certificado en la primera ejecución, fija su clave pública en `signer-policy.json`, mantiene la clave privada **no exportable** en tu almacén de certificados y escribe una copia protegida con contraseña fuera del repositorio. Aquí no hay nada que hacer a mano. (`secure-signing-key.cmd`, en la raíz del repositorio, puede volver a proteger esa copia con una frase tuya, si quieres que sobreviva a la pérdida de este equipo.)

Al subir un tag `v*` se lanza la misma compilación en CI — ver
[`.github/workflows/build-windows.yml`](.github/workflows/build-windows.yml). El
workflow sube un exe **sin firmar** a una release **draft** y ahí se detiene. La firma de
código se queda en tu propia máquina **por diseño**: una clave de firma guardada en los
secretos de CI puede ser usada por cualquiera que pueda editar un workflow, lo que
reduciría la firma a mera decoración.

Antes de publicar tu propio fork debes rellenar dos marcadores de posición:

- `REPO` en `modules/file_vault/ui/electron/updateService.ts` → tu `owner/repo`

Es el único que editas a mano. El certificado de firma se **crea solo** la primera vez que
ejecutas `npm run build:standalone`, y la huella de su clave pública se escribe en
`signer-policy.json` para que la confirmes en el repositorio. Cada compilación posterior
vuelve a comprobar que el certificado con el que va a firmar sigue siendo el fijado, y se
detiene si no lo es: de lo contrario, recompilar en otra máquina produciría en silencio
una versión que todos los clientes instalados rechazan, sin que nada parezca ir mal hasta
que un usuario pulse Actualizar.

**El certificado no tiene por qué costar dinero.** Incrustar la firma dentro del exe y
pagar a una autoridad de certificación son dos cosas independientes: al formato
Authenticode no le importa quién emitió el certificado, y fijar la clave pública en el
cliente es de hecho *más fuerte* que confiar en una CA: un atacante necesitaría tu clave
privada, no un certificado con el mismo nombre emitido por cualquiera de los cientos de
CA públicas. Lo que se compra con dinero es reputación en SmartScreen, es decir, que no
aparezca el aviso "Windows protegió su PC" en la primera ejecución.

Si dejas `SIGNER_POLICY` vacío, la aplicación sigue funcionando: simplemente se niega a
actualizarse a sí misma y lo explica.

Procedimiento completo, incluido qué ocurre cuando caduca el certificado y por qué la
release contiene exactamente dos:
[`docs/standalone-exe-release.md`](docs/standalone-exe-release.md)

---

## Licencia

Tessera tiene **licencia dual**. Eliges una; no necesitas cumplir ambas.

| Tu caso | Licencia | Coste |
|---|---|---|
| Uso personal, o interno en tu empresa, sin redistribuir | **AGPL-3.0** | Gratis |
| Estudiar, bifurcar, parchear, publicar tu fork | **AGPL-3.0** | Gratis |
| Distribuirlo (o código derivado) dentro de un producto de **código cerrado** | **Comercial** | De pago |
| Ofrecerlo como **servicio alojado** sin publicar tu código | **Comercial** | De pago |

**Edición comunitaria — [GNU AGPL-3.0](LICENSE).** Úsala para lo que quieras, incluido
uso comercial. La única obligación: si distribuyes el software, una versión modificada,
o permites que otros lo usen **a través de una red**, esos usuarios deben poder obtener
el código fuente completo correspondiente, también bajo AGPL. El uso personal y el uso
interno en una empresa no activan ninguna de las dos cosas.

**Edición comercial — [condiciones](LICENSE-COMMERCIAL.md).** El mismo software, sin la
obligación de divulgar el código, con garantías y un canal de soporte que la edición
AGPL descarta explícitamente. Consultas: `<your-licensing-email>`.

El árbol de decisión, las condiciones para contribuir que hacen posible la licencia
dual, y los dos puntos de la AGPL que casi todo el mundo malinterpreta están en
[`LICENSING.md`](LICENSING.md).

Los componentes de terceros (Electron, React, SQLCipher, la biblioteca de Python
`cryptography`, tesseract.js y otros) conservan sus propias licencias: ninguna de las
dos licencias anteriores las modifica, y nada de lo aquí escrito restringe los derechos
que ya tengas bajo ellas. Inventario completo:
[`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md), visible también en la aplicación en
**Ajustes → Acerca de**.

Las licencias cubren el código, no el nombre del proyecto ni su logotipo. Los forks son
bienvenidos: ponles otro nombre.
⁢‌‌​​‌‌‌⁠‌‌‌⁡‌​⁡‌​⁡​⁡‌⁡⁡​‌‌‌‌​⁡​⁠‌​‌⁠‌⁡​⁡‌⁠‌​‌​‌⁡‌‌‌⁠‌⁠⁠⁡‌‌⁠​​⁡​‌​⁡⁠​‌⁡‌⁡‌⁠‌⁠‌‌‌​‌‌‌⁡‌​⁠​​⁠⁠⁡‌⁠​⁡‌⁡‌​​⁡​‌‌⁠⁠⁡​⁡​⁠‌​⁡‌‌⁡⁠‌​⁡‌⁡‌​⁡‌‌⁠​⁠‌⁡⁠​‌⁠⁡⁡‌​​‌​⁠⁠⁡​⁡‌⁠‌⁠⁠​‌​⁠⁠‌‌‌⁡​⁠⁡⁡‌⁡​⁡​⁡⁠‌‌⁡‌​‌⁡​⁡‌⁡​⁠‌⁠⁠⁠‌‌‌⁠‌​​⁡‌‌​⁠‌⁡​‌‌​⁡⁠‌​​⁡‌⁠⁠​‌​⁠‌‌​⁡⁠‌⁠​⁠‌​⁡‌​⁡‌⁠​⁡‌​‌​​⁠‌⁡‌‌‌⁠‌‌‌⁠‌⁡‌‌‌‌‌⁠⁠⁠​⁡​‌‌⁡‌‌‌​‌‌‌⁠​⁡‌⁠‌​‌⁡⁠‌‌‌​⁠‌​​⁠‌⁠⁡​​⁡​⁠​⁠⁡⁡‌​⁠⁠‌⁡​‌‌‌‌⁠​⁡​⁡​⁠⁠⁡‌⁠‌⁡​⁡‌⁠​⁡⁠​‌​​⁡‌‌‌​​⁡⁠‌‌‌⁠‌‌⁡‌⁡​⁡‌‌​⁡‌⁠‌‌‌​‌⁡​​​⁡⁠‌​⁡​​‌⁠​‌‌​‌​‌​⁠​‌​⁡⁠‌‌‌​‌‌⁠​‌⁡​‌‌⁠‌⁡‌​⁡‌‌⁠⁡‌‌‌⁠‌​⁡​‌‌⁠‌⁡‌‌⁠‌‌⁠⁠‌‌​⁠⁠‌‌⁠‌‌​⁡⁠‌⁡​‌​⁡​‌‌​⁡‌‌​‌⁠‌⁠⁠​‌⁡⁠⁠‌‌⁠⁠‌​⁠‌‌⁠​⁡‌⁡‌​‌‌​‌‌⁠⁡⁠‌​⁡⁠​⁡​​‌⁡⁠⁠​⁡​⁠‌⁠⁡⁠​⁡​⁡‌⁠⁠⁠‌​​⁠​⁡​​‌​⁡⁡‌⁡​⁠‌​⁠⁡‌⁡⁠‌‌​⁠⁠​⁡​​‌⁠⁠⁠‌​⁠⁠‌⁡​⁡‌⁠⁠​‌⁡⁠​‌‌⁠​‌​​‌‌⁡⁠⁠‌‌​​‌‌‌‌‌⁠⁠‌‌⁠⁠​‌⁡⁠​​⁡⁠​‌​‌‌‌​​⁡​⁡​​‌​⁠‌‌​⁡​‌⁡‌​‌​⁡⁠​⁡‌⁡‌​⁡​‌⁡⁠⁠​⁡​‌‌⁠⁡‌‌‌​‌‌⁠‌​‌⁡‌⁡‌‌​‌​⁡⁠‌‌⁠⁠⁠‌​⁡‌‌​​‌‌‌⁠⁠‌⁠⁡​‌‌⁠‌‌​⁡⁡‌⁠⁡⁡‌⁠‌​‌⁡⁠‌‌​​⁠‌⁡⁠‌​⁡​⁡​⁡‌⁡​⁡​‌‌​‌⁠‌⁠⁡⁡‌​⁠⁠‌⁠‌‌‌​⁠​‌‌​‌‌‌⁠⁠‌​⁠⁠‌‌​‌‌⁠​⁠‌​⁡⁡​⁡​⁠​⁡‌​‌‌​‌‌​⁠​‌‌⁠⁠‌‌‌​‌‌‌‌‌⁠‌⁡‌⁡‌​‌⁠⁡⁡‌⁠⁠‌​⁡‌⁠‌​‌⁡​⁡⁠‌‌⁠‌⁡‌​​‌‌‌⁠‌‌‌‌​​⁡​⁡‌​⁡⁠‌‌‌‌‌⁡​‌​⁡⁠​​⁡​‌‌⁠⁡‌​⁡​⁠‌​​⁠‌​​‌​⁡‌⁡‌‌⁠⁠‌⁡‌⁠‌​‌​‌⁡​⁡‌⁠‌⁡‌​‌⁡‌⁡​⁠‌⁡​⁠‌‌‌⁠‌‌‌⁠​⁡‌‌‌⁠⁡⁠‌‌⁠‌‌‌​‌‌⁡⁠​‌⁠⁡​‌⁠‌⁡​⁡‌⁠‌‌​‌‌⁠​‌​⁡​⁡‌‌‌⁡‌⁡​⁠‌‌⁠​​⁠⁠⁡‌‌​​‌​⁠⁡​⁡‌‌‌‌​‌‌⁡‌‌‌​⁠‌‌⁡‌⁠‌​⁠⁡‌⁠​‌​⁡​​‌⁠‌⁡‌​​‌‌⁠​⁠​⁠⁠⁡​⁡‌​​⁡​⁠‌⁡​​‌‌‌‌​⁡⁠​‌‌⁠⁠​⁡​​‌⁠⁡⁡‌‌⁠‌‌⁡​​‌​‌⁡‌​⁡​‌‌‌​‌​⁠⁠‌⁠⁠⁠‌‌‌⁠‌​⁡​‌​⁡⁡‌⁠⁡‌‌⁠⁡⁠‌​⁠⁡‌​⁡​‌‌​⁡‌⁠‌⁠​⁠⁡⁡‌⁡‌​​⁠⁡⁡‌‌⁠⁠‌⁠​⁠​⁡​⁠‌​⁡‌‌‌⁠⁠​⁠⁠⁡‌‌⁠‌‌⁡‌​​⁡⁠‌‌⁠​‌‌​‌‌‌​⁡‌‌‌​⁠‌⁠‌‌‌‌​​‌​⁡⁠‌⁠⁡​‌⁠⁠⁡‌⁠‌‌‌​⁡‌​⁠⁡⁡‌⁠⁠​‌⁠⁠⁠‌​‌‌‌⁠⁡​‌⁡⁠‌‌⁡​‌‌​⁠​‌⁠⁠‌‌⁠‌⁡‌⁠​‌‌⁠⁠​‌​⁡‌​⁡​‌‌‌⁠⁠‌⁠‌⁡‌⁡⁠‌‌​⁡‌‌​⁠​‌​​⁠‌‌‌‌‌‌⁠​‌⁠​⁠‌⁡‌⁠‌⁠​‌‌‌‌‌‌⁠‌​‌⁡⁠⁠‌‌⁠‌​⁡⁠‌‌​⁡⁠‌‌‌‌‌⁡‌⁠‌⁠‌‌‌​⁠⁠‌​⁡‌‌​⁡⁠‌​‌‌​⁠⁡⁡‌​⁠⁡​⁡‌⁡‌​​⁠‌‌‌‌‌‌​⁡​⁡​‌‌⁠⁠⁠‌​⁡⁠‌⁠‌‌‌​​‌‌​‌⁠​⁡​⁡‌​​⁠‌​⁡⁠‌⁠⁡⁠‌⁡‌⁠‌‌​⁠‌​‌⁡‌⁡⁠​​⁡‌⁡‌⁡​⁡‌⁡​​‌⁠​‌​⁡⁠​‌⁠‌‌‌⁡⁠‌‌⁠‌⁡‌⁠⁠⁠‌‌​‌‌⁠⁠⁡‌⁠​⁡​⁠⁠⁡‌​⁠⁠‌⁡‌⁡‌⁠⁠⁡‌⁠⁡⁠‌⁡​‌​⁡​⁡‌⁠⁡⁡‌⁠​⁡‌⁡‌⁠‌​⁡​‌⁠‌​‌⁠⁠⁡‌‌‌‌‌⁠​⁡‌⁡⁠​‌⁠‌​‌⁠‌‌‌‌⁠⁠‌⁠‌‌‌⁠⁡⁡‌⁡‌​‌​‌​‌​‌⁡‌⁠⁠⁡‌​​‌​⁡‌‌​⁡‌⁠‌⁡⁠⁠​⁡​‌​⁡⁠‌‌⁠‌​‌⁠⁡⁡‌⁡​⁡‌​​‌‌​⁡​‌​⁡⁠​⁡‌​‌⁡⁠⁠​⁡‌⁠‌​⁠​​⁠⁠⁡​⁡‌⁡‌‌‌​‌‌​​​⁡​⁠‌‌​‌‌‌​​​⁡​⁠‌​‌‌‌‌​‌​⁠⁠⁡‌​‌⁠‌⁠‌⁡‌‌‌⁡‌⁠​‌‌‌‌‌‌​⁡‌‌​​‌‌⁠⁡⁠‌​⁠⁡‌‌⁠​‌⁡‌⁠​⁡​​‌⁠​⁠‌⁠​‌​⁡‌‌‌‌⁠⁠‌⁡‌​‌⁡‌⁡‌⁡​‌‌⁠⁠⁡‌‌​​‌‌‌​‌⁡‌⁠‌⁠​‌‌​⁠⁡‌⁠⁡⁡‌​⁠‌‌‌⁠⁠​⁡‌‌‌‌⁠‌‌​⁡‌‌⁠⁠​‌⁠​⁡​⁡‌​‌⁠⁡⁡‌⁡⁠​‌⁠​⁠​⁡​⁡‌​⁡​‌​⁠⁠‌​⁡⁠‌‌​​​⁡​⁡​⁡​​​⁠⁠⁡‌⁠‌​‌​⁠‌‌⁠​⁡‌​​⁠‌⁠⁡‌‌‌​‌‌⁠⁠⁠​⁡​⁠‌⁠⁠⁠‌‌‌‌​⁡‌​‌​⁡⁠‌​‌‌‌⁡‌​‌⁡​⁡‌‌‌‌‌⁡‌‌​⁡⁠‌‌⁡​⁡‌⁠⁠​‌⁠⁠​​⁡‌‌‌‌‌​‌⁠​⁡​⁡‌⁠‌​⁠⁡​⁡‌⁠‌​⁡‌​⁡⁠​‌​‌‌‌⁠‌⁠‌‌‌⁡‌⁠​⁠‌​⁡⁠‌⁠⁠​‌⁠​⁡​⁡⁠​​⁡‌⁠‌‌​​‌​⁠⁡‌​​⁠‌⁡‌⁠‌⁡‌⁡‌⁠⁡⁠​⁡​‌‌​⁡⁡​⁠⁠⁡‌​‌⁠‌‌‌⁡‌​⁡​‌⁠​⁠‌​⁡‌‌⁡⁠⁠‌‌​‌‌⁡‌⁡‌‌⁠⁠‌‌‌⁠‌‌‌​‌⁠‌⁡‌​​⁠​⁡‌⁠‌⁠‌‌‌⁠​⁡‌⁡​‌‌‌‌​​⁡‌‌‌⁠⁠⁡‌​⁡⁠‌⁠‌‌‌⁠⁠‌‌⁠‌⁠‌⁡‌​‌⁡​‌‌⁠⁡⁠‌⁠‌⁡‌​⁠⁠‌‌⁠‌‌⁠⁠‌​⁡⁠​‌‌​⁠‌‌​⁡​⁡​​‌⁠‌‌‌​⁡‌​⁡​‌​⁡⁠​‌​⁠​​⁡‌‌‌​‌⁠‌⁠⁡⁠‌​‌​‌⁠⁡​‌⁠‌‌‌​⁠‌‌‌⁠‌‌⁠​‌‌⁡​‌‌⁡⁠‌‌⁠⁡​​⁠⁡⁡‌‌⁠​‌⁠⁡⁡‌‌‌‌​⁡⁠​‌‌‌‌‌⁠⁡​‌⁡​⁠‌⁠⁡⁡‌​​⁠‌​⁠⁠​⁡​⁠‌‌⁠⁠‌⁠​⁠‌⁠‌⁡‌⁠⁡​​⁡​⁡‌‌⁠‌​⁡​⁡‌⁠‌⁡​⁡⁠​‌⁡​⁡‌⁠⁡⁡​⁡‌⁡‌​​⁡‌⁠⁠⁡‌⁠⁠​​⁡⁠‌‌⁠​⁠‌⁡​​‌⁠⁡‌‌​‌‌‌⁡‌⁡‌⁠⁡⁠‌​⁠‌‌​⁡⁡‌⁡⁠​‌‌‌⁡‌⁠‌​‌⁠⁠⁠‌⁠⁠​​⁡⁠‌‌​⁠⁡‌​⁡​‌​⁠​‌⁠​‌‌⁠​⁠‌​​⁠‌‌‌​‌⁡​​‌​‌‌‌⁠⁠⁡‌​⁡​‌⁠⁠⁡‌​​‌‌⁡‌⁡‌⁡​⁡‌⁠⁠⁡​⁡​⁡‌⁡⁠⁠‌​‌​​⁡‌​‌​‌‌‌‌⁠‌‌⁠​⁡‌⁠⁠⁠​⁡​‌‌⁠⁠​‌⁠‌​​⁡‌⁡​⁡​​‌​⁠⁡‌⁡‌​​⁡‌⁡‌​⁠‌‌⁡‌⁡‌⁡⁠​‌⁠⁠‌‌⁠‌‌‌‌‌​‌⁡‌⁡​⁡‌⁠‌​‌⁡‌​⁠⁠‌​​‌‌⁠⁡⁠​⁠⁡⁡‌​⁡​‌⁠⁡​‌⁠‌⁡‌⁡​​​⁡‌‌‌⁠⁡⁡‌⁠​‌‌⁡​‌‌⁠⁠​‌​​⁠‌​⁡⁡‌⁠⁠​‌⁠⁠⁠‌‌​‌​⁡​⁠​⁡⁠​‌⁠​⁠‌​⁠​​⁡‌⁠‌⁡‌​‌​⁡‌‌​⁠‌​⁡⁡‌⁢