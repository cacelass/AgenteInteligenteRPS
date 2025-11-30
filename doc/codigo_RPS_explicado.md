1.3.4. Caso práctico en Python: ¡Piedra, Papel o Tijeras!
========================================================
# 1.X. Caso práctico en Python: Piedra, Papel o Tijeras con Estrategia Basada en Frecuencias

> **Atribución:**  
> El contenido de este documento se inspira en el estilo expositivo y académico de la Escuela Superior de Informática de Ciudad Real (Universidad de Castilla-La Mancha), adaptado ahora para describir una variante del juego *Piedra, Papel o Tijeras* que incorpora análisis de patrones por parte de la máquina.

---

## Introducción

Este proyecto desarrolla de manera incremental el clásico juego **Piedra, Papel o Tijeras**, implementado en Python. A diferencia de versiones puramente aleatorias, esta variante introduce una **memoria interna** y un **análisis de frecuencias** de las decisiones del usuario, permitiendo a la máquina detectar patrones no aleatorios y mejorar su desempeño.

Desde el punto de vista de la **teoría de juegos**, se trata de un juego **de suma cero**, donde la ganancia de un jugador implica necesariamente la pérdida del otro. Las reglas básicas se mantienen: piedra vence a tijeras, papel vence a piedra y tijeras vence a papel.

---

## Tipos enumerados utilizados

El juego emplea tipos enumerados para modelar tanto las acciones posibles como los resultados de cada ronda:

```python
class GameAction(IntEnum):
    Rock = 0
    Paper = 1
    Scissors = 2

class GameResult(IntEnum):
    Victory = 0
    Defeat = 1
    Tie = 2
```

Estas estructuras permiten una comparación clara, semántica y escalable.

---

## Diccionario de victorias

Las reglas del juego se almacenan en un diccionario que indica qué acción vence a cuál:

```python
Victories = {
    GameAction.Rock: GameAction.Paper,
    GameAction.Paper: GameAction.Scissors,
    GameAction.Scissors: GameAction.Rock
}
```

Este diseño respeta el **principio abierto/cerrado (OCP)**, ya que permite añadir nuevas reglas sin modificar la lógica principal.

---

## Función `assess_game()`

Esta función determina el resultado de la ronda comparando la acción del usuario con la acción de la máquina:

```python
def assess_game(user_action, computer_action):
    ...
```

La función:

- Evalúa la jugada mediante condiciones controladas.  
- Muestra mensajes explicativos.  
- Devuelve un valor de `GameResult` que clasifica la ronda en victoria, derrota o empate.

---

## Memoria y análisis de frecuencias

La máquina incorpora una **memoria global** que almacena todas las acciones del usuario:

```python
memoria = []
```

Tras registrar suficientes jugadas (más de 5), la función `get_computer_action()` emplea un análisis de frecuencias:

- Calcula el porcentaje de uso de cada acción del usuario.  
- Si alguna supera el **40 %**, la máquina elige la opción ganadora contra ella.  
- Si no hay patrones claros, o si hay pocas jugadas registradas, la máquina actúa de forma aleatoria.

Esta estrategia representa una aproximación básica a modelos de predicción del comportamiento.

---

## Función `get_user_action()`

La entrada del usuario se gestiona mediante código escalable:

```python
def get_user_action():
    game_choices = [f"{action.name}[{action.value}]" for action in GameAction]
    ...
```

Características destacadas:

- Las opciones se generan dinámicamente en función del enumerado.  
- Permite extender el juego sin modificar la función.  
- La validación se delega a la función principal.

---

## Función `main()`

La función principal aplica el **principio de responsabilidad única (SRP)** y coordina el flujo del juego:

```python
def main():
    while True:
        try:
            user_action = get_user_action()
        ...
        memoria.append(user_action.value)
        computer_action = get_computer_action()
        assess_game(user_action, computer_action)
        if not play_another_round():
            break
```

Responsabilidades:

- Captura errores por entradas inválidas.  
- Almacena las decisiones del usuario.  
- Llama a las funciones especializadas para cada acción.  
- Controla si el usuario desea continuar.

---

## Función `play_another_round()`

Una función simplificada que determina si se juega otra ronda:

```python
def play_another_round():
    return input("\nAnother round? (y/n): ").lower() == 'y'
```

---

## Mejoras principales de esta implementación

- ✔ Uso de `IntEnum` para modelar acciones y resultados  
- ✔ Diccionario de victorias siguiendo OCP  
- ✔ Estrategia inteligente basada en memorizar jugadas del usuario  
- ✔ Análisis de frecuencias para predicción básica  
- ✔ Uso de SRP en todas las funciones  
- ✔ Entrada escalable para ampliaciones futuras  

---

## Ejecución del programa

Para ejecutar el juego:

```bash
python3 rps_frequency.py
```

---

## Conclusiones

Esta versión del juego *Piedra, Papel o Tijeras* demuestra cómo un programa simple puede enriquecerse mediante buenas prácticas de diseño, separación de responsabilidades y estrategias básicas de análisis de comportamiento. El código está preparado para futuras extensiones como:

- Nuevas acciones (ej. Rock-Paper-Scissors-Lizard-Spock)  
- Estadísticas avanzadas  
- Predicción con modelos probabilísticos o de ML  
- Interfaz gráfica o modo multijugador  

---