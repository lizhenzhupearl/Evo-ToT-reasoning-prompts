
<h1 align="center"> 🌲Evo-ToT-reasoning</h1>

🧪 **Prompt-Driven Hypothesis Evolution Framework for Thermoelectric Materials**

This repository implements a multi-stage prompting system using OpenAI's GPT-4o and reasoning models (like o1 and o3-mini) and a vast range of models (check the model map below) to simulate expert thinking in generating, evaluating, refining, and summarizing hypotheses for thermoelectric materials discovery.
![Prompting mindmap](tot.png)
📌 **Features**

🧠 Tree-of-Thought style evolution across expert agents

🔁 Sequential module prompts simulating scientific workflows

📊 Hypothesis evaluation with scoring and decision tree logic

📦 Exportable results as CSV tables (ZT, feasibility, risk, etc.)

📈 Optional visualizations for tracking hypothesis evolution

🧬 **Prompt Modules**

Each stage in the hypothesis generation pipeline is structured as a named module, simulating a logical thought progression:

Module Name --	Purpose

`Setting the rules for evolutionary tree of thought` -- Defines thinking structure, agents, and evaluation pipeline

`check_evolution` -- Validates improved hypotheses after crossover/mutation

`generating hypothesis with chemical formulas` -- Forces concrete chemical proposal and rule-based reasoning

`novelty evaluation` -- Evaluates whether hypotheses are genuinely novel

`context guidance` -- Provides background research to inspire further refinements

`compare_hypotheses` -- Performs comparative analysis of generated hypotheses

`summarising_hypothesis` -- Formats hypotheses into structured tables

`removing_low_feasibility_hypotheses` -- Filters out weak proposals

`zoom_into_low_temperature_hypotheses` -- Focuses on low-T materials (<600K)

`summarising_all_hypotheses` -- Outputs final comparative table and trend summary

🧠 **How It Works**
1. Sequential Prompt Chaining

```
final_output = run_sequential_prompt_pipeline(
    module_names=[
        "Setting the rules for evolutionary tree of thought",
        "check_evolution",
        ...
        "summarising_all_hypotheses"
    ],
    model_name="gpt-4o"
)
```

Each module prompt is dynamically retrieved and appended to the previous output for rich context evolution.

2. 📂 LLM Response Logging
   
All responses are saved:

As logs in the current folder

Hypothesis tables are extracted and saved as .csv [Examples](examples)

🧬 **Full coverage of LLMs via OpenRouter**

In the model map, we provides the following choices, and you can customise it based on your own needs.
```
    model_map = {
        "4o": "gpt-4o",  # T=0-2 
        "o1": "o1", #T = 1
        "o3-mini": "o3-mini" , # T=1
        "mistral" : "mistralai/mistral-large-2407", #T 0-3.2
        "claude" : "anthropic/claude-3.7-sonnet", #T 0-1
        "gemini" :"google/gemini-pro-1.5",#"google/gemini-2.0-pro-exp-02-05:free",#, "google/gemini-2.5-pro-exp-03-25:free", #T 0-2, topK, topP, slow response
        "deepseek":"deepseek/deepseek-chat-v3-0324", #"deepseek/deepseek-r1:free", # T 0-1.5, slow response
        "llama":"meta-llama/llama-3.1-405b-instruct"
    }
```

🧬 **Diversity and novelty evaluation through existing embeddings**

The model `all-MiniLM-L6-v2` is used to encode each log file from the test models. 
```
pip install --upgrade torch torchvision torchaudio transformers sentence-transformers
```
After installation, the novelty score can be calculated.
```
baseline_embeddings = model.encode(baseline_hypotheses, convert_to_tensor=True)
hypothesis_embeddings = model.encode(hypothesis_log, convert_to_tensor=True)

novelty_score = 1 - util.cos_sim(hypothesis_embedding, baseline_embeddings).mean().item()
```


🧩 Try out our [JupyterNotebook_OpenAI](prompting.ipynb) and [JupyterNotebook_OpenRouter](prompting_openrouter.ipynb)!

📜 License
MIT — feel free to use or build upon this for scientific or educational purposes.
