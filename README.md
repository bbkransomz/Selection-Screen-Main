
import sys
import time
import pygame


#Main GUI file
#Ver 1.01

pygame.init()   #Intialising the module

###Setting the resolution
display_width = 600
display_height = 500
gameDisplay = pygame.display.set_mode((display_width, display_height))

###Setting the color

black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)
red_bright = (150,0,0)
green = (0, 255, 0)
green_bright =(0,150,0)
blue = (0, 0, 255)
blue_bright = (0, 0 , 150)

#Images
carImage = pygame.image.load('car.gif')
backgroundImage = pygame.image.load('Main_Screen.png')
gameImage = pygame.image.load('Delivering_objects.png')
blankImage = pygame.image.load('selection_screen.png')





###The game clock//used for FPS

clock = pygame.time.Clock()

###Setting the title

pygame.display.set_caption('Amazon Delivery Service')
###Functions for displaying images

def background_image(x, y):
    gameDisplay.blit(backgroundImage, (x,y))
x = 0
y = 0

def blank_image(x, y):
    gameDisplay.blit(blankImage, (x, y))

def game_image(x, y):
    gameDisplay.blit(gameImage, (x,y))

def car(x, y):
    gameDisplay.blit(carImage, (x, y))

#Displaying Text to screen

def text_objects(text, font):
    textSurface = font.render(text, True, black)
    return textSurface, textSurface.get_rect()

def message_display(text):
    largeText = pygame.font.Font('freesansbold.ttf',50)
    TextSurf, TextRect = text_objects(text, largeText)
    TextRect.center = ((display_width/2),100)
    gameDisplay.blit(TextSurf, TextRect)

def logo():
    message_display("Amazon Delivery Guise")

#button Function
b_width = 163
b_height = 48

def button(msg,x,y,w,h,ic,ac, action=None):

    mouse = pygame.mouse.get_pos()
    click = pygame.mouse.get_pressed()

    if x+w > mouse[0] > x and y+h > mouse[1] > y:
        pygame.draw.rect(gameDisplay, ac, (x,y,w,h))

        if click[0] == 1 and action != None:
            if action == "play":
                game_select_items_menu()
            elif action == "Exit":
                pygame.quit()
                quit()
            elif action == "Options":
                game_options()
            elif action == "Intro":
                game_intro()

    else:
        pygame.draw.rect(gameDisplay, ic, (x,y,w,h))

    smallText = pygame.font.Font("freesansbold.ttf", 20)
    textSurf, textRect = text_objects(msg, smallText)
    textRect.center = ( (x+(w/2)), (y+(h/2)) )
    gameDisplay.blit(textSurf, textRect)


#Game Intro Loop
def game_intro():

    intro = True

    while intro:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()

        background_image(x, y)

        button("Start", 220, 280, 163, 48, green, green_bright, "play")
        button("Options", 220, 340, 163, 48, green, green_bright, "Options")
        button("Exit", 220, 400, 163, 48, green, green_bright, "Exit")
        button("Test", 220, 460, 163, 35, green, green_bright, "Test")


        pygame.display.update()

        clock.tick(80) #Setting the fps

def game_select_items_menu():

    gameSelectItems = False
    while not gameSelectItems:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()

        blank_image(x, y)


        # button("Back", 220, 280, 163, 48, green, green_bright, "play")

        pygame.display.update()

        clock.tick(80) #Setting the fps

#Game Options Loop
def game_options():

    gameExit = False
    while not gameExit:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()


        button("Volume", 220, 280, 163, 48, green, green_bright, "play")
        button("Music", 220, 340, 163, 48, green, green_bright)
        button("Randomness", 220, 400, 163, 48, green, green_bright,)
        button("Back", 220, 460, 163, 48, green, green_bright, "Intro")

        pygame.display.update()

        clock.tick(80) #Setting the fps





#Main Game Loop
def game_loop():

    #car variables for testing purposes
    car_x = 150
    car_y = 150
    car_x_change = 0
    car_y_change = 0

    gameExit = False
    while not gameExit:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
            #Events for keys pressed down
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    car_x_change = -5
                elif event.key == pygame.K_RIGHT:
                    car_x_change = 5
                elif event.key == pygame.K_UP:
                    car_y_change = -5
                elif event.key == pygame.K_DOWN:
                    car_y_change = 5
            #Events for keys released
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    car_x_change = 0
                if event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                    car_y_change = 0

            print(event) #just prints events

        car_x += car_x_change
        car_y += car_y_change

        game_image(x, y)
        car(car_x, car_y)
        button("Back", 220, 460, 40, 40, green, green_bright, "Intro")
        pygame.display.update()

        clock.tick(80) #Setting the fps




game_intro()
pygame.quit()
quit()

