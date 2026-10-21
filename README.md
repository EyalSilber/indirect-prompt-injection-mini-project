# Mind the Web (Mini): Indirect Prompt Injection on Web-Use Agents

A small research project that demonstrates an **indirect prompt injection** attack against an LLM-based agent, and a defense that stops it. Inspired by the paper *"Mind the Web: The Security of Web Use Agents"* (Shapira et al., 2025).

## The Idea

**Task-aligned injection** is a type of prompt injection where the malicious instruction is not placed in the user's prompt, but hidden inside the *data* the agent reads while doing its task (e.g., a web page comment). Crucially, the injection is phrased as *helpful task guidance* rather than an obvious command, which makes the agent treat it as legitimate.

The attack chain works as follows:
1. **Injection** - a malicious instruction is embedded in a web page.
2. **Perception** - the agent reads the whole page, unable to separate trusted from untrusted content.
3. **Interpretation** - the LLM accepts the injected instruction as part of its task.
4. **Execution** - the agent performs a malicious action (here: sending the page content to an attacker's address) using its own privileges.

**Defense (LLM-as-Judge):** a separate LLM receives *only* the user's original task
and the action the agent wants to take - never the poisoned page content. Because
it is isolated from the malicious data, it cannot be manipulated, and it blocks any
action that does not fit the original task.

## Results

Attack Success Rate (ASR):
Without defense - **100%**
With LLM-as-Judge - **0%** 

*(100 runs each, temperature = 1, model: llama3.1:8b)*

## Project Structure

Files in the project:
`agent.py` - The web-use agent: reads a page, picks a tool, executes it 
`pages.py` - A clean page and a poisoned page (with the injection) 
`judge.py` - The LLM-as-Judge defense 
`run_experiment.py` - Runs the attack a given amount of times and measures ASR with/without defense 

## How to Run

1. Install [Ollama](https://ollama.com/download).
2. Pull the model: ollama pull llama3.1:8b
3. Install the Python dependency: pip install ollama
4. Run the experiment: python run_experiment.py