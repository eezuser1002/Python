## indoor.py

```python
user_input = str(input("Type something: ").strip().lower())
print(f"Here is the lowercase version: {user_input}")
```

---
## playback.py

```python
userInput = str(input("Type a sentence: "))

addSpace = userInput.replace(" ","...")

print(addSpace)
```

---
## faces.py
```python
#!/usr/bin/env python

import emoji

# Convert function
def convert(arg):
    if arg == "Hello :)":
        return (emoji.emojize("Hello" +  " " + ":slightly_smiling_face:"))
    elif arg == "Goodbye :(":
        return (emoji.emojize("Goodbye" + " " + ":slightly_frowning_face:"))
    elif arg == "Hello :) Goodbye :(":
        return emoji.emojize("Hello :slightly_smiling_face: Goodbye :slightly_frowning_face:")
    else:
        return ("Invalid input...how do you really feel?")


# User prompt
def main():
    response = convert(str(input("How do you feel? ")))
    print(response)



if __name__ == "__main__":
    main()

```