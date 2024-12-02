# sneck-and-turtle
import turtle as t
import random as rd

# Setup window
t.bgcolor('pink')

# Snake setup
snake = t.Turtle()
snake.shape('square')
snake.color('black')
snake.speed(0)
snake.penup()
snake.hideturtle()

# Fruit setup
fruit = t.Turtle()
fruit_shape = ((0, 0), (14, 2), (18, 6), (20, 20), (6, 18), (2, 14))
t.register_shape('fruit', fruit_shape)
fruit.shape('fruit')
fruit.color('black')
fruit.penup()
fruit.hideturtle()
fruit.speed(0)

# Game state variables
game_started = False
text_turtle = t.Turtle()
text_turtle.write('Press SPACE to Start', align='center', font=('Arial', 16, 'bold'))
text_turtle.hideturtle()

score_turtle = t.Turtle()
score_turtle.hideturtle()
score_turtle.speed(0)

# Check if the snake is outside the window
def outside_window():
    left_wall = -t.window_width() / 2
    right_wall = t.window_width() / 2
    top_wall = t.window_height() / 2
    bottom_wall = -t.window_height() / 2
    (x, y) = snake.pos()
    outside = x < left_wall or x > right_wall or y < bottom_wall or y > top_wall
    return outside

# End the game
def game_over():
    snake.color('black')
    fruit.color('black')
    snake.hideturtle()
    fruit.hideturtle()
    text_turtle.clear()
    text_turtle.write('GAME OVER!', align='center', font=('Arial', 30, 'normal'))

# Display the score
def display_score(current_score):
    score_turtle.clear()
    score_turtle.penup()
    x = (t.window_width() / 2) - 50
    y = (t.window_height() / 2) - 50
    score_turtle.setpos(x, y)
    score_turtle.write(str(current_score), align='right', font=('Arial', 40, 'bold'))

# Place the fruit randomly
def place_fruit():
    fruit.hideturtle()
    x = rd.randint(-t.window_width() // 2 + 20, t.window_width() // 2 - 20)
    y = rd.randint(-t.window_height() // 2 + 20, t.window_height() // 2 - 20)
    fruit.setpos(x, y)
    fruit.showturtle()

# Start the game
def start_game():
    global game_started
    if game_started:
        return
    game_started = True

    score = 0
    text_turtle.clear()
    snake_speed = 1
    snake_length = 2
    snake.shapesize(1, snake_length, 1)
    snake.showturtle()
    display_score(score)
    place_fruit()

    while True:
        snake.forward(snake_speed)
        # Check if the snake eats the fruit
        if snake.distance(fruit) < 20:
            place_fruit()
            snake_length += 1
            snake.shapesize(1, snake_length, 1)
            snake_speed += 0.1  # Increase speed slightly
            score += 10
            display_score(score)
        
        # Check if the snake goes outside the window
        if outside_window():
            game_over()
            break

# Snake movement controls
def move_up():
    if snake.heading() != 270:  # Prevent backward movement
        snake.setheading(90)

def move_down():
    if snake.heading() != 90:
        snake.setheading(270)

def move_right():
    if snake.heading() != 180:
        snake.setheading(0)

def move_left():
    if snake.heading() != 0:
        snake.setheading(180)

# Bind keys to actions
t.onkey(start_game, 'space')
t.onkey(move_up, 'Up')
t.onkey(move_right, 'Right')
t.onkey(move_down, 'Down')
t.onkey(move_left, 'Left')
t.listen()
t.mainloop()
