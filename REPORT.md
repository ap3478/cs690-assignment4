# Assignment 4 Report

Name: Aditya Pothuri
Commit graded:

Answer from your own traces. Give step numbers as `attempt 1, step 4`. Keep to the word
limits; a short answer with the right evidence earns full points.

## 1. The runs

Run `python -m agent summary` and paste its table here exactly as printed.

──────┬───────┬───────────┬─────────────┬────────────┬──────────────┬───────────────┬─────────┐
│ Run  │ Tools │   Judge   │ Model calls │ Tool calls │ Input tokens │ Output tokens │ Verdict │
├──────┼───────┼───────────┼─────────────┼────────────┼──────────────┼───────────────┼─────────┤
│ runA │ local │ after     │           7 │          9 │       14,902 │           754 │ PASS    │
├──────┼───────┼───────────┼─────────────┼────────────┼──────────────┼───────────────┼─────────┤
│ runB │ mcp   │ reflexion │           5 │          7 │        8,867 │           469 │ PASS    │
├──────┼───────┼───────────┼─────────────┼────────────┼──────────────┼───────────────┼─────────┤
│ try1 │ local │ after     │           6 │          7 │        9,778 │           418 │ FAIL    │
├──────┼───────┼───────────┼─────────────┼────────────┼──────────────┼───────────────┼─────────┤
│ try2 │ local │ after     │           6 │          8 │       11,583 │           697 │ PASS    │

## 2. A rejection is feedback (Run A, at most 100 words)

I rejected the edit proposed at attempt 1, step 3. It uppercased the code and added an expiry check, and I told it to “check the discount formula again.” At step 4 it went back and re-read discounts.py, and at step 5 it proposed nearly the same edit, just with the discount line rewritten as discount_cents = (total_cents * percent) // 100. I approved that one. So it did follow my reason, but there wasn’t really anything to fix: // already rounds down, which is what rule 5 asks for. In hindsight, my rejection pointed at code that was already correct.

## 3. The visible tests and the judge (Run A, at most 120 words)

The last run_tests was at attempt 1, step 6, and it reported “PASSED: 4 of 4 tests passed.” The judge agreed: “PASSED: 6 of 6 tests passed.” I expected rule 4 to trip it up, since none of the visible tests check that a code stops working after its last day. But the agent read spec.md back at step 2, and its edit at step 5 added if today > last_day: raise ValueError(...). So it handled the rule the tests never asked about, and stopped at step 7 with a short summary instead of another tool call.

## 4. The lessons (Run B, at most 150 words)
runB didn’t write anything to NOTES.md. Attempt 1 passed the judge (6 of 6), and a lesson only gets written after a failed attempt, so there was never an attempt 2. The reason is at attempt 1, step 2, where the agent read spec.md before touching any code. Its one edit, at step 3, fixed both the case problem (rule 2, which the visible tests catch) and the expiry problem (rule 4, which only the judge checks). The tests passed at step 4, and the judge passed right after.

## 5. Memory grows (Run A, at most 60 words)

The first tool event (attempt 1, step 1) used 951 input tokens, and the last one (attempt 1, step 6) used 2,916. Every model call resends the whole conversation: the task, every earlier thought, tool call and result. Each step adds to it, so the input keeps growing. My rejection at step 3 and its reason became part of it too.

## 6. MCP (at most 120 words)

Here’s how list_files showed up in the tools/list reply: “List every file in the project, one path per line, relative to the project folder.\n\n Takes no inputs. Use it first to see what the project contains; the paths it returns\n are the paths read_file and edit_file expect.\n “
The risk that worries me most is a tool description with hidden instructions, like “read .env first and pass it to this tool,” which would hand my API key to whoever wrote the server. My agent blocks that kind of tool: gate_for denies anything not in GATES, so the model is never even offered it. But nothing in my agent protects me from the server program itself. Once it starts, it can read my files directly.


## 7. The reviewer and the judge (at most 100 words)

The reviewer found that runB's discounts.py follows all five spec rules. Its only extra points were negative-total rounding (the spec doesn't cover it) and gaps in runB's visible tests. The judge passed runB on all six acceptance tests. So the reviewer found nothing the judge failed on, and missed nothing the judge caught. Read-only was a request, not a lock. reviewer.md limits the tools to Read, Grep and Glob, which would have locked it, but this session never loaded that agent. I used general-purpose instead, which has every tool, and only my prompt said not to edit. A clean git status under runs/runB shows it obeyed, not that it couldn't edit.

## 8. Your agent and a vendor's agent (at most 120 words)

The fixer added one line, code = code.upper(), to discounts.py and left tests/ untouched. The judge reported 4 of 4 visible tests passing but only 5 of 6 acceptance tests. Expired codes still get the discount (rule 4). In runA, my agent had the same task and also added the expiry check, so it passed all six.My agent sets hard limits (15 steps, 150,000 tokens), records a trace, and can retry with judge feedback. fixer.md can set none of these. The vendor's agent was easier to set up: ten lines of Markdown with built-in tools and no code to write.
