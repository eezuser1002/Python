# Test twttr

```python
from twttr import shorten

def test_default():
    assert shorten("applesauce") == "pplsc"

def test_vowels_lowercase():
    assert shorten("twitter") == "twttr"

def test_vowels_uppercase():
    assert shorten("TWITTER") == "TWTTR"

def test_numbers():
    assert shorten("CS50") == "CS50"

def test_punctuation():
    assert shorten("Hello, world!") == "Hll, wrld!"

def test_mixed_case():
    assert shorten("PyThOn") == "PyThn"

```
---

# Test Fuel

```python
import pytest
from fuel import convert, gauge


def test_convert():
    assert convert("1/2") == 50
    assert convert("1/4") == 25
    assert convert("3/4") == 75
    assert convert("99/100") == 99


def test_convert_empty():
    assert convert("0/100") == 0
    assert convert("1/100") == 1


def test_convert_full():
    assert convert("100/100") == 100
    assert convert("99/100") == 99


def test_convert_value_error():
    with pytest.raises(ValueError):
        convert("3/2")

    with pytest.raises(ValueError):
        convert("cat/dog")


def test_convert_zero_division():
    with pytest.raises(ZeroDivisionError):
        convert("1/0")
        
def test_convert_negative():
    with pytest.raises(ValueError):
        convert("-1/2")

    with pytest.raises(ValueError):
        convert("1/-2")

def test_gauge():
    assert gauge(0) == "E"
    assert gauge(1) == "E"
    assert gauge(50) == "50%"
    assert gauge(99) == "F"
    assert gauge(100) == "F"

```
---
# Test Bank
```python
from bank import value


def test_hello():
    assert value("hello") == 0


def test_hello_uppercase():
    assert value("HELLO") == 0


def test_hello_mixed_case():
    assert value("HeLLo") == 0


def test_h():
    assert value("hi") == 20


def test_h_uppercase():
    assert value("HOW are you") == 20


def test_else():
    assert value("welp") == 100
```
---
# Test Plates
```python
from plates import is_valid

# used ai help to come up with some tests
def test_valid_plates():
    assert is_valid("CS50") is True
    assert is_valid("AAA222") is True
    assert is_valid("HELLO") is True
    assert is_valid("AB123") is True


def test_length_rules():
    assert is_valid("A") is False
    assert is_valid("ABCDEFG") is False


def test_first_two_letters():
    assert is_valid("1ABC") is False
    assert is_valid("A1BC") is False
    assert is_valid("12") is False


def test_alphanumeric_only():
    assert is_valid("PI3.14") is False
    assert is_valid("HELLO!") is False
    assert is_valid("AA 22") is False


def test_numbers_at_end_only():
    assert is_valid("CS50P") is False
    assert is_valid("AB12CD") is False


def test_first_number_not_zero():
    assert is_valid("CS05") is False
    assert is_valid("AB012") is False

```