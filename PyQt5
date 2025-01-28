import sys
import sqlite3
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout, QLabel, QLineEdit, QPushButton, QTableWidget, QTableWidgetItem, QMessageBox

class EmployeeApp(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle('Управление сотрудниками')
        self.setGeometry(100, 100, 600, 400)

        # Поля ввода
        self.name_input = QLineEdit(self)
        self.position_input = QLineEdit(self)
        self.salary_input = QLineEdit(self)

        # Кнопки
        self.add_button = QPushButton('Добавить', self)
        self.update_button = QPushButton('Обновить', self)
        self.delete_button = QPushButton('Удалить', self)

        # Таблица для отображения данных
        self.table = QTableWidget(self)
        self.table.setColumnCount(4)
        self.table.setHorizontalHeaderLabels(['ID', 'Имя', 'Должность', 'Зарплата'])

        # Разметка
        layout = QVBoxLayout()

        form_layout = QHBoxLayout()
        form_layout.addWidget(QLabel('Имя:'))
        form_layout.addWidget(self.name_input)
        form_layout.addWidget(QLabel('Должность:'))
        form_layout.addWidget(self.position_input)
        form_layout.addWidget(QLabel('Зарплата:'))
        form_layout.addWidget(self.salary_input)

        button_layout = QHBoxLayout()
        button_layout.addWidget(self.add_button)
        button_layout.addWidget(self.update_button)
        button_layout.addWidget(self.delete_button)

        layout.addLayout(form_layout)
        layout.addLayout(button_layout)
        layout.addWidget(self.table)

        self.setLayout(layout)

        # Подключение событий
        self.add_button.clicked.connect(self.add_employee)
        self.update_button.clicked.connect(self.update_employee)
        self.delete_button.clicked.connect(self.delete_employee)
        self.table.itemSelectionChanged.connect(self.load_employee_data)

        # Загрузка данных в таблицу
        self.load_data()

    def load_data(self):
        conn = sqlite3.connect('employees.db')
        c = conn.cursor()
        c.execute('SELECT * FROM employees')
        rows = c.fetchall()
        self.table.setRowCount(len(rows))

        for i, row in enumerate(rows):
            for j, value in enumerate(row):
                self.table.setItem(i, j, QTableWidgetItem(str(value)))

        conn.close()

    def add_employee(self):
        name = self.name_input.text()
        position = self.position_input.text()
        salary = self.salary_input.text()

        if name and position and salary:
            conn = sqlite3.connect('employees.db')
            c = conn.cursor()
            c.execute('INSERT INTO employees (name, position, salary) VALUES (?, ?, ?)', (name, position, salary))
            conn.commit()
            conn.close()
            self.load_data()
            self.clear_inputs()
        else:
            QMessageBox.warning(self, 'Ошибка', 'Все поля должны быть заполнены')

    def update_employee(self):
        selected_row = self.table.currentRow()
        if selected_row >= 0:
            id = self.table.item(selected_row, 0).text()
            name = self.name_input.text()
            position = self.position_input.text()
            salary = self.salary_input.text()

            if name and position and salary:
                conn = sqlite3.connect('employees.db')
                c = conn.cursor()
                c.execute('UPDATE employees SET name=?, position=?, salary=? WHERE id=?', (name, position, salary, id))
                conn.commit()
                conn.close()
                self.load_data()
                self.clear_inputs()
            else:
                QMessageBox.warning(self, 'Ошибка', 'Все поля должны быть заполнены')
        else:
            QMessageBox.warning(self, 'Ошибка', 'Выберите сотрудника для обновления')

    def delete_employee(self):
        selected_row = self.table.currentRow()
        if selected_row >= 0:
            id = self.table.item(selected_row, 0).text()

            conn = sqlite3.connect('employees.db')
            c = conn.cursor()
            c.execute('DELETE FROM employees WHERE id=?', (id,))
            conn.commit()
            conn.close()
            self.load_data()
            self.clear_inputs()
        else:
            QMessageBox.warning(self, 'Ошибка', 'Выберите сотрудника для удаления')

    def load_employee_data(self):
        selected_row = self.table.currentRow()
        if selected_row >= 0:
            name = self.table.item(selected_row, 1).text()
            position = self.table.item(selected_row, 2).text()
            salary = self.table.item(selected_row, 3).text()

            self.name_input.setText(name)
            self.position_input.setText(position)
            self.salary_input.setText(salary)

    def clear_inputs(self):
        self.name_input.clear()
        self.position_input.clear()
        self.salary_input.clear()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = EmployeeApp()
    ex.show()
    sys.exit(app.exec_())
