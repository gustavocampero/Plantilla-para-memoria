# Contexto de sesión: memoria `sfy-BLE`

## Objetivo actual

Preparar una memoria académica en LaTeX sobre el desarrollo del componente
`sfy-BLE` del sistema Sensify N150. El capítulo 3 debe contener solamente las
secciones 3.1 y 3.2, dedicadas a la arquitectura y al diseño por módulos.

## Criterios de redacción acordados

- Usar redacción impersonal.
- Usar presente para describir el sistema y pasado para el trabajo realizado.
- No incluir código fuente en la memoria; usar diagramas y tablas cuando aporte
  claridad.
- Usar `\texttt{}` solo para identificadores literales, archivos, módulos y
  variables.
- Escribir anglicismos generales en cursiva, por ejemplo `\textit{advertising}`.
  Excepción (corrección del docente): "callback(s)" va sin cursiva.
- Desarrollar cada sigla la primera vez que aparece, con el formato
  "nombre en español (SIGLA, del inglés, \textit{English name})", y luego usar
  solo la sigla.
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
- Se insertó `Figures/arquitecturaDiagrama.png` en la sección 3.1, con la
  etiqueta `fig:arquitectura_sistema`.
- Se creó `Figures/diagramaArquitecturaSistema.md` con la fuente Mermaid del
  diagrama arquitectónico. Resalta `sfy-BLE`, muestra módulos preexistentes en
  gris y omite `ble-Scanner`.
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
- `Chapters/Chapter3.tex`: capítulo activo, limitado a 3.1 y 3.2.
- `references.bib`: bibliografía.
- `Figures/arquitecturaDiagrama.png`: figura usada en 3.1.
- `Figures/diagramaArquitecturaSistema.md`: fuente Mermaid del diagrama.
- `C:\Users\Gus\Documents\GitHub\Sensify-N150V2r2\sfy-core\sfy-BLE`: fuente
  de la implementación relevada.
- `C:\Users\Gus\Documents\GitHub\Sensify-N150V2r2\main\_src\main.cpp`:
  inicialización e integración de BLE.
