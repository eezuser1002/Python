# Deep Thought

```python
#!/usr/bin/env python

def main():
    answer = input("What is the answer to the Great Question of Life, the Universe, and Everything? ").strip().casefold()

    if answer in ("42", "forty-two", "forty two"):
        print("Yes")
    else:
        print("No")
main()
```

---
# Home Federal Savings Bank
```python
greeting = input("Give me a greeting ").strip().casefold()

if greeting.startswith("hello"):
    print("$0")
elif greeting.startswith("h"):
    print("$20")
else:
    print("$100")
```

---
# File Extensions
```python
#!/usr/bin/env python

def main():
    file = input("Give me the name of a file: ")

    if file.endswith(".gif"):
        print("image/gif")
    elif file.endswith(".jpg"):
        print("image/jpg")
    elif file.endswith(".jpeg"):
        print("image/jpeg")
    elif file.endswith(".png"):
        print("image/png")
    elif file.endswith(".pdf"):
        print("application/pdf")
    elif file.endswith(".txt"):
        print("text/plain")
    elif file.endswith(".zip"):
        print("application/zip")
    else:
        print("application/octet-stream")
main()
```

---
# Math Interpreter
```python
def main():
    expression = input("Expression: ")

    x, y, z = expression.split()

    x = float(x)
    z = float(z)

    if y == "+":
        print(x + z)

    elif y == "-":
        print(x - z)

    elif y == "*":
        print(x * z)

    elif y == "/":
        print(x / z)

if __name__ == "__main__":
    main
```
---
# Meal Time