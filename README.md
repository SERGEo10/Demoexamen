# Demoexamen

[https://ru.yougile.com/board/50w5pze86m21](https://ru.yougile.com/board/3taxw03x0hjp) - YouGile доска под демоэкзамен

https://bom.firpo.ru/Public/86

![image](https://github.com/user-attachments/assets/55c003bb-a684-4dce-8bf9-da0e11a69330)

![image](https://github.com/user-attachments/assets/dce48e67-2abf-4caa-9017-68d606c280e9)

Страница 23-25 пример задания по демоэкзамену. 

Доска: https://ru.yougile.com/board/3taxw03x0hjp

Видео по дистанту: https://drive.google.com/file/d/1OMzGNYCr31Zi6EgKqHhheNGERmNOhvxs/view?usp=sharing

Зеркало на папку с видео: https://drive.google.com/drive/folders/19MY8OBAlu8oXtgOV6MSWU2U9FvWBRESr?usp=drive_link

# Скрины установленного ПО:

**QTCreator**
![image](https://github.com/user-attachments/assets/436b2ca4-2ca5-4865-ae85-01f04041ba9d)

**VS Code**
![image](https://github.com/user-attachments/assets/d6df9a55-f020-4254-a943-b904f254f165)

**Drawio**
![image](https://github.com/user-attachments/assets/c50e9ec6-d01d-4668-8a8d-75757157b131)

**DB Browser for SQLite**
![image](https://github.com/user-attachments/assets/f4818c61-9e58-4258-bf4b-599ddbb074df)

**DBeaver**
![image](https://github.com/user-attachments/assets/2042b13a-e9cf-4f85-a6ed-7102de6e3323)

# Полюс заявки задание 2024
![image](https://github.com/user-attachments/assets/c521e407-ee3a-4202-a6a3-6c04063a7d5d)



import sqlite3
from PyQt5 import QtWidgets
from PyQt5.QtWidgets import QDialog
from PyQt5.uic import loadUi

class WelcomeScreen(QDialog):
    def __init__(self):
        super(WelcomeScreen, self).__init__()
        loadUi("welcomescreen.ui", self)  # загружаем интерфейс
        self.PasswordField.setEchoMode(QtWidgets.QLineEdit.Password)
        self.SignInButton.clicked.connect(self.signupfunction)
    
    def signupfunction(self):
        user = self.LoginField.text()
        password = self.PasswordField.text()
        print(user, password)

        if len(user) == 0 or len(password) == 0:
            self.ErrorField.setText("Заполните все поля")
        else:
            conn = sqlite3.connect("uchet.db")
            cur = conn.cursor()

            user_info = [user, password]
            cur.execute('SELECT typeID FROM users WHERE login=(?) and password=(?)', user_info) 
            typeUser = cur.fetchone()

            if typeUser is not None:
                # Если авторизация успешна, переключаемся на другую страницу
                self.ErrorField.setText("Авторизация успешна!")
                self.parentWidget().setCurrentIndex(1)  # Переключаемся на страницу с индексом 1
            else:
                self.ErrorField.setText("Неверный логин или пароль")

