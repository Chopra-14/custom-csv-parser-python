# Custom CSV Parser in Python

## 1. Overview

This project is about building a CSV parser completely from scratch in Python.  
I implemented two classes:

- **CustomCsvReader** → reads CSV files row by row  
- **CustomCsvWriter** → writes CSV files with proper quoting  

The goal was to handle common CSV features such as:

- commas inside fields  
- fields wrapped in double quotes  
- escaped quotes (`""`)  
- newline characters inside quoted fields  
- streaming read (not loading the entire file into memory)

I also added a benchmark script to compare my code with Python’s built-in `csv` module.

---

## 2. Setup Instructions

### Clone the repository

```bash
git clone https://github.com/Chopra-14/custom-csv-parser-python.git
cd custom-csv-parser-python
```


```bash
python -m venv venv
source venv/Scripts/activate   
```

### Install dependencies

```bash
pip install -r requirements.txt
```

There are no external libraries required for this project.

---

## 3. Usage Examples

### Writing CSV data

```python
from custom_csv import CustomCsvWriter

data = [
    ["id", "name", "note"],
    ["1", 'Alice "A"', "hello,world"],
    ["2", "Bob", "multi\nline text"],
]

with open("demo.csv", "w", encoding="utf-8", newline="") as f:
    writer = CustomCsvWriter(f)
    writer.writerows(data)
```

### Reading CSV data

```python
from custom_csv import CustomCsvReader

with open("demo.csv", "r", encoding="utf-8", newline="") as f:
    reader = CustomCsvReader(f)
    for row in reader:
        print(row)
```

---

## 4. Benchmark

Run the benchmark:

```bash
python benchmark.py
```

This script creates test data (10,000 rows × 5 columns) and measures the average read/write time for both my custom parser and Python’s `csv` module.

---

## 5. Benchmark Results

| Operation       | Avg Time (s) |
| --------------- | ------------ |
| csv.writer      | 0.025959     |
| CustomCsvWriter | 0.026989     |
| csv.reader      | 0.029148     |
| CustomCsvReader | 0.219198     |

---

![Benchmark Results](images/benchmark.png)


## 6. Benchmark Analysis

The built-in `csv` module is faster, which is expected because it is implemented in C.
My custom writer is almost as fast as the standard writer, which means the quoting and escaping logic is efficient.

The custom reader is slower because it processes the file character by character and keeps track of parsing state manually.

Even though it is slower, it correctly handles:

* quoted fields
* escaped quotes
* commas inside quotes
* newline characters inside fields

The output matches what Python’s `csv.reader` produces.

---

## 7. Implementation Details

### CustomCsvReader

* Works as an iterator (`__iter__`, `__next__`)
* Uses a simple state machine using a boolean flag (`in_quotes`)
* Supports escaped double quotes (`""`)
* Reads one character at a time
* Returns a list of strings for each row

### CustomCsvWriter

* Checks each field to see if it needs quotes
* Escapes existing double quotes
* Uses comma as delimiter
* Writes lines using `\r\n` as per CSV standards

---

## 8. References

These resources helped me understand how CSV rules work:

* Python `csv` module documentation
* RFC 4180 (CSV format rules)
* Python `timeit` documentation

---

## 9. Conclusion

This project helped me understand how CSV parsing works internally.
Writing my own reader and writer showed me how much logic is involved in handling edge cases like quotes and newlines.

The parser works correctly, produces valid CSV output, and the benchmark shows how it performs compared to Python’s standard module.

