#Tic Tac Toe Game - Version1 simply to build rules for a functional user interface to play against the user.
#Version2 will implement logic for CPU to be competitive using brute force logic, where for every move, the computer will have a given if statement to execute a following move

#Let's start with random to play, in order to master the Version1 functionality first
import random

#Apparently to end a game, I need to import a command from the sys library
import sys

#Lets set the tic-tac-toe board as a global variable to access from all functions
global_board = ['___|___|___', '___|___|___', '   |   |   ']



def start(string1):
    #This function is to ask the user if they would like to start the game off or not
            string1 = string1.lower()
            if string1 == 'yes':
                user_move()
            elif string1 == 'no':
                rcol = [1, 5, 9]
                rrow = [1, 2, 3]
                comp_move()


def display_board():
    #This function is for printing the board/array in a presentable manner in the console
                board_string = ''
                for x in range(len(global_board)):
                    board_string +=(global_board[x]) + '\n'
                return board_string
                    #Double indexes can print the string content of the list and not the list brackets

def update_board(choice,row,column):
                #This formula updates the global board using parameters given
                #Iterates through the strings and creates a new string with updated choice

            global_board[row - 1] = global_board[row-1][:column]+ choice +global_board[row-1][column + 1:]
            return global_board



def comp_move():

    #Define the variables
        options = 'O'
        rrow = [1, 2, 3]
        rcol = [1, 5, 9]
        frow = random.choice(rrow)
        fcol = random.choice(rcol)
        
        #Lets check if the spot has been taken
        occupancy(fcol,frow,2)
        
        #If not, lets update the board
        update_board(options,frow,fcol)
        print('My move')
        end_game('Computer wins!')
        print(display_board())
        print('Your move')
        user_move()

def vertical(options,frow,fcol):
    for x in [0,1,2]:
        if global_board[x][fcol] == 'X':
            return False
    frow = x
    update_board(options, frow, fcol)


def user_move():
        options = 'X'
        #Lets ask the user for their choice
        urow = int(input('Row: '))
        ucol = int(input('Column: '))

        #Lets prevent out of range entries
        range = [1,2,3]
        if ucol not in range or urow not in range:
            print('Out of Range, Give me a number from 1 to 3')
            user_move()

        #Lets translate the user input to indexes in the array
        if ucol == 2:
            ucol = 5
        elif ucol == 3:
            ucol = 9

        #Lets check if the space has been taken
        occupancy(ucol,urow,1)

        #If not lets update the board
        update_board(options, urow, ucol)
        #If the board meets the end_game parameters, it will end or continue to print
        end_game('Player wins!')
        print(display_board())

        #Lets give the user the option to proceed or take back their choice(In case of mistakes)
        print('Confirm(type yes) or Undo(type no): ')
        cont = input()
        cont = cont.lower()
        if cont == 'yes':
            comp_move()
        elif cont == 'no':
            #Call the undo move function
            undo_move(urow,ucol)


def undo_move(row,col):
        #Undo move function is handy for mistakes or hasty moves
        if row in [1,2]:
            options = '_'
        else:
            options = ' '
        #Let's replace the previously altered character back to the '_'
        update_board(options, row, col)
        print(display_board())
        user_move()

def occupancy(col,row,turn):
    #This function is for checking if the box has been taken or not
    if global_board[row-1][col] == 'X' or global_board[row-1][col] == 'O':

        #If the space has a character that isn't free, it will requeue the choice
        if turn == 1:
            print('That spot is taken')
            user_move()
        elif turn== 2:
            comp_move()

def end_game(string):
    #Let's build a function to end the game once 3 X's or 3 O's are in a series
    for x in range(0,3):
        if global_board[x][1] == global_board[x][5] and global_board[x][1] == global_board[x][9] and global_board[x][1] in ('O','X'):
            #This is the horizontal case
            print(display_board())
            print('Game Over!')
            print(string)
            sys.exit()

    for y in [1,5,9]:
        if global_board[0][y] == global_board[1][y] and global_board[2][y] == global_board[0][y] and global_board[0][y] in ('O','X'):
            #This is the vertical case
            print(display_board())
            print('Game Over!')
            print(string)
            sys.exit()

        #This is the right to left(Up to down) diagonal case
    if global_board[0][9] == global_board[1][5] and  global_board[2][1] == global_board[0][9] and global_board[0][9] in ('O','X'):
        print(display_board())
        print('Game Over!')
        print(string)
        sys.exit()

        #This is the left to right(Up to down) diagonal case
    if global_board[0][1] == global_board[1][5] and  global_board[2][9] == global_board[0][1] and global_board[0][1] in ('O','X'):
        print(display_board())
        print('Game Over!')
        print(string)
        sys.exit()


print(display_board())
print('Would you like to start?: ')
answer = input('yes or no ')
while answer not in ('yes','no'):
    answer = input('invalid answer: yes or no: ')
start(answer)
