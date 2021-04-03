---
title: Verificación de la firma de los binarios de Monero 
---
# Verificar binarios de Monero 

La verificación debe realizarse **antes de extraer el archivo y antes de usar Monero**.

Las instrucciones se probaron en Linux. También deberían funcionar en macOS con ligeras modificaciones.

## 1. Importar clave PGP del responsable de mantenimiento 

Esta es una acción única. Omita este paso para las versiones posteriores de Monero.

Los desarrolladores centrales de Monero firman una lista de hashes de binarios lanzados.

BinaryFate es el desarrollador principal de Monero que firma los lanzamientos. Su clave pública está disponible en GitHub en el código fuente del proyecto. Importe la clave pública de binaryFate a su llavero: 

`curl https://raw.githubusercontent.com/monero-project/monero/master/utils/gpg_keys/binaryfate.asc | gpg --import`

Confíe en la clave pública de binaryFate (la huella digital debe ser exactamente esta):

    gpg --edit-key '81AC591FE9C4B65C5806AFC3F0AF4D462A0BDF92'
    trust
    4

!!! peligro
    Si no se encontró la clave con esta huella digital, elimine la clave importada inmediatamente (gpg --delete-keys ...).
    Eso significaría que la clave cambió (probablemente se vio comprometida).

## 2. Verifique la firma de la lista hash (hashes.txt) 

La lista de binarios y sus hashes se publica en [getmonero.org] (https://www.getmonero.org/downloads/hashes.txt) y en algunos otros lugares como notas de la versión en [r / monero] (https: / /reddit.com/r/monero). 
¡Tenga en cuenta que el canal de publicación no importa siempre que verifique correctamente la firma!                                                                         u

Para verificar que estos son hashes reales (no alterados), ejecute: 

`curl https://www.getmonero.org/downloads/hashes.txt | gpg --verify`

La salida esperada debe contener la línea: 

`gpg: Good signature from "binaryFate <binaryfate@getmonero.org>"`

## 3. Verifica el hash 

En este paso, verificamos que los hashes publicados no hayan sido alterados. 

El último paso es comparar el hash publicado con el hash SHA-256 del archivo descargado. 

[Descarga Monero](/interacting/download-monero-binaries) si aún no lo hiciste (pero no desempaques). 

Reemplace el nombre del archivo de ejemplo con uno real: 

    file_name=monero-gui-linux-x64-v0.17.1.9.tar.bz2

    file_hash=`sha256sum $file_name | cut -c 1-64`

    curl https://www.getmonero.org/downloads/hashes.txt > /tmp/reference-hashes.txt

    # verificar la firma (el paso anterior se repite aquí para completar) 
    gpg --verify /tmp/reference-hashes.txt

    # grep debe imprimir el hash (la salida no puede estar vacía) 
    grep $file_hash /tmp/reference-hashes.txt

!!! peligro
    Si la salida de grep está vacía, vuelva a verificar todo porque aparentemente los hashes no coinciden. 

Si grep imprimió el nombre de archivo y el hash, ¡todo está bien! 
