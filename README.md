import pygame
from random import randint
pygame.init()
window_size = 1200, 800
window = pygame.display.set_mode(window_size)
clock = pygame.time.Clock()
player_rect = pygame.Rect(50, 500, 100, 100)
def generate_pipes(count, pipe_width=140, gap=280, min_height=50, max_height=440, distance=650):
    pipes = []
    start_x = window_size[0]
    for i in range(count):
        height = randint(min_height, max_height)
        top_pipe = pygame.Rect(start_x, 0, pipe_width, height)
        bottom_pipe = pygame.Rect(start_x, height + gap, pipe_width, window_size[1] - (height + gap))
        pipes.extend([top_pipe, bottom_pipe])
        start_x += distance
    return pipes
pipes = generate_pipes(150)
lose = False
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    keys = pygame.key.get_pressed()
    if not lose:
        if keys[pygame.K_w]:
            player_rect.y -= 10
        if keys[pygame.K_s]:
            player_rect.y += 10
    if not lose:
        if len(pipes) < 8:
            pipes += generate_pipes(150)
    window.fill((135, 206, 235))
    for pipe in pipes[:]:
        if not lose:
            pipe.x -= 10 
        pygame.draw.rect(window, 'green', pipe)
        if pipe.x <= -100:
            pipes.remove(pipe)
        if player_rect.colliderect(pipe):
            lose = True
    pygame.draw.rect(window, 'yellow', player_rect)
    if player_rect.top <= 0 or player_rect.bottom >= window_size[1]:
        lose = True
    pygame.display.flip()
    clock.tick(60)
pygame.quit()

