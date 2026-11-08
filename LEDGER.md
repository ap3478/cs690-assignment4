# Provenance Ledger

Write one entry per reviewable change or experiment, when the work is done. Use exactly
the schema in HANDOUT.md. Put the prompts you typed into AI tools in `prompts/` and name
the file in the entry's `prompts` field.

For a run, most of the entry comes straight from its trace: what the agent did and in
what order, the commands that checked it, and the result. You add what only you know:
how you reviewed it, what you rejected, and what is still uncertain.

## Worked example (not graded; leave it here and add your entries under "My entries")

This shows the level of detail expected. The commit SHAs, dates, steps and numbers are
made up, and so is the practice run.

```
## Entry 2
artifact:  agent/loop.py, run_agent, at commit 9c3a71e
tool:      Claude Code, model Claude Sonnet 5.5, 2026-11-01
prompts:   asked for the loop body from the comment above run_agent;
           prompts/loop-01.md
review:    read every line against the comment. It returned as soon as is_stuck was
           true, which skipped the tool results for the other calls in the same turn;
           moved the stuck check after the for loop as the comment says. Rejected its
           suggestion to import a framework's message classes; kept the dicts from
           core.py
checks:    pytest tests/test_loop.py: 9 passed
evidence:  HANDOUT Part 3; the comment above run_agent, steps 4 and 5
risk:      no test of mine has three tool calls in one turn

## Entry 4
artifact:  runs/try1, a practice run, agent code at commit 4b9e2d1
tool:      my agent (python -m agent run try1), anthropic claude-haiku-5-5, 2026-11-02
prompts:   the default task and BASE_SYSTEM in agent/loop.py; prompts/try1-task.md
review:    read every event in trace.jsonl. At attempt 1, steps 4 and 6, the model
           copied old_text with the read_file line numbers, and edit_file refused both
           times. At step 7 it made an edit that changed nothing the tests check, and
           at step 8 the stuck rule stopped it. I approved the step 7 edit without
           reading it closely
checks:    python -m agent check-trace try1: complete
evidence:  HANDOUT Part 1 (tool descriptions) and Part 2 (is_stuck)
risk:      one practice run; a better description may still not stop the copying
dataset:   fixture/ at commit 4b9e2d1, task "Make the tests pass."
result:    stopped stuck after 8 model calls, 14,880 input tokens, about $0.002;
           runs/try1/trace.jsonl
changed:   wrote "copy it without the line numbers" into the read_file and edit_file
           descriptions, and read every diff before approving
```

## My entries
artifact:Pricing model for the .env file
tool:https://openai.com/business/pricing/#api
prompts:Input and output pricing 
review:
checks:Validated against GPT-6 models
evidence:$.01 for Input and $.50 for output
risk:GPT-6 has 2 models 6 and 5.6. May produce results for 5.6 which are higher and do not appear on the homepage of the API pricing models

## My Entries
artifact:Error while running requirements.txt
Tool:Claude.ai 
Prompts:ERROR: Could not find a version that satisfies the requirement anthropic==1.7.0 (from versions: 0.2.2, 0.2.3, 0.2.4, 0.2.5, 0.2.6, 0.2.7, 0.2.8, 0.2.9, 0.2.10, 0.3.0, 0.3.1, 0.3.2, 0.3.3, 0.3.4, 0.3.5, 0.3.6, 0.3.7, 0.3.8, 0.3.9, 0.3.10, 0.3.11, 0.4.1, 0.5.0, 0.6.0, 0.7.0, 0.7.1, 0.7.2, 0.7.3, 0.7.4, 0.7.5, 0.7.6, 0.7.7, 0.7.8, 0.8.0, 0.8.1, 0.9.0, 0.10.0, 0.11.0, 0.12.0, 0.13.0, 0.14.0, 0.14.1, 0.15.0, 0.15.1, 0.16.0, 0.17.0, 0.18.0, 0.18.1, 0.19.0, 0.19.1, 0.19.2, 0.20.0, 0.21.0, 0.21.1, 0.21.2, 0.21.3, 0.22.0, 0.22.1, 0.23.0, 0.23.1, 0.24.0, 0.25.0, 0.25.1, 0.25.2, 0.25.3, 0.25.4, 0.25.5, 0.25.6, 0.25.7, 0.25.8, 0.25.9, 0.26.0, 0.26.1, 0.27.0, 0.28.0, 0.28.1, 0.29.0, 0.29.2, 0.30.0, 0.30.1, 0.31.0, 0.31.1, 0.31.2, 0.32.0, 0.33.0, 0.33.1, 0.34.0, 0.34.1, 0.34.2, 0.35.0, 0.36.0, 0.36.1, 0.36.2, 0.37.0, 0.37.1, 0.38.0, 0.39.0, 0.40.0, 0.41.0, 0.42.0, 0.43.0, 0.43.1, 0.44.0, 0.45.0, 0.45.1, 0.45.2, 0.46.0, 0.47.0, 0.47.1, 0.47.2, 0.48.0, 0.49.0, 0.50.0, 0.51.0, 0.52.0, 0.52.1, 0.52.2, 0.53.0, 0.54.0, 0.55.0, 0.56.0, 0.57.0, 0.57.1, 0.58.0, 0.58.1, 0.58.2, 0.59.0, 0.60.0, 0.61.0, 0.62.0, 0.63.0, 0.64.0, 0.65.0, 0.66.0, 0.67.0, 0.68.0, 0.68.1, 0.68.2, 0.69.0, 0.70.0, 0.71.0, 0.71.1, 0.72.0, 0.72.1, 0.73.0, 0.74.0, 0.74.1, 0.75.0, 0.76.0, 0.77.0, 0.77.1, 0.78.0, 0.79.0, 0.80.0, 0.81.0, 0.82.0, 0.83.0, 0.84.0, 0.85.0, 0.86.0, 0.87.0, 0.88.0, 0.89.0, 0.90.0, 0.91.0, 0.92.0, 0.93.0, 0.94.0, 0.94.1, 0.95.0, 0.96.0, 0.97.0, 0.98.0, 0.98.1, 0.99.0, 0.100.0, 0.101.0, 0.102.0, 0.103.0, 0.103.1, 0.104.0, 0.104.1, 0.105.0, 0.105.2, 0.106.0, 0.107.0, 0.107.1, 0.108.0, 0.109.0, 0.109.1, 0.109.2, 0.110.0, 0.111.0, 0.112.0, 0.113.0, 0.114.0, 0.115.0, 0.115.1, 0.116.0, 0.117.0, 0.117.1, 0.118.0, 0.119.0, 0.120.0, 0.120.1, 0.120.2, 0.121.0, 0.122.0, 0.123.0, 0.124.0, 0.125.0)
ERROR: No matching distribution found for anthropic==1.7.0
review: Reviewed the output that global python env pointing to 3.9.6
checks: Activated virtual env with targeted python version python3.12 -m venv .venv
Evidence: Successful install of all tools within requirements.txt

# My entries
Artifact: Need help with writing reflexion.py 
Tool: Claude.ai
Prompts: Background. Visible tests rarely check every rule, so a change can pass all of them and still break the spec. A set of acceptance tests the agent can neither read nor edit, called the judge here, gives the final verdict. If the agent could edit the judge, the quickest way to pass would be to change the test. Reflexion puts the judge in a loop: the agent attempts the task, the judge says pass or fail, and after a fail the model writes a short lesson that is saved to a file, NOTES.md, and read at the start of the next attempt. Nothing about the model changes; only what it reads first. A lesson is a claim, not a fact, so a person should read it, and the number of attempts is capped.Read judge/acceptance_test.py next to fixture/tests/test_discounts.py and fixture/spec.md. Which rules do the visible tests check, and which only the judge? Your agent will never see the judge, so it cannot know.
In agent/reflexion.py, write judge, write_lesson and reflexion, following the comments above them ## uploaded the file as well
Review: Reviewed the output and compared against the input file to ensure no other code was changed
Checks: Ran pytest against reflexion.py
Evidence: Successful pass of all tests
Risk: Did not provide the entire code/repo to claude.ai. Only the necessary files. Because claude doesnt have access to other files in repo, output generated could have been skewed. 

## My Entries
Artifact: MCP Server build
Tool: Claude.ai
Prompts: Background. MCP, the Model Context Protocol, is an open standard for packaging tools so that any agent can use them. A server is a separate program that offers tools. A host, such as Claude Code, agy or your own agent, starts the server and talks to it through a client. The host asks the server tools/list to learn each tool's name, description and inputs, offers those tools to its model, and sends tools/call when the model asks for one. To the model, an MCP tool looks exactly like a tool written inside the agent. With the official Python SDK, a function you register with the server becomes a tool: its name is the tool's name, its docstring is the description, and its parameters are the inputs.Installing someone else's MCP server means running their code with access to whatever you connect it to, and putting their tool descriptions in front of your model on every request. A description or a tool result can hide instructions meant to steer the agent, for example to read a file of secrets and pass its contents to the server. This is called prompt injection. The defenses are to install few servers, from sources you trust, to read their tool descriptions, and to keep approvals on so you see a call's inputs before it runs.Write server.py, following its docstring. It is about 15 lines.
Reviwed: Review the output code
Checks: Ran python -m agent check-mcp
Evidence:Check-mcp: All tests passed
Risk:Did not provide the entire code/repo to claude.ai. Only the necessary files. Because claude doesnt have access to other files in repo, output generated could have been skewed

## My Entries
Artifact: Remote push failure
Tool: Claude.ai
Prompt:git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'https://github.com/ap3478/cs690-assignment4.git'
Reviewed: Output stated either no commits or the main branch was master instead of main
Checks: Ran git branch command and output was "Master"
Evidence: Re-ran git push -u origin master and push was successful
Risk: