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

### API Calls through LangChain

```python

from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate

# Initialize the ChatOpenAI instance
chat = ChatOpenAI(temperature=0.0, model="gpt-3.5-turbo")

# Define a prompt template
template_string = """
Translate the text that is delimited by triple backticks 
into a style that is {style}.
text: ```{text}```
"""

prompt_template = ChatPromptTemplate.from_template(template_string)

# Create a customer message
customer_style = "American English in a respectful and polite tone"
customer_email = """
[Insert your customer email here]
"""

customer_messages = prompt_template.format_messages(
    style=customer_style,
    text=customer_email
)

# Make a request to the LangChain chat model
customer_response = chat(customer_messages)

print(customer_response.content)
```

### Output Parsers

```python

from langchain.output_parsers import ResponseSchema, StructuredOutputParser

# Define response schemas
gift_schema = ResponseSchema(name="gift", description="Was the item purchased as a gift?")
delivery_days_schema = ResponseSchema(name="delivery_days", description="How many days did it take for the product to arrive?")
price_value_schema = ResponseSchema(name="price_value", description="Extracted sentences about the value or price")

response_schemas = [gift_schema, delivery_days_schema, price_value_schema]

# Initialize the structured output parser
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

# Define a review template
review_template = """
For the following text, extract the following information:

gift: Was the item purchased as a gift? Answer True if yes, False if not or unknown.
delivery_days: How many days did it take for the product to arrive? If this information is not found, output -1.
price_value: Extract any sentences about the value or price and output them as a comma-separated list.

text: {text}
"""

# Create a prompt template
prompt_template = ChatPromptTemplate.from_template(review_template)

# Format the prompt and pass in variables
messages = prompt_template.format_messages(
    text=customer_email,
    format_instructions=output_parser.get_format_instructions()
)

# Make a request to LangChain with the prompt
response = chat(messages)

# Parse the response content using the output parser
response_dict = output_parser.parse(response.content)

print(response_dict)
```

These are the lecture notes covering the LangChain API, API calls to OpenAI, and output parsing.

Replace `[Insert your customer email here]` in the lecture notes with your actual customer email content.

Remember to replace `"YOUR_API_KEY"` with your actual OpenAI API key.
