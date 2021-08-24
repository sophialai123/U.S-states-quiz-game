# U.S-states-quiz-game
"""
Use turtle library and csv data to create a US states quiz game
https://www.sporcle.com/games/g/states
"""
# in order to display an image in turtle , the image has to covert into a gif file

import turtle
import pandas

# create a screen and the title
screen = turtle.Screen()
screen.title("U.S.States Game")

# set a turtle shape to a new shape by using an image:
# to get the image path from gif:
image = "blank_states_img.gif"
screen.addshape(image)

# add a shape to the turtle
turtle.shape(image)

""" 
 how the x and y value , all in the 50_states.csv file.
# how to get x , y coordinates of each states in turtle
def get_mouse_clink_coordinate(x, y):
    print(x, y)
# when you clink the area of the state, the x and y value will be printed. eg: -251.0  56.0
turtle.onscreenclick(get_mouse_clink_coordinate)
"""

# to read the csv date use pandas:
data = pandas.read_csv("50_states.csv")
# covert data into a list
state_list = data['state'].to_list()
print(state_list)

#create an empty list, so later on all the correct answers can be appended to this list
correct_guesses = []

# Use a loop to allow the user to keep guessing  # while loop
while len(correct_guesses) < 50:
    # create a user input box in the screen :
    # ask user to input their answers: # 6. Keep track of the score use title = len() method
    guess_state = screen.textinput(title=f"{len(correct_guesses)} / 50 State Correct",
                                   prompt="What is another state's name?").title()

    # if users want to exit the game: if we use break statement here, then we do not have to use turtle.mainloop()
    if guess_state == "Exit":
        # print out all the miss state list that users did  not guess right:
        # use List Comprehension:
        # if the condition is passed, then the first state will be add in the missing_states list:
        missing_states = [state for state in state_list if state not in correct_guesses]
        print(missing_states)
        
        # crate a new dataframe
        new_data = pandas.DataFrame(missing_states)
        new_data.to_csv("missing_states.csv")
        break
        
    # Check if the user guess is among the 50 states 
    if guess_state in state_list:
        # Record the correct guesses in a list
        correct_guesses.append(guess_state)
        t = turtle.Turtle()  # create a turtle attribute
        t.hideturtle()  # hide the turtle
        t.penup()  
        
        # how to get the single row value from in the csv file:
        one_row_value = data[data.state == guess_state]
        t.goto(int(one_row_value.x), int(one_row_value.y))  # turtle goto x and y values must be int
        t.write(guess_state)  # turtle write one single value x , y on the map , match user's guess
        # t.write(one_row_value.state.item()) # get the first item in that row use pandas.series.item(), same as above
