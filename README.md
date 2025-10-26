# Repositorio de Conceptos de Programación y Estructuras de Datos

Este repositorio contiene una colección de ejemplos de código en Python que ilustran conceptos fundamentales de programación, incluyendo listas por comprensión, estructuras de datos como Colas (Queues) y Pilas (Stacks), y un ejemplo de aplicación web interactiva.

## 📝 Licencia

Este proyecto está bajo la Licencia MIT.

**Copyright (c) 2024 Joseph Alexander Morales Cardona**

Se concede permiso, libre de cargos, a cualquier persona que obtenga una copia de este software y de los archivos de documentación asociados (el "Software"), para utilizar el Software sin restricción, incluyendo sin limitación los derechos a usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar, y/o vender copias del Software, y a permitir a las personas a las que se les proporcione el Software a hacer lo mismo, sujeto a las siguientes condiciones:

El aviso de copyright anterior y este aviso de permiso se incluirán en todas las copias o porciones sustanciales del Software.

EL SOFTWARE SE PROPORCIONA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O IMPLÍCITA, INCLUYENDO PERO NO LIMITADO A GARANTÍAS DE COMERCIALIZACIÓN, IDONEIDAD PARA UN PROPÓSITO PARTICULAR Y NO INFRACCIÓN. EN NINGÚN CASO LOS AUTORES O PROPIETARIOS DE LOS DERECHOS DE AUTOR SERÁN RESPONSABLES DE NINGUNA RECLAMACIÓN, DAÑOS U OTRAS RESPONSABILIDADES, YA SEA EN UNA ACCIÓN DE CONTRATO, AGRAVIO O CUALQUIER OTRO MOTIVO, DERIVADAS DE, FUERA DE O EN CONEXIÓN CON EL SOFTWARE O EL USO U OTRAS OPERACIONES CON EL SOFTWARE.

## 👨‍💻 Autor

| Rol | Nombre |
| :--- | :--- |
| Desarrollador Principal | **Joseph Alexander Morales Cardona** |
| Lenguaje de Programación | Python |

## 🧩 Códigos y Explicaciones

A continuación, se detallan los diferentes bloques de código encontrados en el archivo `ppy.ipynb` y su propósito.

### 1. LISTA POR COMPRENSIÓN ANIDADAS: Gestión de Inventario

Este código en Python demuestra el uso de listas por comprensión para realizar operaciones de análisis de datos de inventario de manera concisa y eficiente.

**Conceptos Clave:**
*   **Listas por Comprensión:** Se utilizan para crear nuevas listas a partir de iterables existentes, aplicando una expresión a cada elemento.
*   **Validación de Entrada:** Incluye bucles `while` y bloques `try-except` para asegurar que el usuario ingrese valores numéricos no negativos para el precio y la cantidad.
*   **Análisis de Inventario:** Las listas por comprensión se usan para:
    *   Extraer solo los nombres de los productos.
    *   Calcular los subtotales (precio \* cantidad) de cada producto.
    *   Calcular el valor total del inventario (`sum(...)`).
    *   Filtrar productos con un precio superior a $100.
    *   Filtrar productos con stock bajo (cantidad < 10).

```python
# Ingresar datos de productos (3 productos con precio y cantidad)
productos = []
for i in range(3):
    nombre = input(f"Producto {i+1}: ").strip()
    # validar precio: reintentar hasta que sea entero no-negativo
    while True:
        try:
            precio = int(input("Precio $: "))
            if precio < 0:
                print("El precio no puede ser negativo. Intenta de nuevo.")
                continue
            break
        except ValueError:
            print("Precio inválido. Introduce un número entero.")
    # validar cantidad: reintentar hasta que sea entero >= 0
    while True:
        try:
            cantidad = int(input("Cantidad: "))
            if cantidad < 0:
                print("La cantidad no puede ser negativa. Intenta de nuevo.")
                continue
            break
        except ValueError:
            print("Cantidad inválida. Introduce un número entero.")
    productos.append([nombre, precio, cantidad])

print("\n=== ANÁLISIS DE INVENTARIO ===")
print("1. Datos originales:", productos)
print("2. Nombres:", [p[0] for p in productos])
print("3. Subtotales:", [p[1] * p[2] for p in productos])
print("4. Total inventario: $", sum(p[1] * p[2] for p in productos))
print("5. Productos con precio > $100:", [p[0] for p in productos if p[1] > 100])
print("6. Stock bajo (cantidad < 10):", [p[0] for p in productos if p[2] < 10])
```

### 2. LISTA COMO COLAS: Sistema de Gestión de Pedidos (Restaurante)

Este ejemplo ilustra la implementación de la estructura de datos **Cola (Queue)** utilizando `collections.deque` de Python, ideal para escenarios donde el orden de procesamiento es **FIFO (First In, First Out)**.

**Conceptos Clave:**
*   **`collections.deque`:** Una lista optimizada para inserciones y eliminaciones rápidas en ambos extremos, perfecta para implementar colas.
*   **Clase `SistemaPedidos`:** Modela un sistema de pedidos de restaurante.
    *   `agregar_pedido`: Añade un nuevo pedido al final de la cola (`append`).
    *   `procesar_siguiente_pedido`: Elimina y retorna el pedido del principio de la cola (`popleft`), simulando el procesamiento.
    *   `mostrar_estado`: Proporciona una visión general de los pedidos pendientes y completados.
*   **Uso de Colas:** Se utiliza para simular el flujo de trabajo en un restaurante: los pedidos se toman en un orden y se procesan en ese mismo orden.

```python
# Ejemplo real: Sistema de gestión de pedidos de restaurante
from collections import deque
import time
import random

class SistemaPedidos:
    def __init__(self):
        self.pendientes = deque()  # Cola de pedidos pendientes
        self.completados = []     # Lista de pedidos completados
        self.numero_pedido = 1

    def agregar_pedido(self, cliente, items):
        """Agrega un nuevo pedido a la cola"""
        pedido = {
            'numero': self.numero_pedido,
            'cliente': cliente,
            'items': items,
            'hora': time.strftime("%H:%M:%S"),
            'estado': 'pendiente'
        }
        self.pendientes.append(pedido)
        self.numero_pedido += 1
        print(f"📝 Pedido #{pedido['numero']} agregado para {cliente}")
        return pedido['numero']

    def procesar_siguiente_pedido(self):
        """Procesa el siguiente pedido en la cola"""
        if not self.pendientes:
            print("⚠️ No hay pedidos pendientes")
            return None

        pedido = self.pendientes.popleft()
        pedido['estado'] = 'completado'
        pedido['hora_completado'] = time.strftime("%H:%M:%S")
        self.completados.append(pedido)

        print(f"✅ Pedido #{pedido['numero']} completado para {pedido['cliente']}")
        return pedido

    def mostrar_estado(self):
        """Muestra el estado actual del sistema"""
        print(f"\n📊 ESTADO DEL SISTEMA")
        print(f"🔄 Pedidos pendientes: {len(self.pendientes)}")
        print(f"✅ Pedidos completados: {len(self.completados)}")

        if self.pendientes:
            print("\n📋 Próximos pedidos:")
            for i, pedido in enumerate(self.pendientes):
                print(f"   {i+1}. #{pedido['numero']} - {pedido['cliente']} ({len(pedido['items'])} items)")

# Simulación del sistema
sistema = SistemaPedidos()

# Agregar varios pedidos
sistema.agregar_pedido("Ana", ["Hamburguesa", "Papas fritas", "Refresco"])
sistema.agregar_pedido("Carlos", ["Pizza familiar", "Ensalada"])
sistema.agregar_pedido("María", ["Sushi", "Sopa miso"])
sistema.agregar_pedido("Juan", ["Tacos", "Agua de horchata"])

# Mostrar estado inicial
sistema.mostrar_estado()

# Procesar algunos pedidos
print("\n🍳 Procesando pedidos...")
sistema.procesar_siguiente_pedido()
sistema.procesar_siguiente_pedido()

# Mostrar estado final
sistema.mostrar_estado()
```

### 3. LISTA COMO PILAS: Implementación de la Estructura Pila (Stack)

Este código implementa la estructura de datos **Pila (Stack)** utilizando una lista de Python, adhiriéndose al principio **LIFO (Last In, First Out)**.

**Conceptos Clave:**
*   **Clase `Pila`:** Implementa los métodos esenciales de una Pila.
    *   `apilar(elemento)`: Agrega un elemento al final de la lista (`append`), que se considera el "tope" de la pila.
    *   `desapilar()`: Elimina y retorna el último elemento agregado (`pop`), respetando el principio LIFO.
    *   `ver_tope()`: Retorna el elemento en el tope sin eliminarlo.
    *   `esta_vacia()`: Verifica si la pila no tiene elementos.
*   **Métodos Mágicos (`__len__`, `__str__`, `__repr__`):** Mejoran la usabilidad de la clase, permitiendo usar `len(pila)` y `print(pila)`.
*   **Aplicación Práctica:** Se demuestra cómo la Pila se puede usar para invertir una palabra, ya que el último carácter en entrar es el primero en salir.

```python
class Pila:
    """
    Implementación de una estructura de datos Pila (Stack).
    Funciona bajo el principio LIFO (Last In, First Out) usando una lista.
    Las operaciones clave (apilar/desapilar) tienen complejidad O(1).
    """
    def __init__(self):
        """Inicializa una pila vacía."""
        self.items = []
 
    def apilar(self, elemento):
        """Agrega un elemento al tope de la pila (O(1))."""
        self.items.append(elemento)
 
    def desapilar(self):
        """
        Elimina y retorna el elemento del tope de la pila (O(1)).
        Retorna: El elemento del tope, o None si la pila está vacía.
        """
        if self.esta_vacia():
            return None
        return self.items.pop()
 
    def ver_tope(self):
        """
        Retorna el elemento del tope sin eliminarlo (O(1)).
        Retorna: El elemento del tope, o None si la pila está vacía.
        """
        if self.esta_vacia():
            return None
        # Acceder al último elemento de la lista
        return self.items[-1]
 
    def esta_vacia(self):
        """Verifica si la pila está vacía."""
        return not self.items # Es True si la lista está vacía (len == 0)
 
    def tamano(self):
        """Retorna el número de elementos en la pila."""
        return len(self.items)
    # --- Métodos Especiales de Python ---
    def __len__(self):
        """Permite usar len(pila) para obtener el tamaño."""
        return len(self.items)
 
    def __str__(self):
        """Representación legible para el usuario (p.ej., print(pila))."""
        # Muestra los elementos de la pila de forma intuitiva
        return f"Pila(Tope <- {self.items})"
 
    def __repr__(self):
        """Representación para desarrolladores (cómo se crearía el objeto)."""
        return f"Pila(items={self.items})"
 
# --------------------------------------------------
# ===== DEMOSTRACIÓN DE USO (Bloque principal) =====
# --------------------------------------------------
 
if __name__ == "__main__":
    print("=== Demostración de la estructura de datos Pila (Stack) ===\n")
    mi_pila = Pila()
    print(f"1. Pila recién creada: {mi_pila} | ¿Vacía? {mi_pila.esta_vacia()}")
    # --- Apilar elementos ---
    print("\n2. Apilando (10, 20, 30, 40)...")
    mi_pila.apilar(10)
    mi_pila.apilar(20)
    mi_pila.apilar(30)
    mi_pila.apilar(40)
    print(f"   Estado actual: {mi_pila}")
    print(f"   Tope (peek): {mi_pila.ver_tope()}")
    print(f"   Tamaño: {len(mi_pila)}")
    # --- Desapilar elementos (LIFO) ---
    print("\n3. Desapilando dos elementos (LIFO)...")
    elemento_1 = mi_pila.desapilar() # Sale 40
    elemento_2 = mi_pila.desapilar() # Sale 30
    print(f"   Desapilado primero: {elemento_1}")
    print(f"   Desapilado segundo: {elemento_2}")
    print(f"   Estado actual: {mi_pila}")
    print(f"   Nuevo tope: {mi_pila.ver_tope()}")
    # --- Ejemplo práctico: Invertir una palabra ---
    print("\n4. Ejemplo práctico: Invertir la palabra 'DATA' con la Pila.")
    palabra = "DATA"
    pila_letras = Pila()
    # Llenar la pila
    for letra in palabra:
        pila_letras.apilar(letra)
    # Vaciar la pila para obtener la palabra invertida
    palabra_invertida = ""
    while not pila_letras.esta_vacia():
        palabra_invertida += pila_letras.desapilar()
    print(f"   Palabra original: {palabra}")
    print(f"   Palabra invertida: {palabra_invertida}")
```

### 4. ANIDADAS: Ejemplo de Listas por Comprensión Anidadas (Creación de Matriz)

Este código es un ejemplo didáctico que muestra cómo se utilizan las listas por comprensión anidadas, una técnica avanzada para crear estructuras de datos multidimensionales como matrices de forma compacta.

**Conceptos Clave:**
*   **Listas por Comprensión Anidadas:** Permiten generar una lista de listas (una matriz) en una sola línea de código. La sintaxis es similar a un bucle `for` anidado, donde el bucle más externo se escribe al final.
*   **Creación de una Matriz:** El ejemplo crea una matriz 3x3 donde cada elemento es el resultado de multiplicar el índice de fila (`i`) por el índice de columna (`j`).

```python
# EJEMPLOS DE LISTAS POR COMPRENSIÓN ANIDADAS
"""
Las listas por comprensión son una forma elegante de crear listas en Python.
Pueden incluir:
1. Una expresión que define cada elemento
2. Un bucle for para iterar sobre un iterable
3. Opcionalmente, una o más condiciones if
"""

# 1. Creación de una matriz 3x3
# [expresión for elemento_columna in iterable_columna for elemento_fila in iterable_fila]
matriz = [[i * j for j in range(3)] for i in range(3)]

# Resultado:
# [[0*0, 0*1, 0*2],
#  [1*0, 1*1, 1*2],
#  [2*0, 2*1, 2*2]]

print("Matriz 3x3 generada con comprensión anidada:")
for fila in matriz:
    print(fila)

# 2. Aplanamiento de una matriz (convirtiendo una matriz a una lista simple)
matriz_2d = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
lista_plana = [num for fila in matriz_2d for num in fila]

print("\nLista plana (aplanamiento de matriz):", lista_plana)
```

### 5. Código HTML/CSS/JS (Aplicación Web Educativa)

El resto del archivo `.ipynb` contiene una aplicación web completa escrita en HTML, CSS y JavaScript. Aunque no es código Python, es una aplicación educativa interactiva sobre un tema de salud.

**Conceptos Clave:**
*   **Aplicación de una Sola Página (SPA):** Utiliza JavaScript para mostrar y ocultar diferentes secciones (`pages`) sin recargar la página.
*   **Diseño Web Responsivo:** El CSS utiliza `flexbox` y `grid` (`grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));`) para asegurar que la aplicación se vea bien en dispositivos móviles y de escritorio.
*   **Funcionalidad Interactiva:**
    *   **Autoevaluación de Síntomas:** Permite al usuario seleccionar síntomas y recibir una evaluación de riesgo (simulada).
    *   **Búsqueda de Centros Médicos:** Incluye lógica para obtener la ubicación del usuario (`navigator.geolocation`) y calcular la distancia a centros médicos predefinidos.
    *   **Quiz Educativo:** Un sistema de preguntas y respuestas con seguimiento de puntaje y explicaciones.

Este código representa una aplicación práctica de desarrollo web para crear herramientas educativas interactivas.

---
*Este README fue generado automáticamente para Joseph Alexander Morales Cardona.*

