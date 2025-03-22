# Informe-comecocos.1 codigo de depuracion
import pygame
pygame.init()

ANCHO, ALTO = 600, 400
pantalla = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption("Mi primera ventana con pygame")

corriendo = True
while corriendo:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            corriendo = False
    pantalla.fill((0, 0, 0))  # Fondo negro
    pygame.display.flip()

    pantalla.fill((255, 255, 255))  # Fondo blanco
    pygame.draw.rect(pantalla, (0, 0, 255), (50, 50, 100, 100))  # Rectángulo azul
    pygame.draw.circle(pantalla, (255, 0, 0), (300, 200), 50)  # Círculo rojo
    pygame.display.flip()

    x, y = 250, 150
velocidad = 5

corriendo = True
while corriendo:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            corriendo = False
    
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_LEFT]:
        x -= velocidad
    if teclas[pygame.K_RIGHT]:
        x += velocidad
    if teclas[pygame.K_UP]:
        y -= velocidad
    if teclas[pygame.K_DOWN]:
        y += velocidad
    
    pantalla.fill((255, 255, 255))  # Fondo blanco
    pygame.draw.rect(pantalla, (0, 0, 255), (x, y, 50, 50))
    pygame.display.flip()
    

pygame.quit()

import tkinter as tk
from tkinter import messagebox

root = tk.Tk()
root.withdraw()  # Oculta la ventana principal

messagebox.showinfo("Bienvenida", "Este es un mensaje emergente con tkinter.")


def mostrar_mensaje():
    messagebox.showinfo("Aviso", "Has presionado el botón")

root = tk.Tk()
root.title("Ventana con botón")

boton = tk.Button(root, text="Haz clic aquí", command=mostrar_mensaje)
boton.pack(pady=20)

root.mainloop()

def aceptar():
    messagebox.showinfo("Resultado", "Has aceptado")

def cancelar():
    messagebox.showinfo("Resultado", "Has cancelado")

root = tk.Tk()
root.title("Opciones")

btn_aceptar = tk.Button(root, text="Aceptar", command=aceptar)
btn_aceptar.pack(side=tk.LEFT, padx=10, pady=20)

btn_cancelar = tk.Button(root, text="Cancelar", command=cancelar)
btn_cancelar.pack(side=tk.RIGHT, padx=10, pady=20)

root.mainloop()


import pygame
import random
import sys
import tkinter as tk
from tkinter import messagebox

# Inicializar pygame
pygame.init()

# Definir constantes
MAPA_ANCHO = 20
MAPA_ALTO = 9
ANCHO, ALTO = 1200, 800
TAMANO_CELDA = min(ANCHO // MAPA_ANCHO, ALTO // MAPA_ALTO)

pantalla = pygame.display.set_mode((MAPA_ANCHO * TAMANO_CELDA, MAPA_ALTO * TAMANO_CELDA))
pygame.display.set_caption("Comecocos")

# Definir colores
NEGRO = (0, 0, 0)
AZUL = (0, 0, 255)
AMARILLO = (255, 255, 0)
BLANCO = (255, 255, 255)
ROJO = (255, 0, 0)

# Definir variables del juego
pac_x, pac_y = 1, 1
fan_x, fan_y = 10, 5
puntos = 0
velocidad = 100

# Definir el mapa del juego (0: vacío, 1: pared, 2: punto)
mapa = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0, 2, 0, 1, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 1],
    [1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1],
    [1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1],
    [1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1],
    [1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1],
    [1, 0, 0, 0, 0, 2, 0, 1, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
]


# Función para reiniciar el juego
def reiniciar_juego():
    """Restablece el estado del juego a sus valores iniciales."""
    global pac_x, pac_y, puntos
    pac_x, pac_y = 1, 1
    fan_x, fan_y = 10, 5
    puntos = 0
    mapa = [[0] * MAPA_ANCHO for _ in range(MAPA_ALTO)]
    mapa[5][5] = 2


    # Reiniciar la posición del jugador a la coordenada inicial
    # Restablecer el puntaje a cero
    # Volver a cargar el mapa con sus elementos originales

# Función para mover el personaje principal
def mover_pacman(dx, dy):
    global pac_x, pac_y, puntos
    nuevo_x = pac_x + dx
    nuevo_y = pac_y + dy
    if 0 <= nuevo_x < MAPA_ANCHO and 0 <= nuevo_y < MAPA_ALTO and mapa[nuevo_y][nuevo_x] != 1:
        pac_x, pac_y = nuevo_x, nuevo_y
        if mapa[pac_y][pac_x] == 2:
            puntos += 10
            mapa[pac_y][pac_x] = 0

    """Mueve el Pac-Man en la dirección dada y actualiza la recolección de puntos."""
    # Verificar que el movimiento no atraviese paredes
    # Si hay un punto en la nueva posición, incrementar el puntaje
    # Actualizar la posición del jugador en el mapa

# Función para mover el fantasma
def mover_fantasma():
    global fan_x, fan_y
    movimientos = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    random.shuffle(movimientos)
    for dx, dy in movimientos:
        nuevo_x = fan_x + dx
        nuevo_y = fan_y + dy
        if 0 <= nuevo_x < MAPA_ANCHO and 0 <= nuevo_y < MAPA_ALTO and mapa[nuevo_y][nuevo_x] != 1:
            fan_x, fan_y = nuevo_x, nuevo_y
            break


    """Mueve el fantasma de forma aleatoria en direcciones permitidas."""
    # Seleccionar una dirección aleatoria entre arriba, abajo, izquierda o derecha
    # Comprobar que el movimiento no atraviese paredes
    # Actualizar la posición del fantasma en el mapa

# Función para mostrar la pantalla de Game Over
def mostrar_game_over():
    pantalla.fill(NEGRO)
    font = pygame.font.Font(None, 74)
    texto = font.render("GAME OVER", True, ROJO)
    pantalla.blit(texto, (ANCHO // 2 - 100, ALTO // 2 - 50))
    pygame.display.flip()
    pygame.time.delay(2000)
    preguntar_volver_a_jugar("Game Over. ¿Quieres jugar de nuevo?")



    """Muestra la pantalla de Game Over y pregunta si el jugador quiere reiniciar."""
    # Dibujar el mensaje de Game Over en la pantalla
    # Esperar un tiempo y preguntar al usuario si desea reiniciar el juego

# Función para preguntar si el jugador quiere volver a jugar
def preguntar_volver_a_jugar(mensaje):
    root = tk.Tk()
    root.withdraw()
    respuesta = messagebox.askyesno("Fin del juego", mensaje)
    if respuesta:
        reiniciar_juego()
    else:
        pygame.quit()
        sys.exit()

    """Muestra un cuadro de diálogo preguntando si el jugador quiere volver a jugar."""
    # Crear una ventana emergente con tkinter para mostrar un mensaje de juego terminado
    # Preguntar al usuario si desea jugar nuevamente con opciones "Sí" o "No"
    # Si el usuario elige "Sí", reiniciar el juego
    # Si el usuario elige "No", cerrar pygame y terminar el programa

    # Bucle principal
ejecutando = True
while ejecutando:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            ejecutando = False
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_LEFT]:
        mover_pacman(-1, 0)
    if teclas[pygame.K_RIGHT]:
        mover_pacman(1, 0)
    if teclas[pygame.K_UP]:
        mover_pacman(0, -1)
    if teclas[pygame.K_DOWN]:
        mover_pacman(0, 1)
    mover_fantasma()
    if (pac_x, pac_y) == (fan_x, fan_y):
        mostrar_game_over()
    pantalla.fill(NEGRO)
    for y in range(MAPA_ALTO):
        for x in range(MAPA_ANCHO):
            if mapa[y][x] == 1:
                pygame.draw.rect(pantalla, AZUL, (x * TAMANO_CELDA, y * TAMANO_CELDA, TAMANO_CELDA, TAMANO_CELDA))
            elif mapa[y][x] == 2:
                pygame.draw.circle(pantalla, BLANCO, (x * TAMANO_CELDA + TAMANO_CELDA // 2, y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 4)
    pygame.draw.circle(pantalla, AMARILLO, (pac_x * TAMANO_CELDA + TAMANO_CELDA // 2, pac_y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 2)
    pygame.draw.circle(pantalla, ROJO, (fan_x * TAMANO_CELDA + TAMANO_CELDA // 2, fan_y * TAMANO_CELDA + TAMANO_CELDA // 2), TAMANO_CELDA // 2)
    pygame.display.flip()
    pygame.time.delay(velocidad)

pygame.quit()     
