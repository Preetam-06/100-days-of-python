# Day 63 - Virtual Bookshelf (Flask + SQLAlchemy)

Simple Flask app to store and list books using SQLite, Flask-WTF and SQLAlchemy 2.0.

## Project Structure

```
day-63/
├── main.py              # Flask app, DB model, routes
├── requirements.txt     # Pinned dependencies (Python 3.14 compatible)
├── instance/
│   └── project.db       # SQLite DB (auto-created)
├── templates/
│   ├── index.html       # List all books
│   └── add.html         # Add book form (CSRF protected)
└── .venv/               # Virtual environment
```

## Stack

- **Python** 3.14.2
- **Flask** 3.0.3, **Flask-SQLAlchemy** 3.1.1, **SQLAlchemy** 2.0.43
- **Flask-WTF** 1.3.0, **WTForms** 3.2.2

## How It Works

### 1. App Setup (`main.py:11-18`)

```python
app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key'  # required for Flask-WTF CSRF
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
db = SQLAlchemy(model_class=Base)
db.init_app(app)
```

Config must be set **before** `db.init_app(app)`.

### 2. Model (`main.py:20-24`)

```python
class Book(db.Model):
    id: Mapped[int] = mapped_column(Integer, primary_key=True)      # autoincrement
    title: Mapped[str] = mapped_column(String(250), nullable=False)
    author: Mapped[str] = mapped_column(String(250), nullable=False)
    ratings: Mapped[float] = mapped_column(Float, nullable=False)
```

`id` is auto-generated - not in the form. `db.create_all()` (`main.py:26-27`) creates `instance/project.db` on first run.

### 3. Form (`main.py:29-33`)

```python
class BookForm(FlaskForm):
    title = StringField('Book Title', validators=[DataRequired()])
    author = StringField('Book Author', validators=[DataRequired()])
    ratings = FloatField('Rating', validators=[DataRequired()])
    submit = SubmitField('Add Book')
```

### 4. Routes

| Route | Methods | File | Description |
|-------|---------|------|-------------|
| `/` | GET | `main.py:35-39` | Queries `Book` via `db.session.execute(db.select(Book))` and renders `index.html` |
| `/add` | GET, POST | `main.py:43-55` | Validates `BookForm` with `form.validate_on_submit()`, creates `Book`, commits, redirects to `/` |
| `/static/<path>` | GET | Flask | For CSS if added |

### 5. Templates

- `templates/index.html:10-18` - Shows `No books` if empty, loops `books`
- `templates/add.html:11-18` - Uses `{{ form.hidden_tag() }}` for CSRF (Django `{% csrf_token %}` does not work in Flask)

## Setup & Run

```powershell
cd day-63
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt

# run (from project root)
python main.py
# or
python -m flask --app main run --debug
```

Open http://127.0.0.1:5000 and http://127.0.0.1:5000/add

## Fix History

- `Python 3.14 + SQLAlchemy 2.0.25` crash (`sqlalchemy/sql/elements.py:810 TypingOnly`) - upgraded to `SQLAlchemy==2.0.43`
- Missing `SECRET_KEY` caused CSRF failure - added `main.py:12`
- `all_books = []` in-memory list ignored DB - replaced with `db.session` query/commit
- `templates/add.html` used `{% csrf_token %}` - fixed to `{{ form.hidden_tag() }}`
- `requirements.txt` now pins compatible versions

## Next Improvements

- [ ] Add `static/css/style.css` and link via `{{ url_for('static', filename='css/style.css') }}` in `templates/*.html:6`
- [ ] Style WTForms fields via `render_kw={"class": "input"}` in `main.py:30-32`
- [ ] Add Edit/Delete: `/edit/<int:id>` and `/delete/<int:id>`
- [ ] Add `Category` model + `ForeignKey` for multi-category support
- [ ] Switch to PostgreSQL + Blueprints for concurrent multi-user API (`/api/v1/books`)
