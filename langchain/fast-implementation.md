# 🏃 Fast Implementation

- [🏃 Fast Implementation](#-fast-implementation)
  - [🚀 Installation](#-installation)
  - [🛠️ Usage](#️-usage)
  - [🤔 Yeah, but where is the chain?](#-yeah-but-where-is-the-chain)

> Here, we will learn how to quickly implement LangChain in your project.
>
> We will use OpenAI's llm as an example.

## 🚀 Installation

Install everything you need to get started with LangChain.

```bash
pip install langchain langchain-openai python-dotenv
```

## 🛠️ Usage

> Before you start coding, you need to get API keys from OpenAI. You can get them by signing up on their [website](https://platform.openai.com/api-keys).

Create a `.env` file in the root directory of your project and add the following line to it.

```.env
OPENAI_API_KEY=your-api-key
```

Now, you can start using LangChain in your project.

Create a new Python file and add the following code to it.

```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv
from os import getenv
from langchain_core.messages import HumanMessage, SystemMessage

# Load the environment variables
load_dotenv()

# Check if the environment variables are loaded
api_key = getenv("OPENAI_API_KEY")
if api_key is None:
    raise Exception("Please set the OPENAI_API_KEY environment variable.")
else:
    print("API Key is loaded successfully.")

# Create a new message object
messages = [
    SystemMessage(content="Translate the following text to Korean"),
    HumanMessage(content="Hello, how are you?"),
]
# Create a new instance of the ChatOpenAI class
model = ChatOpenAI(
    model="gpt-4o",
)

# Invoke(Start) the model by passing the messages object in to the invoke method.
result = model.invoke(messages)

# Print the result
print(result.content)

# Output: 안녕하세요, 어떻게 지내세요?
```

That's it! You have successfully implemented LangChain in your project.

## 🤔 Yeah, but where is the chain?

At the previous code, we created a **messages** object and passed it to the **invoke** method of the **model** object. However, we didn't see any chain in the code. In this section, we will create a chain and see how it works.

We will create a chain by chaining three objects: `prompt_template`, `model`, and `parser`.

- `prompt_template` is a template that will be used to generate prompts.
- `model` is the language model that will be used to generate responses.
- `parser` is a parser that will be used to parse the responses.

And we will create a `chain` object by chaining these three objects.

```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv
from os import getenv
from langchain_core.messages import HumanMessage, SystemMessage
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# Load the environment variables
load_dotenv()

# Check if the environment variables are loaded
api_key = getenv("OPENAI_API_KEY")
if api_key is None:
    raise Exception("Please set the OPENAI_API_KEY environment variable.")
else:
    print("API Key is loaded successfully.")

# Create the prompt template which will take in two user variables:
# - language: The language to translate the text into
# - text: The text to translate
prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system", "Translate the following into {language}:"),
        ("user", "{text}"),
    ]
)

# Create the model instance
model = ChatOpenAI()

# Create the output parser
parser = StrOutputParser()

# Chain the model, prompt template, and output parser together to create a chain instance
chain = prompt_template | model | parser

# Invoke the chain with variables
result = chain.invoke({"language": "Korean", "text": "Hello, how are you?"})

# Print the result
print(result) # Output: 안녕하세요, 어떻게 지내세요?
```

Here, we created a chain by chaining three objects: `prompt_template`, `model`, and `parser`. We then invoked the chain with the variables `language` and `text`. The chain generated a prompt using the `prompt_template`, passed it to the `model`, and parsed the output using the `parser`. Finally, it returned the translated text.
