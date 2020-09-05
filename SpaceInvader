import pygame
import random
import math
from pygame import mixer

pygame.init()

screen = pygame.display.set_mode((800, 600))

background = pygame.image.load('background3.png')
# mixer.music.load('Platformer2.mp3')
# mixer.music.play(-1)

pygame.display.set_caption("Space Raiders")
icon = pygame.image.load('icon.png')
pygame.display.set_icon(icon)

playerImg = pygame.image.load('ship.png')
playerX = 375
playerY = 475
playerX_move = 0

bulletImg = pygame.image.load('bullet.png')
bulletX = 0
bulletY = 475
bulletX_move = 0
bulletY_move = 10
bullet_state = "ready"

raiderImg = []
raiderX = []
raiderY = []
raiderX_move = []
raiderY_move = []
num_of_raiders = 6

for i in range(num_of_raiders):
    raiderImg.append(pygame.image.load('alien.png'))
    raiderX.append(random.randint(0, 736))
    raiderY.append(random.randint(30, 150))
    raiderX_move.append(3)
    raiderY_move.append(30)

score_v = 0
font = pygame.font.Font('freesansbold.ttf', 32)

textX = 10
textY = 10

game_over_font = pygame.font.Font('freesansbold.ttf', 64)
play_again_font = pygame.font.Font('freesansbold.ttf', 32)


def show_score(x, y):
    score = font.render("Score: " + str(score_v), True, (255, 255, 255))
    screen.blit(score, (x, y))


def game_over():
    game_over_text = game_over_font.render("GAME OVER!!!", True, (255, 0, 0))
    screen.blit(game_over_text, (200, 250))


def play_again():
    play_again_text = play_again_font.render("Press R to Play Again", True, (0, 0, 0))
    screen.blit(play_again_text, (200, 350))


def player(x, y):
    screen.blit(playerImg, (x, y))


def raider(x, y, i):
    screen.blit(raiderImg[i], (x, y))


def fire_bullet(x,y):
    global bullet_state
    bullet_state = "fire"
    screen.blit(bulletImg, (x+16, y+10))


def isCollision(raiderX, raiderY, bulletX, bulletY):
    d = math.sqrt((math.pow(raiderX-bulletX, 2)) + (math.pow(raiderY-bulletY, 2)))
    if d < 20:
        return True
    else:
        return False

# Game loop


title_font = pygame.font.SysFont("comicsans", 70)
run = True
while run:
    # screen.blit(background, (0, 0))
    title_label = title_font.render("Press the mouse to begin", 1, (0, 0, 0))
    screen.blit(title_label, (100, 350))
    pygame.display.update()
    for event1 in pygame.event.get():
        if event1.type == pygame.QUIT:
            run = False
        if event1.type == pygame.MOUSEBUTTONDOWN:

            running = True
            while running:

                screen.blit(background, (0, 0))

                for event in pygame.event.get():
                    if event.type == pygame.QUIT:
                        running = False
                    if event.type == pygame.KEYDOWN:
                        if event.key == pygame.K_LEFT:
                            playerX_move = -6
                        if event.key == pygame.K_RIGHT:
                            playerX_move = 6
                        if event.key == pygame.K_SPACE:
                            if bullet_state == "ready":
                                bullet_sound = mixer.Sound('laser1.wav')
                                bullet_sound.play()
                                bulletX = playerX
                                fire_bullet(bulletX, bulletY)
                        if event.type == pygame.KEYUP:
                            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                                playerX_move = 0

                playerX += playerX_move
                if playerX <= 0:
                    playerX = 0
                elif playerX >= 736:
                    playerX = 736

                for i in range(num_of_raiders):
                    # Game over
                    if raiderY[i] > 440:
                        for j in range(num_of_raiders):
                            raiderY[j] = 2000
                        game_over()
                        # play_again()
                        break
                    raiderX[i] += raiderX_move[i]
                    if raiderX[i] <= 0:
                        raiderX_move[i] = 3
                        raiderY[i] += raiderY_move[i]
                    elif raiderX[i] >= 736:
                        raiderX_move[i] = -3
                        raiderY[i] += raiderY_move[i]

                    collision = isCollision(raiderX[i], raiderY[i], bulletX, bulletY)
                    if collision:
                        blast_sound = mixer.Sound('explosion.wav')
                        blast_sound.play()
                        bulletY = 475
                        bullet_state = "ready"
                        score_v += 10

                        raiderX[i] = random.randint(0, 736)
                        raiderY[i] = random.randint(30, 150)

                    raider(raiderX[i], raiderY[i], i)

                if bulletY <= 0:
                    bulletY = 475
                    bullet_state = "ready"
                if bullet_state == "fire":
                    fire_bullet(bulletX, bulletY)
                    bulletY -= bulletY_move

                player(playerX, playerY)
                show_score(textX, textY)
                pygame.display.update()
