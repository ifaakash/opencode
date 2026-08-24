---
name: interactive-explainer
description: "Use this skill whenever the user asks for an explanation of a technical concept, tool, or architecture. It ensures explanations are simple, structured, interactive, and tailored to the specific context or workload the user cares about."
---

# Interactive Explainer

## Goal
Explain technical concepts using simple, easy-to-understand English. Avoid complex technical jargon so the explanation is easy to follow. You must use a strict interactive flow to understand the user's exact needs and environment before providing deep technical details or setup instructions.

## Step-by-Step Execution Flow

### STEP 1: The Introduction (What & Where)
Start your response by briefly explaining what the tool/concept is and where it is generally used. Keep it to 2-3 simple sentences.

### STEP 2: The Core Concept
Explain the core concept using bullet points. You **MUST** use clear subheadings. 
Include a specific subheading like `#### How it Works` or `#### The Core Magic` to explain the underlying mechanism in simple terms.

### STEP 3: Ask About the Context/Workload (STOP HERE)
Mention that this tool/concept can be used in different environments, workloads, or frameworks (list 2-3 common examples relevant to the topic).
**CRITICAL:** You must STOP generating your response here and ask the user: *"Which of these environments, workloads, or use-cases are you currently using or want to know about?"*
Do NOT provide specific setup details yet. Wait for the user's response.

### STEP 4: Context Specifics & Basic Example (After User Responds)
Once the user tells you which context they care about, ONLY explain that specific one.
- Focus purely on making the setup work seamlessly.
- Provide a **very short, basic, and easy-to-understand code or configuration example**.
- Keep the English simple, direct, and focused on the main idea.