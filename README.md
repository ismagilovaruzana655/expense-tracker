import tkinter as tk
from tkinter import messagebox
import json
from datetime import datetime

expenses = []

def add_expense():
    amount = entry_amount.get()
    category = entry_category.get()
    date = entry_date.get()

    try:
        amount = float(amount)
        if amount <= 0:
            raise ValueError
        datetime.strptime(date, "%Y-%m-%d")
    except:
        messagebox.showerror("Ошибка", "Проверьте ввод данных")
        return

    expense = {
        "amount": amount,
        "category": category,
        "date": date
    }

    expenses.append(expense)
    listbox.insert(tk.END, f"{date} | {category} | {amount}")

    save_data()

def save_data():
    with open("expenses.json", "w", encoding="utf-8") as f:
        json.dump(expenses, f, ensure_ascii=False, indent=4)

def load_data():
    try:
        with open("expenses.json", "r", encoding="utf-8") as f:
            data = json.load(f)
            for e in data:
                expenses.append(e)
                listbox.insert(tk.END, f"{e['date']} | {e['category']} | {e['amount']}")
    except:
        pass

root = tk.Tk()
root.title("Expense Tracker")

tk.Label(root, text="Сумма").pack()
entry_amount = tk.Entry(root)
entry_amount.pack()

tk.Label(root, text="Категория").pack()
entry_category = tk.Entry(root)
entry_category.pack()

tk.Label(root, text="Дата (YYYY-MM-DD)").pack()
entry_date = tk.Entry(root)
entry_date.pack()

tk.Button(root, text="Добавить расход", command=add_expense).pack()

listbox = tk.Listbox(root, width=50)
listbox.pack()

load_data()

root.mainloop()
