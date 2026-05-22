# Fuel Gauge
```python
def fuel(X, Y):
    try:
        if X < 0 or Y < 0:
            raise ValueError("Number must be greater than 0.")
        if Y == 0:
            raise ZeroDivisionError("Cannot divide by zero.")
        return X / Y
    except ZeroDivisionError as e:
        print(e)
        return None
    except ValueError as e:
        print(e)
        return None

def calculate_fuel_level(fraction):
    try:
        numerator, denominator = map(int, fraction.split('/'))
        return fuel(numerator, denominator) * 100
    except ValueError as e:
        print("Invalid input:", e)
        return None

def format_fuel_level(fuel_level):
    if fuel_level is not None:
        rounded_fuel_level = round(fuel_level)
        if rounded_fuel_level <= 1:
            return "E"
        elif rounded_fuel_level >= 99:
            return "F"
        else:
            return f"{rounded_fuel_level}%"
    else:
        return "Fuel level undetermined"

def main():
    while True:
        fraction = input("Fraction: ").strip()
        fuel_level = calculate_fuel_level(fraction)
        formatted_level = format_fuel_level(fuel_level)

        if formatted_level != "Fuel level undetermined":
            print(formatted_level)
            break

if __name__ == "__main__":
    main()

```
---
# Felipe's Taqueira
```python
menu = {
    "Baja Taco": 4.25,
    "Burrito": 7.50,
    "Bowl": 8.50,
    "Nachos": 11.00,
    "Quesadilla": 8.50,
    "Super Burrito": 8.50,
    "Super Quesadilla": 9.50,
    "Taco": 3.00,
    "Tortilla Salad": 8.00
}

def main():
    total = 0.0
    while True:
        try:
            item = input("Item: ").lower()
            found_item = False
            for key in menu.keys():
                if key.lower() == item:
                    total += menu[key]
                    print(f"Total: ${total:.2f}")
                    found_item = True
                    break
                if not found_item:
                    pass
        except EOFError:
            print("\n")
            break

if __name__ == "__main__":
    main()

```

---
# Grocery List
```python
#1. prompt the user for grocery list
#2. increment grocery list
#3. store the quantities of each item
#4. capitalize input



def generate_list():
    groceries = {}
    try:
        while True:
            item = input("Add item to list (or type 'done' to finish): ").strip()
            if item.lower() == 'done':
                break
            if item not in groceries:
                groceries[item] = 1
            else:
                groceries[item] += 1
    except EOFError:
        print("\nGrocery list done.")

    # Output the grocery list with counts and capitalized items
    for item, count in groceries.items():
        print(f"{item.capitalize()}: {count}")

def main():
    generate_list()

if __name__ == "__main__":
    main()

```

---
# Outdated