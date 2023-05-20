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
