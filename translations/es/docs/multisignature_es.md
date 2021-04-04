---
title: Multifirma 
---
# Multifirma

En las criptomonedas, la función de firma múltiple permite firmar una transacción con más de una clave privada. Los fondos protegidos con multisig solo se pueden gastar firmando con claves M-of-N.

Casos de uso de ejemplo:

* cuenta compartida (1 de 2; tanto el esposo como la esposa tienen acceso individual a sus fondos)
* cuenta de consenso (2 de 2; tanto el esposo como la esposa deben estar de acuerdo en gastar sus fondos)
* cuenta de umbral (2 de 3; un servicio de depósito en garantía está involucrado como un tercero independiente, para firmar conjuntamente con el vendedor o con el comprador, si el vendedor y el comprador no están de acuerdo)
* cuenta segura (2 de 3; un solo propietario controla las 3 claves pero las protege a través de un medio diferente para diversificar los riesgos)
* cuenta de umbral arbitrario (M-of-N; algunas criptomonedas brindan total flexibilidad en el número de firmantes) 

## Multifirma de Monero

Monero no implementa directamente las firmas múltiples (al menos no en un sentido clásico). Monero emula la función mediante la división secreta.

Las transacciones aún se firman con una única clave de gasto. La clave de gasto es una suma de todas las N claves privadas. El fundamento de tal diseño es desacoplar las firmas múltiples de las firmas de anillo.

Consideremos el esquema 2 de 3. Tenemos 3 participantes. A cada participante se le otorgan exactamente 2 claves privadas de una manera que los pares no se repiten entre los participantes. De esta manera, 2 participantes juntos tienen las 3 claves privadas necesarias para crear la clave de gasto privada.

La firma múltiple es una función a nivel de billetera. No hay forma de aprender de la cadena de bloques qué transacciones se crearon con múltiples firmas.

También vale la pena señalar que en Monero no hay direcciones multifirma como tal. [Estructura de direcciones](/ public-address / standard-address /) no le importa cómo se creó la clave de gasto privada subyacente.

Después de la configuración de la billetera multifirma, cada participante termina conociendo la dirección pública y la clave de vista privada. Esto es necesario para que los participantes reconozcan y descifren las transacciones que se supone que deben ser co-firmantes. 

## Configuración de billetera multifirma

La función Multisig solo está disponible a través de una billetera de línea de comandos. Debe estar familiarizado con la billetera CLI antes de jugar con multisig.

Consideremos un esquema 2 de 3 ya que se generaliza bien.

### 0. Abre tu billetera

Acceda a su billetera (stagenet): 

```
./monerod --stagenet --daemon                     # Run your full node; make sure it is fully synced    
./monero-wallet-cli --stagenet --wallet-file=...  # Run your wallet; make sure you have some funds to play with    
```

### 1. preparar_multisig

Cada participante genera de forma independiente **datos de inicialización**.Esta**no** es una dirección.

Cada participante envía sus datos de inicialización manualmente a todos los demás participantes a través de un canal seguro. 

### 2. hacer_multisig

Cada participante aplica los datos de inicialización de otros participantes. Esto da como resultado una **segunda ronda de datos de inicialización**. Esto sigue siendo **no** una dirección.

Cada participante envía su segunda ronda de datos de inicio a todos los demás participantes a través de un canal seguro. 

### 3. finalizar_multisig

Cada participante finaliza la creación de la billetera aplicando la segunda ronda de datos de inicio de todos los demás participantes. Esto finalmente da como resultado una billetera **dirección pública** y **clave de vista privada** que todos los participantes deben conocer.

Tenga en cuenta que las acciones son simétricas para todos los participantes. Aunque consideramos un esquema de 2 de 3, cada participante coopera con todos los demás. La división secreta la realiza internamente la billetera.

El intercambio seguro de datos de inicialización entre participantes es manual. La billetera en sí no proporciona ningún canal de comunicación seguro. Esto está fuera de alcance. 

## Recibir fondos

La dirección creada por la configuración de múltiples firmas es como cualquier otra dirección.

Puede generar direcciones y subdirecciones integradas basadas en él.

Todos los participantes pueden ver los fondos entrantes mientras comparten la clave de vista privada.

Con una CLI, use los siguientes comandos para ver los pagos entrantes:

     dirección
     actualizar
     show_transfers

## Fondos para gastos

QUE HACER

## Referencia 

* [https://monero.stackexchange.com/questions/5646/how-to-use-monero-multisignature-wallets-2-2-2-3](https://monero.stackexchange.com/questions/5646/how-to-use-monero-multisignature-wallets-2-2-2-3)
