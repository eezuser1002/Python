# Emojize
```python
import emoji

#1. take input from user
#2. convert input into emoji

def convert(emote):
    return (emoji.emojize(emote, language="alias"))

def main():
    user_input = convert(input("Input: "))
    print(f"Output: {user_input}")

if __name__ == "__main__":
    main()
```

---
