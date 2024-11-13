import tkinter as tk
import bcrypt
import sqlite3


# Функция для подключения к базе данных
def connect_db():
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS users 
                      (username TEXT PRIMARY KEY, password TEXT)''')
    conn.commit()
    return conn, cursor


def check_user(username, password):
    conn, cursor = connect_db()
    cursor.execute("SELECT password FROM users WHERE username=?", (username,))
    result = cursor.fetchone()
    conn.close()

    if result and bcrypt.checkpw(password.encode('utf-8'), result[0]):
        return True
    return False


def register_user(username, password):
    conn, cursor = connect_db()
    hashed_password = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())
    try:
        cursor.execute("INSERT INTO users (username, password) VALUES (?, ?)",
                       (username, hashed_password))
        conn.commit()
        label_result.config(text="Регистрация успешна!")
    except sqlite3.IntegrityError:
        label_result.config(text="Пользователь уже существует!")
    finally:
        conn.close()


def on_login():
    username = entry_username.get()
    password = entry_password.get()

    if check_user(username, password):
        label_result.config(text="Успешный вход!")
    else:
        label_result.config(text="Неправильный логин или пароль!")


def on_register():
    username = entry_reg_username.get()
    password = entry_reg_password.get()
    register_user(username, password)


# Настройка окна Tkinter
root = tk.Tk()
root.title("Регистрация и Вход")

# Поля для входа
label_username = tk.Label(root, text="Имя пользователя (вход):")
label_username.pack()

entry_username = tk.Entry(root)
entry_username.pack()

label_password = tk.Label(root, text="Пароль (вход):")
label_password.pack()

entry_password = tk.Entry(root, show='*')
entry_password.pack()

# Кнопка входа
button_login = tk.Button(root, text="Войти", command=on_login)
button_login.pack()

# Метка для результата
label_result = tk.Label(root, text="")
label_result.pack()

# Поля для регистрации
label_reg_username = tk.Label(root, text="Имя пользователя (регистрация):")
label_reg_username.pack()

entry_reg_username = tk.Entry(root)
entry_reg_username.pack()

label_reg_password = tk.Label(root, text="Пароль (регистрация):")
label_reg_password.pack()

entry_reg_password = tk.Entry(root, show='*')
entry_reg_password.pack()

# Кнопка регистрации
button_register = tk.Button(root, text="Зарегистрироваться", command=on_register)
button_register.pack()

# Запуск основного цикла Tkinter
root.mainloop()
