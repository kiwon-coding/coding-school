# pipenv
* install: `pip install pipenv`
* create or set: `pipenv shell`
* install packages: `pipenv install flask openai`
* create requirement: `pipenv requirements > requirements.txt`
* install packages using requirements.txt : `pipenv install -r requirements.txt`

# Parsing JSON

```
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/data')
def get_data():
    data = {'name': 'John', 'age': 30, 'city': 'New York'}
    return jsonify(data)
```

```
<!DOCTYPE html>
<html>
<head>
    <title>JSON Iteration Example</title>
</head>
<body>
    <ul id="data-list"></ul>

    <script>
        // Fetch the JSON data from the server
        fetch('/data')
            .then(response => response.json())
            .then(data => {
                // Get the <ul> element to append the list items
                const dataList = document.getElementById('data-list');

                // Iterate over the properties of the JSON object
                for (const property in data) {
                    if (data.hasOwnProperty(property)) {
                        // Create a <li> element for each property and add it to the list
                        const listItem = document.createElement('li');
                        listItem.innerHTML = `${property}: ${data[property]}`;
                        dataList.appendChild(listItem);
                    }
                }
            })
            .catch(error => console.error(error));
    </script>
</body>
</html>
```
