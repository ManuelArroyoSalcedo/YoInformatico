# REPARACIÓN DE PENDRIVES

Los siguientes pasos pueden arreglar pendrives averiados o mal formateados mediante la utilidad **diskpart** de Windows, y pueden solucionar varios tipos de problemas relacionados con el sistema de archivos, particiones o atributos del dispositivo. A continuación se explica qué tipo de fallos pueden arreglarse con ese procedimiento y por qué funciona:

#### Problemas que estos pasos pueden solucionar

**1º Pendrive que aparece como RAW**

- Síntoma: Windows lo detecta, pero no puede acceder a él ni formatearlo normalmente.
- Causa común: Corrupción del sistema de archivos o de la tabla de particiones.
- Solución en los pasos: El comando `clean` elimina todas las particiones y tablas, y luego se crea una nueva desde cero.

**2º Pendrive protegido contra escritura (Write protected)**

- Síntoma: No se puede formatear ni copiar archivos ("el disco está protegido contra escritura").
- Causa común: Atributo de sólo lectura activado accidentalmente o por malware.
- Solución en los pasos: `attributes disk clear readonly` quita esa protección.

**3º Pendrive sin letra de unidad asignada**

- Síntoma: El dispositivo aparece en el Administrador de discos pero no tiene letra y no puedes acceder desde el Explorador.
- Causa común: Error al montar el volumen, o conflicto con letras de unidad.
- Solución en los pasos: `assign letter=X` le asigna una letra manualmente.

**4º Pendrive no detectado correctamente (no muestra partición o espacio)**

- Síntoma: Se detecta como "espacio no asignado" o no muestra capacidad.
- Causa común: Tabla de particiones corrupta (MBR/GPT), o partición perdida.
- Solución en los pasos: `clean` seguido de `convert mbr` y `create partition primary` crea una nueva estructura reconocible.

**5º Pendrive que no se puede formatear con herramientas gráficas**

- Síntoma: Error al formatear desde el Explorador o Administración de discos.
- Causa común: Problemas con el volumen, atributos de protección, sistema de archivos ilegible.
- Solución en los pasos: El formateo en línea de comandos (format fs=...) suele forzar la escritura incluso en casos problemáticos. 

#### Secuencia de pasos

1º Abre CMD como Administrador y lanza DiskPart

`diskpart  `

2º Selecciona tu pendrive

`list disk`

`select disk 1 `      

3º    Quita posibles bloqueos de sólo lectura

`attributes disk clear readonly  `

4º    Limpia todo el disco (esto borra particiones, MBR/GPT, tablas, etc.)

`clean  `

5º    Convierte a MBR (opcional pero suele funcionar mejor para USBs que van a usarse en Windows y BIOS)

`convert mbr  `

6º    Crea y marca activa la partición

`create partition primary`  

`select partition X`   ← sustituye X por el número correcto 

`active ` 

7º    Fuerza a DiskPart a re-detectar volúmenes

`rescan`  

8º    Localiza el volumen que corresponde a tu partición

`list volume`  

9º    Selecciona ese volumen y asígnale letra

`select volume X`      ← sustituye X por el número correcto  

`assign letter=t`      ← o la letra que prefieras  

10º    Formatea rápido en FAT32 (o NTFS si lo prefieres)

`format fs=fat32 label=MI_USB quick`  

— Para NTFS:

`format fs=ntfs label=MI_USB quick`  

11º    Sal de DiskPart

`exit`






