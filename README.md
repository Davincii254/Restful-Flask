### Camping Fun


## Setup

To download the dependencies for the frontend and backend, run:

```console
pipenv install
pipenv shell
npm install --prefix client
```

You can run your Flask API on [`localhost:5555`](http://localhost:5555) by
running:

```console
python server/app.py
```

You can run the React app on [`localhost:4000`](http://localhost:4000) by
running:

```sh
npm start --prefix client
```

---
Use the following commands to create the initial database `app.db`:

```console
cd server
flask db init
flask db migrate -m 'initialize model'
flask db upgrade head
python seed.py
```

### Model Relationships
- A `Camper` has many `Activity`s through `Signup`s
- An `Activity` has many `Camper`s through `Signup`s
- A `Signup` belongs to a `Camper` and belongs to a `Activity`


---

## Validations impimented

`Camper` model:

- Have a `name`
- Have an `age` between 8 and 18

`Signup` model:

- Have a `time` between 0 and 23 (referring to the hour of day for the
  activity)

---

## Routes

### GET /campers

Return JSON data in the format below.

```json
[
  {
    "id": 1,
    "name": "Caitlin",
    "age": 8
  },
  {
    "id": 2,
    "name": "Lizzie",
    "age": 9
  }
]
```

### GET /campers/<int:id>

If the `Camper` exists, return JSON data in the format below. Includes a list of signups for the camper.

```json
{
  "age": 12,
  "id": 1,
  "name": "Nicholas Martinez",
  "signups": [
    {
      "activity": {
        "difficulty": 2,
        "id": 5,
        "name": "Hiking by the stream."
      },
      "activity_id": 5,
      "camper_id": 1,
      "id": 39,
      "time": 8
    },
    {
      "activity": {
        "difficulty": 1,
        "id": 7,
        "name": "Listening to the birds chirp."
      },
      "activity_id": 7,
      "camper_id": 1,
      "id": 42,
      "time": 1
    }
  ]
}
```

If the `Camper` does not exist, returns the following JSON data, along with the
appropriate HTTP status code:

```json
{
  "error": "Camper not found"
}
```

### PATCH /campers/:id

This route should update an existing `Camper`. It accepts an object with
the following properties in the body of the request:

```json
{
  "name": "some name",
  "age": 10
}
```

If the `Camper` exists and is updated successfully (passes validations), updates name and age and returns JSON data in the format below (excluding the signups):

```json
{
  "id": 1,
  "name": "some name",
  "age": 10
}
```

If the `Camper` does not exist, returns the following JSON data, along with the
appropriate HTTP status code:

```json
{
  "error": "Camper not found"
}
```

If the `Camper` is **not** updated successfully (does not pass validations),
return the following JSON data, along with the appropriate HTTP status code:

```json
{
  "errors": ["validation errors"]
}
```

### POST /campers

This route should create a new `Camper`. It  accepts an object with the
following properties in the body of the request:

```json
{
  "name": "Zoe",
  "age": 11
}
```

If the `Camper` is created successfully, sends a response with the new
`Camper`:

```json
{
  "id": 2,
  "name": "Zoe",
  "age": 11
}
```

If the `Camper` is **not** created successfully, returns the following JSON data,along with the appropriate HTTP status code.

```json
{ "errors": ["validation errors"] }
```

### GET /activities

Return JSON data in the format below:

```json
[
  {
    "id": 1,
    "name": "Archery",
    "difficulty": 2
  },
  {
    "id": 2,
    "name": "Swimming",
    "difficulty": 3
  }
]
```

### DELETE /activities/<int:id>

If the `Activity` exists, it should be removed from the database, along with any
`Signup`s that are associated with it (a `Signup` belongs to an `Activity`. If
you did not set up your models to cascade deletes, you need to delete associated
`Signups` before the `Activity` can be deleted.

After deleting the `Activity`, return an _empty_ response body, along with the
appropriate HTTP status code.

If the `Activity` does not exist, returns the following JSON data, along with the appropriate HTTP status code:

```json
{
  "error": "Activity not found"
}
```

### POST /signups

This route should create a new `Signup` that is associated with an existing
`Camper` and `Activity`. It should accept an object with the following
properties in the body of the request:

```json
{
  "camper_id": 1,
  "activity_id": 3,
  "time": 9
}
```

If the `Signup` is created successfully, sends back a response with the data
related to the new `Signup`:

```json
{
  "id": 100,
  "camper_id": 1,
  "activity_id": 3,
  "time": 9,
  "activity": {
    "difficulty": 3,
    "id": 3,
    "name": "Swim in the lake."
  },
  "camper": {
    "age": 11,
    "id": 1,
    "name": "Ashley Delgado"
  }
}
```

If the `Signup` is **not** created successfully, returns the following JSON data, along with the appropriate HTTP status code:

```json
{ "errors": ["validation errors"] }
```
