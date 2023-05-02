import pygame
import random

# initialize Pygame
pygame.init()

# set up the game window
screen_width = 600
screen_height = 400
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Action Game")

# set up the player character
player_width = 50
player_height = 50
player = pygame.Rect(screen_width/2, screen_height-player_height, player_width, player_height)
player_speed = 5

# set up the target objects
target_width = 30
target_height = 30
targets = []
for i in range(5):
    target_x = random.randint(0, screen_width-target_width)
    target_y = random.randint(0, screen_height//2)
    target = pygame.Rect(target_x, target_y, target_width, target_height)
    targets.append(target)

# set up the bullets
bullet_width = 5
bullet_height = 15
bullets = []
bullet_speed = 10

# set up the clock
clock = pygame.time.Clock()

# define a function to move the player character
def move_player(keys_pressed):
    if keys_pressed[pygame.K_LEFT] and player.x > 0:
        player.x -= player_speed
    if keys_pressed[pygame.K_RIGHT] and player.x < screen_width-player_width:
        player.x += player_speed

# define a function to move the bullets
def move_bullets():
    for bullet in bullets:
        bullet.y -= bullet_speed

# define a function to check for collisions between bullets and targets
def check_collisions():
    global targets, bullets
    for target in targets:
        for bullet in bullets:
            if target.colliderect(bullet):
                targets.remove(target)
                bullets.remove(bullet)

# define the game loop
def game_loop():
    global bullets
    game_over = False
    while not game_over:
        # handle events
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    # shoot a bullet
                    bullet_x = player.x + player_width/2 - bullet_width/2
                    bullet_y = player.y - bullet_height
                    bullet = pygame.Rect(bullet_x, bullet_y, bullet_width, bullet_height)
                    bullets.append(bullet)

        # move the player character
        keys_pressed = pygame.key.get_pressed()
        move_player(keys_pressed)

        # move the bullets
        move_bullets()

        # check for collisions between bullets and targets
        check_collisions()

        # draw the game objects
        screen.fill((0, 0, 0))
        pygame.draw.rect(screen, (255, 0, 0), player)
        for target in targets:
            pygame.draw.rect(screen, (0, 255, 0), target)
        for bullet in bullets:
            pygame.draw.rect(screen, (0, 0, 255), bullet)
        pygame.display.flip()

        # update the clock
        clock.tick(60)

    pygame.quit()

# start the game loop
game_loop()

