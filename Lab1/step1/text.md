# Step 1: Create a Flask application

First, create a virtual environment:

`python3 -m venv venv`{{exec}}

Activate it:

`source venv/bin/activate`{{exec}}

Install Flask:

`pip install flask`{{exec}}

Create `app.py` with the following content:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Flask!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)