# LangChain: Models, Prompts, and Output Parsers

## Outline

1. Direct API Calls to OpenAI
2. API Calls through LangChain:
    - Prompts
    - Models
    - Output Parsers
3. Get Your OpenAI API Key

## Direct API Calls to OpenAI

```python
import openai

# Set your OpenAI API key
openai.api_key = "YOUR_API_KEY"

# Define a function for making completions
def get_completion(prompt):
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=50
    )
    return response.choices[0].text.strip()

# Example usage:
result = get_completion("What is 2 + 2?")
print(result)  # Output: "2 + 2 equals 4."
```
