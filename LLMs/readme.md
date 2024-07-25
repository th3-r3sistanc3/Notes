# LLM

## Definition
- Large Language Models (LLMs) are AI algorithms that can process user inputs and create plausible responses by predicting sequences of words.
- Many web LLM attacks rely on a technique known as prompt injection.
- Prompt injection can result in the AI taking actions that fall outside of its intended purpose, such as making incorrect calls to sensitive APIs or returning content that does not correspond to its guidelines.

## Methodology
1. Identify the LLM's inputs, including both direct (such as a prompt) and indirect (such as training data) inputs.
2. Work out what data and APIs the LLM has access to.
3. Probe this new attack surface for vulnerabilities.

## Explotation
  LLMs are often hosted by dedicated third party providers. A website can give third-party LLMs access to its specific functionality by describing local APIs for the LLM to use.
For example, a customer support LLM might have access to APIs that manage users, orders, and stock.

## Mapping LLM API attack surface
The term "excessive agency" refers to a situation in which an LLM has access to APIs that can access sensitive information and can be persuaded to use those APIs unsafely. This enables attackers to push the LLM beyond its intended scope and launch attacks via its APIs.
