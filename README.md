## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Develop a Python application that calculates the volume of a cylinder based on its radius and height. Integrate the Python function with an LLM-based chat completion system using function calling, allowing the LLM to identify the required function, extract the necessary parameters from the user's prompt, invoke the function, and generate a natural-language response containing the calculated result.

### DESIGN STEPS:

#### STEP 1:Define the Mathematical Function
Import the math module.
Define a Python function named calculate_cylinder_volume().
Accept radius and height as input parameters.
Calculate the cylinder's volume using the mathematical formula.
Return the calculated volume.

#### STEP 2:Integrate the Function with the LLM
Configure the OpenAI API key using an environment variable.
Define the function schema with its name, description, parameters, and required arguments.
Send the user's prompt to the LLM through the chat completion API.
Enable automatic function calling using function_call="auto".
Allow the LLM to identify the appropriate function and extract its arguments.


#### STEP 3:Execute the Function and Generate the Response
Retrieve the function name and arguments from the LLM response.
Parse the arguments using the Python json module.
Execute the corresponding Python function with the extracted parameters.
Send the function's result back to the LLM.
Generate the final response in natural language.

### PROGRAM:
```
import os
import json
import math
import openai

openai.api_key = os.environ['OPENAI_API_KEY']


def calculate_cylinder_volume(radius, height):
    volume = math.pi * radius ** 2 * height
    return volume


radius = 5
height = 10

result = calculate_cylinder_volume(radius, height)

print("Volume of cylinder:", result)



functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder using its radius and height.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "Radius of the cylinder"
                },
                "height": {
                    "type": "number",
                    "description": "Height of the cylinder"
                }
            },
            "required": ["radius", "height"]
        }
    }
]


response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": "Calculate the volume of a cylinder with radius 5 and height 10."
        }
    ],
    functions=functions,
    function_call="auto"
)

message = response["choices"][0]["message"]

print(message)


function_call = message["function_call"]

function_name = function_call["name"]

arguments = json.loads(function_call["arguments"])

result = calculate_cylinder_volume(
    arguments["radius"],
    arguments["height"]
)

print("Function result:", result)


messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a cylinder with radius 5 and height 10."
    },
    message,
    {
        "role": "function",
        "name": function_name,
        "content": str(result)
    }
]

final_response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages
)

print(final_response["choices"][0]["message"]["content"])


```

### OUTPUT:
<img width="1087" height="387" alt="ex01" src="https://github.com/user-attachments/assets/7cf108a5-2e18-4343-94ad-fe82cda46a82" />


### RESULT:
Thus, a Python function was successfully designed and implemented to calculate the volume of a cylinder. The function was integrated with an LLM-based chat completion system using the function-calling feature. The LLM successfully identified the appropriate function, extracted the required parameters, invoked the Python function, and generated a natural-language response containing the calculated volume.
