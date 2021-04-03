---
title: Interactuar con Monero
---
# Interactuar con Monero

Puede interactuar con Monero a través de la GUI de escritorio, la interfaz de línea de comandos y la API de programación. 

Además de eso, los nodos de Monero interactúan entre sí en una red de igual a igual. 

## Descripción general del directorio de instalación 

Una vez descomprimido, verá varios archivos ejecutables. También encontrará una buena guía en PDF para la billetera GUI. 

El proyecto Monero desacopla muy bien la lógica del nodo de red de la lógica de la billetera.
La lógica de billetera se ofrece a través de tres interfaces de usuario independientes: la GUI, la CLI y la API HTTP. 

```
# cd monero-gui-v0.17.1.9

# ---- guide to Monero GUI ----

monero-gui-wallet-guide.pdf

# ---- main executable files -----------

monerod
monero-wallet-gui

# ---- extra executable files -----------

extras/monero-wallet-cli
extras/monero-wallet-rpc

extras/monero-blockchain-prune
extras/monero-gen-trusted-multisig
extras/monero-gen-ssl-cert

extras/monero-blockchain-export
extras/monero-blockchain-import

# ---- don't bother with these ----------

extras/monero-blockchain-stats
extras/monero-blockchain-mark-spent-outputs
extras/monero-blockchain-prune-known-spent-data
extras/monero-blockchain-usage
extras/monero-blockchain-ancestry
extras/monero-blockchain-depth
```

## Ejecutables

| Ejecutables                 | Descripción
| -------------------------- |:-----------------------------------------------------------------------------------------------------------------------------------
| `monerod`                  | El daemon de nodo completo. No requiere billetera. <br />[Documentación](/interacting/monerod-reference/).
| `monero-wallet-gui`        | Lógica de billetera e interfaz __gráfica__ de usuario. <br />Requiere `monerod` en ejecución. 
| `monero-wallet-cli`        | Interfaz de usuario de __línea de comandos__ y lógica de billetera.  <br />Requiere `monerod` en ejecución. 
| `monero-wallet-rpc`        | Lógica de billetera y __HTTP API__ (protocolo JSON-RPC) . <br />Requiere `monerod` en ejecución.
| `monero-blockchain-prune`  | Pode la cadena de bloques local existente. Esto ahorra 2/3 del espacio en disco (hasta 31 GB en enero de 2021). Esto es preferible a `monerod --prune-blockchain` que solo libera espacio lógicamente dentro del archivo mientras el archivo permanece grande. El `monero-blockchain-prune` crea una copia reducida del archivo blockchain. Ver [tutorial1](https://monero.stackexchange.com/questions/11454/how-do-i-utilize-blockchain-pruning-in-the-gui-monero-wallet-gui), [tutorial2](https://www.publish0x.com/solareclipse/howto-prune-shrink-the-database-of-the-monero-blockchain-on-xpgwjx).
| `monero-gen-ssl-cert`      | Genere una clave privada RSA de 4096 bits y un certificado TLS autofirmado para usar con la interfaz RPC `monerod`. Tenga en cuenta que el daemon de Monero genera automáticamente un certificado TLS en cada reinicio. La generación manual con esta herramienta solo es útil si desea anclar la huella digital del certificado TLS en su billetera monero. Ver el [pull request](https://github.com/monero-project/monero/pull/5495).
| `monero-gen-trusted-multisig`          | Herramienta para generar un conjunto de carteras multifirma . <br />Ver capítulo sobre [multisignatures](/multisignature).
| `monero-blockchain-export` | Herramienta para exportar blockchain al archivo `blockchain.raw`. 
| `monero-blockchain-import` | Herramienta para importar [blockchain.raw] [blockchain.raw](https://downloads.getmonero.org/blockchain.raw) - idealmente su propia copia de confianza. 

## Ejecutables - heredado 

Lo más probable es que no deba preocuparse por estas herramientas heredadas o muy especializadas. 

| Ejecutables                 | Descripción
| -------------------------- |:-----------------------------------------------------------------------------------------------------------------------------------
| `monero-blockchain-stats`              | Genere estadísticas como tx / día, bloques / día, bytes / día según su cadena de bloques local. 
| `monero-blockchain-mark-spent-outputs` | Herramienta avanzada para mitigar posibles problemas de privacidad relacionados con las bifurcaciones Monero. Normalmente no deberías preocuparte por eso .<br />Ver el [commit](https://github.com/monero-project/monero/commit/df6fad4c627b99a5c3e2b91b69a0a1cc77c4be14#diff-0410fba131d9a7024ed4dcf9fb4a4e07) y [pull request](https://github.com/monero-project/monero/pull/3322).
| `monero-blockchain-prune-known-spent-data`  | Herramienta de poda limitada anterior para podar salidas de transacciones seleccionadas "gastadas conocidas" (de la era anterior a RCT). Hoy en día prefieren `monero-blockchain-prune`. Esto solo ahorra ~ 200 MB . Ver el [commit](https://github.com/monero-project/monero/commit/d855f9bb92dbfab707a0e37505906366de818a14).
| `monero-blockchain-usage`              | Herramienta avanzada para mitigar posibles problemas de privacidad relacionados con las bifurcaciones Monero. Normalmente no deberías preocuparte por eso .<br />Ver el [commit](https://github.com/monero-project/monero/commit/0590f62ab64cf023d397b995072035986931a6b4) y el [pull request](https://github.com/monero-project/monero/pull/3322).
| `monero-blockchain-ancestry`           | Herramienta de investigación avanzada para conocer los antepasados de una transacción, bloque o cadena. Irrelevante para usuarios normales. Mira esto [pull request](https://github.com/monero-project/monero/pull/4147/files).
| `monero-blockchain-depth`              | Herramienta de investigación avanzada para conocer la profundidad de una transacción, bloque o cadena. Irrelevante para usuarios normales. Mira esto  [commit](https://github.com/monero-project/monero/commit/289880d82d3cb206a2cf4ae67d2deacdab43d4f4#diff-34abcc1a0c100efb273bf36fb95ebfa0).


## Interactuando

Hay varias formas de interactuar con el software Monero. 
Quizás lo más sorprendente para los recién llegados es que el daemon `monerod` acepta comandos de teclado interactivos mientras se ejecuta. 

Además, tenga en cuenta que la API HTTP se divide en `monerod` y` monero-wallet-rpc`. Debe ejecutar y llamar a ambos daemons para explorar la API completa. 
Esto sigue la división de lógica de nodo vs lógica de billetera mencionada anteriormente. 

Todas las implementaciones de billetera dependen de la ejecución de `monerod`. 

| Ejecutable               | Red p2p          | comandos de nodo a través del teclado | nodo HTTP API | comandos de billetera a través del teclado | billetera HTTP API | billetera via GUI
| -------------------------- | ------------------- | -------------------------- | ------------- | ---------------------------- | --------------- | --------------
| `monerod`                  | ✔                   | ✔                          | ✔             |                              |                 |
| `monero-wallet-cli`        |                     |                            |               | ✔                            |                 |
| `monero-wallet-rpc`        |                     |                            |               |                              | ✔               |
| `monero-wallet-gui`        |                     |                            |               |                              |                 | ✔

## Directorio de datos

Aquí es donde se almacenan la cadena de bloques, los archivos de registro y la memoria de red p2p.

Por defecto, el directorio de datos está en:

* `$HOME/.bitmonero/` en Linux y macOS
* `C:\ProgramData\bitmonero\` en Windows

Por favor, tenga en cuenta: 

* el directorio de datos está oculto según la convención del sistema operativo 
* el nombre del directorio `bitmonero` es un artefacto histórico de antes de que Monero se separara de Bitmonero, aproximadamente 2000 años antes de Cristo 

El directorio de datos contiene: 

* `lmdb/` - el directorio de la base de datos de blockchain 
* `p2pstate.bin` - memoria guardada de compañeros descubiertos y clasificados
* `bitmonero.log` - archivo de registro 

También puede contener subdirectorios para stagenet y testnet, reflejando la misma estructura: 

* `stagenet/` - directorio de datos para Stagenet 
* `testnet/` - directorio de datos para Testnet
