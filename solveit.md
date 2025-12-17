# Solve it with code

Notes on the [course][1] from [AnswerAI][2].

## General

- Edit the AI output to be what you want, don't have a conversation with the AI to get there
- If feeding GitHub links to an AI, always use the raw version

## Projects

- Finish and polish dbtbnb (Cortex Analyst Layer)
- AixPert
- GradeMate
- pym2v async and polishment
- AoC

## Creating AI agents from scratch

- An agent is a LLM running tools in a loop to achieve a goal (c.f. [Simon Willison][7])
- Use &`<FUNCTION_NAME>` (or `&[<FUNCTION_NAME_1>, <FUNCTION_NAME_2>]`) to let SolveIt use your python functions as tools
- Use `globals()["<OBJECT_NAME>"]` to get any Python object

## Stuff

- Try [MacWhisper][3]
- Check out [Jina AI Reader][4]
- Read [build to last][5] article
- Try [shellSage][6]


[1]: https://solve.it.com/
[2]: https://www.answer.ai/
[3]: https://goodsnooze.gumroad.com/l/macwhisper
[4]: https://jina.ai/reader
[5]: https://www.fast.ai/posts/2025-10-30-build-to-last.html
[6]: https://ssage.answer.ai/index.html
[7]: https://simonwillison.net/2025/Sep/18/agents/
