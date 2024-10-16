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



from PyQt5 import QtWidgets
from PyQt5.QtWidgets import QDialog
from PyQt5.uic import loadUi
import sqlite3

# Окно приветствия
class WelcomeScreen(QDialog):
    """
    Это класс окна приветствия.
    """
    def __init__(self):
        """
        Это конструктор класса
        """
        super(WelcomeScreen, self).__init__()
        loadUi("welcomescreen.ui", self)  # загружаем интерфейс.
        self.PasswordField.setEchoMode(QtWidgets.QLineEdit.Password)  # скрываем пароль
        self.SignInButton.clicked.connect(self.signupfunction)  # нажати на кнопку и вызов функции

    def signupfunction(self):  # создаем функцию регистрации
        user = self.LoginField.text()  # создаем пользователя и получаем из поля ввода логина введенный текст
        password = self.PasswordField.text()  # создаем пароль и получаем из поля ввода пароля введенный текст
        print(user, password)  # выводит логин и пароль

        if len(user) == 0 or len(password) == 0:  # если пользователь оставил пустые поля
            self.ErrorField.setText("Заполните все поля")  # выводим ошибку в поле
            return

        try:
            conn = sqlite3.connect("uchet.db")  # подключение к базе данных в () изменить на название своей БД
            cur = conn.cursor()  # переменная для запросов

            cur.execute('SELECT typeID FROM users WHERE login=(?) and password=(?)', [user, password])  # получаем тип пользователя, логин и пароль которого был введен
            typeUser = cur.fetchone()  # получает только один тип пользователя

            if typeUser is None:
                self.ErrorField.setText("Неверный логин или пароль")  # выводим ошибку в поле
                return

            print(typeUser[0])  # выводит тип пользователя без скобок
            if typeUser[0] == 4:
                self.stackedWidget.setCurrentWidget(self.Zakazchik)
                self.lybaya = Zakazchik()
            elif typeUser[0] == 2:
                self.stackedWidget.setCurrentWidget(self.Master)
                self.lybaya = Master()
            elif typeUser[0] == 3:
                self.stackedWidget.setCurrentWidget(self.Operator)
                self.lybaya = Operator()
            elif typeUser[0] == 1:
                self.stackedWidget.setCurrentWidget(self.Manager)
                self.lybaya = Manager()

            conn.commit()  # сохраняет в подключении запросы
        except sqlite3.Error as e:
            self.ErrorField.setText(f"Ошибка базы данных: {e}")
        finally:
            conn.close()  # закрываем подключение


class Zakazchik(QDialog):
    def __init__(self):
        super(Zakazchik, self).__init__()
        print("sgaclusa")

class Master(QDialog):
    def __init__(self):
        super(Master, self).__init__()
        print("sgaclusa")

class Operator(QDialog):
    def __init__(self):
        super(Operator, self).__init__()
        print("sgaclusa")

class Manager(QDialog):
    def __init__(self):
        super(Manager, self).__init__()
        print("sgaclusa")

if __name__ == "__main__":
    import sys
    app = QtWidgets.QApplication(sys.argv)
    welcome_screen = WelcomeScreen()
    welcome_screen.show()
    sys.exit(app.exec_())
