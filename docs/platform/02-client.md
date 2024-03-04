import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Client code

We provide client codes in both Python and Javascript.

## Installation

Follow installation instructions in the repository for our [Python Client](https://github.com/mistralai/client-python) or [Javascript Client](https://github.com/mistralai/client-js).

## Chat Completion

The chat completion API allows you to chat with a model fine-tuned to follow instructions.

<Tabs>
  <TabItem value="python" label="python" default>

### No streaming
```python
from mistralai.client import MistralClient
from mistralai.models.chat_completion import ChatMessage

api_key = os.environ["MISTRAL_API_KEY"]
model = "mistral-large-latest"

client = MistralClient(api_key=api_key)

messages = [
    ChatMessage(role="user", content="What is the best French cheese?")
]

# No streaming
chat_response = client.chat(
    model=model,
    messages=messages,
)

print(chat_response.choices[0].message.content)
```

### With streaming 
```python
from mistralai.client import MistralClient
from mistralai.models.chat_completion import ChatMessage

api_key = os.environ["MISTRAL_API_KEY"]
model = "mistral-large-latest"

client = MistralClient(api_key=api_key)

messages = [
    ChatMessage(role="user", content="What is the best French cheese?")
]

# With streaming
stream_response = client.chat_stream(model=model, messages=messages)

for chunk in stream_response:
    print(chunk.choices[0].delta.content)
```

### With async 
```python
from mistralai.async_client import MistralAsyncClient
from mistralai.models.chat_completion import ChatMessage

api_key = os.environ["MISTRAL_API_KEY"]
model = "mistral-large-latest"

client = MistralAsyncClient(api_key=api_key)

messages = [
    ChatMessage(role="user", content="What is the best French cheese?")
]

# With async
async_response = client.chat_stream(model=model, messages=messages)

async for chunk in async_response: 
    print(chunk.choices[0].delta.content)
```


  </TabItem>
  <TabItem value="javascript" label="javascript">
```javascript
import MistralClient from '@mistralai/mistralai';

const apiKey = process.env.MISTRAL_API_KEY;

const client = new MistralClient(apiKey);

const chatResponse = await client.chat({
  model: 'mistral-large-latest',
  messages: [{role: 'user', content: 'What is the best French cheese?'}],
});

console.log('Chat:', chatResponse.choices[0].message.content);
```
  </TabItem>
  <TabItem value="curl" label="curl">
```bash
curl --location "https://api.mistral.ai/v1/chat/completions" \
     --header 'Content-Type: application/json' \
     --header 'Accept: application/json' \
     --header "Authorization: Bearer $MISTRAL_API_KEY" \
     --data '{
    "model": "mistral-large-latest",
    "messages": [
     {
        "role": "user",
        "content": "What is the best French cheese?"
      }
    ]
  }'
```
  </TabItem>
</Tabs>

We allow users to provide a custom system prompt (see [API reference](../../api)). We also allow a convenient `safe_prompt` flag to force chat completion to be moderated against sensitive content (see [Guardrailing](../guardrailing)).

## JSON mode

Uers have the option to set `response_format` to `{"type": "json_object"}` to enable JSON mode. It's important to explicitly ask the model to generate JSON output in your message.

<Tabs>
  <TabItem value="python" label="python" default>

```python
from mistralai.client import MistralClient
from mistralai.models.chat_completion import ChatMessage

api_key = os.environ["MISTRAL_API_KEY"]
model = "mistral-large-latest"

client = MistralClient(api_key=api_key)

messages = [
    ChatMessage(role="user", content="What is the best French cheese? Return the product and produce location in JSON format")
]

chat_response = client.chat(
    model=model,
    response_format={"type": "json_object"},
    messages=messages,
)

print(chat_response.choices[0].message.content)
```


  </TabItem>
  <TabItem value="javascript" label="javascript">
```javascript
import MistralClient from '@mistralai/mistralai';

const apiKey = process.env.MISTRAL_API_KEY;

const client = new MistralClient(apiKey);

const chatResponse = await client.chat({
  model: 'mistral-large-latest',
  response_format: {'type': 'json_object'},
  messages: [{role: 'user', content: 'What is the best French cheese? Return the product and produce location in JSON format'}],
});

console.log('Chat:', chatResponse.choices[0].message.content);
```
  </TabItem>
  <TabItem value="curl" label="curl">
```bash
curl --location "https://api.mistral.ai/v1/chat/completions" \
     --header 'Content-Type: application/json' \
     --header 'Accept: application/json' \
     --header "Authorization: Bearer $MISTRAL_API_KEY" \
     --data '{
    "model": "mistral-large-latest",
    "messages": [
     {
        "role": "user",
        "response_format": {"type": "json_object"},
        "content": "What is the best French cheese? Return the product and produce location in JSON format"
      }
    ]
  }'
```
  </TabItem>
</Tabs>

## Embeddings

The embeddings API allows you to embed sentences.

<Tabs>
  <TabItem value="python" label="python" default>
```python
from mistralai.client import MistralClient

api_key = os.environ["MISTRAL_API_KEY"]
client = MistralClient(api_key=api_key)

embeddings_batch_response = client.embeddings(
      model="mistral-embed",
      input=["Embed this sentence.", "As well as this one."],
  )
```
  </TabItem>
  <TabItem value="javascript" label="javascript">
```javascript
import MistralClient from '@mistralai/mistralai';

const apiKey = process.env.MISTRAL_API_KEY;

const client = new MistralClient(apiKey);

const input = [];
for (let i = 0; i < 10; i++) {
  input.push('What is the best French cheese?');
}

const embeddingsBatchResponse = await client.embeddings({
  model: 'mistral-embed',
  input: input,
});

console.log('Embeddings Batch:', embeddingsBatchResponse.data);
```
  </TabItem>
  <TabItem value="curl" label="curl">
```bash
curl --location "https://api.mistral.ai/v1/embeddings" \
     --header 'Content-Type: application/json' \
     --header 'Accept: application/json' \
     --header "Authorization: Bearer $MISTRAL_API_KEY" \
     --data '{
    "model": "mistral-embed",
    "input": [
      "Embed this sentence.", 
      "As well as this one."
    ]
  }'
```
  </TabItem>
</Tabs>

# Third-Party Clients

Here are some clients built by the community for various other languages:

## CLI
[icebaker/nano-bots](https://github.com/icebaker/ruby-nano-bots)

## Go
[Gage-Technologies](https://github.com/Gage-Technologies/mistral-go)

## Ruby
[gbaptista/mistral-ai](https://github.com/gbaptista/mistral-ai)

## Rust
[ivangabriele/mistralai-client-rs](https://github.com/ivangabriele/mistralai-client-rs)
