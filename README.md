# LangChain Structured Output Examples

This repository demonstrates various approaches to implementing structured output with LangChain, showcasing different schema definition methods and their practical applications.

## 📋 Overview

The project contains examples of how to extract structured data from unstructured text using LangChain's `with_structured_output()` method. All examples use a product review analysis use case to demonstrate consistent functionality across different schema approaches.

## 🚀 Features

- **Multiple Schema Approaches**: Pydantic, TypedDict, and JSON Schema implementations
- **Real-world Example**: Product review sentiment analysis and information extraction
- **Multiple LLM Support**: Examples with OpenAI and Hugging Face models
- **Type Safety**: Demonstrates proper type annotations and validation

## 📁 Project Structure

```
├── README.md
├── json_schema.json                     # Sample JSON schema
├── students_dataset.csv                 # Sample dataset (placeholder)
├── pydantic_demo.py                     # Basic Pydantic usage demonstration
├── typeddict_demo.py                    # Basic TypedDict usage demonstration
├── with_structured_output_pydantic.py   # Structured output using Pydantic models
├── with_structured_output_typeddict.py  # Structured output using TypedDict
├── with_structured_output_json.py       # Structured output using JSON Schema
└── with_structured_output_llama.py      # Structured output with Hugging Face models
```

## 🛠️ Installation

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd langchain-structured-output
   ```

2. Install dependencies:
   ```bash
   pip install langchain-openai langchain-huggingface python-dotenv pydantic
   ```

3. Create a `.env` file with your API keys:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   HUGGINGFACE_API_TOKEN=your_hf_token_here  # Optional, for Hugging Face examples
   ```

## 📚 Examples

### 1. Pydantic Schema (`with_structured_output_pydantic.py`)

Uses Pydantic BaseModel for schema definition with built-in validation:

```python
from pydantic import BaseModel, Field
from typing import Optional, Literal

class Review(BaseModel):
    key_themes: list[str] = Field(description="Key themes in the review")
    summary: str = Field(description="Brief summary")
    sentiment: Literal["pos", "neg"] = Field(description="Sentiment analysis")
    pros: Optional[list[str]] = Field(default=None, description="Positive aspects")
    cons: Optional[list[str]] = Field(default=None, description="Negative aspects")
    name: Optional[str] = Field(default=None, description="Reviewer name")
```

**Advantages:**
- Strong type validation
- Rich field descriptions
- Default value support
- Excellent IDE support

### 2. TypedDict Schema (`with_structured_output_typeddict.py`)

Uses Python's TypedDict for lightweight type hints:

```python
from typing import TypedDict, Annotated, Optional, Literal

class Review(TypedDict):
    key_themes: Annotated[list[str], "Key themes in the review"]
    summary: Annotated[str, "Brief summary"]
    sentiment: Annotated[Literal["pos", "neg"], "Sentiment analysis"]
    pros: Annotated[Optional[list[str]], "Positive aspects"]
    cons: Annotated[Optional[list[str]], "Negative aspects"]
    name: Annotated[Optional[str]], "Reviewer name"]
```

**Advantages:**
- Lightweight and minimal
- Good for simple use cases
- Native Python typing support

### 3. JSON Schema (`with_structured_output_json.py`)

Uses raw JSON Schema for maximum flexibility:

```python
json_schema = {
    "title": "Review",
    "type": "object",
    "properties": {
        "key_themes": {
            "type": "array",
            "items": {"type": "string"},
            "description": "Key themes in the review"
        },
        # ... more properties
    },
    "required": ["key_themes", "summary", "sentiment"]
}
```

**Advantages:**
- Platform agnostic
- Maximum flexibility
- Works with any JSON Schema validator

### 4. Hugging Face Integration (`with_structured_output_llama.py`)

Demonstrates structured output with open-source models:

```python
from langchain_huggingface import ChatHuggingFace, HuggingFaceEndpoint

llm = HuggingFaceEndpoint(
    repo_id="TinyLlama/TinyLlama-1.1B-Chat-v1.0",
    task="text-generation"
)
model = ChatHuggingFace(llm=llm)
```

## 🔧 Usage

Run any of the examples:

```bash
# Pydantic example
python with_structured_output_pydantic.py

# TypedDict example  
python with_structured_output_typeddict.py

# JSON Schema example
python with_structured_output_json.py

# Hugging Face example
python with_structured_output_llama.py
```

Each script will process the same sample product review and extract structured information including:
- Key themes
- Summary
- Sentiment analysis
- Pros and cons
- Reviewer name

## 📊 Sample Output

```python
{
    'key_themes': ['performance', 'camera', 'battery', 'price', 'usability'],
    'summary': 'Comprehensive review of Samsung Galaxy S24 Ultra highlighting excellent performance and camera but noting size and price concerns',
    'sentiment': 'pos',
    'pros': [
        'Powerful Snapdragon 8 Gen 3 processor',
        'Excellent 200MP camera with great zoom',
        'Long battery life with fast charging',
        'S-Pen functionality'
    ],
    'cons': [
        'Heavy and difficult for one-handed use',
        'Samsung bloatware',
        'High price point ($1,300)'
    ],
    'name': 'Nitish Singh'
}
```

## 🤔 When to Use Each Approach

| Approach | Best For | Pros | Cons |
|----------|----------|------|------|
| **Pydantic** | Production apps, complex validation | Type safety, validation, documentation | Additional dependency |
| **TypedDict** | Simple schemas, minimal dependencies | Lightweight, native Python | Limited validation |
| **JSON Schema** | Cross-platform, API integration | Universal, flexible | More verbose, no native validation |

## 🔍 Key Concepts

- **Structured Output**: Converting unstructured text into structured data formats
- **Schema Definition**: Different ways to define the expected output structure
- **Type Safety**: Ensuring data conforms to expected types and formats
- **LLM Integration**: Using different language models for structured extraction

## 🤝 Contributing

Feel free to contribute by:
- Adding new schema examples
- Improving documentation
- Adding test cases
- Optimizing existing code

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Troubleshooting

### Common Issues:

1. **Missing API Keys**: Ensure your `.env` file contains valid API keys
2. **Import Errors**: Install all required dependencies using pip
3. **Model Access**: Some Hugging Face models may require authentication

### Getting Help:

- Check the [LangChain documentation](https://python.langchain.com/docs/get_started/introduction)
- Review the [Pydantic documentation](https://docs.pydantic.dev/)
- Open an issue for project-specific problems

---

**Happy coding! 🎉**