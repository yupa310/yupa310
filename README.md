- 👋 Hi, I’m @yupa310
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
yupa310/yupa310 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import time
import random

# Inicializar pygame
pygame.init()

# Colores
blanco = (255, 255, 255)
amarillo = (255, 255, 102)
negro = (0, 0, 0)
rojo = (213, 50, 80)
verde = (0, 255, 0)
azul = (50, 153, 213)

# Dimensiones de la pantalla
ancho_pantalla = 800
alto_pantalla = 600

# Crear la pantalla
pantalla = pygame.display.set_mode((ancho_pantalla, alto_pantalla))
pygame.display.set_caption('Juego de la Serpiente')

# Reloj
reloj = pygame.time.Clock()

# Tamaño de la serpiente y velocidad
tamaño_serpiente = 10
velocidad_serpiente = 15

# Fuente
fuente_estilo = pygame.font.SysFont("bahnschrift", 25)
fuente_puntuacion = pygame.font.SysFont("comicsansms", 35)


def puntuacion(punt):
    valor = fuente_puntuacion.render("Tu puntuación: " + str(punt), True, azul)
    pantalla.blit(valor, [0, 0])


def nuestra_serpiente(tamaño_serpiente, lista_serpiente):
    for x in lista_serpiente:
        pygame.draw.rect(pantalla, negro, [x[0], x[1], tamaño_serpiente, tamaño_serpiente])


def mensaje(msg, color):
    mesg = fuente_estilo.render(msg, True, color)
    pantalla.blit(mesg, [ancho_pantalla / 6, alto_pantalla / 3])


def juego():
    game_over = False
    game_close = False

    x1 = ancho_pantalla / 2
    y1 = alto_pantalla / 2

    x1_cambio = 0
    y1_cambio = 0

    lista_serpiente = []
    longitud_serpiente = 1

    comida_x = round(random.randrange(0, ancho_pantalla - tamaño_serpiente) / 10.0) * 10.0
    comida_y = round(random.randrange(0, alto_pantalla - tamaño_serpiente) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            pantalla.fill(blanco)
            mensaje("Perdiste! Presiona Q-Salir o C-Jugar de nuevo", rojo)
            puntuacion(longitud_serpiente - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        juego()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_cambio = -tamaño_serpiente
                    y1_cambio = 0
                elif event.key == pygame.K_RIGHT:
                    x1_cambio = tamaño_serpiente
                    y1_cambio = 0
                elif event.key == pygame.K_UP:
                    y1_cambio = -tamaño_serpiente
                    x1_cambio = 0
                elif event.key == pygame.K_DOWN:
                    y1_cambio = tamaño_serpiente
                    x1_cambio = 0

        if x1 >= ancho_pantalla or x1 < 0 or y1 >= alto_pantalla or y1 < 0:
            game_close = True
        x1 += x1_cambio
        y1 += y1_cambio
        pantalla.fill(blanco)
        pygame.draw.rect(pantalla, verde, [comida_x, comida_y, tamaño_serpiente, tamaño_serpiente])
        cabeza_serpiente = []
        cabeza_serpiente.append(x1)
        cabeza_serpiente.append(y1)
        lista_serpiente.append(cabeza_serpiente)
        if len(lista_serpiente) > longitud_serpiente:
            del lista_serpiente[0]

        for x in lista_serpiente[:-1]:
            if x == cabeza_serpiente:
                game_close = True

        nuestra_serpiente(tamaño_serpiente, lista_serpiente)
        puntuacion(longitud_serpiente - 1)

        pygame.display.update()

        if x1 == comida_x and y1 == comida_y:
            comida_x = round(random.randrange(0, ancho_pantalla - tamaño_serpiente) / 10.0) * 10.0
            comida_y = round(random.randrange(0, alto_pantalla - tamaño_serpiente) / 10.0) * 10.0
            longitud_serpiente += 1

        reloj.tick(velocidad_serpiente)

    pygame.quit()
    quit()


juego()
