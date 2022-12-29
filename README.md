# Puzzle-in-Python
Move arrows , play and solve puzzle.
from cProfile import label
from ipaddress import collapse_addresses
from tkinter import *
from tkinter import messagebox
from PIL import ImageTk , Image
from functools import partial

#FOR INITIALIZING MAIN WINDOW

root = Tk()

root.title("GUI Game")
root.iconbitmap("images/right_arrow.png")
root.geometry("400x250")

#SHAPE VARIABLES

buttonarray = []

currshape = 0
currangle = 0
shape1 = 0
shape2 = 0
shape3 = 0
shape4 = 0
shape5 = 0
shape6 = 0

def clear_record():
    currangle = 0
    currshape = 0
    shape1 = 0
    shape2 = 0
    shape3 = 0
    shape4 = 0
    shape5 = 0
    shape6 = 0

#FUNCTION TO IMPLEMENT EASY GAME MODE

def Easygame():
    global playmode
    playmode = Toplevel()
    playmode.title("Play Screen")
    playmode.iconbitmap("images/right_arrow.png")
    playmode.geometry("800x900")


    global buttonarray
    global level1arrows
    global level1solution
    buttonarray = []
    level1arrows = ['up' , 'down' , 'up' , 'left' , 'left' , 'down' , 'left' , 'left' , 'up' , 'right' , 'right' , 'left' , 'down' , 'down' , 'left' , 'down' , 'up' , 'right']
    level1colors = []

    level1solution = ['green' , 'green' , 'blue' , 'blue' , 'purple' , 'red' , 'yellow' , 'green' , 'blue' , 'orange' , 'purple' , 'red' , 'yellow' , 'yellow' , 'blue' , 'orange' , 'orange' , 'red']

    clear_record()
    for i in range(0,18):
        level1colors.append('white')

    level1colors[5] = 'red'
    level1colors[12] = 'yellow'



    for i in range(0 , 18):

        buttonarray.append(myButton(level1arrows[i] , level1colors[i] , i + 1))

        if i < 6:
            buttonarray[i].button.grid(row=0 , column=i)
        elif i < 12:
            buttonarray[i].button.grid(row=1 , column=(i%6))
        else:
            buttonarray[i].button.grid(row=2 , column=(i%6))


    label11 = Label(playmode , text=' ')
    label11.grid(row=3 , column=0)

    label11 = Label(playmode , text=' ')
    label11.grid(row=4 , column=0)

    label11 = Label(playmode , text=' Choose a shape ')
    label11.grid(row=5 , column=2)

    imagelabel1 = Button(playmode , image=shape1image , padx=50 , pady=50 , command=checkAvail1)
    imagelabel2 = Button(playmode , image=shape2image , padx=50 , pady=50 , command=checkAvail2)
    imagelabel3 = Button(playmode , image=shape3image , padx=50 , pady=50 , command=checkAvail3)
    imagelabel4 = Button(playmode , image=shape4image , padx=50 , pady=50 , command=checkAvail4)
    imagelabel5 = Button(playmode , image=shape5image , padx=50 , pady=50 , command=checkAvail5)
    imagelabel6 = Button(playmode , image=shape6image , padx=50 , pady=50 , command=checkAvail6)

    imagelabel1.grid(row=6 , column=0)
    imagelabel2.grid(row=6 , column=1)
    imagelabel3.grid(row=6 , column=2)
    imagelabel4.grid(row=6 , column=3)
    imagelabel5.grid(row=6 , column=4)
    imagelabel6.grid(row=6 , column=5)

    rotatebuttonrow = []

    for i in range(0 , 6):
        rotatebuttonrow.append(Button(playmode , text='Rotate 90*' , padx=20 , pady=10 , command=rotate90))
        
    for i in range(0 , 6):
        rotatebuttonrow.append(Button(playmode , text='Rotate 180*' , padx=20 , pady=10 , command=rotate180))

    for i in range(0 , 6):
        rotatebuttonrow.append(Button(playmode , text='Rotate -90*' , padx=20 , pady=10 , command=rotate270))

    j = 0
    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=7 , column=i)
        j += 1

    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=8 , column=i)
        j += 1

    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=9 , column=i)
        j += 1

    gap = Label(playmode , text=' ' , padx=20 , pady=40)
    gap.pack(row=10, column=0)

    submitbutton = Button(playmode , text='Submit' , padx=40 , pady=30 , command=easycheckAnswer)
    submitbutton.grid(row=11 , column=5)

    exitbutton = Button(playmode , text='Exit' , padx=40 , pady=30 , command=easyexitProgram)
    exitbutton.grid(row=11 , column=0)



def easyexitProgram():
    playmode.destroy()
    return

def easycheckAnswer():
    global buttonarray
    global level1solution

    for i in range(0,18):
        if(buttonarray[i].button.color == 'white'):
            messagebox.showerror("Please fill all the boxes first!")
            return

    for i in range(0 , 18):
        if(buttonarray[i].button.cget('bg') == level1solution[i]):
            continue
        else:
            messagebox.showinfo("You Answer is incorrect. YOU LOSE!")
            break
    
    messagebox.showinfo("Congratulations! YOU WON!")


#----------------------------------------------------------------------------------------------------------
#FUNCTION TO IMPLEMENT EXPERT MODE

def Hardgame():
    global hardmode
    hardmode = Toplevel()
    hardmode.title("Play Screen")
    hardmode.iconbitmap("images/right_arrow.png")
    hardmode.geometry("800x900")

    clear_record()
    global buttonarray
    global hardlevel1arrows
    global hardlevel1solution
    buttonarray = []
    hardlevel1arrows = ['up' , 'up' , '' , '' , '' , '' , '' , '' , 'up' , 'right' , '' , '' , '' , '' , '' , '' , 'up' , 'up']
    level1colors = []


    hardlevel1solution = ['yellow' , 'yellow' , 'blue' , 'blue' , 'purple' , 'red' , 'green' , 'yellow' , 'blue' , 'orange' , 'purple' , 'red' , 'green' , 'green' , 'blue' , 'orange' , 'orange' ,'red']


    for i in range(0,18):
        level1colors.append('white')

    for i in range(0 , 18):
        buttonarray.append(myhardButton(hardlevel1arrows[i] , level1colors[i] , i+1))

        if i < 6:
            buttonarray[i].button.grid(row=0 , column=i)
        elif i < 12:
            buttonarray[i].button.grid(row=1 , column=(i%6))
        else:
            buttonarray[i].button.grid(row=2 , column=(i%6))

    label11 = Label(hardmode , text=' ')
    label11.grid(row=3 , column=0)

    label11 = Label(hardmode , text=' ')
    label11.grid(row=4 , column=0)

    label11 = Label(hardmode , text=' Choose a shape ')
    label11.grid(row=5 , column=2)

    imagelabel1 = Button(hardmode , image=shape1image , padx=50 , pady=50 , command=checkAvail1)
    imagelabel2 = Button(hardmode , image=shape2image , padx=50 , pady=50 , command=checkAvail2)
    imagelabel3 = Button(hardmode , image=shape3image , padx=50 , pady=50 , command=checkAvail3)
    imagelabel4 = Button(hardmode , image=shape4image , padx=50 , pady=50 , command=checkAvail4)
    imagelabel5 = Button(hardmode , image=shape5image , padx=50 , pady=50 , command=checkAvail5)
    imagelabel6 = Button(hardmode , image=shape6image , padx=50 , pady=50 , command=checkAvail6)

    imagelabel1.grid(row=6 , column=0)
    imagelabel2.grid(row=6 , column=1)
    imagelabel3.grid(row=6 , column=2)
    imagelabel4.grid(row=6 , column=3)
    imagelabel5.grid(row=6 , column=4)
    imagelabel6.grid(row=6 , column=5)
    
    rotatebuttonrow = []

    for i in range(0 , 6):
        rotatebuttonrow.append(Button(hardmode , text='Rotate 90*' , padx=20 , pady=10 , command=rotate90))
        
    for i in range(0 , 6):
        rotatebuttonrow.append(Button(hardmode , text='Rotate 180*' , padx=20 , pady=10 , command=rotate180))

    for i in range(0 , 6):
        rotatebuttonrow.append(Button(hardmode , text='Rotate -90*' , padx=20 , pady=10 , command=rotate270))

    j = 0
    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=7 , column=i)
        j += 1

    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=8 , column=i)
        j += 1

    for i in range(0 , 6):
        rotatebuttonrow[j].grid(row=9 , column=i)
        j += 1

    gap = Label(hardmode , text=' ' , padx=20 , pady=40)
    gap.grid(row=10 , column=0)

    submitbutton = Button(hardmode , text='Submit' , padx=40 , pady=30 , command=hardcheckAnswer)
    submitbutton.grid(row=11 , column=5)

    exitbutton = Button(hardmode , text='Exit' , padx=40 , pady=30 , command=hardexitProgram)
    exitbutton.grid(row=11 , column=0)

def hardexitProgram():
    playmode.destroy()
    return

def hardcheckAnswer():
    global buttonarray
    global hardlevel1solution

    for i in range(0,18):
        if(buttonarray[i].button.color == 'white'):
            messagebox.showerror("Please fill all the boxes first!")
            return

    for i in range(0 , 18):
        if(buttonarray[i].button.cget('bg') == hardlevel1solution[i]):
            continue
        else:
            messagebox.showinfo("You Answer is incorrect. YOU LOSE!")
            break
    
    messagebox.showinfo("Congratulations! YOU WON!")


#FUNCTION TO HANDLE BUTTON CLICK

def easybuttonAction(i):
    global level1arrows
    global buttonarray
    global currangle
    try:
        if(currangle == 0):
            if(currshape == 1):
                if(level1arrows[i-1] == 'up') and (level1arrows[i] == 'right') and (level1arrows[i+1] == 'right') and (level1arrows[i+7] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i].button.configure(bg= 'blue')
                    buttonarray[i+1].button.configure(bg= 'blue')
                    buttonarray[i+7].button.configure(bg= 'blue')
                    global shape1
                    shape1 += 1
            elif(currshape == 2):
                if(level1arrows[i-1] == 'left') and (level1arrows[i] == 'down') and (level1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    global shape2
                    shape2 += 1
            elif(currshape == 3):
                if(level1arrows[i-1] == 'down') and (level1arrows[i] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i].button.configure(bg= 'purple')
                    global shape3
                    shape3 += 1
            elif(currshape == 4):
                if(level1arrows[i-1] == 'down') and (level1arrows[i+5] == 'left') and (level1arrows[i+11] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i+5].button.configure(bg= 'red')
                    buttonarray[i+11].button.configure(bg= 'red')
                    global shape4
                    shape4 += 1
            elif(currshape == 5):
                if(level1arrows[i-1] == 'up') and (level1arrows[i+1] == 'up') and (level1arrows[i+7] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+1].button.configure(bg= 'yellow')
                    buttonarray[i+7].button.configure(bg= 'yellow')
                    global shape5
                    shape5 += 1
            elif(currshape == 6):
                if(level1arrows[i-1] == 'right') and (level1arrows[i+5] == 'up') and (level1arrows[i+6] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    buttonarray[i+6].button.configure(bg= 'green')
                    global shape6
                    shape6 += 1



        elif(currangle == 90):
            if(currshape == 1):
                if(level1arrows[i-1] == 'right') and (level1arrows[i+5] == 'down') and (level1arrows[i+11] == 'down') and (level1arrows[i+10] == 'right'):
                    if(buttonarray[i-1].button.color and buttonarray[i+5].button.color and buttonarray[i+11].button.color and buttonarray[i+10].button.color == 'while'):
                        buttonarray[i-1].button.configure(bg= 'blue')
                        buttonarray[i+5].button.configure(bg= 'blue')
                        buttonarray[i+11].button.configure(bg= 'blue')
                        buttonarray[i+10].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(level1arrows[i-1] == 'down') and (level1arrows[i] == 'up') and (level1arrows[i+6] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i].button.configure(bg= 'orange')
                    buttonarray[i+6].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(level1arrows[i-1] == 'left') and (level1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i+5].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(level1arrows[i-1] == 'down') and (level1arrows[i] == 'up') and (level1arrows[i+1] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i].button.configure(bg= 'red')
                    buttonarray[i+1].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(level1arrows[i-1] == 'right') and (level1arrows[i+5] == 'right') and (level1arrows[i+4] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    buttonarray[i+4].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(level1arrows[i-1] == 'right') and (level1arrows[i] == 'down') and (level1arrows[i+5] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    shape6 += 1



        elif (currangle == 180):
            if(currshape == 1):
                if(level1arrows[i-1] == 'down') and (level1arrows[i+5] == 'left') and (level1arrows[i+6] == 'left') and (level1arrows[i+7] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i+5].button.configure(bg= 'blue')
                    buttonarray[i+6].button.configure(bg= 'blue')
                    buttonarray[i+7].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(level1arrows[i-1] == 'left') and (level1arrows[i+5] == 'right') and (level1arrows[i+4] == 'up '):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    buttonarray[i+4].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(level1arrows[i-1] == 'down') and (level1arrows[i] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(level1arrows[i-1] == 'left') and (level1arrows[i+5] == 'right') and (level1arrows[i+11] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i+5].button.configure(bg= 'red')
                    buttonarray[i+11].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(level1arrows[i-1] == 'left') and (level1arrows[i+5] == 'down') and (level1arrows[i+6] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    buttonarray[i+6].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(level1arrows[i-1] == 'up') and (level1arrows[i] == 'down') and (level1arrows[i+6] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i].button.configure(bg= 'green')
                    buttonarray[i+6].button.configure(bg= 'green')
                    shape6 += 1


        elif(currangle == 270):
            if(currshape == 1):
                if(level1arrows[i-1] == 'up') and (level1arrows[i] == 'left') and (level1arrows[i+5] == 'up') and (level1arrows[i+11] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i].button.configure(bg= 'blue')
                    buttonarray[i+5].button.configure(bg= 'blue')
                    buttonarray[i+11].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(level1arrows[i-1] == 'right') and (level1arrows[i+5] == 'down') and (level1arrows[i+6] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    buttonarray[i+6].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(level1arrows[i-1] == 'left') and (level1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i+5].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(level1arrows[i-1] == 'right') and (level1arrows[i] == 'down') and (level1arrows[i+1] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i].button.configure(bg= 'red')
                    buttonarray[i+1].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(level1arrows[i-1] == 'left') and (level1arrows[i] == 'up') and (level1arrows[i+5] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(level1arrows[i-1] == 'right') and (level1arrows[i+5] == 'left') and (level1arrows[i+4] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    buttonarray[i+4].button.configure(bg= 'green')
                    shape6 += 1
    except:
        pass

def hardbuttonAction(i):
    global hardlevel1arrows
    global buttonarray
    global currangle
    try:
        if(currangle == 0):
            if(currshape == 1):
                if(hardlevel1arrows[i-1] == 'up') or (hardlevel1arrows[i] == 'right') or (hardlevel1arrows[i+1] == 'right') or (hardlevel1arrows[i+7] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i].button.configure(bg= 'blue')
                    buttonarray[i+1].button.configure(bg= 'blue')
                    buttonarray[i+7].button.configure(bg= 'blue')
                    global shape1
                    shape1 += 1
            elif(currshape == 2):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i] == 'down') or (hardlevel1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    global shape2
                    shape2 += 1
            elif(currshape == 3):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i].button.configure(bg= 'purple')
                    global shape3
                    shape3 += 1
            elif(currshape == 4):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i+5] == 'left') or (hardlevel1arrows[i+11] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i+5].button.configure(bg= 'red')
                    buttonarray[i+11].button.configure(bg= 'red')
                    global shape4
                    shape4 += 1
            elif(currshape == 5):
                if(hardlevel1arrows[i-1] == 'up') or (hardlevel1arrows[i+1] == 'up') or (hardlevel1arrows[i+7] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+1].button.configure(bg= 'yellow')
                    buttonarray[i+7].button.configure(bg= 'yellow')
                    global shape5
                    shape5 += 1
            elif(currshape == 6):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i+5] == 'up') or (hardlevel1arrows[i+6] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    buttonarray[i+6].button.configure(bg= 'green')
                    global shape6
                    shape6 += 1



        elif(currangle == 90):
            if(currshape == 1):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i+5] == 'down') or (hardlevel1arrows[i+11] == 'down') or (hardlevel1arrows[i+10] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i+5].button.configure(bg= 'blue')
                    buttonarray[i+11].button.configure(bg= 'blue')
                    buttonarray[i+10].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i] == 'up') or (hardlevel1arrows[i+6] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i].button.configure(bg= 'orange')
                    buttonarray[i+6].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i+5].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i] == 'up') or (hardlevel1arrows[i+1] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i].button.configure(bg= 'red')
                    buttonarray[i+1].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i+5] == 'right') or (hardlevel1arrows[i+4] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    buttonarray[i+4].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i] == 'down') or (hardlevel1arrows[i+5] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    shape6 += 1



        elif (currangle == 180):
            if(currshape == 1):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i+5] == 'left') or (hardlevel1arrows[i+6] == 'left') or (hardlevel1arrows[i+7] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i+5].button.configure(bg= 'blue')
                    buttonarray[i+6].button.configure(bg= 'blue')
                    buttonarray[i+7].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i+5] == 'right') or (hardlevel1arrows[i+4] == 'up '):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    buttonarray[i+4].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(hardlevel1arrows[i-1] == 'down') or (hardlevel1arrows[i] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i+5] == 'right') or (hardlevel1arrows[i+11] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i+5].button.configure(bg= 'red')
                    buttonarray[i+11].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i+5] == 'down') or (hardlevel1arrows[i+6] == 'down'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    buttonarray[i+6].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(hardlevel1arrows[i-1] == 'up') or (hardlevel1arrows[i] == 'down') or (hardlevel1arrows[i+6] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i].button.configure(bg= 'green')
                    buttonarray[i+6].button.configure(bg= 'green')
                    shape6 += 1


        elif(currangle == 270):
            if(currshape == 1):
                if(hardlevel1arrows[i-1] == 'up') or (hardlevel1arrows[i] == 'left') or (hardlevel1arrows[i+5] == 'up') or (hardlevel1arrows[i+11] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'blue')
                    buttonarray[i].button.configure(bg= 'blue')
                    buttonarray[i+5].button.configure(bg= 'blue')
                    buttonarray[i+11].button.configure(bg= 'blue')
                    shape1 += 1
            elif(currshape == 2):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i+5] == 'down') or (hardlevel1arrows[i+1] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'orange')
                    buttonarray[i+5].button.configure(bg= 'orange')
                    buttonarray[i+1].button.configure(bg= 'orange')
                    shape2 += 1
            elif(currshape == 3):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i+5] == 'right'):
                    buttonarray[i-1].button.configure(bg= 'purple')
                    buttonarray[i+5].button.configure(bg= 'purple')
                    shape3 += 1
            elif(currshape == 4):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i] == 'down') or (hardlevel1arrows[i+1] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'red')
                    buttonarray[i].button.configure(bg= 'red')
                    buttonarray[i+1].button.configure(bg= 'red')
                    shape4 += 1
            elif(currshape == 5):
                if(hardlevel1arrows[i-1] == 'left') or (hardlevel1arrows[i] == 'up') or (hardlevel1arrows[i+5] == 'left'):
                    buttonarray[i-1].button.configure(bg= 'yellow')
                    buttonarray[i].button.configure(bg= 'yellow')
                    buttonarray[i+5].button.configure(bg= 'yellow')
                    shape5 += 1
            elif(currshape == 6):
                if(hardlevel1arrows[i-1] == 'right') or (hardlevel1arrows[i+5] == 'left') or (hardlevel1arrows[i+4] == 'up'):
                    buttonarray[i-1].button.configure(bg= 'green')
                    buttonarray[i+5].button.configure(bg= 'green')
                    buttonarray[i+4].button.configure(bg= 'green')
                    shape6 += 1
    except:
        pass


def rotate90():
    global currangle
    currangle = 90

def rotate180():
    global currangle
    currangle = 180

def rotate270():
    global currangle
    currangle = 279

def checkAvail1():
    if shape1 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else:
        global currshape
        currshape = 1

def checkAvail2():
    if shape2 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else:
        global currshape
        currshape = 2

def checkAvail3():
    if shape3 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else: 
        global currshape
        currshape = 3

def checkAvail4():
    if shape4 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else:
        global currshape
        currshape = 4

def checkAvail5():
    if shape5 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else:
        global currshape
        currshape = 5

def checkAvail6():
    if shape6 >= 1:
        messagebox.showinfo("Try Again", "You cannot choose the same shape twice. Try another shape.")
    else:
        global currshape
        currshape = 6


#MAIN HOME BUTTON FUNCTIONS

def EasyButton():
    global mode
    mode = 'Starter'
    Easygame()

def HardButton():
    global mode
    mode = 'Hard'
    Hardgame()

#SETTING VARIABLES FOR ARROW IMAGES

up_arrow = ImageTk.PhotoImage(Image.open('images/up_arrow.png'))
down_arrow = ImageTk.PhotoImage(Image.open('images/down_arrow.png'))
left_arrow = ImageTk.PhotoImage(Image.open('images/left_arrow.png'))
right_arrow = ImageTk.PhotoImage(Image.open('images/right_arrow.png'))

#SETTING VARIABLES FOR SHAPES IMAGES

i1 = Image.open('images/shape1.png')
i2 = Image.open('images/shape2.png')
i3 = Image.open('images/shape3.png')
i4 = Image.open('images/shape4.png')
i5 = Image.open('images/shape5.png')
i6 = Image.open('images/shape6.png')


r1 = i1.resize((100, 90))
r2 = i2.resize((100, 96))
r3 = i3.resize((100, 96))
r4 = i4.resize((70, 96))
r5 = i5.resize((100, 96))
r6 = i6.resize((100, 96))

shape1image = ImageTk.PhotoImage(r1)
shape2image = ImageTk.PhotoImage(r2)
shape3image = ImageTk.PhotoImage(r3)
shape4image = ImageTk.PhotoImage(r4)
shape5image = ImageTk.PhotoImage(r5)
shape6image = ImageTk.PhotoImage(r6)


#MAIN WINDOW CONTENT

myLabelhome = Label(root , text="Choose Mode")
myLabelhome.pack()


myLabelhome1 = Label(root , text=" ")
myLabelhome1.pack()


buttoneasy = Button(root , text="Starter" , padx=40 , pady=20 , command=EasyButton)
buttoneasy.pack()


buttonhard = Button(root , text="Expert" , padx=40 , pady=20 , command=HardButton)
buttonhard.pack()

#CLASSES FOR PLAYBUTTONS

class myButton:
    def __init__(self , img , color , i):
        if (img == 'up'):
            self.button = Button(playmode , image=up_arrow , padx=40 , pady=40 , bg=color , command=partial(easybuttonAction,i))
        elif (img == 'down'):
            self.button = Button(playmode , image=down_arrow , padx=40 , pady=40 , bg=color , command=partial(easybuttonAction,i))
        elif (img == 'left'):
            self.button = Button(playmode , image=left_arrow , padx=40 , pady=40 , bg=color , command=partial(easybuttonAction,i))
        elif (img == 'right'):
            self.button = Button(playmode , image=right_arrow , padx=40 , pady=40 , bg=color , command=partial(easybuttonAction,i))
        else:
            self.button = Button(playmode , padx=40 , pady=40 , command=partial(easybuttonAction,i))

class myhardButton:
    def __init__(self , img , color , i):
        if (img == 'up'):
            self.button = Button(hardmode , image=up_arrow , padx=40 , pady=40 , bg=color , command=partial(hardbuttonAction,i))
        elif (img == 'down'):
            self.button = Button(hardmode , image=down_arrow , padx=40 , pady=40 , bg=color , command=partial(hardbuttonAction,i))
        elif (img == 'left'):
            self.button = Button(hardmode , image=left_arrow , padx=40 , pady=40 , bg=color , command=partial(hardbuttonAction,i))
        elif (img == 'right'):
            self.button = Button(hardmode , image=right_arrow , padx=40 , pady=40 , bg=color , command=partial(hardbuttonAction,i))
        else:
            self.button = Button(hardmode , padx=63 , pady=56 , bg=color , command=partial(hardbuttonAction,i))


root.mainloop()
