# Pendientes del autor: capítulo 4 (Ensayos y resultados)

Fuentes: `sfy-base/workspace/Sensify-N150V2r2/sfy-core/sfy-BLE/docs/ensayos/` (rama `docs/ensayos-ble`):
`plan.md`, `README.md`, `fichas/` y `evidencia/`. No se modificó ningún archivo de esa carpeta.

## Contradicciones entre fuentes

1. **CM-10, historial real en E2.** La consigna dice que el historial real se probó "en E2 y E3". La ficha
   `CM-10.md` y `ES-E2-matriz.json` muestran que en E2 hubo **0 eventos pendientes** en las 6 pasadas
   (1 frame, la cantidad). El historial real solo aparece en E3. El capítulo sigue la ficha: "con historial
   real, en las repeticiones del escenario E3".
2. **UT-03.** La consigna lo lista entre los "aprobados sin reservas"; la ficha dice `APROBADO, con una
   limitación de alcance` (el CRC de la ROM se reemplaza en PC por una reimplementación). El capítulo lo
   informa como aprobado y aclara la limitación.
3. **Cantidad de casos unitarios.** `plan.md` dice 41 casos en el apartado 6 y "8 ensayos, 40 casos" en el
   apartado 8. La suma de las fichas da 41 (4+7+3+6+3+4+9+5). El capítulo usa 41.
4. **`README.md` desactualizado.** La tabla "Estado del trabajo" (paso 4) dice "E3 no aprobado"; el veredicto
   vigente es inconcluso. El capítulo usa el veredicto de la ficha `ES-E3.md`.
5. **Sondas de la NVC3.** `IN-03.md` (2026-10-28) dice que la placa no tiene controlador conectado (las tres
   sondas en circuito abierto); `CM-04.md` (2026-10-29) dice que tenía una sonda interior conectada
   (`tint:OK`). Son días distintos. El capítulo informa cada uno en su contexto; confirmar que no hay
   contradicción.

## Datos faltantes

6. **Prueba de `LOG-SEND` (10 de 10).** No tiene evidencia archivada, fecha, hora ni método. El capítulo lo
   dice así. Si existe registro de esa prueba, conviene archivarlo.
7. **Repetición de E3 con la PC con Windows.** No está hecha; E3, R3 y R6 quedan sin veredicto definitivo.

## Decisiones a confirmar

8. ~~Primera persona en 4.1.4~~: resuelto, el autor pidió redacción impersonal ("el autor").
9. **Hallazgos de seguridad no incluidos.** No se incluyeron los hallazgos 1, 2, 3, 5 y 17 (clave global
   común a todos los equipos, comandos destructivos por BLE, reenvío del intercambio inicial, byte reservado
   sin autenticar, segunda llamada a `ble_build_key()`), porque en la sección 3.6 se decidió no incluir
   limitaciones del esquema de seguridad. Confirmar si van en el capítulo 4.
10. **Wi-Fi.** El hallazgo 19 (el firmware levanta un punto de acceso Wi-Fi tras 60 min sin MQTT y consume
    ~72 KB de RAM interna) se redactó sin nombrar Wi-Fi ("una función propia del firmware"), por la decisión
    de no mencionarlo en la memoria. Confirmar la redacción.
11. **SE-02 y `BLINK`.** La ficha apoya que ningún comando se ejecutó en la ausencia de líneas `BLINK` en el
    log. Como `BLINK` está excluido de la memoria, el capítulo no menciona esa verificación.
12. **Hallazgos 8 a 23 resumidos.** El análisis de resultados incluye los hallazgos 8, 9, 12, 13, 16, 18, 19 y
    20 en forma resumida; quedaron fuera 6 (documentación del CRC, mencionado en SE-06), 10 (`APP_LINK=1`,
    mencionado en CM-06), 11 (versión de git en metadatos, mencionado en CM-08) y 14, 21, 22 y 23
    (mencionados en sus ensayos). El hallazgo 15 (la línea comentada en `get_data_from_storage()`) no se
    incluyó porque la ficha `CM-10.md` dice que no se confirmó con historial real.
