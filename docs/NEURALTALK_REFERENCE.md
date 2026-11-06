# NeuralTalk as a sequence-generation reference

[NeuralTalk](https://github.com/karpathy/neuraltalk), created by [Andrej Karpathy](https://github.com/karpathy), is an educational image-captioning project that generates a sentence one token at a time from image context. It is useful background for understanding the generation half of a Retrieval-Augmented Generation system.

## How it connects to this course

NeuralTalk and this Agentic RAG course solve different problems, but they share a useful sequence-generation idea:

1. NeuralTalk encodes image information as context.
2. Its recurrent model predicts the next word from that context and the words already generated.
3. A RAG application retrieves text evidence and supplies that evidence as context to a language model.
4. The language model generates an answer token by token while conditioned on the retrieved evidence and preceding tokens.

This comparison helps explain why retrieval quality matters. The generator can only condition its output on the context it receives, whether that context comes from an image model or a search system.

## Scope and attribution

NeuralTalk is a separate, deprecated Python 2 project for image captioning. It is not an Agentic RAG or LangGraph implementation, is not installed by this course, and is not required to run the application. Refer to its repository for its source code, history, and BSD license notice.

The Agentic RAG implementation in this repository remains under `src/services/agents/`. NeuralTalk is linked here as an external educational code reference; no NeuralTalk source files are copied into this repository.
