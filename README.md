import sys
from random import randint
from PyQt6.QtWidgets import QApplication, QWidget, QLabel, QVBoxLayout, QLineEdit, QPushButton, QMessageBox


class GuessNumber(QWidget):
    def __init__(self):
        super().__init__()

        self.number_to_guess = randint(1, 100)
        self.attempts_left = 5

        self.setWindowTitle("Угадай число")
        self.layout = QVBoxLayout()

        self.info_label = QLabel(self)
        self.layout.addWidget(self.info_label)

        self.line_edit = QLineEdit(self)
        self.layout.addWidget(self.line_edit)

        self.submit_button = QPushButton("Отправить", self)
        self.submit_button.clicked.connect(self.check_guess)
        self.layout.addWidget(self.submit_button)

        self.setLayout(self.layout)
        self.info_label.setText("Угадай число")

    def check_guess(self):
        guess = int(self.line_edit.text())
        self.line_edit.clear()

        if guess == self.number_to_guess:
            QMessageBox.information(self, "Ура!", "Вы угадали число!")
            self.close()
        else:
            self.attempts_left -= 1

            if self.attempts_left == 0:
                QMessageBox.information(self, "Поражение", f"Вы проиграли! Загаданное число: {self.number_to_guess}")
                self.close()
            else:
                advice = "меньше" if guess > self.number_to_guess else "больше"
                self.info_label.setText(f"Неверно. У вас осталось {self.attempts_left} попыток. Загаданное число {advice}.")


if __name__ == '__main__':
    app = QApplication(sys.argv)
    game = GuessNumber()
    game.show()
    sys.exit(app.exec())
