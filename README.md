# alien-invasion-
Game
# Import necessary modules and libraries
import pygame
import random

# Initialize the game
pygame.init()
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Alien Invasion")

# Define the player class
class Player:
    def __init__(self):
        self.image = pygame.image.load("player.png")
        self.rect = self.image.get_rect()
        self.rect.x = screen_width // 2
        self.rect.y = screen_height - self.rect.height
        self.speed = 5
        self.health = 100
        self.score = 0
        
    def move_left(self):
        self.rect.x -= self.speed
        
    def move_right(self):
        self.rect.x += self.speed
        
    def fire_projectile(self):
        pass # TODO: Implement this method
        
# Define the projectile class
class Projectile:
    def __init__(self, x, y):
        self.image = pygame.image.load("projectile.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = 10
        self.damage = 10
        
    def move(self):
        self.rect.y -= self.speed
        
# Define the enemy class
class Enemy:
    def __init__(self):
        self.image = pygame.image.load("enemy.png")
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, screen_width - self.rect.width)
        self.rect.y = random.randint(-500, -50)
        self.speed = random.randint(1, 3)
        self.health = 50
        
    def move(self):
        self.rect.y += self.speed
        
    def fire_projectile(self):
        pass # TODO: Implement this method
        
# Main game loop
player = Player()
enemies = []
projectiles = []
game_running = True
clock = pygame.time.Clock()

while game_running:
    # Handle user input events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                player.move_left()
            elif event.key == pygame.K_RIGHT:
                player.move_right()
            elif event.key == pygame.K_SPACE:
                projectiles.append(Projectile(player.rect.x + player.rect.width // 2, player.rect.y))
    
    # Move the player and enemies
    for enemy in enemies:
        enemy.move()
        
    for projectile in projectiles:
        projectile.move()
    
    # Check for collisions
    for projectile in projectiles:
        for enemy in enemies:
            if projectile.rect.colliderect(enemy.rect):
                enemy.health -= projectile.damage
                if enemy.health <= 0:
                    player.score += 10
                    enemies.remove(enemy)
                projectiles.remove(projectile)
                
    # Draw the game elements
    screen.fill((0, 0, 0))
    screen.blit(player.image, player.rect)
    for enemy in enemies:
        screen.blit(enemy.image, enemy.rect)
    for projectile in projectiles:
        screen.blit(projectile.image, projectile.rect)
        
    # Update the screen
    pygame.display.update()
    clock.tick(60)

# Clean up the game
pygame.quit()
