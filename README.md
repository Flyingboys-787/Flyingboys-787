import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
CAR_WIDTH, CAR_HEIGHT = 50, 30
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Set up the display
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Car Simulation')

# Load car image
car_image = pygame.Surface((CAR_WIDTH, CAR_HEIGHT))
car_image.fill(BLACK)
car_rect = car_image.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2))

# Car variables
car_speed = 5
car_direction = pygame.math.Vector2(0, 0)

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Key states
    keys = pygame.key.get_pressed()
    car_direction.x = keys[pygame.K_RIGHT] - keys[pygame.K_LEFT]
    car_direction.y = keys[pygame.K_DOWN] - keys[pygame.K_UP]

    # Normalize direction to prevent faster diagonal movement
    if car_direction.length() > 0:
        car_direction = car_direction.normalize()

    # Update car position
    car_rect.x += car_direction.x * car_speed
    car_rect.y += car_direction.y * car_speed

    # Boundary conditions
    if car_rect.left < 0:
        car_rect.left = 0
    if car_rect.right > SCREEN_WIDTH:
        car_rect.right = SCREEN_WIDTH
    if car_rect.top < 0:
        car_rect.top = 0
    if car_rect.bottom > SCREEN_HEIGHT:
        car_rect.bottom = SCREEN_HEIGHT

    # Draw everything
    screen.fill(WHITE)
    screen.blit(car_image, car_rect)

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    pygame.time.Clock().tick(60)

# Quit Pygame
pygame.quit()
sys.exit()
