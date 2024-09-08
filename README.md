# Actividad-2

##Ejercicio 1
```do
#include <stdio.h>
#include <stdlib.h>

typedef struct Nodo {
    float dato;
    struct Nodo *siguiente;
} Nodo;

Nodo *crearNodo(float dato) {
    Nodo *nuevoNodo = (Nodo *)malloc(sizeof(Nodo));
    if (nuevoNodo == NULL) {
        printf("Error al asignar memoria.\n");
        exit(1);
    }
    nuevoNodo->dato = dato;
    nuevoNodo->siguiente = NULL;
    return nuevoNodo;
}

void insertarOrdenado(Nodo **cabeza, float dato) {
    Nodo *nuevoNodo = crearNodo(dato);
    Nodo *actual = *cabeza;
    Nodo *anterior = NULL;

    while (actual != NULL && actual->dato < dato) {
        anterior = actual;
        actual = actual->siguiente;
    }

    if (anterior == NULL) {
        // Insertar al principio
        nuevoNodo->siguiente = *cabeza;
        *cabeza = nuevoNodo;
    } else {
        // Insertar en medio o al final
        nuevoNodo->siguiente = actual;
        anterior->siguiente = nuevoNodo;
    }
}

void mostrarLista(Nodo *cabeza) {
    Nodo *actual = cabeza;
    while (actual != NULL) {
        printf("%.1f ", actual->dato);
        actual = actual->siguiente;
    }
    printf("\n");
}

int main() {
    Nodo *cabeza = NULL;

    // Insertar datos en la lista ordenada
    insertarOrdenado(&cabeza, 10.0);
    insertarOrdenado(&cabeza, 5.0);
    insertarOrdenado(&cabeza, 15.0);
    insertarOrdenado(&cabeza, 20.0);
    insertarOrdenado(&cabeza, 10.0);

    // Mostrar la lista ordenada
    printf("Lista ordenada: ");
    mostrarLista(cabeza);

    return 0;
}
```
-Función matematica 
```do
f(L, x) = {
  {x}                        si L es vacía
  {x, L[0], ..., L[n-1]}      si x < L[0]
  {L[0], ..., L[n-1], x}      si x > L[n-1]
  {L[0], ..., L[i-1], x, L[i], ..., L[n-1]}  si L[i-1] < x < L[i]
}
```
-Explicación

Si x es el primer elemento en la lista, es decir, la lista está vacía, entonces L = {x}.

Si x es menor que el primer elemento de la lista L, insertamos x al inicio de la lista: L = {x, L[0], L[1], ..., L[n-1]}.

Si x es mayor que el último elemento de la lista, insertamos x al final de la lista: L = {L[0], L[1], ..., L[n-1], x}.

Si x está entre dos elementos L[i-1] y L[i] de la lista, insertamos x entre esos dos elementos: L = {L[0], ..., L[i-1], x, L[i], ..., L[n-1]}.

##Ejercicio 2
```do
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Estructura para el nodo de la lista enlazada
struct Nodo {
    int dato;
    struct Nodo *siguiente;
};

// Función para crear un nuevo nodo
struct Nodo* crearNodo(int dato) {
    struct Nodo *nuevoNodo = (struct Nodo*)malloc(sizeof(struct Nodo));
    nuevoNodo->dato = dato;
    nuevoNodo->siguiente = NULL;
    return nuevoNodo;
}

// Función para insertar un nodo en la lista enlazada en orden creciente
void insertarNodo(struct Nodo **cabeza, int dato) {
    struct Nodo *nuevoNodo = crearNodo(dato);
    if (*cabeza == NULL || (*cabeza)->dato >= dato) {
        nuevoNodo->siguiente = *cabeza;
        *cabeza = nuevoNodo;
    } else {
        struct Nodo *actual = *cabeza;
        while (actual->siguiente != NULL && actual->siguiente->dato < dato) {
            actual = actual->siguiente;
        }
        nuevoNodo->siguiente = actual->siguiente;
        actual->siguiente = nuevoNodo;
    }
}

// Función para imprimir la lista enlazada
void imprimirLista(struct Nodo *cabeza) {
    if (cabeza == NULL) {
        printf("La lista está vacía.\n");
        return;
    }
    struct Nodo *actual = cabeza;
    while (actual != NULL) {
        printf("%d ", actual->dato);
        actual = actual->siguiente;
    }
    printf("\n");
}

// Función para liberar la memoria de la lista enlazada
void liberarLista(struct Nodo *cabeza) {
    struct Nodo *actual = cabeza;
    while (actual != NULL) {
        struct Nodo *siguiente = actual->siguiente;
        free(actual);
        actual = siguiente;
    }
}

// Función para sumar los datos de los nodos en posiciones pares e impares
void sumarDatos(struct Nodo *cabeza, int *sumaPares, int *sumaImpares) {
    int posicion = 1;
    struct Nodo *actual = cabeza;
    while (actual != NULL) {
        if (posicion % 2 == 0) {
            *sumaPares += actual->dato;
        } else {
            *sumaImpares += actual->dato;
        }
        posicion++;
        actual = actual->siguiente;
    }
}

// Función para eliminar los nodos en posiciones pares o impares
void eliminarNodos(struct Nodo **cabeza, int eliminarPares) {
    struct Nodo *actual = *cabeza;
    struct Nodo *anterior = NULL;
    int posicion = 1;
    while (actual != NULL) {
        if ((eliminarPares && posicion % 2 == 0) || (!eliminarPares && posicion % 2 != 0)) {
            if (anterior == NULL) {
                *cabeza = actual->siguiente;
            } else {
                anterior->siguiente = actual->siguiente;
            }
            free(actual);
            actual = anterior->siguiente;
        } else {
            anterior = actual;
            actual = actual->siguiente;
        }
        posicion++;
    }
}

int main() {
    srand(time(NULL));
    struct Nodo *cabeza = NULL;

    // Insertar nodos con datos aleatorios en orden creciente
    for (int i = 0; i < 10; i++) {
        int dato = rand() % 100;
        insertarNodo(&cabeza, dato);
    }

    printf("Lista original: ");
    imprimirLista(cabeza);

    int sumaPares = 0;
    int sumaImpares = 0;

    // Sumar los datos de los nodos en posiciones pares e impares
    sumarDatos(cabeza, &sumaPares, &sumaImpares);

    printf("Suma de los datos en posiciones pares: %d\n", sumaPares);
    printf("Suma de los datos en posiciones impares: %d\n", sumaImpares);

    // Eliminar nodos según la suma de los datos
    if (sumaPares != sumaImpares) {
        eliminarNodos(&cabeza, 1); // Eliminar nodos en posiciones pares
        printf("Lista después de eliminar nodos en posiciones pares: ");
    } else if (sumaPares == sumaImpares) {
        eliminarNodos(&cabeza, 0); // Eliminar nodos en posiciones impares
        printf("Lista después de eliminar nodos en posiciones impares: ");
    } else {
        printf("La lista está vacía.\n");
        return 0;
    }

    imprimirLista(cabeza);

    liberarLista(cabeza);
    return 0;
}
```
-Funciones matematicas 

1.
```do
g(x) = {
    ∑(x_i), donde i es par
    ∑(x_i), donde i es impar
}
```
2.
```do
h(x) = {
    eliminar(x_i), si ∑(x_i) es par
    eliminar(x_i), si ∑(x_i) es impar
}
```
-Explicación 

1.Suma de los datos de los nodos en posiciones pares e impares:

+La función g(x) calcula la suma de los datos de los nodos dependiendo de su posición:

g(x) = ∑(x_i) donde i es par (es decir, se suman los elementos en posiciones pares).

g(x) = ∑(x_i) donde i es impar (es decir, se suman los elementos en posiciones impares).

2.Eliminación de nodos según la suma de los datos:

+La función h(x) es la eliminación de nodos en una lista enlazada según la suma de los datos:

h(x) = eliminar(x_i) si la suma ∑(x_i) es par.

h(x) = eliminar(x_i) si la suma ∑(x_i) es impar.

