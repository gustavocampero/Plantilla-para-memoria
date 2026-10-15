# Contexto de sesión: memoria `sfy-BLE`

## Objetivo actual

Preparar una memoria académica en LaTeX sobre el desarrollo del componente
`sfy-BLE` del sistema Sensify N150. El capítulo 3 debe contener solamente las
secciones 3.1 y 3.2, dedicadas a la arquitectura y al diseño por módulos.

## Criterios de redacción acordados

- Usar redacción impersonal.
- Usar presente para describir el sistema y pasado para el trabajo realizado.
- No incluir código fuente en la memoria; usar diagramas y tablas cuando aporte
  claridad. Excepción aprobada: listados breves con `lstlisting` (estilo en el
  preámbulo, caption "Código"), como el fragmento de la tabla GATT en 3.5.2
  (`lst:tabla_gatt`). Nunca capturas de pantalla de código.
- Usar `\texttt{}` solo para identificadores literales, archivos, módulos y
  variables.
- Escribir anglicismos generales en cursiva, por ejemplo `\textit{advertising}`.
  Excepción (corrección del docente): "callback(s)" va sin cursiva.
- Desarrollar cada sigla la primera vez que aparece, con el formato
  "nombre en español (SIGLA, del inglés, \textit{English name})", y luego usar
  solo la sigla.
- Términos alineados con la figura `flujoAdvDatos.jpg` (draw.io):
  `\textit{scan response}`, `\textit{scan request}`, `\textit{display} virtual`,
  "subsistema de `\textit{data}`".
- Denominar CloudLogic como "lógica de nube y conectividad remota".
- Excluir de la memoria y los diagramas `ble-Scanner`, escaneo pasivo, `BLINK`
  y señalización visual.

## Alcance técnico confirmado

- El desarrollo documentado es únicamente `sfy-BLE`.
- El cliente BLE y los subsistemas propios de la N150 son componentes externos
  o preexistentes.
- Las integraciones por `\textit{callbacks}` comprenden metadatos, diagnóstico,
  parámetros, comandos locales y lógica de nube y conectividad remota.
- Las dependencias directas son el almacenamiento persistente/histórico y el
  sistema de registros.
- `ble-rcvMsg` se separó para reutilizar la lógica de fragmentación.
- `ble-Security` es transversal a la autenticación y a la protección de los
  mensajes.
- `ble-Logs` se utilizó para soporte y depuración de la comunicación BLE.
- La modularización buscó separar responsabilidades, facilitar el mantenimiento
  y permitir pruebas independientes.
- La versión efectiva de ESP-IDF identificada en el repositorio es 5.3.2.

## Cambios completados

- Se revisaron los capítulos 1 y 2.
- `Chapters/Chapter2.tex` y `references.bib` fueron ajustados a ESP-IDF 5.3.2.
- Se añadieron o actualizaron las referencias `bluetooth:core53`,
  `espressif:esp-idf-ble` y `ashrae:refrigeration2022`.
- La figura 3.1 (`fig:arquitectura_sistema`) usa
  `Figures/arquitecturaDiagrama2.png`, provista por el usuario. La versión
  anterior y su fuente Mermaid se borraron.
- Se reescribió la sección 3.1 de `Chapters/Chapter3.tex` para precisar el
  alcance de `sfy-BLE`, sus límites, las dependencias, los `\textit{callbacks}`
  y el flujo funcional.
- Se eliminaron las secciones 3.3 a 3.7. `Chapters/Chapter3.tex` ahora contiene
  solamente 3.1 y 3.2.
- La sección 3.2 incluye estos módulos reales: `ble-Main`, `ble-Gap`,
  `ble-Gatt`, `ble-rcvMsg`, `ble-Security`, `ble-Data`, `ble-Params`,
  `ble-Commands` y `ble-Logs`.

## Estado de compilación

El PDF se generó correctamente en `memorianueva.pdf` (35 páginas) usando:

```powershell
pdflatex -interaction=nonstopmode -halt-on-error memorianueva.tex
bibtex memorianueva
pdflatex -interaction=nonstopmode -halt-on-error memorianueva.tex
```

`latexmk` no está disponible porque MiKTeX no encuentra Perl.

Advertencias actuales no bloqueantes:

- Aviso genérico de referencias indefinidas y solicitud de volver a ejecutar
  BibTeX, aunque BibTeX procesa correctamente las entradas citadas.
- Destinos de página duplicados en el capítulo 1.
- Cajas subllenadas en una tabla del capítulo 1.
- Dos cajas sobresalientes en referencias bibliográficas largas.
- `Figures/fondo.pdf` usa PDF 1.6, superior a la versión 1.5 admitida por
  `pdflatex`.

## Correcciones del docente (en curso, una por una)

- Hecho: figuras `Sensify.jpg` (1.1, antes del párrafo de datos de 2026),
  `N150_render.jpg` y `oem.jpg` (placa con BG95). Se borró el PNG del render.
- Hecho: el apartado 1.1.1 "El controlador N150" pasó al capítulo 2 como
  sección 2.2 (`sec:controlador_n150`); el capítulo 1 solo lo nombra.

- Correcciones del docente terminadas.
- Se escribió la sección 3.3 "Configuración BLE implementada"
  (`sec:configuracion_ble`): 3.3.1 Pila NimBLE (tabla `tab:config_nimble`,
  movida desde `ble-Main`), 3.3.2 Advertising y modos (tabla
  `tab:modos_app_link` y modo con datos, movidos desde `ble-Gap`), 3.3.3
  Parámetros de conexión, 3.3.4 Servicios GATT (tabla `tab:servicios_gatt`).
  En 3.2 quedaron referencias a 3.3.
- Nueva estructura del capítulo 3 (pedida por el usuario):
  3.3 Configuración BLE implementada (solo opciones de Bluetooth de ESP-IDF:
  disponibles, por defecto, modificadas y motivo) — escrita;
  3.4 Diseño de la capa GAP; 3.5 Diseño de la capa GATT;
  3.6 Capa de autenticación y encriptación; 3.7 Integración del sistema.
  El material de advertising/APP_LINK/conexión/GATT retirado de 3.3 quedó en
  `Chapters/borrador_gap_gatt.tex` (no incluido) para 3.4 y 3.5.
- Valores por defecto verificados en el Kconfig de ESP-IDF 5.3.2
  (`C:\Users\Gus\esp\esp-idf-v5.3.2\components\bt`). Para ESP32: MSYS_1 12x256,
  MSYS_2 24x320, ACL 24x255, 3 conexiones, todos los roles, log INFO, host
  Bluedroid. La medición 43 740 → 13 980 B es respecto de la configuración del
  comienzo del desarrollo, no de los valores por defecto.
- El ESP32 del N150 tiene 2 MB de PSRAM, que el firmware no usaba antes de
  BLE. Sin PSRAM la pila no entraba en la memoria interna. NimBLE asigna en
  PSRAM (`BT_NIMBLE_MEM_ALLOC_MODE_EXTERNAL`); el controlador reserva 56 156 B
  fijos de RAM interna (`BTDM_RESERVE_DRAM=0xdb5c`). Documentado en 3.3.3.
- Sección 3.4 "Diseño de la capa GAP" escrita (`sec:diseno_gap`): identificación,
  contenido del advertising (tabla `tab:contenido_adv`), modos APP_LINK (tabla
  `tab:modos_app_link`), advertising con datos ("visor virtual" de heladeras
  cercanas sin conectarse; payload = formato binario de telemetría a la nube,
  23 B), gestión de la conexión. Motivos dados por el usuario: APP_LINK para
  adaptar a cada cliente; modo LINK = menor visibilidad + menor actividad de
  radio; modo con datos = consola de diagnóstico en la app sin conectarse.
- Sección 3.5 "Diseño de la capa GATT" escrita (`sec:diseno_gatt`):
  organización (tabla `tab:servicios_gatt`; solicitud por escritura + respuesta
  por indicación por tamaño/asincronía y confirmación de entrega), extensión
  (tabla declarativa, UUID base + byte, mecanismos genéricos de indicación y
  notificación, comandos binarios), comandos/estados/formatos (tabla
  `tab:comandos_gatt`, sin BLINK), transmisión, descarga transaccional del
  histórico (propósito: recuperar datos pendientes sin cobertura). El usuario
  no quiere mencionar limitaciones de la extensibilidad.
  Se borró `Chapters/borrador_gap_gatt.tex`. En 3.5.4, la trama
  `SEQ=..;N=..;LEN=..;CRC=..;F=..;<payload>` con tabla `tab:trama_fragmento`.
  JSON se mantuvo por ser el formato de los canales de conectividad remota
  existentes (no nombrar Wi-Fi).
- Sección 3.6 "Capa de autenticación y encriptación" escrita
  (`sec:autenticacion`): motivos para no usar bonding (experiencia del técnico,
  muchos teléfonos por equipo, autorización de la app oficial, control del
  protocolo), autenticación por desafío-respuesta en AUTH-SYNC (HMAC-SHA256,
  clave de sesión por conexión), trama segura AES-256-GCM con tipo y contador,
  gestión de sesión y fallas. Fuente: `ble-Security.cpp` y la conversación
  exportada `autenticaci-n-y-cifrado-bt-sin-bond.json`.
  Información sensible: NO incluir figuras, formatos de trama a nivel de bytes,
  fórmula de derivación, ofuscación/almacenamiento de la clave ni limitaciones.
- Sección 3.7 "Integración del sistema" escrita (`sec:integracion`): tabla
  `tab:integracion` y apartados Histórico y sincronización horaria (TIME-SYNC
  va aquí, no como apartado propio: se hizo porque sin hora correcta los datos
  guardados no servían), Parámetros (incluye APP_LINK como parámetro y reporte
  forzado a la plataforma), Pruebas de actuadores, Metadatos y diagnóstico
  (metadata = buildMeta de la lógica de nube), Comandos para la lógica de nube,
  Registros (LOG-SEND, LOG-TERMINAL, LOG-CONTROL). Se borró el borrador.
  Figuras (draw.io del usuario): `historico.jpg` (3.7.1, `fig:flujo_historico`),
  `params.jpg` (3.7.2, solo escritura, `fig:flujo_parametros`),
  `comando.jpg` (3.7.3, comando genérico, `fig:flujo_comando`; 3.7.4 remite a él).
- Seguridad: se tratará en un apartado posterior. Por ahora no describir el
  mecanismo (se quitaron la fila "Control de acceso" de la tabla 2.1, el párrafo
  de emparejamiento de 2.4.2 y la frase de seguridad de 3.3.3). El N150 no usa
  emparejamiento BLE; la protección es de capa de aplicación (`ble-Security`).
- Preámbulo: `placeins`, `array` con columna `L{}` (sin justificar) y páginas
  de flotantes alineadas arriba (`\@fptop=0pt`).

## Próximo paso recomendado

`ble-Main` ya fue revisado contra el código (estados, mutex recursivo compartido
con `ble-Params`, `APP_LINK`, inicio en sincronización, detención con
reversión). Se agregó la tabla `tab:config_nimble` (decisión de diseño de
memoria de NimBLE; no mencionar el rol OBSERVER, queda configurado pero sin uso)
y, en `ble-Gap`, la tabla `tab:modos_app_link` y el modo de *advertising* con
datos (último registro de telemetría sin necesidad de conexión). El usuario no
quiere corregir la frase de `ble-Gap` sobre la construcción del nombre.
La versión a entregar tiene OBSERVER desactivado y 12 bloques MSYS_1; la
medición (43 740 → 13 980 B, 29 760 B liberados) se presenta como exacta.
No justificar en la memoria que los datos del *advertising* van sin cifrar.

Continuar la revisión de la sección 3.2 módulo por módulo, desde `ble-Gap`, para
confirmar que cada descripción sea fiel a la implementación y no repita la
arquitectura ya explicada en 3.1. Consultar al usuario antes de afirmar
decisiones estratégicas que no se puedan comprobar en el código.

## Archivos relevantes

- `memorianueva.tex`: documento principal.
- `memorianueva.pdf`: PDF generado.
- `Chapters/Chapter1.tex`: capítulo 1 revisado.
- `Chapters/Chapter2.tex`: capítulo 2; usa ESP-IDF 5.3.2.
- `Chapters/Chapter3.tex`: capítulo activo (3.1 a 3.7; 3.6 pendiente).
- `references.bib`: bibliografía.
- `Figures/arquitecturaDiagrama2.png`: figura usada en 3.1.
- `C:\Users\Gus\Documents\GitHub\Sensify-N150V2r2\sfy-core\sfy-BLE`: fuente
  de la implementación relevada.
- `C:\Users\Gus\Documents\GitHub\Sensify-N150V2r2\main\_src\main.cpp`:
  inicialización e integración de BLE.
