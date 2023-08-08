import tkinter as tk
from tkinter import messagebox, simpledialog

class LibraryApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Library Management System")

        self.books_file = "books.txt"
        self.load_books()

        self.library = Library(self.books)
        self.student = Student('Your Name', self.library)

        self.menu_label = tk.Label(root, text="LIBRARY MENU", font=('Helvetica', 16))
        self.menu_label.pack()

        self.display_button = tk.Button(root, text="Display Available Books", command=self.show_avail_books)
        self.display_button.pack()

        self.borrow_button = tk.Button(root, text="Borrow a Book", command=self.request_book)
        self.borrow_button.pack()

        self.return_button = tk.Button(root, text="Return a Book", command=self.return_book)
        self.return_button.pack()

        self.view_button = tk.Button(root, text="View Your Books", command=self.view_borrowed)
        self.view_button.pack()

        self.exit_button = tk.Button(root, text="Exit", command=self.save_and_exit)
        self.exit_button.pack()

    def load_books(self):
        try:
            with open(self.books_file, "r") as file:
                self.books = dict(line.strip().split(":") for line in file)
        except FileNotFoundError:
            self.books = {
                'Story books': 'Free',
                'Harry Potter': 'Free',
                'Rich Dad Poor Dad': 'Free'
            }

    def save_books(self):
        with open(self.books_file, "w") as file:
            for book, borrower in self.books.items():
                file.write(f"{book}:{borrower}\n")

    def show_avail_books(self):
        avail_books = "\n".join([book for book, borrower in self.books.items() if borrower == 'Free'])
        if avail_books:
            messagebox.showinfo("Available Books", f"Our Library Can Offer You The Following Books:\n{avail_books}")
        else:
            messagebox.showinfo("Available Books", "No books available at the moment.")

    def request_book(self):
        book = simpledialog.askstring("Borrow a Book", "Enter the name of the book you'd like to borrow:")
        if book:
            if self.library.lend_book(book, self.student.name):
                self.student.books.append(book)
                messagebox.showinfo("Borrow Book", f"You have borrowed '{book}'.")
            else:
                messagebox.showinfo("Borrow Book", f"Sorry, '{book}' is currently on loan.")

    def return_book(self):
        book = simpledialog.askstring("Return a Book", "Enter the name of the book you'd like to return:")
        if book in self.student.books:
            self.library.return_book(book)
            self.student.books.remove(book)
            messagebox.showinfo("Return Book", f"You have returned '{book}'.")
        else:
            messagebox.showinfo("Return Book", f"You haven't borrowed '{book}', try another...")

    def view_borrowed(self):
        if not self.student.books:
            messagebox.showinfo("View Borrowed Books", "You haven't borrowed any books.")
        else:
            borrowed_books = "\n".join(self.student.books)
            messagebox.showinfo("View Borrowed Books", f"You have borrowed the following books:\n{borrowed_books}")

    def save_and_exit(self):
        self.save_books()
        self.root.destroy()

class Library:
    def __init__(self, books):
        self.books = books

    def lend_book(self, requested_book, name):
        if self.books[requested_book] == 'Free':
            self.books[requested_book] = name
            return True
        else:
            return False

    def return_book(self, returned_book):
        self.books[returned_book] = 'Free'

class Student:
    def __init__(self, name, library):
        self.name = name
        self.books = []
        self.library = library

if __name__ == '__main__':
    root = tk.Tk()
    app = LibraryApp(root)
    root.mainloop()
