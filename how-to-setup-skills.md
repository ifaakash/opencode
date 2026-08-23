# How to Set Up and Use Agent Skills

## What is a skill?
A skill is just a markdown file that contains specific rules, templates, or workflows for your AI agent (like Claude or Gemini) to follow. It helps the agent do a specific task the exact way you want it done.

## Option 1: Local Skills (For a single project)
Use this method if you only need the skill for one specific folder or project.

1. Create a markdown file in your project folder. Give it a clear name, like `SKILL-recap.md`.
2. Write your instructions inside. Tell the agent exactly what to do, what to avoid, and provide a template if you want a specific output format.
3. To use it, just tell the agent: "Please do this task using the rules in SKILL-recap.md". The agent will read the file and follow your rules.

## Option 2: Global Skills (Available Everywhere)
Use this method if you want the agent to know the skill no matter what folder you are working in.

1. Open your global agent configuration folder. For many local setups, this is located at `~/.claude/skills/` or `~/.agents/skills/`.
2. Create a new folder for your specific skill. For example: `~/.claude/skills/topic-recap/`.
3. Inside that new folder, create a file named `SKILL.md` and write your instructions there.
4. Some setups require you to register the skill in a main configuration file, pointing to the new folder. 
5. To use it, simply say: "Use the topic-recap skill". The agent will automatically load the rules from your global folder.

## Tips for Writing Good Skills
* **Keep it simple:** Write in plain, human readable English.
* **Be direct:** Tell the agent exactly what you want step by step.
* **Provide templates:** If you want a specific document structure, write out the exact headers and layout you expect.
* **Define triggers:** Include a section that tells the agent when it should use the skill. For example: "Use this skill when the user asks for a summary."