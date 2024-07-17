# packman
import pygame
import random

# Инициализация Pygame
pygame.init()

# Параметры окна
width = 400
height = 400
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Pac-Man")

# Цвета
black = (0, 0, 0)
white = (255, 255, 255)
yellow = (255, 255, 0)
red = (255, 0, 0)
blue = (0, 0, 255)

# Размер клетки
cell_size = 20

# Класс для Pac-Man
class Pacman:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.direction = "right"  # Начальное направление
        self.open_mouth = True  # Pac-Man с открытым ртом
        self.speed = 2

    def draw(self):
        if self.open_mouth:
            pygame.draw.circle(screen, yellow, (self.x + cell_size // 2, self.y + cell_size // 2), cell_size // 2)
            pygame.draw.polygon(screen, yellow, [(self.x, self.y + cell_size // 2), (self.x + cell_size // 2, self.y), (self.x + cell_size, self.y + cell_size // 2)])
        else:
            pygame.draw.circle(screen, yellow, (self.x + cell_size // 2, self.y + cell_size // 2), cell_size // 2)

    def move(self):
        if self.direction == "right":
            self.x += self.speed
        elif self.direction == "left":
            self.x -= self.speed
        elif self.direction == "up":
            self.y -= self.speed
        elif self.direction == "down":
            self.y += self.speed

        # Анимация открывания/закрывания рта
        self.open_mouth = not self.open_mouth

# Класс для призрака
class Ghost:
    def __init__(self, x, y, color):
        self.x = x
        self.y = y
        self.color = color
        self.direction = random.choice(["right", "left", "up", "down"])
        self.speed = 1

    def draw(self):
        pygame.draw.rect(screen, self.color, (self.x, self.y, cell_size, cell_size))

    def move(self):
        if self.direction == "right":
            self.x += self.speed
        elif self.direction == "left":
            self.x -= self.speed
        elif self.direction == "up":
            self.y -= self.speed
        elif self.direction == "down":
            self.y += self.speed

        # Изменение направления
        if random.randint(1, 100) == 1:
            self.direction = random.choice(["right", "left", "up", "down"])

# Создание Pac-Man и призраков
pacman = Pacman(width // 2 - cell_size // 2, height // 2 - cell_size // 2)
ghost1 = Ghost(width // 4 - cell_size // 2, height // 4 - cell_size // 2, red)
ghost2 = Ghost(width * 3 // 4 - cell_size // 2, height * 3 // 4 - cell_size // 2, blue)

# Основной цикл игры
running = True
while running:
    # Обработка событий
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                pacman.direction = "right"
            elif event.key == pygame.K_LEFT:
                pacman.direction = "left"
            elif event.key == pygame.K_UP:
                pacman.direction = "up"
            elif event.key == pygame.K_DOWN:
                pacman.direction = "down"

    # Обновление положения Pac-Man и призраков
    pacman.move()
    ghost1.move()
    ghost2.move()

    # Отрисовка
    screen.fill(black)
    pacman.draw()
    ghost1.draw()
    ghost2.draw()

    # Обновление экрана
    pygame.display.flip()

    # Задержка для регулирования скорости игры
    pygame.time.delay(100)

# Выход из Pygame
pygame.quit()
