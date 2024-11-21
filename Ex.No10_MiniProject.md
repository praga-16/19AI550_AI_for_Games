# Ex.No: 11  Mini Project 
#### DATE: 08/10/2024                                                                            
#### REGISTER NUMBER : 212221240002
### AIM: 
The aim of this program is to create a two-player Connect Four game where one player is controlled by an AI.

### ALGORITHM:


1. **Initialize Game:**
   - Set up a 7x6 grid to represent the Connect Four board.
   - Define players: Human (red) and AI (yellow).
   - Track the number of discs in each column and the grid state (empty or filled by a player).

2. **Draw Grid:**
   - Draw vertical lines and empty spots for each cell.
   - Update the display.

3. **Handle Human Move:**
   - Detect which column the human clicked on.
   - If the column is valid (not full), place a red disc in the lowest available row.
   - Update the grid state and column count.
   - Check if the move results in a win (4 consecutive discs in any direction).

4. **Check for Win:**
   - For the latest disc, check in all four directions (horizontal, vertical, diagonal-right, diagonal-left).
   - Count consecutive discs of the same color in both directions for each direction.
   - If 4 or more discs align, declare the player as the winner.

5. **Handle AI Move:**
   - After the human move, if no win occurs, allow the AI to take its turn.
   - AI randomly selects a valid column.
   - Place a yellow disc in the lowest available row in that column.
   - Update the grid state and column count.
   - Check if the AI's move results in a win.

6. **End Game:**
   - If a player wins, display the winning message and stop accepting further moves.
   - If the board fills up with no winner, the game ends in a draw.

7. **Repeat:**
   - If no win, switch turns and repeat the process. The human and AI continue to alternate turns until a win or draw occurs.

### PROGRAM:

```python
from turtle import *
from freegames import line
import random

# Define the grid size (7x6)
COLUMNS = 7
ROWS = 6

# Define the turns for players
turns = {'red': 'yellow', 'yellow': 'red'}
state = {
    'player': 'red',  # Human starts
    'rows': [0] * COLUMNS,  # Track how many discs are in each column
    'grid': [[''] * ROWS for _ in range(COLUMNS)]  # Track the grid state (color of each cell)
}

def grid():
    """Draw Connect Four grid."""
    bgcolor('light blue')

    for x in range(-150, 200, 50):  # Vertical lines
        line(x, -200, x, 200)

    for x in range(-175, 200, 50):
        for y in range(-175, 200, 50):
            up()
            goto(x, y)
            dot(40, 'white')  # Empty spots

    update()

def check_winner(column, row, player):
    """Check if the current move leads to a win."""
    # Directions: (dx, dy) -> (horizontal, vertical, diagonal-right, diagonal-left)
    directions = [(1, 0), (0, 1), (1, 1), (1, -1)]

    def count_discs(dx, dy):
        """Count the number of consecutive discs of the same player in a direction."""
        count = 0
        x, y = column, row
        while 0 <= x < COLUMNS and 0 <= y < ROWS and state['grid'][x][y] == player:
            count += 1
            x, y = x + dx, y + dy
        return count

    for dx, dy in directions:
        # Check both directions (positive and negative) for a total of 4 discs
        total = count_discs(dx, dy) + count_discs(-dx, -dy) - 1
        if total >= 4:
            return True
    return False

def ai_move():
    """AI makes a move by choosing a random available column."""
    available_columns = [c for c in range(COLUMNS) if state['rows'][c] < ROWS]
    
    if not available_columns:
        return  # No moves left (grid is full)
    
    # Choose a random column for the AI move
    column = random.choice(available_columns)
    row = state['rows'][column]
    
    x = column * 50 - 200 + 25  # Adjust x position to column
    y = row * 50 - 200 + 25  # Adjust y position based on row count
    
    up()
    goto(x, y)
    dot(40, 'yellow')  # AI plays yellow
    update()
    
    # Update the state with the AI move
    state['rows'][column] += 1
    state['grid'][column][row] = 'yellow'
    
    # Check if AI won
    if check_winner(column, row, 'yellow'):
        up()
        goto(-150, 210)
        color('black')
        write('YELLOW WINS!', font=('Arial', 24, 'bold'))
        onscreenclick(None)  # Stop further clicks
        return
    
    # Switch back to human player
    state['player'] = 'red'

def tap(x, y):
    """Handle human move (red) and then AI move (yellow)."""
    if state['player'] != 'red':
        return  # Wait for AI move if it's AI's turn

    rows = state['rows']
    grid = state['grid']

    column = int((x + 200) // 50)  # Determine which column was clicked
    if column < 0 or column >= COLUMNS or rows[column] >= ROWS:
        return  # Ignore clicks outside valid range or full columns

    row = rows[column]
    x = column * 50 - 200 + 25  # Adjust x position to column
    y = row * 50 - 200 + 25  # Adjust y position based on row count

    up()
    goto(x, y)
    dot(40, 'red')  # Human plays red
    update()

    # Update the state with the human move
    rows[column] = row + 1
    grid[column][row] = 'red'

    # Check if the human player won
    if check_winner(column, row, 'red'):
        up()
        goto(-150, 210)
        color('black')
        write('RED WINS!', font=('Arial', 24, 'bold'))
        onscreenclick(None)  # Stop further clicks
        return

    # Switch to AI's turn and make AI move
    state['player'] = 'yellow'
    ai_move()

# Set up the game window and start the game
setup(420, 420, 370, 0)
hideturtle()
tracer(False)
grid()
onscreenclick(tap)
done()
```



### OUTPUT:
![image](https://github.com/user-attachments/assets/27f4095b-13f9-4f00-afe5-ea1ee07d1d91)



### RESULT:
Thus, a two-player Connect Four game where one player is controlled by an AI is created.
