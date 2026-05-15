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
In a file called `faces.py`, implement a function called `convert` that accepts a `str` as input and returns that same input with any `:)` converted to ![🙂](https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/72x72/1f642.png) (otherwise known as a [slightly smiling face](https://emojipedia.org/slightly-smiling-face/)) and any `:(` converted to ![🙁](https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/72x72/1f641.png) (otherwise known as a [slightly frowning face](https://emojipedia.org/slightly-frowning-face/)). All other text should be returned unchanged.

Then, in that same file, implement a function called `main` that prompts the user for input, calls `convert` on that input, and prints the result. You’re welcome, but not required, to prompt the user explicitly, as by passing a `str` of your own as an argument to `input`. Be sure to call `main` at the bottom of your file.
