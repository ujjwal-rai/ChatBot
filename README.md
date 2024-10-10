---

# Chatbot Using Sentence Embeddings and Cosine Similarity

This repository implements a simple chatbot that leverages sentence embeddings and cosine similarity to retrieve responses from a predefined set of utterances. The chatbot responds to user inputs based on the closest matching utterance, and can handle basic greetings as well as fallback when no good match is found. This project also allows for easy extension by adding more utterances or customizing the model.

## Project Structure

- `speakers.json`: A JSON file that stores information about different speakers (not used in the core chatbot logic but can be utilized for speaker identification in advanced scenarios).
- `index.json`: A JSON file containing an index or mapping of conversation IDs (not directly used in this code but can be useful for organizing larger conversation datasets).
- `conversations.json`: A JSON file that stores structured conversation data.
- `utterances1.jsonl`: A JSONL (JSON Lines) file that contains individual utterances with their associated metadata (e.g., utterance ID and text). This file is where the chatbot retrieves its responses from.
- `corpus.json` (Optional): A file containing an aggregated corpus of text data. This can be used if you prefer to process a larger set of utterances at once.

The dataset used in this project is available for download here:
[**Download Test Dataset**]([https://drive.google.com/file/d/your-link-here](https://drive.google.com/drive/folders/1oMe132eTxh37Az8UDHZLk5F433j_SmS9?usp=sharing))

## Requirements

- Python 3.x
- `nltk`, `scikit-learn`, `sentence-transformers`, `numpy`, `json`

The required Python packages can be installed by running:

```bash
pip install -r requirements.txt
```

Where `requirements.txt` contains:
```
sentence-transformers==2.2.2
nltk
scikit-learn
numpy
```

## Setup

1. **Install Required Libraries:**

   The code requires several Python libraries for processing text and generating embeddings:

   ```bash
   pip install sentence-transformers==2.2.2
   ```

   NLTK data files like stopwords, wordnet, etc., also need to be downloaded:

   ```python
   nltk.download('punkt')
   nltk.download('wordnet')
   nltk.download('omw-1.4')
   nltk.download('stopwords')
   ```

2. **Preprocessing and Lemmatization:**

   The `preprocess` function handles text preprocessing by:
   - Converting text to lowercase
   - Removing punctuation
   - Tokenizing the text
   - Removing stopwords
   - Lemmatizing the tokens

3. **Loading Data:**

   - `load_json`: Loads standard JSON files.
   - `load_jsonl`: Loads JSONL (JSON Lines) files, where each line contains a valid JSON object. This is used for the `utterances1.jsonl` file, which contains individual utterances.

4. **Embedding Generation:**

   The chatbot uses the **Sentence-BERT** model (`paraphrase-MiniLM-L6-v2`) to generate sentence embeddings for all utterances. These embeddings are precomputed using the `SentenceTransformer` library and stored in memory for fast retrieval.

5. **Cosine Similarity for Response Retrieval:**

   The chatbot compares user input with the precomputed utterance embeddings using cosine similarity. The closest match is selected as the response.

6. **Greeting and Fallback Responses:**

   - The chatbot can detect and respond to greetings using predefined greeting phrases (e.g., "hello", "hi").
   - If no good match is found for the user input, the chatbot returns a random fallback response.

## Code Breakdown

### `preprocess` Function
Preprocesses the input text by:
- Lowercasing
- Removing punctuation
- Tokenizing the text into words
- Removing stopwords
- Lemmatizing each word

### `load_json` and `load_jsonl` Functions
- `load_json`: Loads standard JSON files.
- `load_jsonl`: Loads JSONL files (each line is a separate JSON object). This is useful for handling large datasets like utterances.

### `get_response` Function
Generates a chatbot response:
- Encodes the user's input into an embedding.
- Computes cosine similarity between the user's input and all utterance embeddings.
- Returns the closest matching response or a fallback if no good match is found.

### `greet` Function
Checks if the user's input is a greeting. If so, it responds with one of the predefined greeting responses.

### `fallback` Function
Returns a random fallback response if the bot doesn't understand the user's input.

### `chat` Function
Starts the chatbot interaction loop. Users can type messages, and the chatbot will respond based on the best matching utterance or fallback when necessary.

### Running the Chatbot
Run the chatbot by executing:

```bash
python chatbot.py
```

The bot will greet you and start a conversation. You can end the conversation by typing `bye`.

## Example Interaction

```
Bot: Hello! I am the retrieval learning bot. Start typing your message after a greeting to talk to me. To end the conversation, type 'bye'!
You: Hi
Bot: Hello!
You: What's your name?
Bot: I'm not sure I understand. Could you elaborate?
You: bye
Bot: Goodbye! Take care.
```

## Customizing the Chatbot

You can customize the chatbot by:
1. **Adding more utterances:** Add more utterances in the `utterances1.jsonl` file.
2. **Changing the model:** You can swap the `SentenceTransformer` model for another one by choosing from the `sentence-transformers` library.
3. **Modifying responses:** Update the `GREETING_RESPONSES` and `FALLBACK_RESPONSES` lists to change the bot's behavior.
---
