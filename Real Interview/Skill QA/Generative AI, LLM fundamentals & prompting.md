# Generative AI Interview Questions and Answers (50)

### **1. What is Prompt Engineering?**

**Answer:**

Prompt engineering is the process of writing clear and effective
instructions to get the desired output from an LLM.

In simple terms, we guide the AI model on what to do, how to respond,
and what kind of output we expect.

For example, instead of asking:

"Explain Java."

We can write:

"Explain Java in simple language with a real-world example, suitable for
a beginner."

This helps the model understand exactly what we need and generate a more
relevant response.

A good prompt can include the role of the model, the task, relevant
context, instructions, and the expected output format.

**Real-world use:** In an AI interview application, we use prompt
engineering to instruct the LLM to behave like a technical interviewer,
ask questions, evaluate candidate answers, and generate feedback.

------------------------------------------------------------------------

### **2. What is Temperature in Language Models and How Does It Affect Output Accuracy?**

**Answer:**

Temperature is a parameter that controls the randomness of the model's
output.

It determines how predictable or creative the model's responses will be.
A lower temperature makes the model more likely to choose
high-probability words, while a higher temperature allows more
variation.

-   **Low temperature (for example, 0.1):** Responses are generally more
    consistent and predictable.
-   **Medium temperature (for example, 0.5):** Responses have a balance
    between consistency and variation.
-   **High temperature (for example, 0.9):** Responses can be more
    creative and diverse.

**Example:**

Suppose we ask an LLM: "What is Java?"

-   At a low temperature, it will generally give a straightforward and
    consistent explanation.
-   At a high temperature, it may explain Java using different examples
    or wording.

**Important:** Low temperature does not guarantee factual accuracy. It
mainly controls randomness. The correctness of an answer also depends on
the model, prompt, and information provided to it.

**Real-world use:** For interview evaluation or answering factual
questions, I generally prefer a lower temperature because I want
consistent responses. For creative content generation, a higher
temperature may be useful.

------------------------------------------------------------------------

### **3. What is Tokenization?**

**Answer:**

Tokenization is the process of breaking text into smaller units called
tokens before sending it to a language model.

A token can be a complete word, part of a word, a number, or even a
punctuation mark. It does not always represent one complete word.

For example, the sentence:

`I love programming.`

Might be divided into tokens such as:

`["I", " love", " programming", "."]`

The exact tokenization depends on the tokenizer used by the model.

LLMs do not directly process normal text in the same way humans read it.
First, the text is converted into tokens, and then those tokens are
mapped to numerical IDs. The model processes those IDs to understand
patterns and generate the next tokens.

**Real-world use:** Tokenization is important because API usage, input
limits, output limits, and context window capacity are generally
measured in tokens. Before sending a large prompt to an LLM, we need to
estimate how many tokens it contains.

------------------------------------------------------------------------

### **4. What is Context in AI Models?**

**Answer:**

Context refers to the information available to an AI model while
generating a response.

It helps the model understand what the conversation is about and respond
accordingly. Without relevant context, the model may misunderstand a
follow-up question or give a generic answer.

**Example:**

User: My name is Arbaaz.

User: What is my name?

The model can answer "Your name is Arbaaz" because the previous message
provides context.

In an LLM application, context can include:

-   Previous conversation messages.
-   System instructions.
-   Information provided by the user.
-   Relevant documents retrieved from a knowledge base.
-   Results returned by tools or APIs.

**Real-world use:** In a chatbot, we maintain relevant conversation
history so that the model can understand follow-up questions instead of
treating every message as a completely new conversation. We should not
send unlimited history; we need to manage it within the model's context
window.

------------------------------------------------------------------------

### **5. What is Fine-Tuning, and Why Is It Used?**

**Answer:**

Fine-tuning is the process of taking an already pre-trained model and
training it further on a specific dataset to improve its performance for
a particular task.

A pre-trained model already understands general language patterns.
Fine-tuning helps adapt its behavior to a particular domain, task, or
response style.

**Example:**

Suppose we have a general-purpose LLM, but we want to use it
specifically for technical interviews.

We can prepare a dataset containing:

-   Technical interview questions.
-   Expected answers.
-   Candidate responses.
-   Evaluation criteria.

We then fine-tune the model using these examples so it can become better
suited to the interview-related task.

**Why do we use it?**

1.  To adapt a model to a specific domain.
2.  To improve consistency on a particular task.
3.  To teach the model a preferred response format or behavior.
4.  To improve performance on repeated, well-defined use cases.

**Important:** Fine-tuning is not always necessary. If we only need to
provide the model with company documents or frequently changing
information, RAG is often a better option. Prompt engineering may also
be enough for simpler tasks.

**Real-world use:** Fine-tuning can be useful when an application needs
a consistent domain-specific style or task behavior that is difficult to
achieve through prompting alone.

------------------------------------------------------------------------

### **6. What is Hugging Face and Why Do We Use It?**

**Answer:**

Hugging Face is an AI platform and ecosystem that provides pre-trained
models, datasets, tokenizers, and tools for working with machine
learning and deep learning models.

In simple terms, it is like GitHub for AI models, where developers can
find, download, share, and use models.

For example, if I want to use an open-source language model like Llama,
I can find its model on Hugging Face and use the available libraries to
load it.

Some important components are:

1.  **Model Hub:** A repository where developers can discover and share
    pre-trained models.
2.  **Transformers:** A library used to load and work with
    transformer-based models.
3.  **Datasets:** Provides datasets for training and evaluation.
4.  **Tokenizers:** Helps convert text into tokens and token IDs.
5.  **PEFT:** Provides parameter-efficient fine-tuning techniques such
    as LoRA.

**Real-world use:** We can use Hugging Face Transformers to load an
open-source LLM and integrate it into our Python application. We can
also use its PEFT library to fine-tune a model with fewer trainable
parameters.

------------------------------------------------------------------------

### **7. What is a Context Window in Language Models?**

**Answer:**

A context window is the maximum number of tokens an LLM can process and
consider at a time while generating a response.

It includes the input prompt, conversation history, system instructions,
and other information provided to the model. The generated response also
needs to fit within the model's applicable context limits.

**Example:**

Suppose a model has a context window of 128K tokens.

It can process up to approximately 128,000 tokens in its context,
subject to the model and API's input/output limits.

If we keep sending a long conversation, eventually we may reach the
context limit. At that point, the request may fail or we may need to
reduce the amount of information being sent.

We can manage this by:

-   Removing irrelevant conversation history.
-   Summarizing older messages.
-   Retrieving only relevant document chunks.
-   Limiting the size of the prompt.
-   Reserving enough tokens for the model's response.

**Real-world use:** In a chatbot, we manage the context window to avoid
sending unnecessary conversation history with every API request. This
also helps reduce token consumption and cost.

**Remember the difference:**

-   Context = Information available to the model.
-   Context Window = Maximum token capacity available for that
    information.

------------------------------------------------------------------------

### **8. What Does It Mean When LLMs Say They Have 40 Billion Parameters?**

**Answer:**

Parameters are the internal numerical values, also called weights, that
a neural network learns during training.

When we say an LLM has 40 billion parameters, it means the model
contains approximately 40 billion learned numerical values.

These parameters help the model identify patterns, understand
relationships between words, and generate responses. During training,
the model adjusts these values based on the training data.

**Example:**

Suppose the model is trained on a large amount of text containing
relationships such as:

-   King and Queen.
-   Java and Programming.
-   India and New Delhi.

The model learns patterns from these examples, and its parameters are
adjusted to represent those learned patterns.

**What does a higher number of parameters mean?**

Generally, more parameters provide greater model capacity, but they do
not automatically mean better performance.

Performance also depends on:

-   Quality and quantity of training data.
-   Model architecture.
-   Training methodology.
-   Fine-tuning and evaluation.

**Real-world use:** Models with billions of parameters generally require
significant GPU memory and computational resources to run. This is one
reason quantization and parameter-efficient fine-tuning techniques are
useful.

------------------------------------------------------------------------

### **9. What is the Difference Between LoRA and QLoRA?**

**Answer:**

Both LoRA and QLoRA are parameter-efficient fine-tuning techniques used
to fine-tune large language models without updating all their original
parameters.

**LoRA (Low-Rank Adaptation):**

LoRA freezes the original model weights and introduces small trainable
matrices into selected layers.

Instead of updating billions of original parameters, we train only these
additional smaller matrices. This reduces the amount of computation and
memory required for training.

**QLoRA (Quantized LoRA):**

QLoRA works similarly to LoRA, but first quantizes the original model
weights, generally to 4-bit precision.

This reduces the memory required to load the base model. The original
weights remain frozen, and the LoRA adapters are trained.

**Difference:**

  -----------------------------------------------------------------------
  Feature                 LoRA                    QLoRA
  ----------------------- ----------------------- -----------------------
  Original weights        Frozen                  Frozen and quantized

  Quantization            Not required            Generally 4-bit

  Memory usage            Lower than full         Usually lower than LoRA
                          fine-tuning             

  Trainable parameters    LoRA adapters           LoRA adapters

  Main benefit            Efficient fine-tuning   More memory-efficient
                                                  fine-tuning
  -----------------------------------------------------------------------

**Example:**

Suppose I want to fine-tune a 7B parameter model.

-   With LoRA, I load the base model and train small adapter matrices
    while keeping the original weights frozen.
-   With QLoRA, I first load the base model in a 4-bit quantized format
    and then train the adapters.

**Real-world use:** QLoRA is useful when we have limited GPU resources
but still want to fine-tune large language models.

------------------------------------------------------------------------

### **10. What is the Difference Between LLM and Other Models?**

**Answer:**

An LLM (Large Language Model) is a type of AI model specifically
designed to understand and generate human language.

Other machine learning models are often designed for specific tasks,
such as predicting house prices, detecting fraud, classifying images, or
identifying spam emails.

An LLM can perform multiple language-related tasks using the same model,
depending on the instructions and context we provide.

**Example:**

Suppose we build a customer support application.

-   A traditional ML model might classify a customer message as positive
    or negative.
-   An LLM can understand the message, identify the problem, generate a
    response, summarize the conversation, and answer follow-up
    questions.

  -----------------------------------------------------------------------
  Traditional ML Model                LLM
  ----------------------------------- -----------------------------------
  Often designed for a specific task  Can perform multiple language tasks

  Usually works with task-specific    Can process natural language
  features                            

  May require separate models for     One model can handle several
  different tasks                     language tasks

  Examples: Decision Tree, Random     Examples: GPT, Llama, Gemini
  Forest                              
  -----------------------------------------------------------------------

**Important:** LLMs are not completely separate from other AI models.
They are a specialized category of deep learning models, generally based
on Transformer architectures.

------------------------------------------------------------------------

### **11. What are Two Ways to Reduce Hallucinations in an LLM-Based RAG System? \[VERY IMPORTANT\]**

**Answer:**

Hallucination happens when an LLM generates information that sounds
correct but is actually incorrect or unsupported.

In a RAG system, we provide relevant documents to the LLM so it can
generate answers based on those documents. However, if the retrieval
process returns irrelevant information or the model does not follow the
context properly, hallucinations can still happen.

There are two important ways to reduce hallucinations:

**1. Improve Retrieval Quality**

First, we need to make sure that the correct information is retrieved
from the knowledge base.

We can improve retrieval by:

-   Using proper document chunking.
-   Selecting a suitable embedding model.
-   Using metadata filters.
-   Applying hybrid search (keyword + semantic search).
-   Using a reranker to select the most relevant chunks.

For example, if a user asks about a company's refund policy, we should
retrieve the actual refund-policy section instead of unrelated company
documents.

**2. Use Grounded Prompting and Validation**

We should instruct the LLM to answer only from the retrieved context.

For example, our prompt can say:

"Answer the user's question using only the provided context. If the
answer is not available in the context, clearly say that you do not have
enough information. Do not make up an answer."

We can also ask the model to provide source references and validate
important claims against the retrieved content.

**Real-world use:** In an enterprise chatbot, I would combine good
retrieval, reranking, grounded prompts, and source citations to reduce
unsupported answers.

**Important:** RAG reduces hallucinations, but it does not completely
eliminate them.

------------------------------------------------------------------------

### **12. What is the Reflection Pattern in Generative AI and Agentic AI? \[VERY IMPORTANT\]**

**Answer:**

The reflection pattern is a technique in which an AI model reviews its
own generated output, identifies mistakes or missing information, and
improves the result.

In simple terms, instead of generating an answer once and returning it
immediately, we give the model an opportunity to review and correct its
work.

The basic flow is:

**Generate → Review → Identify Issues → Improve → Final Answer**

**Example:**

Suppose an AI agent is asked to generate a SQL query.

1.  The LLM first generates the SQL query.
2.  A reflection step reviews whether the query matches the user's
    requirement.
3.  It checks whether the correct tables and columns are used.
4.  If it finds an issue, it generates a corrected query.
5.  The final query is returned or executed after validation.

In Agentic AI, reflection can also happen after the agent performs an
action. The agent checks the result and decides whether another action
is required.

**Real-world use:** In an AI coding assistant, the agent can generate
code, review it for errors, run tests, and improve the code based on
test results.

**Important:** Reflection can improve output quality, but it also adds
extra LLM calls, latency, and cost. We should use it when the task
benefits from an additional review step.

------------------------------------------------------------------------

### **13. What is the Difference Between AI, Machine Learning, Deep Learning, and Generative AI? \[IMPORTANT\]**

**Answer:**

These are related concepts, but they represent different levels or areas
of artificial intelligence.

**1. Artificial Intelligence (AI)**

AI is the broad field of creating systems that can perform tasks that
normally require human intelligence, such as understanding language,
recognizing images, making decisions, and solving problems.

**2. Machine Learning (ML)**

Machine Learning is a subset of AI where systems learn patterns from
data instead of relying only on explicitly written rules.

For example, an ML model can learn from previous transactions to
identify potentially fraudulent transactions.

**3. Deep Learning (DL)**

Deep Learning is a subset of Machine Learning that uses neural networks
with multiple layers to learn complex patterns.

For example, deep learning models are used in speech recognition, image
recognition, and language processing.

**4. Generative AI**

Generative AI focuses on creating new content such as text, images,
audio, video, and code.

For example, ChatGPT generates text, while image-generation models
create images from text descriptions.

**Relationship:**

-   AI is the broadest field.
-   ML is a subset of AI.
-   DL is a subset of ML.
-   Modern Generative AI commonly uses deep learning techniques.

**Real-world use:** ChatGPT is a Generative AI application that uses a
deep learning model, generally based on the Transformer architecture.

------------------------------------------------------------------------

### **14. What Do Tokens Represent in LLMs?**

**Answer:**

Tokens are the basic units of text that an LLM processes.

A token does not always represent one complete word. It can be a
complete word, part of a word, a number, or punctuation.

For example, the word "playing" might be represented as one token or
split into smaller tokens, depending on the tokenizer.

When we send a prompt to an LLM, the tokenizer converts the text into
token IDs. The model processes those IDs and predicts the next token
based on the previous tokens.

**Example:**

If we ask:

"What is Java?"

The model converts the input into token IDs, processes them, and
generates the answer token by token.

**Real-world use:** Tokens are important because they determine how much
text we can send within a context window and how much we pay for many
LLM API calls.

------------------------------------------------------------------------

### **15. What is the Purpose of a System Prompt?**

**Answer:**

A system prompt is a set of high-level instructions that tells the LLM
how it should behave throughout a conversation or a particular task.

It defines the role, behavior, rules, tone, and boundaries of the AI
assistant.

**Example:**

Suppose we are building an AI technical interviewer.

We can provide a system prompt like:

"You are a professional technical interviewer. Ask one question at a
time. Evaluate the candidate's response. Do not reveal the expected
answer. Keep the conversation friendly and professional."

Now, whenever the user interacts with the AI, the model uses these
instructions to guide its responses.

A system prompt can also define:

-   What the model should and should not do.
-   The expected response format.
-   The language and tone.
-   How to handle unknown information.
-   When to use tools or escalate a request.

**Real-world use:** In our AI interview application, we use a system
prompt to control the interviewer's behavior and maintain a consistent
interview flow.

**Important:** A system prompt guides model behavior, but it is not a
replacement for backend authorization, security checks, or
application-level validation.

------------------------------------------------------------------------

### **16. What Does Temperature Control in Text Generation?**

**Answer:**

Temperature controls the randomness of the next-token selection process
during text generation.

When an LLM generates a response, it calculates probabilities for
possible next tokens. Temperature changes how strongly the model favors
high-probability tokens compared with less likely alternatives.

-   **Low temperature:** The model generally selects more probable
    tokens, producing more predictable responses.
-   **High temperature:** The model gives more opportunity to
    lower-probability tokens, producing more varied responses.

**Example:**

Suppose we ask the model to write a welcome message.

At a low temperature, it may generate a simple and consistent welcome
message.

At a higher temperature, it may generate different and more creative
versions.

**Real-world use:** I would use a lower temperature for technical
answers, interview evaluation, and structured responses. For creative
writing or brainstorming, I may use a higher temperature.

------------------------------------------------------------------------

### **17. What is LoRA Used For?**

**Answer:**

LoRA stands for Low-Rank Adaptation. It is a parameter-efficient
fine-tuning technique used to adapt a large pre-trained model to a
specific task without updating all the original model parameters.

Normally, full fine-tuning requires updating a very large number of
model weights, which needs significant GPU memory and computational
resources.

LoRA solves this problem by freezing the original model weights and
adding small trainable matrices to selected layers.

Instead of training the entire model, we train only these additional
matrices, called adapters.

**Example:**

Suppose we have a general-purpose LLM and want to adapt it for legal
document analysis.

We can prepare a legal-domain dataset and use LoRA to train small
adapter matrices while keeping the original model unchanged.

**Real-world use:** LoRA is useful when we want to customize a large
language model but have limited GPU resources or want to maintain
separate adapters for different tasks.

------------------------------------------------------------------------

### **18. What is Chain-of-Thought Prompting, Why Is It Used, and How Does It Differ from Normal Prompting?**

**Answer:**

Chain-of-thought prompting is a prompting approach that encourages a
model to solve a problem through intermediate reasoning steps instead of
directly jumping to the final answer.

It is useful for tasks that require multiple steps, such as mathematical
problems, logical reasoning, and complex problem-solving.

**Normal Prompting Example:**

"What is the result of this problem?"

The model is asked to provide the answer directly.

**Step-Based Prompting Example:**

"Break the problem into smaller steps, solve each part, and then provide
the final answer."

This encourages the model to approach the task in a more structured way.

**Why is it used?**

-   To handle multi-step problems.
-   To make complex tasks more structured.
-   To help the model consider intermediate results.
-   To produce a clearer explanation of the answer.

**Real-world use:** In an AI application, we can ask the model to
analyze a candidate's answer against multiple evaluation criteria before
producing a final score and feedback.

**Important:** Asking for step-by-step reasoning does not guarantee
correctness. In production, we should validate the result and request a
concise explanation or summary rather than relying on hidden internal
reasoning.

------------------------------------------------------------------------

### **19. What is the Solution Approach for the numberOfTokens Function?**

**Answer:**

The numberOfTokens function is generally used to calculate how many
tokens are present in a given input.

The first thing I would do is understand which model or tokenizer the
application is using, because different models can tokenize the same
text differently.

The approach would be:

1.  Accept the input text.
2.  Pass the text to the tokenizer associated with the target model.
3.  Convert the text into token IDs.
4.  Count the number of token IDs.
5.  Return the token count.

**Example:**

Suppose the input is:

`"I am learning Generative AI"`

The tokenizer may split this sentence into multiple tokens. We count
those tokens rather than simply counting the words.

**Important:** We should not use word count as an exact token count. For
chat API requests, message roles, formatting, and tool definitions can
also contribute to the total token usage.

**Real-world use:** We can use this function before sending a large
prompt to an LLM to check whether it fits within the context window and
to estimate API consumption.

------------------------------------------------------------------------

### **20. What is a Context Window, and How Do You Prevent It from Being Exceeded?**

**Answer:**

A context window is the maximum number of tokens an LLM can process in a
single request.

It includes the system prompt, user message, conversation history,
retrieved documents, and other information provided to the model. We
also need to reserve space for the generated response.

Suppose a model supports a context window of 32K tokens. If our prompt
and conversation history consume almost the entire limit, there may not
be enough space for the response.

To prevent this, I would:

1.  Count the tokens before making the API call.
2.  Remove unnecessary conversation history.
3.  Summarize older messages instead of sending the complete history.
4.  Retrieve only relevant document chunks using RAG.
5.  Limit the number of retrieved chunks.
6.  Reserve sufficient tokens for the model's response.

**Real-world use:** In a chatbot, we can maintain a token budget for
every request. Whenever the conversation becomes too long, we summarize
older messages and retain the most relevant context.

------------------------------------------------------------------------

### **21. What is the Difference Between Top-K and Top-P Sampling?**

**Answer:**

Top-K and Top-P are decoding techniques used to control which tokens an
LLM can select while generating text.

Both techniques limit the possible next-token choices, but they work
differently.

**1. Top-K Sampling**

Top-K means the model considers only the K most probable next tokens.

For example, if top-K is set to 5, the model considers only the five
most likely tokens and selects the next token from those candidates.

**2. Top-P Sampling**

Top-P is also called nucleus sampling. Instead of selecting a fixed
number of tokens, it considers the smallest group of tokens whose
combined probability reaches a specified threshold.

For example, if top-P is 0.9, the model considers enough
high-probability tokens to cover approximately 90% of the probability
distribution.

**Difference:**

  -----------------------------------------------------------------------
  Top-K                               Top-P
  ----------------------------------- -----------------------------------
  Uses a fixed number of candidate    Uses a cumulative probability
  tokens                              threshold

  Example: K = 5                      Example: P = 0.9

  Number of candidates stays fixed    Number of candidates can change
  -----------------------------------------------------------------------

**Real-world use:** These settings are useful when we want to control
the diversity and predictability of generated text. The exact behavior
depends on the model and API.

------------------------------------------------------------------------

### **22. How Does Re-Ranking Improve the Accuracy of an LLM Model? \[VERY IMPORTANT\]**

**Answer:**

Re-ranking improves the relevance of the information that we provide to
an LLM in a RAG pipeline.

In a normal RAG system, we first retrieve documents based on similarity
with the user's question. However, the initial search results may
contain some irrelevant or less useful chunks.

A reranker takes the retrieved chunks and evaluates how relevant each
chunk is to the actual user query. It then rearranges them according to
their relevance scores.

**Example:**

Suppose a user asks:

"What is the company's refund policy?"

Our vector database retrieves 20 chunks.

Some chunks may discuss payments, cancellation, delivery, and refunds.

A reranker evaluates these 20 chunks and identifies the ones most
directly related to the refund policy. We then send the best chunks to
the LLM.

**How does it help?**

1.  Removes less relevant information from the final context.
2.  Places the most useful chunks at the top.
3.  Reduces unnecessary context.
4.  Helps the LLM generate more relevant answers.
5.  Can reduce unsupported answers caused by poor retrieval.

**Real-world use:** In an enterprise chatbot, I would use top-K
retrieval followed by reranking before passing the final document chunks
to the LLM.

**Important:** Reranking improves retrieval quality. It does not
directly change the LLM's internal knowledge or guarantee a correct
answer.

------------------------------------------------------------------------

### **23. How to Reduce Token and Credit Consumption in a Chatbot Using RAG and LLM? \[VERY IMPORTANT\]**

**Answer:**

In an LLM-based chatbot, token consumption directly affects API cost.
So, the goal is to send only the information that is actually required
to generate the answer.

I would optimize the application in several ways.

**1. Use RAG Instead of Sending Complete Documents**

Instead of sending an entire PDF or document to the LLM, we split it
into chunks and retrieve only the relevant information.

**2. Optimize Conversation History**

We should not send the complete conversation history every time. We can
summarize older messages and retain only the recent and relevant
information.

**3. Improve Retrieval Quality**

Use metadata filters, hybrid search, and reranking so that only relevant
chunks are sent to the LLM.

**4. Keep Prompts Concise**

Avoid repeating the same instructions or unnecessary information in
every request.

**5. Limit Output Tokens**

If we need a short answer, we can instruct the model to respond
concisely and configure a suitable maximum output-token limit.

**6. Use the Right Model**

We can use a smaller and cheaper model for simple tasks such as
classification or query routing, and use a more capable model for
complex tasks.

**7. Use Caching**

If users repeatedly ask the same questions, we can cache suitable
responses or intermediate results instead of making unnecessary LLM
calls.

**Real-world use:** In a RAG chatbot, I would combine optimized chunk
retrieval, conversation summarization, concise prompts, caching, and
model selection to reduce token consumption without affecting answer
quality.

------------------------------------------------------------------------

### **24. How Does the System Handle Patient Questions That Cannot Be Answered by the AI or LLM?**

**Answer:**

In a healthcare application, the system should never force the LLM to
answer a question when it does not have sufficient or reliable
information.

First, we retrieve relevant information from an approved medical
knowledge base. Then we check whether the retrieved content actually
supports an answer.

If the required information is not available, the system should clearly
tell the patient that it cannot provide a reliable answer and guide them
to contact a qualified healthcare professional.

For example, suppose a patient asks:

"Can I stop taking my prescribed medicine?"

If the system does not have enough patient-specific medical information,
it should not make that decision. It should advise the patient to
consult their doctor.

For urgent symptoms or emergencies, the application should show a
predefined escalation message directing the patient to immediate
professional or emergency care.

**Real-world use:** In a healthcare chatbot, I would implement a
fallback mechanism, confidence or evidence checks, and escalation to a
human professional whenever the AI cannot safely answer.

**Important:** An LLM should support healthcare communication, not
replace a qualified medical professional.

------------------------------------------------------------------------

### **25. Explain How to Call LLMs in an Agent-Based AI System. \[VERY IMPORTANT\]**

**Answer:**

In an agent-based AI system, the LLM is generally called through an
orchestration layer.

The orchestrator manages the interaction between the user, LLM, tools,
APIs, and other components.

Unlike a simple chatbot, an agent can decide that it needs to perform an
action, call a tool, observe the result, and then decide what to do
next.

**Example:**

Suppose a user asks:

"What is the current weather in Delhi?"

The flow would be:

1.  The user sends the request.
2.  The orchestrator receives it and prepares the prompt.
3.  The orchestrator calls the LLM.
4.  The LLM identifies that it needs weather information and requests a
    weather tool.
5.  The backend validates the tool request and calls the weather API.
6.  The API returns the weather information.
7.  The orchestrator sends the tool result back to the LLM.
8.  The LLM generates the final response.
9.  The application returns the answer to the user.

**Architecture:**

`User → Orchestrator → LLM → Tool/API → LLM → Final Response`

The LLM decides what action may be needed, but the application executes
the tool and enforces permissions.

**Real-world use:** In an AI interview application, an agent can call
APIs to fetch interview questions, retrieve candidate information,
evaluate responses, and generate interview reports.

**Important:** We should implement tool validation, timeouts, retry
handling, permissions, and maximum execution steps to prevent
uncontrolled agent behavior.

------------------------------------------------------------------------

### **26. What Makes a Chatbot Different from AI?**

**Answer:**

AI is a broad technology that enables machines to perform tasks that
normally require human intelligence, such as understanding language,
recognizing images, making predictions, and solving problems.

A chatbot is an application that allows users to communicate with a
system through a conversational interface.

A chatbot can use different technologies:

-   Rule-based logic.
-   Machine learning models.
-   Natural Language Processing.
-   Large Language Models.

**Example:**

A simple rule-based chatbot may respond with predefined answers based on
keywords.

An AI-powered chatbot using an LLM can understand natural-language
questions, maintain conversation context, summarize information, and
generate dynamic responses.

**Real-world use:** ChatGPT is an example of an AI-powered chatbot. The
chatbot is the interface, while the LLM is one of the AI technologies
powering it.

**Remember:** Every chatbot does not necessarily use AI, and AI is much
broader than chatbots.

------------------------------------------------------------------------

### **27. How is Function Calling Different from MCP?**

**Answer:**

Function calling and MCP are both used to connect LLM applications with
external tools, but they solve different problems.

**1. Function Calling**

Function calling is a capability provided by an LLM API that allows the
model to request the execution of a function.

For example, suppose a user asks:

"What is the weather in Delhi?"

The model may generate a function call such as:

`get_weather(location="Delhi")`

The backend receives the function call, validates it, executes the
weather API, and sends the result back to the model.

**2. MCP (Model Context Protocol)**

MCP is a standard protocol that allows AI applications to connect with
external tools and context sources through MCP servers.

Instead of implementing a completely different integration for every
tool, an application can use MCP to discover and communicate with tools
exposed by MCP servers.

**Difference:**

  -----------------------------------------------------------------------
  Function Calling                    MCP
  ----------------------------------- -----------------------------------
  A model/API capability              A standard communication protocol

  Allows the model to request a       Connects AI applications to tools
  function                            and context sources

  Application executes the requested  MCP client communicates with MCP
  function                            servers

  Often defined through               Provides a standardized integration
  provider-specific schemas           approach
  -----------------------------------------------------------------------

**Real-world use:** We can use function calling to let an LLM request an
action, while MCP can provide a standardized way to expose external
tools to the AI application.

**Important:** They are not competitors. An agent can use function
calling and MCP together.

------------------------------------------------------------------------

### **28. What Tasks Do You Handle When Working with Large Language Models (LLMs)?**

**Answer:**

When working with LLMs, my responsibilities generally cover the complete
integration of the model into an application.

Some of the important tasks include:

1.  **Model Integration:** Connecting LLM APIs such as OpenAI or
    integrating open-source models.
2.  **Prompt Engineering:** Designing system prompts and task-specific
    instructions.
3.  **RAG Implementation:** Extracting documents, chunking text,
    generating embeddings, and retrieving relevant information.
4.  **Vector Database Integration:** Storing and searching embeddings
    using vector databases.
5.  **Agent Development:** Implementing tool calling and orchestrating
    multiple steps.
6.  **API Development:** Building backend APIs using frameworks such as
    FastAPI.
7.  **Token Optimization:** Managing context windows and reducing
    unnecessary token consumption.
8.  **Performance Optimization:** Reducing latency and improving
    response time.
9.  **Evaluation:** Testing answer quality, retrieval accuracy, and
    hallucinations.
10. **Monitoring and Security:** Tracking usage, errors, cost, and
    protecting user data.

**Real-world use:** In an AI interview application, these tasks can be
used to build an interviewer that retrieves questions, interacts with
candidates, evaluates answers, and generates interview reports.

------------------------------------------------------------------------

### **29. Why Do Many People Use FastAPI to Host LLM Models Instead of Flask? \[VERY IMPORTANT\]**

**Answer:**

FastAPI is widely used for building LLM-based backend services because
it provides asynchronous request handling, automatic request validation,
and API documentation.

LLM applications frequently communicate with external services such as:

-   OpenAI or other LLM providers.
-   Vector databases.
-   Embedding APIs.
-   External tools and services.

These operations involve waiting for network responses. FastAPI supports
asynchronous endpoints, which can help the application handle multiple
I/O-bound requests efficiently.

Some important advantages are:

1.  **Async Support:** Supports `async` and `await` for I/O-bound
    operations.
2.  **Data Validation:** Uses Pydantic to validate incoming requests.
3.  **Automatic Documentation:** Generates Swagger UI and OpenAPI
    documentation.
4.  **Streaming:** Supports streaming responses, which is useful for
    displaying LLM output token by token.
5.  **Type Hints:** Makes API code easier to understand and maintain.

**Example:**

Suppose 100 users send requests to an LLM chatbot at the same time.

With asynchronous handling, the application can manage many requests
while waiting for external LLM API responses, provided the libraries
being used also support non-blocking operations.

**Real-world use:** I would choose FastAPI for an LLM backend when I
need asynchronous API calls, request validation, and streaming
responses.

**Important:** FastAPI does not automatically make the LLM inference
itself faster. Actual inference speed depends on the model, hardware,
serving framework, and configuration.

------------------------------------------------------------------------

### **30. Why is FastAPI Preferred Over Flask and Django for Hosting LLM Models? \[VERY IMPORTANT\]**

**Answer:**

FastAPI is often preferred for LLM-based applications because it
provides a lightweight API framework with built-in support for
asynchronous programming, request validation, and automatic API
documentation.

Let's understand the differences.

**1. FastAPI**

-   Supports asynchronous endpoints using `async` and `await`.
-   Uses Pydantic for request and response validation.
-   Automatically generates Swagger documentation.
-   Supports streaming responses.
-   Works well for API-focused microservices.

**2. Flask**

-   Lightweight and easy to learn.
-   Suitable for simple web applications and APIs.
-   Request validation and documentation generally require additional
    libraries.
-   Can also be used for LLM applications.

**3. Django**

-   Provides a complete web framework.
-   Includes features such as ORM, authentication, admin panel, and
    database management.
-   Useful for large web applications.
-   May be more than what is needed for a dedicated LLM-serving API.

**Comparison:**

  -------------------------------------------------------------------------
  Feature           FastAPI           Flask             Django
  ----------------- ----------------- ----------------- -------------------
  Async support     Built-in          Available with    Supported
                                      limitations and   
                                      extensions        

  Data validation   Pydantic          Usually           Forms/serializers
                    integration       additional        through ecosystem
                                      libraries         

  API documentation Automatic OpenAPI Additional setup  Additional setup

  Framework style   API-focused       Minimal           Full-stack

  Streaming         Supported         Supported         Supported
  -------------------------------------------------------------------------

**Real-world use:** For an LLM microservice that communicates with model
APIs, vector databases, and other services, I would generally choose
FastAPI because it provides convenient async support and API validation.

**Important:** Flask and Django can also host LLM applications. The
choice depends on project requirements, and the framework alone does not
determine model inference performance.

------------------------------------------------------------------------

### **31. How Do You Manage Tokens in API Calls or LLM Integrations? \[Scenario Based\]**

**Answer:**

When integrating an LLM API, I manage tokens to make sure the request
stays within the model's context window and does not consume unnecessary
tokens.

First, I identify the tokenizer and context limit of the model being
used.

Before sending a request, I consider the tokens consumed by:

-   System prompt.
-   User message.
-   Conversation history.
-   Retrieved RAG documents.
-   Tool definitions and other message formatting.
-   Expected model response.

Then I maintain a token budget.

**Example:**

Suppose a model supports a context window of 16K tokens.

I would not use all 16K tokens for the input. I would reserve enough
space for the response and keep a safety margin for message formatting
and other overhead.

If the input becomes too large, I can:

1.  Remove unnecessary conversation history.
2.  Summarize previous messages.
3.  Retrieve fewer document chunks.
4.  Shorten repeated instructions.
5.  Limit the maximum output tokens.

I also track the actual token usage returned by the API.

**Real-world use:** In a production chatbot, token management helps
prevent context-limit errors and control API costs.

------------------------------------------------------------------------

### **32. How Do You Ensure Security and Privacy of Patient Data When Using LLMs, Considering Data Might Be Used Internally for Model Training?**

**Answer:**

When working with patient data, security and privacy are very important
because medical information is sensitive.

First, I would verify the data-handling policy of the LLM provider. We
should not assume that every provider uses API data for training or that
every provider automatically excludes it. We need to check the exact
service terms, retention settings, and contractual commitments.

At the application level, I would follow these practices:

1.  **Data Minimization:** Send only the patient information required
    for the task.
2.  **Data Masking:** Remove or mask personal identifiers wherever
    possible.
3.  **Encryption:** Encrypt patient data both in transit and at rest.
4.  **Access Control:** Use role-based access control so users can
    access only authorized records.
5.  **Secure Logging:** Avoid storing sensitive patient information
    unnecessarily in application logs.
6.  **Retention Policy:** Define how long patient data is stored and
    when it should be deleted.
7.  **Provider Compliance:** Use approved providers and follow
    applicable healthcare privacy regulations and organizational
    policies.

**Example:**

If a patient asks a question about a medical report, we should retrieve
only the records that the authenticated patient is authorized to access.

**Real-world use:** In a healthcare RAG chatbot, I would combine data
masking, access-controlled retrieval, encryption, audit logging, and
approved LLM-provider configurations.

------------------------------------------------------------------------

### **33. How Do Top-K Retrieval, Reranking, and LLM Generation Work in a RAG Pipeline? \[VERY IMPORTANT\]**

**Answer:**

In a RAG pipeline, we retrieve relevant information from a knowledge
base and provide it to the LLM so it can generate an answer based on
that information.

There are three important stages: Top-K retrieval, reranking, and LLM
generation.

**Step 1: User Query**

The user asks a question.

For example:

"What is the company's refund policy?"

**Step 2: Embedding Generation**

The user's question is converted into a numerical vector using an
embedding model.

**Step 3: Top-K Retrieval**

The vector database searches for document chunks that are semantically
similar to the user's query.

Suppose we set K = 10. The system retrieves the 10 most relevant
candidate chunks.

**Step 4: Re-Ranking**

The retrieved chunks are passed to a reranker.

The reranker compares each chunk with the original question and assigns
relevance scores. It then sorts the chunks so that the most relevant
information comes first.

**Step 5: Context Construction**

The application selects the best chunks that fit within the available
token budget and adds them to the prompt.

**Step 6: LLM Generation**

The LLM receives:

-   System instructions.
-   User question.
-   Retrieved document chunks.

It uses this information to generate the final answer.

**Complete Flow:**

`User Query → Embedding → Top-K Retrieval → Re-ranking → Context Building → LLM → Final Answer`

**Real-world use:** In an enterprise chatbot, this approach helps the
LLM answer questions using company documents instead of relying only on
its pre-trained knowledge.

**Remember:**

-   Top-K retrieval finds candidate information.
-   Reranking improves the ordering of that information.
-   LLM generation converts the selected information into a
    natural-language answer.

------------------------------------------------------------------------

### **34. Why Use RAG Instead of Sending an Entire PDF Directly to an LLM? \[VERY IMPORTANT\]**

**Answer:**

We use RAG because sending an entire PDF to an LLM every time can
consume a large number of tokens, increase API costs, and include a lot
of irrelevant information.

Suppose we have a PDF containing 500 pages, and a user asks a question
about one particular section.

If we send the complete PDF to the LLM, the model has to process a huge
amount of information even though the user needs only a small part of
it.

RAG solves this problem.

**How does RAG work?**

1.  First, we extract text from the PDF.
2.  We split the text into smaller chunks.
3.  We generate embeddings for those chunks.
4.  We store the embeddings in a vector database.
5.  When the user asks a question, we retrieve only the relevant chunks.
6.  We send those chunks along with the question to the LLM.
7.  The LLM generates the answer using the retrieved information.

**Benefits of RAG:**

-   Reduces unnecessary token consumption.
-   Reduces API cost.
-   Avoids sending complete documents repeatedly.
-   Supports searching across large document collections.
-   Makes source references and citations possible.
-   Helps keep information up to date by updating the knowledge base.

**Real-world use:** In an enterprise chatbot, we can upload thousands of
company documents and use RAG to retrieve only the information relevant
to each user query.

**Important:** If a PDF is short and fits comfortably within the context
window, sending the complete document may still be a reasonable
approach. RAG is especially useful for large or frequently queried
document collections.

------------------------------------------------------------------------

### **35. How Would You Tune Decoding Parameters and Instructions to Make LLM Completions Concise and Reliable? \[Scenario Based\]**

**Answer:**

Suppose I am building a chatbot, but the LLM generates very long answers
even when the user asks a simple question.

In this situation, I would optimize both the decoding parameters and the
prompt instructions.

**1. Adjust Temperature**

I would start with a low temperature for more consistent and predictable
responses.

**2. Configure Top-P or Top-K**

If the model API supports these settings, I would tune them based on the
required level of response variation.

**3. Set Maximum Output Tokens**

I would configure a suitable output-token limit so the model does not
generate unnecessarily long responses.

**4. Improve the System Prompt**

I would provide clear instructions such as:

"Answer in simple English. Keep the response concise. Use bullet points
only when required. Do not include unnecessary explanations. If the
information is not available in the provided context, say so."

**5. Test the Output**

I would test different settings using real user questions and compare
response length, correctness, and consistency.

**Real-world use:** In a customer support chatbot, these changes can
help generate short, relevant, and consistent answers while reducing
output-token consumption.

**Important:** Temperature and decoding settings do not guarantee
factual correctness. We still need reliable context and proper
evaluation.

------------------------------------------------------------------------

### **36. How Would You Fit the Maximum Number of Consecutive Retrieved Chunks Within an LLM Context Budget? \[Scenario Based\] \[VERY IMPORTANT\]**

**Answer:**

Suppose we retrieve multiple document chunks from a vector database, but
the LLM has a limited context window.

Our goal is to fit the maximum number of consecutive chunks without
exceeding the available token budget.

First, I would calculate the token budget available for document chunks.

**Available Chunk Budget = Total Context Limit − System Prompt Tokens −
User Query Tokens − Conversation History Tokens − Reserved Output Tokens
− Safety Margin**

Now, suppose the retrieved chunks have the following token counts:

  Chunk       Tokens
  --------- --------
  Chunk 1        100
  Chunk 2        200
  Chunk 3        150
  Chunk 4        300
  Chunk 5        100

Suppose our available chunk budget is 500 tokens.

We need to find the maximum number of consecutive chunks whose combined
token count does not exceed 500.

For example:

-   Chunk 1 + Chunk 2 = 300 tokens.
-   Chunk 1 + Chunk 2 + Chunk 3 = 450 tokens.
-   Adding Chunk 4 would exceed the budget.

So, the first three consecutive chunks fit within the budget.

**Solution Approach:**

I would use a sliding-window or two-pointer approach.

1.  Start with an empty window.
2.  Add chunks one by one.
3.  Maintain the total token count.
4.  If the total exceeds the budget, move the starting pointer forward
    and remove earlier chunks.
5.  Track the maximum number of consecutive chunks that fit.

For efficient implementation, we can maintain a running sum of token
counts.

**Real-world use:** This approach is useful in RAG systems when we need
to select a continuous section of a document while respecting the LLM's
context window.

------------------------------------------------------------------------

### **37. Can You Describe a Testing Strategy for LLM Applications?**

**Answer:**

Testing an LLM application is different from testing a normal backend
application because LLM responses can vary even when the input is the
same.

So, I would test the application at multiple levels.

**1. Unit Testing**

I would test individual components such as:

-   Prompt construction.
-   Document chunking.
-   Token counting.
-   Embedding generation.
-   Retrieval filters.
-   Tool argument validation.

**2. Retrieval Testing**

I would prepare sample questions with known relevant documents and
verify whether the correct chunks are retrieved.

**3. LLM Response Testing**

I would check whether the generated answers are:

-   Relevant to the question.
-   Factually supported by the context.
-   Clear and understandable.
-   In the required format.

**4. End-to-End Testing**

I would test the complete flow from user input to retrieval, LLM
generation, and final response.

**5. Security Testing**

I would test prompt injection, unauthorized data access, sensitive
information leakage, and unsafe tool execution.

**6. Performance Testing**

I would measure response time, throughput, token consumption, API
errors, and cost.

**Real-world use:** For a production RAG chatbot, I would maintain a set
of test questions and expected source documents. Whenever we change the
model, prompt, or retrieval settings, I would run the evaluation again
to check whether quality has improved or decreased.

------------------------------------------------------------------------

### **38. What Types of Prompting Have You Used in Generative AI Applications?**

**Answer:**

There are different prompting techniques that we can use depending on
the task and the expected output.

Some common techniques are:

**1. Zero-Shot Prompting**

We give the model an instruction without providing any examples.

Example:

"Classify this customer message as positive or negative."

**2. One-Shot Prompting**

We provide one example to show the model what kind of response we
expect.

**3. Few-Shot Prompting**

We provide multiple examples so the model can understand the expected
pattern.

**4. Role Prompting**

We define a role for the model.

Example:

"You are an experienced Java technical interviewer."

**5. Structured Output Prompting**

We ask the model to return the response in a specific format, such as
JSON.

**6. Context-Grounded Prompting**

We provide relevant documents or retrieved chunks and instruct the model
to answer using that information.

**7. Step-Based Prompting**

We break a complex task into smaller stages so the model can handle it
in a structured way.

**Real-world use:** In an AI interview application, I can use role
prompting to define the interviewer, structured output prompting for
evaluation results, and context-grounded prompting to retrieve candidate
or job-related information.

------------------------------------------------------------------------

### **39. Why Do LLMs Hallucinate, and How Can Hallucinations Be Reduced?**

**Answer:**

Hallucination happens when an LLM generates information that sounds
correct but is actually incorrect, misleading, or unsupported.

LLMs are trained to predict the next token based on patterns learned
from their training data. They do not automatically verify every
statement against a trusted source.

**Why do LLMs hallucinate?**

1.  The model may not have the required information.
2.  The prompt may be unclear or ambiguous.
3.  The training data may contain incorrect or outdated information.
4.  The model may receive irrelevant context.
5.  The model may generate an answer even when it does not know the
    correct answer.

**How can we reduce hallucinations?**

-   Use RAG to provide relevant and trusted information.
-   Improve document chunking and retrieval quality.
-   Apply reranking to select relevant context.
-   Instruct the model not to invent information.
-   Ask the model to provide source references.
-   Allow the model to say when information is unavailable.
-   Validate important answers using deterministic checks or human
    review.

**Example:**

Suppose a user asks about a company's leave policy.

Instead of allowing the LLM to answer from its general training
knowledge, we retrieve the company's actual leave policy and provide it
as context.

**Real-world use:** In an enterprise chatbot, I would combine RAG,
reranking, grounded prompts, and source citations to reduce
hallucinations.

**Important:** These techniques reduce hallucinations but cannot
guarantee that they will never happen.

------------------------------------------------------------------------

### **40. What Happens When an LLM's Temperature Is Set to 1?**

**Answer:**

When the temperature is set to 1, the model generally uses its baseline
sampling distribution without additional sharpening or flattening caused
by a different temperature value.

It does not mean that the model becomes completely random or that every
possible token has an equal probability.

The model still considers the probability of possible next tokens and
selects from them according to the sampling process.

**Example:**

Suppose the model is generating a sentence.

A token with a higher probability is still more likely to be selected
than a token with a very low probability, but other decoding settings
can also affect the selection.

**Remember:**

-   Temperature below 1 generally makes output more predictable.
-   Temperature equal to 1 uses the baseline distribution.
-   Temperature above 1 generally increases variation.

**Real-world use:** I would test temperature 1 for general generation,
but for factual chatbot responses or interview evaluation, I would
usually start with a lower value and evaluate the results.

------------------------------------------------------------------------

### **41. How Would You Implement the numberOfTokens Function? \[Scenario Based\]**

**Answer:**

If I have to implement a numberOfTokens function, I would first identify
which LLM model is being used because every model may have a different
tokenizer.

I would not simply count words or characters because one word can be
represented by multiple tokens.

**Implementation Approach:**

1.  Accept the input text.
2.  Load or access the tokenizer associated with the target model.
3.  Convert the text into token IDs.
4.  Count the token IDs.
5.  Return the total count.

**Example using Python:**

``` python
import tiktoken

def numberOfTokens(text):
    encoding = tiktoken.encoding_for_model("gpt-4o")
    tokens = encoding.encode(text)
    return len(tokens)

print(numberOfTokens("I am learning Generative AI"))
```

The tokenizer should match the target model. For other providers or
models, I would use their corresponding tokenizer or official
token-counting method.

**Real-world use:** I would use this function before making an LLM API
call to estimate token usage and check whether the input fits within the
context window.

------------------------------------------------------------------------

### **42. How Do You Determine the Number of Tokens Still Active After All Commands Execute?**

**Answer:**

This question is generally related to an authentication-token management
problem where we receive commands such as create, reset, and time
advancement.

First, I would understand the rules of the problem, especially how token
expiration works.

I would maintain:

-   A current time.
-   A map containing token IDs and their expiration times.
-   The fixed token lifetime.

**Example:**

Suppose the token lifetime is 5 units.

At time 1, we create token A.

Its expiration time becomes:

`1 + 5 = 6`

At time 3, we reset token A.

Its new expiration time becomes:

`3 + 5 = 8`

At time 7, token A is still active because its updated expiration time
is 8.

At the final time, we count only those tokens that have not expired
according to the problem's exact boundary rule.

**Solution Approach:**

1.  Process commands in the given order.
2.  Create a token with its expiration time.
3.  When resetting, update its expiration time if it is still valid.
4.  Ignore expired tokens.
5.  After all commands execute, count the tokens that are still active.

For larger inputs, we can use a min-heap to process expiration events
efficiently.

**Important:** We must carefully follow whether a token is considered
expired when current time is equal to its expiration time. That boundary
condition depends on the problem statement.

------------------------------------------------------------------------

### **43. How Would You Model Authentication Tokens with Fixed Lifetimes and Renewal Through Create and Reset Commands? \[Scenario Based\]**

**Answer:**

I would implement this using a HashMap to maintain each token and its
expiration time.

The main idea is that every token has a fixed lifetime, and its
expiration time is calculated using the current time.

Suppose the token lifetime is 5 units.

**1. Create Command**

When a token is created at time 2:

`Expiration Time = Current Time + Time To Live`

So:

`2 + 5 = 7`

We store token ID and expiration time in the HashMap.

**2. Reset Command**

If the token is still valid and we reset it at time 4:

`New Expiration Time = 4 + 5 = 9`

We update the expiration time in the map.

**3. Expiration Check**

Whenever we process a command or count active tokens, we check whether
the token has expired.

**4. Final Count**

After all commands execute, we count only the tokens that are still
valid.

**Data Structure:**

A HashMap is sufficient for a straightforward implementation.

For larger workloads, we can use a min-heap to efficiently process
expiration events. If a token is reset, we need to ignore its old
expiration event and use the latest expiration time.

**Real-world use:** This type of logic is useful for understanding token
expiration and renewal. In a real authentication system, we would also
consider secure token storage, refresh-token rotation, revocation, and
server-side validation.

------------------------------------------------------------------------

### **44. How Would You Reduce Latency in an LLM-Based Application? \[Scenario Based\]**

**Answer:**

Suppose I have developed an LLM-based chatbot, but users are
experiencing a delay of 10 to 15 seconds before receiving an answer.

First, I would identify where the delay is happening instead of randomly
changing the model or prompt.

I would measure the time taken by each stage:

-   API request handling.
-   Embedding generation.
-   Vector database retrieval.
-   Reranking.
-   LLM response generation.
-   Final response delivery.

After identifying the bottleneck, I would optimize it.

**1. Reduce Prompt Size**

Remove unnecessary conversation history and retrieve only relevant
document chunks.

**2. Use a Faster Model**

Use a smaller model for simple tasks and a more capable model only when
required.

**3. Implement Streaming**

Instead of waiting for the complete response, stream tokens to the
frontend as they are generated.

**4. Use Asynchronous Processing**

Run independent I/O operations in parallel wherever possible.

**5. Implement Caching**

Cache repeated queries, embeddings, or retrieval results where
appropriate.

**6. Optimize Retrieval**

Use suitable indexing, metadata filters, and efficient reranking.

**7. Avoid Unnecessary LLM Calls**

Combine steps when appropriate and avoid repeated calls for information
already available.

**Real-world use:** In a voice-based AI interview application, I would
measure the time between the candidate finishing an answer and the AI
starting the next question. Then I would optimize the slowest stage to
reduce the overall response delay.

------------------------------------------------------------------------

### **45. How Do You Approach Integrating a Large Language Model into a Production System with Scalability in Mind?**

**Answer:**

When integrating an LLM into a production system, I focus on making the
application scalable, reliable, secure, and cost-efficient.

I would divide the system into separate components so each part can be
maintained and scaled independently.

**1. API Layer**

I would use a backend framework such as FastAPI to receive user
requests, validate input, and handle authentication.

**2. Orchestration Layer**

This layer manages prompts, conversation history, RAG workflows, and LLM
calls.

**3. Retrieval Layer**

If the application uses RAG, I would use an embedding model and vector
database to retrieve relevant information.

**4. LLM Integration**

I would integrate the required model through an API or a self-hosted
model-serving system.

**5. Asynchronous Processing**

For long-running operations, I would use background workers or queues.
For I/O-bound requests, I would use asynchronous APIs where appropriate.

**6. Reliability**

I would implement:

-   Timeouts.
-   Retry mechanisms with backoff.
-   Rate limiting.
-   Circuit breakers.
-   Fallback models or responses.

**7. Monitoring**

I would monitor request volume, latency, token consumption, API errors,
and cost.

**8. Deployment**

I would containerize the application using Docker and deploy it using an
appropriate cloud or container orchestration platform.

**Real-world use:** In a production AI chatbot, this architecture allows
us to scale API services, retrieval services, and background processing
independently as the number of users increases.

------------------------------------------------------------------------

### **46. How Did You Reduce Hallucinations or Factual Errors in the Generative AI Output? \[Scenario Based\]**

**Answer:**

Suppose I have developed an enterprise chatbot using an LLM, but
sometimes it provides incorrect information even when the answer exists
in our company documents.

I would first investigate whether the problem is happening during
retrieval or during answer generation.

**Step 1: Check Retrieval Quality**

I would verify whether the correct document chunks are being retrieved.

If the relevant information is missing, I would improve chunking,
embeddings, metadata filtering, or search configuration.

**Step 2: Add Re-Ranking**

I would introduce a reranker to sort retrieved chunks based on their
relevance to the user's query.

**Step 3: Improve the Prompt**

I would instruct the LLM to answer only using the supplied context and
clearly state when the information is unavailable.

**Step 4: Add Source References**

I would ask the model to provide citations or source references so users
can verify the information.

**Step 5: Evaluate the Results**

I would prepare a test dataset containing questions and their expected
source documents. Then I would check retrieval quality and answer
correctness after every change.

**Real-world use:** In an enterprise chatbot, combining better
retrieval, reranking, grounded prompting, and evaluation helps reduce
factual errors.

------------------------------------------------------------------------

### **47. How Do You Handle Inputs That Exceed the Context Window Token Limit? \[Scenario Based\]**

**Answer:**

Suppose a user uploads a very large document or continues a conversation
for a long time. Eventually, the total input may exceed the LLM's
context window.

I would first calculate the total token count and compare it with the
model's context limit.

If the input is too large, I would not simply send the complete content
again. I would reduce and prioritize the information.

**For Long Documents:**

-   Extract the document text.
-   Split it into meaningful chunks.
-   Generate embeddings.
-   Store the chunks in a vector database.
-   Retrieve only the relevant chunks using RAG.

**For Long Conversations:**

-   Keep recent messages.
-   Summarize older conversation history.
-   Remove irrelevant messages.
-   Preserve important user requirements and decisions.

**For Token Budgeting:**

I would reserve tokens for the system prompt, user query, retrieved
context, and expected response.

**Real-world use:** In a document-based chatbot, RAG allows users to ask
questions about large PDFs without sending the complete PDF to the LLM
for every query.

------------------------------------------------------------------------

### **48. How Do You Handle Hallucinations in an Enterprise LLM? \[Scenario Based\]**

**Answer:**

In an enterprise application, hallucinations can be a serious problem
because the model may generate incorrect information about company
policies, financial data, or customer records.

I would handle this at multiple levels.

**1. Use Trusted Knowledge Sources**

I would connect the LLM to approved company documents, databases, and
APIs rather than depending only on its pre-trained knowledge.

**2. Implement RAG**

Retrieve relevant information from the enterprise knowledge base and
provide it as context.

**3. Apply Re-Ranking**

Use a reranker to ensure the most relevant document chunks are selected.

**4. Use Grounded Prompts**

Instruct the model to answer only from the provided information and
abstain when evidence is missing.

**5. Add Access Control**

Ensure users can retrieve only the documents and records they are
authorized to access.

**6. Validate Important Answers**

For important business operations, validate the output using backend
rules, database checks, or human approval.

**7. Monitor the System**

Track incorrect answers, unsupported claims, retrieval failures, and
user feedback.

**Real-world use:** In an enterprise HR chatbot, I would ensure that
answers about leave policies, salary policies, and company rules are
based on approved and current documents.

**Important:** RAG and prompt engineering reduce hallucinations, but
they do not guarantee that every answer will be correct.

------------------------------------------------------------------------

### **49. Walk Through the Architecture from the User Request to the Final Response, Including the LLM, Retrieval Layer, Orchestration APIs, Deployment, and Monitoring. \[VERY IMPORTANT\]**

**Answer:**

Let me explain this using an example of an enterprise chatbot where
users can ask questions about company documents.

The architecture contains multiple components, and each component has a
specific responsibility.

**Step 1: User Request**

The user enters a question through the frontend application.

For example:

"What is the company's work-from-home policy?"

**Step 2: API Gateway / Backend**

The request reaches the backend API, for example, a FastAPI service.

The backend performs:

-   Authentication.
-   Authorization.
-   Request validation.
-   Rate limiting.

**Step 3: Orchestration Layer**

The orchestrator manages the complete LLM workflow.

It decides which prompt to use, manages conversation history, and
determines whether retrieval or tool calling is required.

**Step 4: Retrieval Layer**

If RAG is required, the user's question is converted into an embedding.

The vector database searches for relevant document chunks. We can also
apply metadata filters and hybrid search.

**Step 5: Re-Ranking**

The retrieved chunks are passed to a reranker, which sorts them based on
relevance to the user's question.

**Step 6: Context Construction**

The orchestrator selects the most relevant chunks and builds the final
prompt within the available token budget.

**Step 7: LLM Integration**

The prompt is sent to the LLM through an API or a self-hosted model.

The LLM generates an answer based on the user's question and the
retrieved context.

**Step 8: Tool Calling (If Required)**

If the model needs external information or an action, it can request a
tool call.

The backend validates the request, checks permissions, executes the
tool, and sends the result back to the LLM.

**Step 9: Response Validation**

The backend validates the response format, applies safety rules, and
attaches source references where available.

**Step 10: Final Response**

The final answer is returned to the frontend. We can also use streaming
so the user receives the response progressively.

**Step 11: Deployment and Monitoring**

The application can be deployed using Docker and a cloud or container
orchestration platform.

We monitor:

-   API response time.
-   LLM latency.
-   Token consumption.
-   API errors.
-   Retrieval quality.
-   Model cost.
-   User feedback.

**Complete Architecture:**

`Frontend → API Gateway → Orchestrator → Retrieval / Tools → LLM → Response Validation → Frontend`

**Real-world use:** This architecture is suitable for enterprise
chatbots, document question-answering systems, and AI assistants that
need to retrieve information from external sources.

**Important:** Authentication, authorization, and business rules must be
enforced by the backend. We should never rely only on the LLM to enforce
security.

------------------------------------------------------------------------

### **50. How Do You Handle Failures in Final Answer Generation Due to LLM Issues? \[Scenario Based\]**

**Answer:**

Suppose a user sends a question to our chatbot, but the LLM API fails to
generate the final answer.

In this situation, I would implement proper error handling and fallback
mechanisms so the application does not crash or return an incorrect
response.

First, I would identify the type of failure.

Common failures include:

-   LLM API timeout.
-   Rate limit exceeded.
-   Provider service unavailable.
-   Invalid or malformed response.
-   Content-filter or safety rejection.
-   Network connectivity issue.

**1. Retry Mechanism**

For temporary failures, I would retry the request using exponential
backoff and a maximum retry limit.

**2. Fallback Model**

If the primary model is unavailable, we can use a configured fallback
model, provided it is suitable for the task and meets the required
quality and security standards.

**3. Response Validation**

If the model returns malformed JSON or an unexpected format, I would
validate the response and use a controlled retry or repair step where
appropriate.

**4. Graceful Error Message**

If all attempts fail, the application should return a clear message such
as:

"We are unable to generate a response right now. Please try again
later."

**5. Logging and Monitoring**

I would log the error type, request trace ID, model name, and relevant
timing information without unnecessarily storing sensitive user data.

**Real-world use:** In a production chatbot, retry mechanisms, fallback
models, timeouts, circuit breakers, and monitoring help maintain
application reliability even when the LLM provider experiences temporary
issues.

------------------------------------------------------------------------

## Quick Interview Preparation Tips

1.  First, understand the concept instead of memorizing the answer word
    by word.
2.  Start your answer with a simple definition.
3.  Explain the concept in your own words.
4.  Give a practical example wherever possible.
5.  For scenario-based questions, explain the problem, your approach,
    and the solution.
6.  For architecture questions, explain the complete flow step by step.
7.  For production-related questions, always consider latency, cost,
    scalability, security, and error handling.
