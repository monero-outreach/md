---
title: Especificación técnica de Monero 
---
# Especificaciones técnicas de Monero 

## En Vivo 

* La cadena de bloques de Monero está disponible desde el 18 de abril de 2014 

## Sin premine, sin instamine, sin ICO, sin token 

* Monero no tuvo premine ni instamine
* Monero no vendió ningún token
* Monero no tuvo preventa de ningún tipo 

## Prueba de trabajo 

* CryptoNight
    ** v0 desde la altura del bloque 0
     * v1 desde la altura del bloque 1546000 (bifurcado el 6 de abril de 2018)
     * v2 desde la altura del bloque 1685555 (bifurcado el 18/10/2018)
     * v3 desde la altura del bloque 1788000 (bifurcado el 2019-03-09); "CryptonightR" 
* RandomX
    * v0 desde la altura del bloque 1978433 (bifurcado el 30/11/2019) 

## Reorientar la dificultad 

* cada bloque
* basado en los últimos 720 bloques (24 h), excluyendo el 20% de los valores atípicos de la marca de tiempo 

## Tiempo de bloque 

* 2 minutos
* puede cambiar en el futuro siempre que se mantenga la curva de emisión 

## Recompensa de bloque 

* disminuyendo suavemente y sujeto a penalizaciones por bloques mayores que el tamaño medio de los últimos 100 bloques (M100) 
* ~1.6 XMR a junio de 2020; para ver la recompensa actual, verifique la transacción de la base de monedas del [último bloque](https://xmrchain.net/)

## Tamaño de bloque

* dinámica
* máximo dos veces el tamaño medio de los últimos 100 bloques (2 * M100)
* ~ 50 KB a junio de 2020; comprobar [el último tamaño de bloque] (https://bitinfocharts.com/comparison/monero-size.html#3m)

## Curva de emisión 

### Emisión principal 

* Primero, la emisión principal está a punto de producir ~ 18.132 millones de monedas para fines de mayo de 2022.
* a partir de junio de 2020, la emisión es de aproximadamente 8 XMR por 10 minutos
* ver [gráficos y detalles] (https://www.reddit.com/r/Monero/comments/512kwh/useful_for_learning_about_monero_coin_emission/)

### Emisión de cola

* la emisión de cola se activa una vez que se realiza la emisión principal
* producirá 0.6 XMR por bloque de 2 minutos
* esto se traduce en una inflación <1% que disminuye con el tiempo 

## Suministro máximo 

* ~18.132 millones de XMR + 0.6 XMR por 2 minutos
* técnicamente infinito pero prácticamente deflacionario si se contabilizan las monedas perdidas 

## Divisibilidad

* Monero es divisible hasta 12 dígitos
* La unidad más pequeña se llama piconero y es igual a 1e-12 XMR, o 0.000000000001 XMR 

## Privacidad del remitente 

* firmas de anillo
     * el tamaño del anillo es 11 (10 señuelos)
* aseguramiento: negación probabilística / plausible 

## Privacidad del destinatario

* direcciones furtivas
* garantía: fuerte

## Cantidad de privacidad

* anillo de transacciones confidenciales
* garantía: fuerte

## Privacidad de la dirección IP

Para el nodo completo (`monerod`):

* dandelion++
* garantía: no protegerá contra el proveedor de ISP / VPN, no protegerá contra el primer nodo remoto en el protocolo Dandellion ++
* para la protección total, el usuario debe envolver manualmente `monerod` con Tor

Para la billetera (`monero-wallet-gui` o` monero-wallet-cli`):

* por lo general, la billetera se ejecuta en la misma máquina que el nodo completo, por lo que no hay riesgo
* si la billetera se conecta al nodo completo remoto, no hay protección IP por defecto
     * el usuario debe envolver manualmente la billetera con Tor 
