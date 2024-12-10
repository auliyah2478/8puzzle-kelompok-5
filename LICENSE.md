import random

class Puzzle8:
    def __init__(self):  # Perbaikan pada konstruktor
        self.board = self.generate_board()
        self.goal = [[1, 2, 3], [4, 5, 6], [7, 8, 0]]  # Goal state
    
    def generate_board(self):
        """Generate a random but solvable 8-puzzle board."""
        tiles = [1, 2, 3, 4, 5, 6, 7, 8, 0]
        while True:
            random.shuffle(tiles)
            board = [tiles[:3], tiles[3:6], tiles[6:]]
            if self.is_solvable(board):
                return board
    
    def is_solvable(self, board):
        """Check if a puzzle is solvable."""
        tiles = [tile for row in board for tile in row if tile != 0]
        inversions = 0
        for i in range(len(tiles)):
            for j in range(i + 1, len(tiles)):
                if tiles[i] > tiles[j]:
                    inversions += 1
        return inversions % 2 == 0

    def display_board(self):
        """Print the current board."""
        for row in self.board:
            print(' '.join(str(tile) if tile != 0 else ' ' for tile in row))
        print()
    
    def find_empty_tile(self):
        """Find the position of the empty tile (0)."""
        for i, row in enumerate(self.board):
            for j, tile in enumerate(row):
                if tile == 0:
                    return i, j
    
    def move(self, direction):
        """Move the empty tile in the given direction."""
        i, j = self.find_empty_tile()
        if direction == 'up' and i > 0:
            self.board[i][j], self.board[i-1][j] = self.board[i-1][j], self.board[i][j]
        elif direction == 'down' and i < 2:
            self.board[i][j], self.board[i+1][j] = self.board[i+1][j], self.board[i][j]
        elif direction == 'left' and j > 0:
            self.board[i][j], self.board[i][j-1] = self.board[i][j-1], self.board[i][j]
        elif direction == 'right' and j < 2:
            self.board[i][j], self.board[i][j+1] = self.board[i][j+1], self.board[i][j]
        else:
            print("Invalid move!")
    
    def is_solved(self):
        """Check if the puzzle is solved."""
        return self.board == self.goal

def main():
    game = Puzzle8()
    print("Welcome to the 8-Puzzle Game!")
    print("Use 'up', 'down', 'left', 'right' to move the empty tile.")
    print("Goal: Arrange the tiles in order (1-8) with the empty tile at the bottom-right.")
    print()
    
    while not game.is_solved():
        game.display_board()
        move = input("Your move: ").strip().lower()
        if move in ['up', 'down', 'left', 'right']:
            game.move(move)
        else:
            print("Invalid input. Use 'up', 'down', 'left', 'right'.")
    
    print("Congratulations! You solved the puzzle!")
    game.display_board()

if __name__ == '__main__':
    main()
