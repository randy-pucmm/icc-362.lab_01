# ICC-362 INTRODUCCION A LA INTELIGENCIA ARTIFICIAL

## Lab #1 – Agentes de Búsqueda en Pacman

### Estudiante:

- Randy Alexander Germosén Ureña

### Profesor

- Jorge Alejandro Luna

### Resumen

En este proyecto, te familiarizarás con Pac-Man World. En esta asignación, el agente Pac-Man encontrará caminos a través de su mundo laberíntico, tanto para llegar a un lugar concreto como para recoger alimentos de manera eficiente. También creará algoritmos de búsqueda generales y los aplicará a escenarios de Pac-Man.

El código base de este proyecto consta de varios archivos Python, algunos de los cuales necesitarás leer y entender para completar las tareas, y algunas de las cuales puedes ignorar. El código Pac-Man fue desarrollado por John DeNero y Dan Klein en UC Berkeley para la clase CS188.

Se adjuntan archivos con codigo de respaldo y la explicacion del laboratorio.

### Entregables

Archivos con el codigo creado por ustedes search.py y searchAgents.py.
Archivo README.txt con indicaciones adicionales de la estructura y dependencias de su código.
Rubrica de Evaluacion

La rubrica esta basada en el autocorrector y esa basada en 25 puntos correspondientes al 100% de la nota.

---

# Estructura y Código

## Estructura del Proyecto

```

├── autograder.py
├── eightpuzzle.py
├── game.py
├── ghostAgents.py
├── grading.py
├── graphicsDisplay.py
├── graphicsUtils.py
├── keyboardAgents.py
├── layout.py
├── pacman.py
├── pacmanAgents.py
├── projectParams.py
├── search.py                  --> Algoritmos de busqueda (DFS, BFS, UCS, A*)
├── searchAgents.py            --> Agentes y problemas de busqueda (CornersProblem, heuristicas, etc.)
├── searchTestClasses.py
├── testClasses.py
├── testParser.py
├── textDisplay.py
└── util.py
```

## Dependencias

- Python 3.12.
- Y también el módulo `tkinter` que es necesario para la interfaz gráfica.


## Realización del laboratorio

### search.py

Contiene las implementaciones de los cuatro algoritmos de busqueda genericos. Todos siguen el patron de busqueda en grafos (graph search) usando un conjunto `visited` para evitar expandir estados repetidos.

- **depthFirstSearch (Q1):** Usa `util.Stack` como frontera (LIFO). Expande el nodo mas profundo primero. Cada elemento en la pila es una tupla `(estado, lista_de_acciones)`.

- **breadthFirstSearch (Q2):** Usa `util.Queue` como frontera (FIFO). Expande el nodo menos profundo primero. Marca los estados como visitados al momento de agregarlos a la cola (no al expandirlos), lo cual evita duplicados en la frontera.

- **uniformCostSearch (Q3):** Usa `util.PriorityQueue` con la prioridad igual al costo acumulado `g(n)`. Cada elemento en la cola es `(estado, acciones, costo)`. Garantiza encontrar el camino de menor costo.

- **aStarSearch (Q4):** Usa `util.PriorityQueue` con prioridad `f(n) = g(n) + h(n)`, donde `g(n)` es el costo acumulado y `h(n)` es el valor de la heuristica. Acepta una funcion heuristica como parametro (por defecto `nullHeuristic` que retorna 0).

### searchAgents.py

Contiene las definiciones de problemas de busqueda y heuristicas.

- **CornersProblem (Q5):** Define un problema de busqueda para visitar las 4 esquinas del laberinto. El estado es una tupla `(posicion, esquinas_visitadas)` donde `esquinas_visitadas` es una tupla de 4 booleanos indicando si cada esquina ha sido alcanzada. `getSuccessors` genera estados sucesores con costo 1, actualizando el registro de esquinas visitadas cuando Pacman llega a una esquina. `isGoalState` retorna `True` cuando las 4 esquinas han sido visitadas.

- **cornersHeuristic (Q6):** Heuristica admisible para el CornersProblem. Calcula la distancia Manhattan acumulada siguiendo una estrategia greedy de vecino mas cercano entre las esquinas no visitadas. Es admisible porque la distancia Manhattan es siempre menor o igual a la distancia real en el laberinto, y la suma de distancias individuales subestima la longitud total del recorrido optimo.

- **foodHeuristic (Q7):** Heuristica admisible para el FoodSearchProblem. Calcula la distancia real en el laberinto (maze distance) desde la posicion actual de Pacman hasta cada punto de comida restante, y retorna la distancia maxima. Es admisible porque Pacman debe recorrer al menos la distancia al punto de comida mas lejano. Las distancias se almacenan en `problem.heuristicInfo` para evitar recalculos.

- **ClosestDotSearchAgent.findPathToClosestDot (Q8):** Utiliza BFS sobre un `AnyFoodSearchProblem` para encontrar el camino mas corto al punto de comida mas cercano. Es una estrategia greedy suboptima pero rapida.

- **AnyFoodSearchProblem.isGoalState (Q8):** Retorna `True` si la posicion actual `(x, y)` contiene comida segun `self.food[x][y]`.
