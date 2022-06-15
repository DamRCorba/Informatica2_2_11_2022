# C de Bajo Nivel

## Operaciones Booleanas

### Negación - NOT "~"
La operación __NOT__ niega al valor del bit

| A    | ~A |
| :--- | :--- |
| 0    | 1    |
| 1    | 0    |

### O - OR "|"
La operación __OR__ devuelve:
  - __0__ si los dos son __0__
  - __1__ si cualquiera de ellos es __1__

<table><th>A</th><th>B</th><th>A | B</th><tbody><tr><td>0</td><td>0</td><td>0</td></tr><tr><td>0</td><td>1</td><td>1</td></tr><tr><td>1</td><td>0</td><td>1</td></tr><tr><td>1</td><td>1</td><td>1</td></tr></tbody></table>

### Y - AND "&"
La operación __AND__ devuelve:
  - __0__ si cualquiera de ellos es __0__
  - __1__ si ambos son __1__

| A    | B    |A & B |
| :--- | :--- | :--- |
| 0    | 0    | 0    |
| 0    | 1    | 0    |
| 1    | 0    | 0    |
| 1    | 1    | 1    |

### O Exclusiva - XOR "^"
La operación __XOR__ devuelve:
  - __0__ si son iguales  
  - __1__ si son distintos

| A    | B    |A ^ B |
| :--- | :--- | :--- |
| 0    | 0    | 0    |
| 0    | 1    | 1    |
| 1    | 0    | 1    |
| 1    | 1    | 0    |

### Desplazamiento
El desplazamiento binario se realiza en ambas direcciones.

Supongamos que tenemos el siguiente bus de 8 bits:
```
  +---+---+---+---+---+---+---+---+
  | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 0 |    8 bits
  +---+---+---+---+---+---+---+---+
    B7  B6  B5  B4  B3  B2  B1  B0
Bin: 0b01100100 - Hexa 0x64 - Dec: 100
```
Si lo desplazamos a la izquierda en 1 __<<__
```
  +---+---+---+---+---+---+---+---+
  | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 0 |    8 bits
  +---+---+---+---+---+---+---+---+
    B7  B6  B5  B4  B3  B2  B1  B0
Bin: 0b11001000 - Hexa 0xC8 - Dec: 200
```
Si al original lo desplazamos a la derecha en 1 __>>__
```
  +---+---+---+---+---+---+---+---+
  | 0 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |    8 bits
  +---+---+---+---+---+---+---+---+
    B7  B6  B5  B4  B3  B2  B1  B0
Bin: 0b00110010 - Hexa 0x32 - Dec: 50
```
Con cada desplazamiento se agrega un 0 en el Bit más significativo o en el menos significativo, dependiendo la dirección de desplazamiento.

- [VER VIDEO EXPLICACION](https://youtu.be/WVXKmYSEDEU)
- [VER VIDEO APLICACION](https://youtu.be/PbeTK-2aM-M)

## Campo de Bits

Es una estructura que utiliza para cada campo una cantidad de bits distinto del tamaño del tipo de variable.

```c
typedef struct {
  unsigned char comando:3;
  unsigned char bandera:1;
  unsigned char dato:4;
} comunicacion_t
```
*comunicacion_t* es un tipo de datos de bits, en el cual comando ocupa 3 bits, bandera 1 y dato los bits restantes

### Práctica Propuesta

__Ejercicio 22__:  Hacer una Función que reciba como parámetro un int correspondiente a un id del archivo *buscar.dat*, de organización secuencial, con la siguiente estructura: id (int), nombre (cadena char), nota (float), estado (unsigned int), b (char) y devuelva un long.
> Nota: *Estado* debe tener valor A (alta), B (baja) o P (procesado)

La función debe buscar el registro del archivo correspondiente al id pasado como parámetro. Si el campo estado del registro tiene los bits 3, 5, 7 y 8 en valor 1, imprimirá el nombre en mayúculas y devolverá -1.
Si esta marcado como Alta creará un registro en el archivo *"procesado.dat"* con el registro y devolverá el lugar donde se grabó.
En caso de estar marcado como *P* se marcará al registro como baja y __utilizando punteros__ devolver cuantas vocales tiene el campo nombre.
Cualquier error debe devolver -2.
> Nota: Los errores pueden ser de apertura del archivo o registro no encontrado, etc..


__Ejercicio 23__: Partiendo del *Ejercicio 19* modificar los datos del Dibujo de Caracteres para que sean representados a nivel de bits, en lugar de bytes.
> El Original planteaba hacerlo con bytes guardando 0 y 1 para prendido o apagado. Aquí se pretende hacerlo de a bit. Si el dibujo es de 8x5 debería guardarse en 5 bytes.
