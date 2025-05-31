## Homework: Introduction

Solutions to the Datatalksclub LLM ZOOMCAMP homework 1 (intro) [Original Homework](https://github.com/DataTalksClub/llm-zoomcamp/blob/main/cohorts/2025/01-intro/homework.md)

In this homework, we'll learn more about search and use Elastic Search for practice. 

> It's possible that your answers won't match exactly. If it's the case, select the closest one.


## Q1. Running Elastic 

Run Elastic Search 8.17.6, and get the cluster information. If you run it on localhost, this is how you do it:

```bash
curl localhost:9200
```

What's the `version.build_hash` value?

**Answer:** 42f05b9372a9a4a470db3b52817899b99a76ee73


## Getting the data

Now let's get the FAQ data. You can run this snippet:

```python
import requests 

docs_url = 'https://github.com/DataTalksClub/llm-zoomcamp/blob/main/01-intro/documents.json?raw=1'
docs_response = requests.get(docs_url)
documents_raw = docs_response.json()

documents = []

for course in documents_raw:
    course_name = course['course']

    for doc in course['documents']:
        doc['course'] = course_name
        documents.append(doc)
```

Note that you need to have the `requests` library:

```bash
pip install requests
```

## Q2. Indexing the data

Index the data in the same way as was shown in the course videos. Make the `course` field a keyword and the rest should be text. 

Don't forget to install the ElasticSearch client for Python:

```bash
pip install elasticsearch
```

Which function do you use for adding your data to elastic?

* `insert`
* **`index`** ✅
* `put`
* `add`

## Q3. Searching

Now let's search in our index. 

We will execute a query "How do execute a command on a Kubernetes pod?". 

Use only `question` and `text` fields and give `question` a boost of 4, and use `"type": "best_fields"`.

What's the score for the top ranking result?

* 84.50
* 64.50
* **44.50** ✅
* 24.50

Look at the `_score` field.

## Q4. Filtering

Now ask a different question: "How do copy a file to a Docker container?".

This time we are only interested in questions from `machine-learning-zoomcamp`.

Return 3 results. What's the 3rd question returned by the search engine?

* **How do I debug a docker container?** ✅
* How do I copy files from a different folder into docker container’s working directory?
* How do Lambda container images work?
* How can I annotate a graph?

## Q5. Building a prompt

Now we're ready to build a prompt to send to an LLM. 

Take the records returned from Elasticsearch in Q4 and use this te### Cost Calculation (June 2024 Pricing)
- Input: $5.00 / 1M tokens
- Output: $15.00 / 1M tokens

For 1000 requests with 150 input and 250 output tokens each:
```
Total input tokens for 1000 requests: 150,000
Total output tokens for 1000 requests: 250,000
Cost for input tokens: 150,000 * ($5.00 / 1,000,000) = $0.75
Cost for output tokens: 250,000 * ($15.00 / 1,000,000) = $3.75
Total estimated cost for 1000 requests: $4.50
```

### Actual Cost for Q6/Q7 RAG Call

#### June 2024 Pricing:
```
Actual input tokens (Q6 prompt): 323
Actual output tokens (Q7 response): 93
Cost for actual input: $0.001615
Cost for actual output: $0.001395
Total cost for the single Q6/Q7 RAG call: $0.003010
```

#### May 2025 Pricing:
```
Actual input tokens (Q6 prompt): 323
Actual output tokens (Q7 response): 93
Cost for actual input: $0.000808
Cost for actual output: $0.000930
Total cost for the single Q6/Q7 RAG call: $0.001738
```

## Bonus: Generating the Answer (ungraded)

Let's send the prompt to OpenAI. What's the response?

**Answer:**
```
To execute a command in a running Docker container, use the `docker exec` command. First, find the container ID using `docker ps`, then execute the desired command using the following syntax:

docker exec -it <container-id> <command>

For example, to open a bash shell, you would use:

docker exec -it <container-id> bash
```

Now use the context you just created along with the "How do I execute a command in a running docker container?" question 
to construct a prompt using the template below:

```
prompt_template = """
You're a course teaching assistant. Answer the QUESTION based on the CONTEXT from the FAQ database.
Use only the facts from the CONTEXT when answering the QUESTION.

QUESTION: {question}

CONTEXT:
{context}
""".strip()
```

What's the length of the resulting prompt? (use the `len` function)

* 946
* **1446** ✅
* 1946
* 2446

## Q6. Tokens

When we use the OpenAI Platform, we're charged by the number of 
tokens we send in our prompt and receive in the response.

The OpenAI python package uses `tiktoken` for tokenization:

```bash
pip install tiktoken
```

Let's calculate the number of tokens in our query: 

```python
encoding = tiktoken.encoding_for_model("gpt-4o")
```

Use the `encode` function. How many tokens does our prompt have?

* 120
* 220
* **320** ✅
* 420

Note: to decode back a token into a word, you can use the `decode_single_token_bytes` function:

```python
encoding.decode_single_token_bytes(63842)
```

## Bonus: Generating the Answer (ungraded)

Let's send the prompt to OpenAI. What's the response?

**Answer:**
```
To execute a command in a running Docker container, use the `docker exec` command. First, find the container ID using `docker ps`, then execute the desired command using the following syntax:

docker exec -it <container-id> <command>

For example, to open a bash shell, you would use:

docker exec -it <container-id> bash
```

## Bonus: Calculating the Costs (ungraded)

### Cost for 1000 Requests (June 2024 Pricing)

```
Total input tokens for 1000 requests: 150,000
Total output tokens for 1000 requests: 250,000
Cost for input tokens: $0.75
Cost for output tokens: $3.75
Total estimated cost for 1000 requests: $4.50
```

### Actual Cost Calculation (Based on Q6 Prompt and Q7 Response)

#### June 2024 Pricing
```
Actual input tokens (Q6 prompt): 323
Actual output tokens (Q7 response): 93
Cost for actual input: $0.001615
Cost for actual output: $0.001395
Total cost for the single Q6/Q7 RAG call: $0.003010
```

#### May 2025 Pricing
```
Actual input tokens (Q6 prompt): 323
Actual output tokens (Q7 response): 93
Cost for actual input: $0.000808
Cost for actual output: $0.000930
Total cost for the single Q6/Q7 RAG call: $0.001738
```


## Submit the results

* Submit your results here: https://github.com/DataTalksClub/llm-zoomcamp/blob/main/cohorts/2025/01-intro/homework.md
* It's possible that your answers won't match exactly. If it's the case, select the closest one.
