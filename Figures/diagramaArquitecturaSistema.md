# Diagrama de arquitectura del sistema

Uso previsto: seccion 3.1, Arquitectura del sistema.

- El contenedor azul representa el componente desarrollado en este trabajo.
- Los bloques internos de `sfy-BLE` corresponden a los modulos de ciclo de vida, protocolo BLE, procesamiento, seguridad e integracion.
- Los bloques grises representan componentes preexistentes del firmware N150.
- Las lineas punteadas representan integracion mediante callbacks.
- Las lineas continuas representan dependencias directas de `sfy-BLE`.
- El diagrama representa el flujo funcional de procesamiento, no un diagrama estricto de llamadas entre archivos fuente.
- El cliente BLE se muestra fuera del limite del firmware porque no forma parte del desarrollo realizado.

```mermaid
flowchart LR
    Client[Cliente BLE externo]

    subgraph N150[Firmware del controlador N150]
        NimBLE[NimBLE<br/>Pila BLE]

        subgraph BLE[sfy-BLE - Modulo desarrollado]
            subgraph Protocol[Gestion de la interfaz BLE]
                Main[ble-Main<br/>Ciclo de vida y host NimBLE]
                Gap[ble-Gap<br/>Advertising y conexion]
                Gatt[ble-Gatt<br/>Servicios y caracteristicas]
            end

            subgraph CrossCutting[Servicios transversales]
                RcvMsg[ble-rcvMsg<br/>Reensamblado]
                Security[ble-Security<br/>Autenticacion y cifrado]
            end

            subgraph Functional[Adaptadores funcionales]
                Commands[ble-Commands<br/>Comandos]
                DataModule[ble-Data<br/>Datos e historico]
                ParamsModule[ble-Params<br/>Parametros]
                Logs[ble-Logs<br/>Registros y terminal]
            end
        end

        Params[Gestion de parametros]
        Control[Controlador y<br/>rutinas de prueba]
        Cloud[CloudLogic y<br/>conectividad remota]
        Diag[Metadatos y<br/>diagnostico]
        Data[Datos persistentes<br/>e historico]
        Logs[Sistema de registros<br/>y terminal]
    end

    Client <--> NimBLE
    NimBLE <--> Main
    Main <--> Gap
    Main <--> Gatt

    Gatt <-->|mensajes fragmentados| RcvMsg
    RcvMsg <-->|mensaje completo| Security
    Gatt <-->|autenticacion y mensajes breves| Security

    Security <--> Commands
    Security <--> DataModule
    Security <--> ParamsModule
    Security <--> Logs

    Commands -. callbacks .-> Control
    Commands -. callbacks .-> Cloud
    DataModule -. callbacks .-> Diag
    ParamsModule -. callbacks .-> Params

    Gap <--> Data
    DataModule <--> Data
    Logs <--> LogsSystem[Sistema de registros<br/>del N150]

    classDef developed fill:#1565c0,color:#ffffff,stroke:#0d47a1,stroke-width:2px;
    classDef existing fill:#e0e0e0,color:#000000,stroke:#757575,stroke-width:1px;
    classDef external fill:#ffffff,color:#000000,stroke:#424242,stroke-width:1px;

    class Main,Gap,Gatt,RcvMsg,Security,Commands,DataModule,ParamsModule,Logs developed;
    class NimBLE,Params,Control,Cloud,Diag,Data,LogsSystem existing;
    class Client external;
    style BLE fill:#bbdefb,stroke:#0d47a1,stroke-width:2px
```

Leyenda para la imagen final:

| Elemento | Significado |
|---|---|
| Azul | Componente desarrollado: `sfy-BLE` y sus bloques internos. |
| Gris | Subsistema preexistente del N150. |
| Linea punteada | Comunicacion mediante callbacks. |
| Linea continua | Dependencia directa del componente BLE. |
| Flujo central | Reensamblado, validacion de seguridad y entrega a los adaptadores funcionales. |

Caption sugerido: `Arquitectura general del firmware N150 e integracion del modulo sfy-BLE.`
