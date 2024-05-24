# Tic-Tac-Toe-
Develop a simple Tic-Tac-Toe game with a graphical user interface. Include features for  player vs. player and player vs. computer modes.


import tkinter as tk
import random

class TicTacToe:
    def __init__(self, master):
        self.master = master
        self.master.title("Tic-Tac-Toe")
        self.board = [' ' for _ in range(9)]
        self.current_player = 'X'
        self.game_over = False
        self.buttons = []

        for i in range(3):
            for j in range(3):
                button = tk.Button(master, text='', width=10, height=5,
                                   command=lambda row=i, col=j: self.make_move(row, col))
                button.grid(row=i, column=j)
                self.buttons.append(button)

        self.mode = tk.StringVar(master)
        self.mode.set("Player vs Player")
        mode_menu = tk.OptionMenu(master, self.mode, "Player vs  Player", "Player vs Computer")
        mode_menu.grid(row=3, columnspan=3)

    def make_move(self, row, col):
        index = 3 * row + col
        if self.board[index] == ' ' and not self.game_over:
            self.board[index] = self.current_player
            self.buttons[index].config(text=self.current_player)
            if self.check_winner(self.current_player):
                self.game_over = True
                tk.messagebox.showinfo("Game Over", f"Player {self.current_player} wins!")
            elif ' ' not in self.board:
                self.game_over = True
                tk.messagebox.showinfo("Game Over", "It's a draw!")
            else:
                self.current_player = 'O' if self.current_player == 'X' else 'X'
                if self.mode.get() == "Player vs Computer" and self.current_player == 'O':
                    self.computer_move()

    def check_winner(self, player):
        win_conditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Columns
            [0, 4, 8], [2, 4, 6]              # Diagonals
        ]
        for condition in win_conditions:
            if all(self.board[i] == player for i in condition):
                return True
        return False

    def computer_move(self):
        empty_cells = [i for i, mark in enumerate(self.board) if mark == ' ']
        index = random.choice(empty_cells)
        self.make_move(index // 3, index % 3)

def main():
    root = tk.Tk()
    game = TicTacToe(root)
    root.mainloop()

if __name__ == "__main__":
    main()
