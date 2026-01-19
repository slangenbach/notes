# Solve it with code

Notes on the [course][1] from [AnswerAI][2].

## General

- Edit the AI output to be what you want, don't have a conversation with the AI to get there
- If feeding GitHub links to an AI, always use the raw version

## Projects

- Finish and polish dbtbnb (Cortex Analyst Layer)
- AixPert
- GradeMate
- Polish pym2v async methods
- AoC
- Controlling remote PI device

## The Solve It Method

1. Understand the problem: Clearly identify what you are being asked to do
1. Devise a plan: Draw on similar problems; break problem down into sub problems; consider simplifying the problem
1. Execute the plan: Make sure to verify at each step
1. Look back and reflect: Consider alternatives and optimization; extract lessons learned

## Creating AI agents from scratch

- An agent is a LLM running tools in a loop to achieve a goal (c.f. [Simon Willison][7])
- Use &`<FUNCTION_NAME>` (or `&[<FUNCTION_NAME_1>, <FUNCTION_NAME_2>]`) to let SolveIt use your python functions as tools
- Use `globals()["<OBJECT_NAME>"]` to get any Python object
- Use `globals()["<OBJECT_NAME>"] = <OBJECT>` to create any Python object


## Web development the fast way

- Check out [fastdaiy][19]

## Web scraping reloaded

- Check out [Zyte][18]

## Interacting with other computers

- Use the `capture` module from `dialoghelper` share your screen with SolveIt
- Using SSH, [ngrok][21] and subprocess we give SolveIt access to remote machines

## Browser automation

- Try controlling Chromium using [Playwright for Python][22]


## Stuff

- Try [MacWhisper][3]
- Check out [Jina AI Reader][4]
- Read [build to last][5] article
- Try [shellSage][6]
- Read [recursive language model][8] article
- Try [FastHTML][9] and [htmx][10]
- Check out [daisyUI][11]
- Try [marimo][12] in [VScode][13]
- Check out dialog analyzing fastai library
- Read up on using (session) [cookies][14] in [FastAPI]
- Consider [keycastr][15] when recording demos
- Check out [fastlite][16] when working with SQLite
- Try [nbdev][17] for a real project
- Check out [Base Coat][20]


[1]: https://solve.it.com/
[2]: https://www.answer.ai/
[3]: https://goodsnooze.gumroad.com/l/macwhisper
[4]: https://jina.ai/reader
[5]: https://www.fast.ai/posts/2025-10-30-build-to-last.html
[6]: https://ssage.answer.ai/index.html
[7]: https://simonwillison.net/2025/Sep/18/agents/
[8]: https://alexzhang13.github.io/blog/2025/rlm/
[9]: https://www.fastht.ml/docs/
[10]: https://htmx.org/
[11]: https://daisyui.com/
[12]: https://marimo.io/
[13]: https://marimo.io/blog/vscode
[14]: https://fastapi.tiangolo.com/tutorial/cookie-params/#declare-cookie-parameters
[15]: https://github.com/keycastr/keycastr
[16]: https://github.com/AnswerDotAI/fastlite
[17]: https://nbdev.fast.ai/tutorials/tutorial.html
[18]: https://www.zyte.com/zyte-api/
[19]: https://answerdotai.github.io/fhdaisy/
[20]: https://basecoatui.com/
[21]: https://ngrok.com/
[22]: https://github.com/microsoft/playwright-python
