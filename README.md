
import pygame
import sys
import random

# تهيئة اللعبة
pygame.init()
width, height = 640, 480
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('لعبة الأفعى')

# الألوان
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# متغيرات اللعبة
snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
food_pos = [random.randrange(1, (width//10)) * 10,
            random.randrange(1, (height//10)) * 10]
food_spawn = True
direction = 'RIGHT'
change_to = direction
score = 0

# تحديث نقاط اللعبة
def show_score(choice, color, font, size):

    # إعداد النص
    score_font = pygame.font.SysFont(font, size)
    score_surf = score_font.render('النقاط : ' + str(score), True, color)
    score_rect = score_surf.get_rect()

    # عرض نقاط اللعبة في المكان المناسب
    if choice == 1:
        score_rect.midtop = (width/10, 15)
    else:
        score_rect.midtop = (width/2, height/1.25)

    # رسم نقاط اللعبة
    screen.blit(score_surf, score_rect)

# الدوران في اللعبة
while True:
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_to = 'UP'
            if event.key == pygame.K_DOWN:
                change_to = 'DOWN'
            if event.key == pygame.K_LEFT:
                change_to = 'LEFT'
            if event.key == pygame.K_RIGHT:
                change_to = 'RIGHT'

    # التوجيه
    if change_to == 'UP' and not direction == 'DOWN':
        direction = 'UP'
    if change_to == 'DOWN' and not direction == 'UP':
        direction = 'DOWN'
    if change_to == 'LEFT' and not direction == 'RIGHT':
        direction = 'LEFT'
    if change_to == 'RIGHT' and not direction == 'LEFT':
        direction = 'RIGHT'

    # التحديد الحركة الأساسية
    if direction == 'UP':
        snake_pos[1] -= 10
    if direction == 'DOWN':
        snake_pos[1] += 10
    if direction == 'LEFT':
        snake_pos[0] -= 10
    if direction == 'RIGHT':
        snake_pos[0] += 10

    # زيادة حجم الأفعى
    snake_body.insert(0, list(snake_pos))
    if snake_pos[0] == food_pos[0] and snake_pos[1] == food_pos[1]:
        score += 1
        food_spawn = False
    else:
        snake_body.pop()
        
    if not food_spawn:
        food_pos = [random.randrange(1, (width//10)) * 10,
                    random.randrange(1, (height//10)) * 10]
    food_spawn = True
    screen.fill(black)

    for pos in snake_body:
        pygame.draw.rect(screen, white,
                         pygame.Rect(pos[0], pos[1], 10, 10))

    pygame.draw.rect(screen, red, pygame.Rect(
        food_pos[0], food_pos[1], 10, 10))

    if snake_pos[0] < 0 or snake_pos[0] > width-10 \
            or snake_pos[1] < 0 or snake_pos[1] > height-10:
        pygame.quit()
        sys.exit()

    for block in snake_body[1:]:
        if snake_pos[0] == block[0] and snake_pos[1] == block[1]:
            pygame.quit()
            sys.exit()

    show_score(1, white, 'Consolas', 20)
    pygame.display.update()

    pygame.time.delay(30)
