# scoobyydooooooo
Experiment-2

Aim - 8 Puzzle Single Player Game (Breadth First Search)
Code-
# Import the necessary libraries
from time import time
from queue import Queue
# Creating a class Puzzle
class Puzzle:
    # Setting the goal state of 8-puzzle 
    goal_state = [1, 2, 3, 8, 0, 4, 7, 6, 5]
    num_of_instances = 0
    # constructor to initialize the class members
    def __init__(self, state, parent, action):
        self.parent = parent
        self.state = state
        self.action = action
        # Incrementing the number of instances by 1
        Puzzle.num_of_instances += 1
    # function used to display a state of 8-puzzle
    def __str__(self):
        return str(self.state[0:3]) + '\n' + str(self.state[3:6]) + '\n' + str(self.state[6:9])
    # method to compare the current state with the goal state
    def goal_test(self):
        # Comparing the current state with the goal state
        if self.state == Puzzle.goal_state:
            return True
        return False
# static method to find the legal action based on the current board position
    @staticmethod
    def find_legal_actions(i, j):
        legal_action = ['U', 'D', 'L', 'R']
        if i == 0:  
            # if row is 0 in board, then 'U' (Up) is disabled
            legal_action.remove('U')
        elif i == 2:  
            # if row is 2 in board, then 'D' (Down) is disabled
            legal_action.remove('D') 
        if j == 0: 
            # if column is 0, then 'L' (Left) is disabled
            legal_action.remove('L')
        elif j == 2:
            # if column is 2, then 'R' (Right) is disabled
            legal_action.remove('R')
        return legal_action
    # method to generate the child of the current state of the board
    def generate_child(self):
        # Create an empty list for children
        children = []
        x = self.state.index(0)
        i = int(x / 3)
        j = int(x % 3)
        # Find the legal actions based on i and j values
        legal_actions = self.find_legal_actions(i, j)
        # Iterate over all legal actions 
        for action in legal_actions:
            new_state = self.state.copy()
  # If the legal action is UP
            if action == 'U':
                # Swapping between current index of 0 with its up element on the board
                new_state[x], new_state[x - 3] = new_state[x - 3], new_state[x]
            elif action == 'D':
                # Swapping between current index of 0 with its down element on the board
                new_state[x], new_state[x + 3] = new_state[x + 3], new_state[x]
            elif action == 'L':
                # Swapping between the current index of 0 with its left element on the board
                new_state[x], new_state[x - 1] = new_state[x - 1], new_state[x]
            elif action == 'R':
                # Swapping between the current index of 0 with its right element on the board
                new_state[x], new_state[x + 1] = new_state[x + 1], new_state[x]
            children.append(Puzzle(new_state, self, action))
        # Return the children
        return children
    # method to find the solution
    def find_solution(self):
        solution = []
        solution.append(self.action)
        path = self
        while path.parent != None:
            path = path.parent
            solution.append(path.action)
        solution = solution[:-1]
        solution.reverse()
        return solution
# method for breadth first search
def breadth_first_search(initial_state):
    start_node = Puzzle(initial_state, None, None)
    print("Initial state:")
    print(start_node)
    if start_node.goal_test():
        return start_node.find_solution()
    q = Queue()
    q.put(start_node)
    explored = []
    # Iterate the queue until empty
    while not q.empty():
        node = q.get()
        # Append the state of node in the explored list
        explored.append(node.state)
        # Generate the child nodes of the current node
        children = node.generate_child()
        # Iterate over each child node in children
        for child in children:
            if child.state not in explored:
                if child.goal_test():
                    return child.find_solution()
                q.put(child)
    return None
# Start executing the 8-puzzle with setting up the initial state
state = [
    [1, 3, 4, 8, 6, 2, 7, 0, 5],
    [2, 8, 1, 0, 4, 3, 7, 6, 5],
    [2, 8, 1, 4, 6, 3, 0, 7, 5]
]

# Iterate over number of initial states
for i in range(0, 3):
    # Initialize the num_of_instances to zero
    Puzzle.num_of_instances = 0
    # Set t0 to current time
    t0 = time()
    # Call breadth_first_search
    bfs = breadth_first_search(state[i])
    # Get the time t1 after executing the breadth_first_search method
    t1 = time() - t0
    # Output the result of BFS, the space used (number of instances created), and the time taken
    print('BFS Solution:', bfs)
    print('Space (number of instances):', Puzzle.num_of_instances)
    print('Time taken:', t1)
    print()
print('------------------------------------------') 

