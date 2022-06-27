# C de Bajo Nivel

## Campo de Bits

Es una estructura que utiliza para cada campo una cantidad de bits distinto del tamaño del tipo de variable.

```C
typedef struct {
  unsigned char comando:3;
  unsigned char bandera:1;
  unsigned char dato:4;
} comunicacion_t
```

*comunicacion_t* es un tipo de datos de bits, en el cual comando ocupa 3 bits, bandera 1 y dato los bits restantes

## Variable bit

```C
typedef struct {
    unsigned char B0 : 1;
    unsigned char B1 : 1;
    unsigned char B2 : 1;
    unsigned char B3 : 1;
    unsigned char B4 : 1;
    unsigned char B5 : 1;
    unsigned char B6 : 1;
    unsigned char B7 : 1;
}bit_t;

```

Podemos usar una macro conveniente.

```C
#define REGBIT(reg,bit)  ((volatile bit_t *)&reg)->B##bit
```
La macro anterior utiliza una sintaxis no muy común o confusa a primera vista. Empecemos de atrás para adelante.

El "##" es una sentencia de preprocesado que indica concatenación. En nuestro campo de bit todas nuestros campos se llaman Bx siendo "x" el bit que le pasamos a la macro.
Al escribir B##bit estamos dando la posibilidad de hacer B0, B1,... etc.

La "->" es la indicación de desplazamiento ya que vamos a tratar al campo de bit como un puntero. Esto es igual que en el caso de las estructuras en la que usábamos el operador "." para acceder a los datos, o "->" si lo que pretendíamos era apuntar a la posición de memoria del campo.

Por último, "(volatile bit_t *)&reg" no es mas que un casteo. Lo que decimos ahí, es que al parámetro de la macro "reg" lo tratemos como un puntero de tipo bit_t.

De esta forma lo que hace la macro es utilizar a la variable o registro que le pasamos en "reg" como un puntero de tipo bit_t desplazado en el campo al que queremos apuntar. Esto nos permite modificar y leer el valor de un bit de esa variable de forma directa.

### Codigo de Ejemplo

```C
#include <stdio.h>

typedef struct
{
    unsigned char B0 : 1;
    unsigned char B1 : 1;
    unsigned char B2 : 1;
    unsigned char B3 : 1;
    unsigned char B4 : 1;
    unsigned char B5 : 1;
    unsigned char B6 : 1;
    unsigned char B7 : 1;
} bit_t;

#define REGBIT(reg, bit) ((volatile bit_t *)&reg)->B##bit


int main(int argc, char const *argv[])
{
    
    char carrera = 0;
    #define FINDECARREA REGBIT(carrera, 5)

    

    FINDECARREA = 0;
    while (1){
        FINDECARREA ^=1;
        if (FINDECARREA)
        {
            printf("El bit 5 Esta en 1, Y carrera vale: %d\n", carrera);
        } else {
            printf("El bit 5 Esta en 0, Y carrera vale: %d\n", carrera);
        }
    }

    return 0;
}

```
[Ver Video Campo de bits](https://youtu.be/HZChLlwLY38)

## Uniones

Una Union es un espacio de datos especial que permite guardar diferentes tipos de datos sobre la misma posicion de memoria.

```c
typedef union
{
    unsigned int bytes_2;
    unsigned char byte_1;
}miUnion_t;
```

### Codigo Ejemplo
```c
#include <stdio.h>

typedef struct
{
    unsigned char B0 : 1;
    unsigned char B1 : 1;
    unsigned char B2 : 1;
    unsigned char B3 : 1;
    unsigned char B4 : 1;
    unsigned char B5 : 1;
    unsigned char B6 : 1;
    unsigned char B7 : 1;
} bit_t;

typedef unsigned char uint8_t;
typedef union {
    bit_t bit;
    uint8_t byte;
    int word;
} miUnion_t;

int main(int argc, char const *argv[])
{
    miUnion_t prueba;
    prueba.word = 100; //0b 0110 0100

    if (prueba.bit.B5)
    {
        printf("Esta en 1\n");
    }
    else
    {
        printf("Esta en 0\n");
    }
    printf("%d\n", sizeof(miUnion_t));
    printf("%d\n", sizeof(int));

    return 0;
}

```

[Ver Video de Uniones](https://youtu.be/5UCp5uEFpF0)



# Ejercicios Propuestos 

Ejercicio 24: Recuperatorio Primer Parcial 2019

1)  Existe un archivo binario de organización directa con la siguiente estructura
Long id, categoría cadena de char, valor float, estado unsigned.

Realizar una función que pase como parámetro el id, la función debe ubicar el registro, si no lo encuentra retorna un 0. Una vez encontrado utilizando puntero recorrer la categoría colocando en mayúscula y regrabar el registro si el bit 3 del campo estado esta en 1 . En este caso retornará la cantidad de registros restantes desde que se encontró el registro. (ej si hay 100 registros y la clave pasada es 10 retornará 90)


2) A. Explicar: Relación entre un vector y un puntero.
    B. Que implica un error cuando abrimos un archivo de modo “w”.
