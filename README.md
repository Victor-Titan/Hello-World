import pygame
from pygame.locals import *
winner=None
s='O'
row=None
col=None
run=1
grid = [ [ None, None, None ], \
         [ None, None, None ], \
         [ None, None, None ] ]
background=pygame.display.set_mode((300,325),RESIZABLE)


def board():
    background.fill((0,0,0))
    pygame.draw.line(background, (250,250,250), (100, 0), (100, 300), 2)
    pygame.draw.line(background, (250,250,250), (200, 0), (200, 300), 2)
    pygame.draw.line(background, (250,250,250), (0, 100), (300, 100), 2)
    pygame.draw.line(background, (250,250,250), (0, 200), (300, 200), 2)
    pygame.display.update()
def check():
    global s,grid,col,row,run
    for row in range (0, 3):
        if ((grid [row][0] == grid[row][1] == grid[row][2]) and \
            (grid [row][0] is not None)):
             grid[row][0]
             pygame.draw.line (background, (250,0,0), (0, (row + 1)*100 - 50), \
                               (300, (row + 1)*100 - 50), 1)
             break
    for col in range (0, 3):
        if (grid[0][col] == grid[1][col] == grid[2][col]) and \
            (grid[0][col] is not None):
            winner = grid[0][col]
            pygame.draw.line (background, (250,0,0), ((col + 1)* 100 - 50, 0), \
                              ((col + 1)* 100 - 50, 300), 1)
            print (s+"lost")
            run=0
            break
        if (grid[0][0] == grid[1][1] == grid[2][2]) and \
           (grid[0][0] is not None):
            winner = grid[0][0]
            pygame.draw.line (background, (250,0,0), (50, 50), (250, 250), 1)
            print (s+"lost")
            run=0
            break
        if (grid[0][2] == grid[1][1] == grid[2][0]) and \
           (grid[0][2] is not None):
            winner = grid[0][2]
            pygame.draw.line (background, (250,0,0), (250, 50), (50, 250), 1)
            print (s+"lost")
            run=0
            break
        c=0
        for col in range (0,3):
            for row in range (0,3):
                if(grid[row][col] is not None):
                     c+=1
        if(c==9):
            run=0
            print ("Draw")          
        pygame.display.update()

		
def drawMove():
        global s,grid,col,row,run
        (mouseX,mouseY) = pygame.mouse.get_pos()
        if (mouseY < 100):
            row = 0
        elif (mouseY < 200):
            row = 1
        else:
            row = 2
        if (mouseX < 100):
            col = 0
        elif (mouseX < 200):
            col = 1
        else:
            col = 2
        centerX = ((col) * 100) + 50
        centerY = ((row) * 100) + 50
        if(grid[row][col] is not None):
            
            drawMove()
        if (s == 'O'):
            pygame.draw.circle (background, (250,250,250), (centerX, centerY), 44, 2)
            grid[row][col]=s
            s='X'
        else:
            pygame.draw.line (background, (250,250,250), (centerX - 22, centerY - 22), \
                              (centerX + 22, centerY + 22), 2)
            pygame.draw.line (background, (250,250,250), (centerX + 22, centerY - 22), \
                              (centerX - 22, centerY + 22), 2)
            grid [row][col] = s
            s='O'
        pygame.display.update()
            
pygame.init()

board()
pygame.display.set_mode ((300, 325))
while(run==1):
    for event in pygame.event.get():
        
        if event.type is QUIT:
            run = 0
        elif event.type is MOUSEBUTTONDOWN:
            drawMove()
            check()
            if(run==0):
                break
            if (winner is None):
                print(s+"'s turn")
            else:
                run=0
    if (run==0):        
        exit()
pygame.display.update()
