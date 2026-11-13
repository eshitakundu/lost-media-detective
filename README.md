# Lost Media Detective

Someone remembers "an eerie rabbit cartoon I saw around 2012" but not its title, creator or URL. This project turns a vague memory like that into search queries, searches permitted sources, ranks the candidates against the clues, and shows the evidence links. When the evidence isn't there, it says "not found yet" instead of guessing.

**Status:** work in progress. The plan and the evaluation design are done; the code is being built phase by phase.

## How it will work

```text
memory → extract clues → plan searches → search → collect evidence → rank
                              ↑                                        │
                              └──────── retry if justified ← verify ←──┘
                                                               │
                                        answer · abstain · ask a follow-up
```

Hard stops on search rounds, tool calls and time, so the loop always ends.

## Build plan

1. A keyword-search baseline with no LLM, so the agent has something to beat
2. Structured clue extraction (Pydantic schema, offline tests with a fake model)
3. Live search through Tavily, plus fixtures for repeatable tests
4. Rule-based ranking that labels each clue as matched, conflicting or unknown, and abstains when unsure
5. A LangGraph workflow with a retry budget
6. LangSmith tracing
7. An eval set from solved r/tipofmytongue threads (titles removed, including unanswerable cases), measuring hit@5 and reciprocal rank against the baseline

## Stack

Python 3.12 · uv · LangChain · LangGraph · LangSmith · OpenRouter · Pydantic · Tavily

## Not in scope

Recovering or downloading media nobody preserved, scraping restricted sites, or posting to communities.
