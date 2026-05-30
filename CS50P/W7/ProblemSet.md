# NUMB3RS
```python
import re


def main():
    print(validate(input("IPv4 Address: ")))


def validate(ip):

    pattern = r"^(?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])$"
    return bool(re.fullmatch(pattern, ip))


if __name__ == "__main__":
    main()

```

---
# Watch On YouTube
```python
import re


def main():
    print(parse(input("HTML: ")))


def parse(s):
   
    pattern = r'<iframe[^>]*src="https?://(?:www\.)?youtube\.com/embed/([\w-]+)"'
    if match := re.search(pattern, s, re.IGNORECASE):
        video_id = match.group(1)
        return f"https://youtu.be/{video_id}"
    return None


if __name__ == "__main__":
    main()

```

---
# Working 9 to 5

```python

import re

def main():
    print(convert(input("Hours: ")))

def convert(s):
    # Define the regex pattern for matching 12-hour format times
    pattern = r"^([0-9]{1,2}):?([0-5][0-9]) ([AP]M) to ([0-9]{1,2}):?([0-5][0-9]) ([AP]M)$"
    
    match = re.match(pattern, s)
    
    if not match:
        raise ValueError("Invalid time format")
    
    # Extract the groups from the match
    start_hour, start_minute, start_period, end_hour, end_minute, end_period = match.groups()
    
    # Convert hours to integers
    start_hour = int(start_hour)
    end_hour = int(end_hour)
    
    # Convert minutes to integers
    if start_minute:
        start_minute = int(start_minute)
    else:
        start_minute = 0
    
    if end_minute:
        end_minute = int(end_minute)
    else:
        end_minute = 0
    
    # Handle AM and PM conversion
    if start_period == "PM":
        start_hour += 12
    
    if end_period == "PM" and end_hour != 12:
        end_hour += 12
    
    # Check for invalid times (e.g., 12:60 AM, 13:00 PM)
    if start_hour > 24 or end_hour > 24:
        raise ValueError("Invalid time format")
    
    # Convert to 24-hour format
    start_time = f"{start_hour:02}:{start_minute:02}"
    end_time = f"{end_hour:02}:{end_minute:02}"
    
    return f"{start_time} to {end_time}"

if __name__ == "__main__":
    main()

```

---
#  Regular, um, Expressions

```python
import re
import sys


def main():
    print(count(input("Text: ")))


def count(s):
    ums = re.findall(r"\bum\b", s, re.IGNORECASE)
    return len(ums)


if __name__ == "__main__":
    main()
```

---
# Response Validation

```python
import validators


def main():
    email = input("What's your email? ").strip()

    if validators.email(email):
        print("Valid")
    else:
        print("Invalid")

if __name__ == "__main__":
    main()

```
