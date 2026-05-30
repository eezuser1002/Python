# Seasons of Love

```python
from datetime import date
import inflect
import sys

p = inflect.engine()

def convert(dob):
    dob = date.fromisoformat(dob)

    today = date.today()
    days_difference = (today - dob).days
    transformed_dob = days_difference * 24 * 60

    words = p.number_to_words(transformed_dob, andword="")
    return words.capitalize() + " minutes"

def main():
    try:
        user_input = input("Date of Birth: ")
        print(convert(user_input))
    except ValueError:
        sys.exit("Invalid date")

if __name__ == "__main__":
    main()

```
---
# Cookie Jar

```python
class Jar:
    def __init__(self, capacity=12):
        if not isinstance(capacity, int) or capacity < 0:
            raise ValueError("Capacity must be a non-negative integer")

        self._capacity = capacity
        self._size = 0

    def __str__(self):
        return "🍪" * self._size

    def deposit(self, n):
        if self._size + n > self._capacity:
            raise ValueError("Jar capacity exceeded")

        self._size += n

    def withdraw(self, n):
        if n > self._size:
            raise ValueError("Not enough cookies")

        self._size -= n

    @property
    def capacity(self):
        return self._capacity

    @property
    def size(self):
        return self._size

```

---
# CS50 Shirtificate
```python
from fpdf import FPDF


def main():
    name = input("Name: ")

    pdf = FPDF(orientation="P", format="A4")
    pdf.add_page()


    pdf.set_font("Helvetica", "B", 24)
    pdf.cell(0, 20, "CS50 Shirtificate", align="C", new_x="LMARGIN", new_y="NEXT")


    shirt_width = 180
    x = (210 - shirt_width) / 2
    pdf.image("shirtificate.png", x=x, y=60, w=shirt_width)

    
    pdf.set_font("Helvetica", "B", 24)
    pdf.set_text_color(255, 255, 255)

    pdf.set_xy(0, 140)
    pdf.cell(210, 10, f"{name} took CS50", align="C")

    pdf.output("shirtificate.pdf")


if __name__ == "__main__":
    main()

```