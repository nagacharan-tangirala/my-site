---
layout: post
title: Vendor lock-in Antipattern
date: 2025-02-06 14:25:00
description: Vendor lock-in Antipattern
tags: java-snippets course anti-patterns
categories: code
--- 

It is not uncommon to depend on a single vendor for a major feature.
There are countless LLM-wrapper companies that add their own layer on top of the LLM models.
Often, the changes in the LLM API break the tools built around it.
The design should be isolated from the vendor's API.
Otherwise, the software development life cycle is dependent on the vendor's own release cycles.
Further, the tight coupling will also prevent from changing the vendor in future.
This is called vendor lock-in antipattern.

A common solution is to isolate the interactions through a middleware.
This separates the application from the vendor's code, allowing for independent growth of the tool's feature set.
A good pattern to address this is the adapter pattern.
Only the adapter talks to the vendor, while the rest of the application is independent of the vendor's code.
Any change in the vendor's code should ideally require only changes in the adapter class.

```java
class SupportApp {

    ...
    private String apiKey;
    ...

    ...
    String promptChatGPT(String prompt) {
        // Send the prompt to ChatGPT, get the response, parse it to a String and return it.
    }
    ...
}
```

This `SupportApp` is dependent on the `ChatGPT` to provide the prompting capabilities.
When a new capable model is released, the `SupportApp` needs too much refactoring.
Instead, we can use adapter pattern and decouple the application from LLM.

```java
class SupportApp {

    ...
    private LLMAdapter llmAdapter;
    ...

    ...
    String promptLLM(String prompt) {
        return this.llmAdapter.promptLLM(prompt);
    }
    ...
}

class LLMAdapter {

    ...
    private String apiKey;
    private LLMPreferred llmPreferred;
    ...

    LLMAdapter(LLMPreferred llmChoice, String apiKey) {
        this.llmPreferred = llmChoice;
        this.apiKey = apiKey;
    }

    ...
    String promptPreferredLLM(String prompt) {
        // Send the prompt to the preferred LLM, get the response, parse it to a String and return it.
    }
    ...
}
```

The way `LLMAdapter` is implemented will allow for plugging in any preferred LLM model.
There is no dependency on a single vendor to provide the necessary service.
When a better model like `DeepSeek` comes along, then it is easier to change the code.
Moreover, the `SupportApp` is not affected when the vendor is changed.
Only change happens in the `LLMAdapter` class.
