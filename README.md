import tkinter as tk
import random
from tkinter import messagebox

def generate_bingo_card():
    columns = [
        random.sample(range(1, 16), 5),
        random.sample(range(16, 31), 5),
        random.sample(range(31, 46), 5),
        random.sample(range(46, 61), 5),
        random.sample(range(61, 76), 5)
    ]
    
    columns[2][2] = "Free"
    
    return [list(row) for row in zip(*columns)]
class BingoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Bingo Card")
        
        self.card = generate_bingo_card()
        
        self.selected = [[False for _ in range(5)] for _ in range(5)]
        
        self.buttons = []
        self.create_grid()

    def create_grid(self):
        for i in range(5):
            row = []
            for j in range(5):
                btn = tk.Button(self.root, text=self.card[i][j], width=10, height=3,
                                command=lambda i=i, j=j: self.select_number(i, j))
                btn.grid(row=i, column=j)
                row.append(btn)
            self.buttons.append(row)

    def select_number(self, row, col):
        if self.card[row][col] == "Free" or not self.selected[row][col]:
            self.buttons[row][col].config(bg="lightblue")
            self.selected[row][col] = True
            self.check_bingo()

    def check_bingo(self):
        if self.check_win():
            messagebox.showinfo("Bingo!","Bingo!")

    def check_win(self):
        for i in range(5):
            if all(self.selected[i]): 
                return True
            if all([self.selected[j][i] for j in range(5)]): 
                return True
        
        if all([self.selected[i][i] for i in range(5)]) or all([self.selected[i][4-i] for i in range(5)]):
            return True
        return False

    def check_reach(self):
        for i in range(5):
            if self.selected[i].count(True) == 4:  
                return True
            if [self.selected[j][i] for j in range(5)].count(True) == 4: 
                return True
        return False
if __name__ == "__main__":
    root = tk.Tk()
    app = BingoApp(root)
    root.mainloop()
