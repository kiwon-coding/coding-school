# Prompt Engineering
* system
```
The assistant's job is to recommend five color codes that match what user's describing.

Response JSON array like [#000000, #000000, ...]. Return only JSON Array. Remove pre-text and post-text.
```
* sample user input
```
description::pine trees in forest
```
* sample output
```
["#1B4D3E", "#2C5F2D", "#355E3B", "#1F3D2C", "#2E4F2E"]
```
* temperature = 0.8
* max_token = 50
* model = gpt-turbo-3.5

# Start point
```
<!DOCTYPE html>
<html>
<head>
    <title>Color Boxes</title>
    <style>
        .color-box {
            width: 50px;
            height: 50px;
            display: inline-block;
            margin: 5px;
        }
    </style>
    <script>
        function generateRandomColor() {
            var letters = '0123456789ABCDEF';
            var color = '#';
            for (var i = 0; i < 6; i++) {
                color += letters[Math.floor(Math.random() * 16)];
            }
            return color;
        }
        
        function createColorBoxes() {
            var colorBoxContainer = document.getElementById('color-box-container');
            colorBoxContainer.innerHTML = ''; // Clear any existing color boxes
            for (var i = 0; i < 5; i++) {
                var colorBox = document.createElement('div');
                colorBox.className = 'color-box';
                colorBox.style.backgroundColor = generateRandomColor();
                colorBoxContainer.appendChild(colorBox);
            }
        }
    </script>
</head>
<body>
    <textarea rows="5" cols="30"></textarea>
    <br>
    <button onclick="createColorBoxes()">Submit</button>
    <br>
    <div id="color-box-container"></div>
</body>
</html>
```
