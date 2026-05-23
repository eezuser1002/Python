# Lines
```python
import sys


def counter(filename):
    if not filename.endswith(".py"):
        sys.exit("Not a Python file")
    
    lines = 0
    with open(filename, "r") as file:
        for line in file:
            line = line.strip()
            if line and not line.startswith("#"):
                lines += 1
    
    print(lines)

def main():
    if len(sys.argv) != 2:
        sys.exit("Usage: python lines.py <file.py>")
    counter(sys.argv[1])

if __name__ == "__main__":
    main()
```

---
#  Pizza
```python
import sys
import csv
from tabulate import tabulate


def main():
    # Check for exactly one command-line argument
    if len(sys.argv) < 2:
        sys.exit("Too few command-line arguments")
    if len(sys.argv) > 2:
        sys.exit("Too many command-line arguments")

    filename = sys.argv[1]

    # Check if file ends with .csv
    if not filename.endswith(".csv"):
        sys.exit("Not a CSV file")

    # Check if file exists
    try:
        with open(filename, "r") as file:
            reader = csv.reader(file)
            rows = list(reader)
    except FileNotFoundError:
        sys.exit("File does not exist")

    # Display the table using tabulate with grid format
    print(tabulate(rows[1:], headers=rows[0], tablefmt="grid"))


if __name__ == "__main__":
    main()

```


---
# Scourgify
```python
import sys
import csv


def main():
    # CLI argument validation
    if len(sys.argv) != 3:
        sys.exit("Too few command-line arguments")
    if not sys.argv[1].endswith(".csv") or not sys.argv[2].endswith(".csv"):
        sys.exit("Not a CSV file")



    # Format before.csv to after.csv
    with open(sys.argv[1]) as file:
        reader = csv.DictReader(file)
        students = []
        for row in reader:
            name = row["name"].split(", ")
            students.append({"first": name[1], "last": name[0], "house": row["house"]})

    with open(sys.argv[2], "w") as file:
        writer = csv.DictWriter(file, fieldnames=["first", "last", "house"])
        writer.writeheader()
        for student in students:
            writer.writerow(student)


if __name__ == "__main__":
    main()

```