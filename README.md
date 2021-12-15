# Line_up_four-no-initial-screen-version-
Line up four

import pygame,sys
import time
import random

FPS=400
WIDTH=500
HEIGHT=500



#[255,255,0] yellow
#[255,0,0] red
tar_x=40
tar_y=40
key_up=0
key_enter_up=0
key_enter=False

check_full=0

drop_x=0
drop_y=0
rem_drop=65

color_time=0


pygame.init()
pygame.display.set_caption('Line up four')# name of the window
screen=pygame.display.set_mode([WIDTH,HEIGHT])
clock=pygame.time.Clock()

arr=[[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0]]



#print(arr)



show_init=True
while show_init:
    clock.tick(FPS)
    screen.fill((255,255,255))
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            sys.exit()
    key_pressed=pygame.key.get_pressed()
    if (key_pressed[pygame.K_KP_ENTER] and key_enter_up==0 and key_enter==False):
        color_time=color_time+1
        key_enter_up=1
        key_enter=True
        drop_x=tar_x+25
        #drop_y=(tar_x+25-65)//60
        check_full=0
        for i in range(5,-1,-1):
            if(arr[i][(drop_x-65)//60]==0):
                drop_y=65+i*70
                check_full=1
                break
        if check_full==0:
            color_time-=1

    elif key_pressed[pygame.K_KP_ENTER]:
        key_enter_up=1
    else:
        key_enter_up=0

    if (rem_drop<drop_y and key_enter==True):
        if color_time%2==1:
            pygame.draw.circle(screen,[255,255,0],[drop_x,rem_drop],25,0)
        else:
            pygame.draw.circle(screen,[255,0,0],[drop_x,rem_drop],25,0)
        rem_drop+=1

    if (rem_drop==drop_y and key_enter==True):
        if color_time%2==1 and arr[(drop_y-65)//70][(drop_x-65)//60]==0:
            arr[(drop_y-65)//70][(drop_x-65)//60]=1
        elif color_time%2==0 and arr[(drop_y-65)//70][(drop_x-65)//60]==0:
            arr[(drop_y-65)//70][(drop_x-65)//60]=2
        rem_drop=65
        key_enter=False
        #check who win the game
        for i in range(6):
            for j in range(7):
                compare=0
                if i<=2 and arr[i][j]!=0:#check ---
                    for k in range(4):
                        if arr[i][j]==arr[i+k][j]:
                            compare=1
                        else:
                            compare=0
                            break
                    if compare==1:
                        show_init=False

                if j<=3 and arr[i][j]!=0:#check l
                    for k in range(4):
                        if arr[i][j]==arr[i][j+k]:
                            compare=1
                        else:
                            compare=0
                            break
                    if compare==1:
                        show_init=False
                if i<=2 and j<=3 and arr[i][j]!=0:
                    for k in range(4):
                        if arr[i][j]==arr[i+k][j+k]:
                            compare=1
                        else:
                            compare=0
                            break
                    if compare==1:
                        show_init=False
                if i<=2 and j>=3 and arr[i][j]!=0:
                    for k in range(4):
                        if arr[i][j]==arr[i+k][j-k]:
                            compare=1
                        else:
                            compare=0
                            break
                    if compare==1:
                        show_init=False
                if compare==1:
                    show_init=False
    
    for j in range(6):
        for i in range(7):
            if arr[j][i]==1:
                pygame.draw.circle(screen,[255,255,0],[65+i*60,65+j*70],25,0)
            elif arr[j][i]==2:
                pygame.draw.circle(screen,[255,0,0],[65+i*60,65+j*70],25,0) 
    




    if key_pressed[pygame.K_LEFT] and key_up==0: 
        tar_x-=60
        key_up=1
    elif key_pressed[pygame.K_LEFT]:
        tar_x+=0
    elif key_pressed[pygame.K_RIGHT] and key_up==0:
        tar_x+=60
        key_up=1
    elif key_pressed[pygame.K_RIGHT]:
        tar_x+=0    
    else:
        key_up=0
    if tar_x<40:
        tar_x=400
    elif tar_x>400:
        tar_x=40
    #pygame.draw.line(screen,[255,255,255],[0,HEIGHT//2+40],[500,HEIGHT//2+40],1)
    pygame.draw.rect(screen,[0,0,0],[tar_x,tar_y,50,50],1)

    for j in range(6):
        for i in range(7):
            pygame.draw.circle(screen,[0,0,0],[65+i*60,65+j*70],25,1)
    #circle(sur, color, center, radius)
    pygame.display.update()
