# camelCase
```python
def main():
    camelCase = input("camelCase: ")
    snake_case = " "

    for letter in camelCase:
        if letter.isupper():
            snake_case += "_" + letter.lower()
        else:
            snake_case += letter
    print(snake_case)


if __name__ == "__main__":
    main()
```

---
# Coke Machine
```python
def main():
    amount_due = 50

    while amount_due > 0:
        print(f"Amount Due: {amount_due}")
        coin = int(input("Insert Coin: "))

        if coin in [25, 10, 5]:
            amount_due -= coin

    print(f"Change Owed: {abs(amount_due)}")

main()

```
---
# twttr
```python
def remover(text):
    return text.translate(str.maketrans("", "", "aeiouAEIOU"))

def main():
    text = input("Insert text: ")
    print(remover(text))

if __name__ == "__main__"
	main()

```

---
# Vanity Plates
```python
def main():
    plate = input("Plate: ")
    if is_valid(plate):
        print("Valid")
    else:
        print("Invalid")

def is_valid(s):
    # Rule 1: Length 2-6
    if len(s) < 2 or len(s) > 6:
        return False

    # Rule 2: Only alphanum (letters + digits)
    if not s.isalnum():
        return False

    # Rule 3: First two letters
    if not s[:2].isalpha():
        return False

    # Rule 4: Digits at end only (no letter after any digit)
    for i in range(len(s)):
        if s[i].isdigit():
            # From first digit to end, all must be digits
            if not s[i:].isdigit():
                return False
            break  # No need to check further

    # Rule 5: First digit != '0'
    for char in s:
        if char.isdigit():
            if char == '0':
                return False
            break  # Only check the first digit

    return True


if __name__ == "__main__":
	main()

```
---
# Nutrition
```python
#!/usr/bin/env python

def main():
    # FDA fruit data - exact names/calories from poster
    # Used AI to generate the dictionary
    fruits = {
        "apple": 130,
        "avocado": 50,
        "banana": 110,
        "cantaloupe": 50,
        "grapefruit": 60,
        "grapes": 90,
        "honeydew": 50,
        "kiwifruit": 90,
        "lemon": 15,
        "lime": 20,
        "nectarine": 60,
        "orange": 80,
        "peach": 60,
        "pear": 100,
        "pineapple": 50,
        "plums": 70,
        "strawberries": 50,
        "sweet cherries": 100,
        "tangerine": 50,
        "watermelon": 80
    }

    fruit = input("Fruit: ").lower().strip()
# Stores the value of the fruit key
    calories = fruits.get(fruit)
# Error handling for unrecognized input from user
    if calories == None:
        print("Unknown fruit")
    else:
        print(f"Calories: {calories}")

if __name__ == "__main__":
	main()

```