# Cafe API - Day 66

A Flask REST API for managing cafe records with SQLite and Flask-SQLAlchemy.

## API Documentation

View the complete Postman documentation here:

[Cafe API Postman Documentation](https://documenter.getpostman.com/view/58179851/2sBYAyt9Km)

## Run the application

From the `day-66` directory, activate the virtual environment and install the dependencies:

```powershell
python -m pip install -r requirements.txt
flask --app main.py run --debug
```

Base URL:

```text
http://127.0.0.1:5000
```

## Endpoints

| Request name | Method | Endpoint | Description |
|---|---|---|---|
| Get Random Cafe | GET | `/random` | Retrieves one random cafe. |
| Get All Cafes | GET | `/all` | Retrieves all cafes. |
| Search Cafe by Location | GET | `/search?location=London` | Finds cafes by location. |
| Add New Cafe | POST | `/add` | Adds a new cafe to the database. |
| Update Cafe Price | PATCH | `/update-price/<cafe_id>?new_price=3.00` | Updates a cafe's coffee price. |
| Delete Cafe | DELETE | `/report-closed/<cafe_id>?api_key=TopSecretAPIKey` | Deletes a cafe after API-key validation. |

## Add New Cafe

Send the cafe details as form data or URL-encoded form data:

```text
name, map_url, img_url, location, seats,
has_toilet, has_wifi, has_sockets, can_take_calls, coffee_price
```

## Authentication

The delete endpoint requires the query parameter:

```text
api_key=TopSecretAPIKey
```

An invalid API key returns `403 Forbidden`. A cafe ID that does not exist returns `404 Not Found`.
