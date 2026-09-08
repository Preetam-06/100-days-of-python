# Cafe and Wifi

A small Flask app for viewing and adding cafes with coffee, WiFi, and power ratings.

## Project Files

```text
main.py
cafe-data.csv
requirements.txt
static/
  css/
    styles.css
templates/
  base.html
  index.html
  cafes.html
  add.html
```

## Setup

Install the required packages:

```bash
python -m pip install -r requirements.txt
```

## Run

Start the Flask app:

```bash
flask --app main.py run --port 5002
```

Open this address in your browser:

```text
http://127.0.0.1:5002
```

## Pages

- `/` - Home page
- `/cafes` - View all cafes
- `/add` - Add a new cafe

Cafe information is stored in `cafe-data.csv`.
