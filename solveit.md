# Solve it with code

Notes on the [course][1] from [AnswerAI][2].

## General

- Edit the AI output to be what you want, don't have a conversation with the AI to get there
- If feeding GitHub links to an AI, always use the raw version
- Use the right tool for the job: Solve It for interactive tasks, Copilot/Claude Code/Codex for agentic stuff

## Projects

- Finish and polish dbtbnb (Cortex Analyst Layer)
- AixPert
- GradeMate
- Polish pym2v async methods
- AoC
- Controlling remote PI device
- PaiReader (**P**ython-powered **AI**-assisted **reading**)
- Read *Designing Data-Intensive Applications* and *AI Engineering* with AI

## The Solve It Method

1. Understand the problem: Clearly identify what you are being asked to do
1. Devise a plan: Draw on similar problems; break problem down into sub problems; consider simplifying the problem
1. Execute the plan: Make sure to verify at each step
1. Look back and reflect: Consider alternatives and optimization; extract lessons learned

## SolveIt goodies

- We can export Python code to a gist and use `import_gist()` from `dialoghelper` to import it into another dialog

## Creating AI agents from scratch

- An agent is a LLM running tools in a loop to achieve a goal (c.f. [Simon Willison][7])
- Use &`<FUNCTION_NAME>` (or `&[<FUNCTION_NAME_1>, <FUNCTION_NAME_2>]`) to let SolveIt use your python functions as tools
- Use `globals()["<OBJECT_NAME>"]` to get any Python object
- Use `globals()["<OBJECT_NAME>"] = <OBJECT>` to create any Python object

## Web development the fast way

- Check out [fastdaisy][19]

## Web scraping reloaded

- Check out [Zyte][18]

## Interacting with other computers

- Use the `capture` module from `dialoghelper` share your screen with SolveIt
- Using SSH, [ngrok][21] and subprocess we give SolveIt access to remote machines

## Browser automation

- Try controlling Chromium using [Playwright for Python][22]

## Neat Python tricks

- Use `list(zip(arr[:-1], arr[1:]))` to convert a list of integers (`arr = [1, 2, 3, 4]`) to a list of tuples mimicking a graph: `[(1,2), (2, 3), (3,4)]`

## AI-assisted reading

- Check out the [How to read a book][29] book
- Use Mistral Document AI to transform PDF into Markdown, then read and ask follow-up questions on it with SolveIt
- Alternatively use the `capture` model to share the PDF directly
- Think about creating stacked summaries of a book. Start with summary of the entire book, then create one part part, then one per chapter. If a particular chapter interests you especially, drill down, read it end to end and ask follow-up questions to AI.
- Consider letting the AI write handoff notes, i.e. a summary of the dialogue with the reader for a chapter, and use it as context for the next chapter
- If links to images include #ai, Solve It can see them

## Stuff

- Try [MacWhisper][3]
- Check out [Jina AI Reader][4]
- Read [build to last][5] article
- Try [shellSage][6]
- Read [recursive language model][8] article
- Try [FastHTML][9] and [htmx][10]
- Check out [daisyUI][11]
- Try [marimo][12] in [VScode][13]
- Check out dialog analyzing fastai library (or do it yourself)
- Read up on using (session) [cookies][14] in [FastAPI]
- Consider [keycastr][15] when recording demos
- Check out [fastlite][16] when working with SQLite
- Try [nbdev][17] for a real project
- Check out [Base Coat][20]
- Consider using [Hetzner][23] as a cloud provider and built a terraform module for it
- Check out [MarkDownMerge][24]
- Check out [fastlucide][25]
- Check out [tinygrad][26] and try the [puzzles][27]
- Check out [datalab][28]
- Check out [tldraw][30]
- Check out [Hypermedia Systems][31] book
- Check out [fastanki][32] and review the [rules][33] for learning effectively. The refactor your existing data structure & algorithms, system design and coding question decks


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
[23]: https://www.hetzner.com/
[24]: https://github.com/AnswerDotAI/markdown_merge
[25]: https://github.com/AnswerDotAI/fastlucide/
[26]: https://tinygrad.org/
[27]: https://github.com/obadakhalili/tinygrad-tensor-puzzles?tab=readme-ov-file
[28]: https://www.datalab.to/
[29]: https://archive.org/details/howtoreadabook1972edition/mode/2up
[30]: https://www.tldraw.com/
[31]: https://hypermedia.systems/
[32]: https://answerdotai.github.io/fastanki/
[33]: https://www.supermemo.com/en/blog/twenty-rules-of-formulating-knowledge
