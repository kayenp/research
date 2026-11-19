# Current sources rewrite Claude research habits

Good online research with Claude in November 2026 has three parts: the correct tool, a clear request, and a check of each important claim. Current Anthropic sources replace most prompt tricks of 2024 and 2025 with settings: the `effort` level, domain filters and the `/deep-research` command. The check is necessary because a cited link that works does not show that the page supports the claim. In a May 2026 benchmark of five Claude 4.5 and 4.6 models, **97 to 99 percent of cited links worked** ([Cited but Not Verified](https://arxiv.org/html/2605.06635v1); independent, page opened). But the cited page **supported the stated fact for only 52 to 77 percent of citations**. No independent study tested the current models or the claude.ai Research product, so this report labels each fact with its trust level.

## Each citation carries a source type and a trust level

The researchers retrieved all pages on 19 November 2026. Anthropic documentation pages show no publication date, so this report gives the retrieval date for them. Each citation names a source type and a trust level.

The source type is one of three. **Vendor** means a statement of a company about its own product. **Independent** means a measurement by a party that does not sell the tested model. **Opinion** means a view with no controlled measurement. The label "practitioner report" marks an opinion that describes the author's own test. The trust level tells how the researcher got the fact.

| Trust level | Meaning |
|---|---|
| raw read | The fetch returned the page text. The researcher read the fact directly. |
| model summary | A small model summarised the page for the researcher. The wording is a paraphrase. |
| page opened | A researcher opened the page. The notes do not say if a model summarised the text. |
| search only | The fact came from a search-result summary. Nobody opened the page. |

A fact with the level "search only" is not a confirmed fact. Read the page before you use such a fact in your own work. Text in quotation marks is the exact text of the source, as the research notes recorded it. Where this report draws a conclusion that no source states, the text says "inference".

## Select the tool by tool-call count, data location and cost

The three products do the same basic work, but each gives different controls. The claude.ai app gives a hosted pipeline that needs no setup. Claude Code gives files, scripts and a model choice for each agent. The API gives a setting for each search and each fetch. No source ran the same question through claude.ai Research and through Claude Code `/deep-research`. Thus no source shows which of the two gives the better report.

| | claude.ai | Claude Code | Claude API |
|---|---|---|---|
| Quick lookup | Normal chat. Claude searches when it helps. | WebSearch, then WebFetch | Web search tool |
| Deep run | Research (`/deep-research`) | Bundled `/deep-research` workflow | Caller's own loop, the Agent SDK, or Managed Agents |
| Model | Opus 5.5, with Opus 4.8 as fallback | The session model, unless the user assigns one | Caller's choice |
| Stated limits | Plan usage only. No number for one run. | 200 searches in one interactive session. 100,000 characters in one fetch. | `max_uses`. $10 for 1,000 searches, plus tokens. |
| Private data | Connectors. Not on mobile. No local MCP servers. | Local files and MCP servers | Caller's own tools |
| Citations | "Every response includes citations" | The workflow report cites each claim | Search: always. Fetch: only on request. |

The table uses these sources: ([Enable and use web search](https://support.claude.com/en/articles/10684626-enable-and-use-web-search); vendor, model summary), ([Use research on Claude](https://support.claude.com/en/articles/11088861-use-research-on-claude); vendor, page opened), ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read), ([Dynamic workflows](https://code.claude.com/docs/en/workflows); vendor, raw read), ([Web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool); vendor, raw read) and ([Web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool); vendor, raw read).

### claude.ai: Claude decides when to search, and Research is a command

For the new claude.ai experience, the help page says: "There's no web search toggle. Claude searches the web when it helps." ([Claude Cowork and chat are one Claude](https://support.claude.com/en/articles/16761823-claude-cowork-and-chat-are-one-claude); vendor, model summary). The rollout is gradual, and an account on the classic interface keeps the toggle under the "+" button. Web search works on all plans, free accounts included ([Enable and use web search](https://support.claude.com/en/articles/10684626-enable-and-use-web-search); vendor, model summary). Research works only on paid plans: Pro, Max, Ultra, Team and Enterprise ([Use research on Claude](https://support.claude.com/en/articles/11088861-use-research-on-claude); vendor, page opened). The same page says: "Research runs on Claude Opus 5.5 and falls back to Claude Opus 4.8 when needed." The model that the user selects for the chat does not change this.

Anthropic's help article of 2 June 2026 gives a rule that counts tool calls ([When should I use web search, extended thinking, and research?](https://support.claude.com/en/articles/11095361-when-should-i-use-web-search-extended-thinking-and-research); vendor, model summary). **Web search fits one or two tool calls, and Research fits five or more.** The Research help page gives no run time, no source count and no numeric cap for one run ([Use research on Claude](https://support.claude.com/en/articles/11088861-use-research-on-claude); vendor, model summary). The page says only that "research sessions can use up your limits faster due to Claude retrieving multiple sources" (vendor, page opened).

Two limits are easy to miss. On mobile, Research searches only the web, and the page says: "You won't see an error or other sign that your connectors were skipped." Research also cannot use tools from local MCP servers in Claude Desktop (same page; vendor, model summary).

Do these steps in claude.ai:

1. For a fact that needs one or two searches, ask in a normal chat.
2. To force a search, write "Search the web" in the request.
3. For a question that needs five or more tool calls, type `/deep-research`, or click "+" and then "Research".
4. If the run needs connectors, start the run on the web or in Claude Desktop, not on mobile.
5. After the first run, open Settings > Usage and read how much of the limit the run used.
6. On a Team or Enterprise plan, ask an Owner to enable web search in Organization settings > Capabilities.

Steps 2 and 6 come from the web search help page, and step 3 comes from the page for the new experience (vendor, model summary for each). The Research help page also gives a start phrase for a normal request: "Claude, please use the research tool to…" ([Use research on Claude](https://support.claude.com/en/articles/11088861-use-research-on-claude); vendor, page opened).

### Claude Code: `/deep-research` cross-checks claims, but the web tools lose detail

Claude Code has two web tools. WebSearch "returns result titles and URLs. It doesn't fetch the result pages." ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read). WebFetch runs the prompt against the page in a separate model call, and Claude gets that result, not the raw page. The documentation says: "This makes WebFetch lossy by design." A result that says a page does not mention a fact can mean only that the prompt did not ask. Thus a "not found" result from WebFetch is weak evidence (inference).

The bundled workflow does the deep run. `/deep-research <question>` "Fans out web searches on a question across several angles, fetches and cross-checks the sources it finds, votes on each claim, and returns a cited report with claims that didn't survive cross-checking filtered out." ([Dynamic workflows](https://code.claude.com/docs/en/workflows); vendor, raw read). If the verifier agents cannot check a claim, the report lists the claim as unverified, not as refuted. Dynamic workflows are available on all paid plans, and a Pro user turns them on in `/config`. The workflow needs the WebSearch tool, and the documentation says: "Amazon Bedrock doesn't expose the server-side web search tool." ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read).

Cost is the main risk. An interactive session has a limit of **200 WebSearch calls**, and subagent searches count against the same limit ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read). A workflow agent runs on the session model unless the user assigns a different model ([Dynamic workflows](https://code.claude.com/docs/en/workflows); vendor, page opened). Claude Code shows a `Large workflow` warning above 25 agents or 1.5 million projected tokens, but the warning does not stop the run. Anthropic publishes no cost figure for one typical `/deep-research` run.

One practitioner report shows how a run can fail. In July 2026, the author reported a broad run that launched 111 agents and verified 25 of 123 claims ([Quesma blog](https://quesma.com/blog/custom-deep-research-pipeline/); practitioner report, page opened). That run hit the Max 5x limit after about 30 minutes with no final report, and the author blamed subagents that inherited the Fable model.

Do these steps in Claude Code:

1. Run `/model` before a large run, and ask Claude to use a smaller model for the search stages.
2. Ask a narrow question first, because Anthropic says to "run the workflow on a small slice first".
3. Start the run with `/deep-research <question>`.
4. Open `/workflows` to see the token use of each agent, and stop the run if the cost is too high.
5. Before a wide run, set `CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION` to a higher number.
6. If WebFetch says that a page does not contain a fact, fetch the page again with a specific prompt.
7. Tell Claude to write each finding to a notes file with the URL, the date and an exact quote.
8. Run `/clear` between tasks that do not relate, and start the report from the notes file.

Steps 1 to 4 come from the workflows page, steps 5 and 6 from the tools reference, and step 8 from the best-practices page. Step 7 comes from a practitioner rule: "nothing enters the knowledge base without a URL and a quote from a primary source" ([Quesma blog](https://quesma.com/blog/custom-deep-research-pipeline/); practitioner report, page opened). No official Claude Code page gives a notes format for research. For a custom workflow, the documentation gives this example prompt: "use a workflow to research how our three competitors handle rate limiting: read their public docs and recent changelog entries in parallel, then compare the approaches" ([Dynamic workflows](https://code.claude.com/docs/en/workflows); vendor, raw read).

Subagents do not improve an answer by themselves. The Claude Code best-practices page says: "Handing the same task to a subagent doesn't make the answer any better. A fresh reviewer does help, because it hasn't seen your reasoning" ([Best practices](https://code.claude.com/docs/en/best-practices); vendor, page opened). Thus a subagent fits two jobs: an independent sub-question, and a fresh check of a claim.

For a different search provider, the documentation says: "The search backend is not configurable. To search with a different provider, add an MCP server that exposes a search tool." ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read). One benchmark, with an update on 10 November 2026, compared six search APIs on 120 questions ([AIMultiple](https://aimultiple.com/agentic-search); independent with limits, page opened). The agent in that test was OpenCode with a GPT model, not Claude, and the test did not include Claude's built-in search. Answer accuracy went from 87.0 percent (Parallel Advanced) to 56.5 percent (Tavily Ultra Fast). But the article says: "These intervals overlap, and each configuration was tested once." Thus the evidence for a second search index is weak (inference).

A page reader that renders JavaScript fills a documented gap, because the API web fetch tool does not read such pages ([Web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool); vendor, raw read). The only numbers for such a reader come from its seller. On 25 March 2026, Firecrawl reported 94 percent accuracy with its tool and 66 percent with the API web fetch tool, on 35 questions ([Firecrawl blog](https://www.firecrawl.dev/blog/claude-web-fetch-vs-firecrawl); vendor, page opened). The notes record no Claude model version for that test. Each added server also adds risk, and the MCP page says: "Servers that fetch external content can expose you to prompt injection risk." ([MCP docs](https://code.claude.com/docs/en/mcp); vendor, page opened).

### Claude API: settings control the sources, the citations and the cost

The web search tool and the web fetch tool are stable server tools ([Web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool); vendor, raw read). Web search costs "**$10 USD per 1,000 searches**, plus standard token costs for search-generated content". Web fetch has no charge above the token cost ([Web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool); vendor, raw read). Web search always returns citations, but web fetch returns citations only if the caller sets `"citations": {"enabled": true}`. Web fetch gets only a URL that appeared earlier in the conversation. Search results also count as input tokens in the current turn and in later turns.

From tool version `20260209`, the two tools support dynamic filtering on Claude 4.6 and later models. For web search, those versions make dynamic filtering the default. With dynamic filtering, "Claude instead writes and runs code that filters the results first, so only relevant content reaches the context window." Anthropic states one cost of this default: "Dynamic filtering for web search is not eligible for ZDR or HIPAA." ([Web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool); vendor, raw read). Claude Managed Agents is a beta product that hosts the agent loop ([Managed Agents overview](https://platform.claude.com/docs/en/managed-agents/overview); vendor, raw read). In Managed Agents, the web tools run "without `max_uses`, `citations` or `cache_control`", so a caller who needs those controls uses the Messages API (inference).

For the model, Anthropic's page says: "If you're unsure which model to use, start with Claude Opus 5.5 for most workloads." ([Models overview](https://platform.claude.com/docs/en/about-claude/models/overview); vendor, raw read). The selection page names Claude Fable 5.2 for "multistep deep research" ([Choosing the right model](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model); vendor, raw read). Fable 5.2 costs $8 for a million input tokens and $40 for a million output tokens. Opus 5.5 costs $4 and $20. The selection page also says: "Tuning effort is often a better lever than switching models." All four current models have a reliable knowledge cutoff of June 2026, so a question about later events needs a search (cutoff: raw read; conclusion: inference).

Do these steps with the API:

1. Start with Claude Opus 5.5, and increase `effort` before you change the model.
2. Set `max_uses` to limit the searches in one turn.
3. Set `allowed_domains` or `blocked_domains`, but not the two together, because the API then returns a 400 error.
4. Set `"citations": {"enabled": true}` on the web fetch tool.
5. If ZDR or HIPAA rules apply to web search, set `allowed_callers: ["direct"]` to search without dynamic filtering.
6. If a response has `stop_reason: "pause_turn"`, send the assistant message back with no change.
7. If you show the output to end users, show the citations with it.

For step 2, the documentation gives a guide: "Single-fact queries typically use 1–3 searches; comparative or multientity research can use 10 or more." Step 5 is an inference from the documented facts about dynamic filtering, and the web fetch tool is not eligible for HIPAA at all. Step 7 is a duty in the documentation: "When displaying API outputs directly to end users, citations must be included to the original source." ([Web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool); vendor, raw read).

## Write a request with purpose, success criteria and source rules

Anthropic's general prompt page covers all current models, from Fable 5.2 to Haiku 4.5 ([Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices); vendor, page opened). Its basic rules did not change in 2026: be explicit, give the reason, name the format, and permit "I don't know". The control surface changed. Depth now comes from the `effort` setting, and current models follow each instruction literally. The page says: "If you want Claude to do more than the task strictly requires, explicitly request it rather than relying on the model to infer this from vague prompts."

The correct guidance depends on the model (inference). Research in claude.ai runs on Opus 5.5, so the Opus guidance applies there. Claude Code or the API with Fable 5.2 follows the Fable pages. Nobody fetched the Opus 5.5 model page. Thus the Opus guidance in this report comes from the Opus 5 page and from Anthropic's Opus 5.5 guide of 12 November 2026.

Do these steps when you write a research request:

1. State the purpose and the reader of the result.
2. Describe the finished output, and say when Claude must stop and ask.
3. Give the success criteria for a good answer.
4. Say in plain words how deep to go: one fact, a comparison, or a broad survey.
5. Give the source rules: prefer original sources, and name the sources to avoid.
6. Permit Claude to say that it does not know.
7. Ask for a URL and an exact quote for each claim.
8. Use normal words, and give the reason for each rule.
9. Set the depth with `effort`, and delete lines such as "think hard".
10. For a long task, ask for competing hypotheses and a notes file.

Step 1 has an official template for Fable 5 ([Prompting Claude Fable 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5); vendor, page opened):

```text
I'm working on [the larger task] for [who it's for]. They need [what the output enables]. With that in mind: [request].
```

Step 2 comes from the Opus 5.5 guide, which says to describe "the goal, the finished output, and when you want it to stop and ask" ([Using Claude Opus 5.5 as your daily driver](https://claude.com/resources/articles/using-claude-opus-5-5-as-your-daily-driver-for-knowledge-work); vendor, page opened, 12 November 2026). Step 3 comes from the general page: "Provide clear success criteria: Define what constitutes a successful answer to your research question." Step 6 comes from the hallucination page: "Explicitly give Claude permission to admit uncertainty. This technique can drastically reduce false information." ([Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations); vendor, page opened). Anthropic gives no number, no date and no model for that statement.

Steps 4 and 5 come from Anthropic's engineering post of 13 June 2025 ([How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system); vendor, page opened). Anthropic wrote that post for a Claude Opus 4 lead agent with Claude Sonnet 4 subagents. The post says: "Simple fact-finding requires just 1 agent with 3-10 tool calls". The same post reports that early agents "consistently chose SEO-optimized content farms" and that source rules in the prompt helped. Anthropic's published lead-agent prompt for the Claude 4 generation says: "Prioritize original sources over news aggregators." ([research_lead_agent.md](https://raw.githubusercontent.com/anthropics/anthropic-cookbook/main/patterns/agents/prompts/research_lead_agent.md); vendor, page opened). The exact tool-call numbers are not safe for 2026 models, because the model pages say to test such settings again (inference).

For step 9, the Fable 5.2 page says that effort "sets how much the model thinks, how many tool calls it makes, how long a turn takes, and how many tokens it uses." ([Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2); vendor, page opened). The same page says: "`high` suits hard coding, research, agentic work, and long unattended runs." At `low` effort, Fable 5.1 "is less likely than Claude Fable 5 to call a search or retrieval tool, and more likely to answer from memory." ([Prompting Claude Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1); vendor, page opened). Thus the first fix for a shallow run on a Fable model is a higher effort level (inference). In Claude Code, the `/effort` command sets the level. The research notes record no effort control for claude.ai.

Step 10 has an official template for all current models ([Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices); vendor, page opened):

```text
Search for this information in a structured way. As you gather data, develop several
competing hypotheses. Track your confidence levels in your progress notes to improve
calibration. Regularly self-critique your approach and plan. Update a hypothesis tree or
research notes file to persist information and provide transparency. Break down this
complex research task systematically.
```

The example below puts the steps in one request. This report made the example from the official parts. No source tested the example as one prompt.

```text
I'm working on a tool decision for a finance team of five people. They need to select
one expense tool this month. With that in mind: compare the current prices and the
export functions of [tool A], [tool B] and [tool C].

This is a comparison, so cover each tool separately.
A good answer gives a URL and an exact quote for each price and each limit.
Prefer the vendor's own pages to review sites and news aggregators.
If you cannot find a fact, or cannot confirm it, say so. Do not fill the gap.
Output: one table, then a list of the claims that you could not confirm.
Stop and ask me if the three tools do not have plans that compare.
```

When a user or a lead agent gives work to a subagent, the brief needs four parts. The 2025 post says: "Each subagent needs an objective, an output format, guidance on the tools and sources to use, and clear task boundaries." In that post, a short brief ("research the semiconductor shortage") made the subagents repeat the same searches. For a follow-up turn, the Fable 5.2 page says to "say exactly what you want rather than sending a simple "continue.""

For long documents, two older rules stay current. Put the documents at the top and the question at the end. Then ask Claude to quote the relevant parts before the analysis. Anthropic says: "Queries at the end can improve response quality by up to 30 percent in tests, especially with complex, multidocument inputs." ([Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices); vendor, page opened). That number is a vendor statement, and the page gives no test date and no model for it.

## Open the cited page, because a working link proves little

### Independent measurements

The table gives each number with its date and the systems that the study tested. The numbers do not compare across studies, because each study used a different task.

| Study and date | Systems tested | Result | Trust |
|---|---|---|---|
| ["Cited but Not Verified"](https://arxiv.org/html/2605.06635v1), arXiv, 7 May 2026 | 14 models. Claude models: Opus 4.5, Haiku 4.5, Sonnet 4.6, Opus 4.6, Sonnet 4.5 | The page supported the fact for 76.8, 68.9, 58.7, 54.2 and 51.8 percent of citations (same order). Links worked for 97.2 to 99.2 percent. All frontier models: support of 39 to 77 percent. | Independent. Page opened. An LLM judge scored, with human calibration. |
| Same study, May 2026 | GPT-5.4 and Claude Opus 4.6 | Tool calls went from 2 to 150. Support fell from 79 to 17 percent (GPT-5.4) and from 80 to 58 percent (Claude Opus 4.6). | Independent. Page opened. |
| ["Detecting and Correcting Reference Hallucinations"](https://arxiv.org/html/2604.03173v1), arXiv, 3 April 2026 | claude-3-5-sonnet-with-search, claude-3-7-sonnet-with-search, claude-sonnet-4-5, with OpenAI and Gemini models | URLs that did not resolve: 7.8, 8.5 and 9.38 percent for the three Claude models. A URL-health tool for the model cut such URLs to less than 1 percent. | Independent. Page opened. The study tested no Claude deep research agent. |
| [DeepTRACE](https://arxiv.org/html/2509.04499v1), arXiv, 2 September 2025 (results of 27 August 2025) | Deep research modes of GPT-5, Gemini, Perplexity and YouChat, with Copilot Think Deeper and GPT-5 web search. Not Claude. | Statements with no support: 12.5 percent (GPT-5 deep research) to 97.5 percent (Perplexity deep research). One-sided answers to debate questions: 54.7 to 94.8 percent. | Independent, with a limit: Microsoft Research is a co-author, and the test includes Copilot. Page opened. |
| [Tow Center / CJR](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php), 6 March 2025 (tests of February 2025) | ChatGPT Search, Perplexity, Perplexity Pro, DeepSeek Search, Copilot, Grok-2, Grok-3 beta, Gemini. Not Claude. | The tools gave incorrect answers to more than 60 percent of queries. The task: find a news article from an excerpt. | Independent. Page opened. |
| [EBU / BBC](https://www.ebu.ch/news/2025/10/ai-s-systemic-distortion-of-news-is-consistent-across-languages-and-territories-international-study-by-public-service-broadcaste), 22 October 2025 | ChatGPT, Copilot, Gemini, Perplexity. The release gives no versions. Not Claude. | 45 percent of news answers had a significant issue. 31 percent had serious sourcing problems. | Independent. Page opened. |
| [NewsGuard monitor](https://www.newsguardtech.com/ai-monitor/january-2026/), January 2026 | 11 chatbots, with Claude. The summary gives no version. | The chatbots together repeated false claims in 28.79 percent of responses (95 of 330). | Independent. Search only. No rate for Claude alone. |

Three points follow from the table (inference). First, the main error in 2026 is a real source with a wrong claim, not an invented source. Second, a newer model did not score higher: Claude Opus 4.6 got 54.2 percent and Claude Opus 4.5 got 76.8 percent (May 2026). Third, all tested Claude versions are older than the models that Anthropic lists as current in November 2026. Thus the results for Opus 5.5 and Fable 5.2 can be better or worse, and no source tells which.

### Vendor statements

Anthropic states the limit plainly: "although these techniques significantly reduce hallucinations, they don't eliminate them entirely. Always validate critical information, especially for high-stakes decisions." ([Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations); vendor, page opened). The help page for Research has no warning about accuracy ([Use research on Claude](https://support.claude.com/en/articles/11088861-use-research-on-claude); vendor, page opened). Do not read that silence as a claim of accuracy (inference). Anthropic's guide of 12 November 2026 describes Opus 5.5 as "careful with figures and sources" ([Using Claude Opus 5.5 as your daily driver](https://claude.com/resources/articles/using-claude-opus-5-5-as-your-daily-driver-for-knowledge-work); vendor, page opened). That guide gives no error rate.

### Opinion

Two practitioner views come from 2025 and from older models. On 23 June 2025, when Claude 4, o3 and Gemini 2.5 were current, Ethan Mollick wrote: "Deep Research reports are not error-free but are far more accurate than just asking the AI for something" ([One Useful Thing](https://www.oneusefulthing.org/p/using-ai-right-now-a-quick-guide); opinion, page opened). On 21 April 2025, in the Claude 3.7 period, Simon Willison wrote: "I still don't trust them not to make mistakes." ([simonwillison.net](https://simonwillison.net/2025/Apr/21/ai-assisted-search/); opinion, page opened).

### The check, in order of evidence

1. Select the claims on which a decision depends.
2. Open the cited page for each such claim, and read the passage that supports the claim.
3. Make sure that the page is the original publisher, not a copy or a page that an AI wrote.
4. Read the date of the page.
5. If you do not know the source, search for what other sources say about it.
6. Compare each quote with the page, word for word.
7. For a contested question, ask Claude separately for the strongest evidence against the first answer.
8. Do not count agreement from a second AI tool as a check, and go to a primary source.
9. In Claude Code, give the check to a fresh agent that did not find the claim.

Step 2 has the strongest evidence, which is the May 2026 benchmark in the table. Step 5 is lateral reading. A 2025 meta-analysis pooled 64 controlled experiments that taught people to assess sources ([Fendt et al. 2025](https://epub.ub.uni-muenchen.de/129131/1/1-s2.0-S104160802500158X-main.pdf); independent, search only). The average effect was g = 0.42, and lateral reading had the largest effects. That evidence is for web sources in general, not for AI output.

Step 6 answers a BBC finding of February 2025 for ChatGPT, Copilot, Gemini and Perplexity ([The Register](https://www.theregister.com/2025/02/12/bbc_ai_news_accuracy/); independent, search only). In that study, 13 percent of quotes from BBC articles were different from the article or were not in it.

Steps 7 to 9 have weaker support. No study measured how much a balanced prompt helps a research report. Step 8 is an inference, because AI tools draw on the same web sources. Step 9 has a vendor source and a practitioner source. Anthropic says for Fable 5: "Separate, fresh-context verifier subagents tend to outperform self-critique." ([Prompting Claude Fable 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5); vendor, page opened). The practitioner rule is: "The agent that finds a claim never verifies it" ([Quesma blog](https://quesma.com/blog/custom-deep-research-pipeline/); practitioner report, page opened, July 2026).

Claude can do a first pass of the check. Anthropic gives this prompt for a press release, and the user changes "press release" to "report" ([Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations); vendor, page opened):

```text
After drafting, review each claim in your press release. For each claim, find a direct quote from the documents that supports it. If you can't find a supporting quote for a claim, remove that claim from the press release and mark where it was removed with empty [] brackets.
```

Anthropic recommends this prompt for documents that the user supplies. No independent source measured its effect in web research. A quote that Claude returns also needs step 6, because the model can change a quote (inference from the BBC finding).

The cost of no check is public. On 18 November 2026, one database listed **2,312 court cases** that address AI hallucinations ([AI Hallucination Cases](https://www.damiencharlotin.com/hallucinations/); independent, page opened). The party was a self-represented person in 1,326 cases and a lawyer in 920 cases. The list includes some cases with an AI use that nobody confirmed, and the page does not name Claude. In May 2025, Anthropic's own outside counsel told a court that Claude gave "an inaccurate title and incorrect authors" for a real source ([TechCrunch](https://techcrunch.com/2025/05/15/anthropics-lawyer-was-forced-to-apologize-after-claude-hallucinated-a-legal-citation); press report, search only). An error of that type passes a link check.

## Limit what Claude can reach when it reads pages that you do not trust

A web page can contain text that tells Claude to do something. Anthropic says that prompt injection is "far from a solved problem" and that "No browser agent is immune to prompt injection" ([Prompt injection defenses](https://www.anthropic.com/research/prompt-injection-defenses); vendor, page opened, 24 November 2025). All attack rates for Claude come from Anthropic's own tests. The table keeps them apart from the outside demonstrations.

| Number | Date | Tested model or product | Type and trust |
|---|---|---|---|
| Attack success of 23.6 percent with no mitigations and 11.2 percent with mitigations (123 test cases) | August 2025 | Claude for Chrome pilot. The notes record no model version. | Vendor. Page opened. [Source](https://claude.com/blog/claude-for-chrome) |
| Attack success with improved safeguards, 100 attempts in each environment: 1.4, 1.9, 2.0 and 2.4 percent | November 2025 | Opus 4.5, Sonnet 4.5, Sonnet 4 and Haiku 4.5 (same order) | Vendor. Page opened. [Source](https://www.anthropic.com/research/prompt-injection-defenses) |
| Attack success "less than 0.08%" on Anthropic's internal test | Support page with no date, retrieved 19 November 2026 | Claude Opus 4.8 | Vendor. Page opened. [Source](https://support.claude.com/en/articles/12902428-use-claude-in-chrome-safely) |
| An attack that ends in code execution worked "80% of the time" | August 2026 | Claude Code with Opus 5 in auto mode | Practitioner report by Johann Rehberger. Page opened. [Source](https://simonwillison.net/2026/Aug/27/breaking-claude-code-opus-5-auto-mode) |
| A hidden instruction in a file made Claude Cowork upload the user's files | January 2026 | Claude Cowork, with Claude Haiku and Claude Opus 4.5 | Independent demonstration by PromptArmor. Search only. [Source](https://the-decoder.com/claude-cowork-hit-with-file-stealing-prompt-injection-days-after-anthropics-launch/) |

Anthropic's rates fell between 2025 and 2026, but the tests and the attack sets changed between the posts. Thus the rates show a trend and not a guarantee (inference). The same support page that gives 0.08 percent also says: "The risk is not zero." Simon Willison's opinion of 16 June 2025 names a dangerous mix of three things ([The lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/); opinion, search only). The three things are private data, content that the user does not trust, and a way to send data out. Research with connectors, a signed-in browser or local files can complete that mix (inference).

Do these steps:

1. In claude.ai, turn off the connectors that a web-only research task does not need.
2. In Claude in Chrome, select "Manually approve".
3. In Claude in Chrome, use a separate browser profile with no bank, health or government accounts.
4. Stop the task if Claude visits a site that you did not expect, or asks for sensitive data.
5. In Claude Code, turn on the sandbox and the permission rules together.
6. Give a research subagent only the tools `WebSearch`, `WebFetch` and `Write`.
7. Deny `curl` and `wget`, or permit them only inside the sandbox with a short domain list.
8. Run an unattended agent in a container or a VM that holds no SSH keys and no cloud credentials.
9. Connect only the MCP servers that you trust.
10. In the API, set `max_uses` and `allowed_domains` on web fetch, and restrict `url_sources`.

Steps 2 to 4 come from Anthropic's safety page ([Use Claude in Chrome safely](https://support.claude.com/en/articles/12902428-use-claude-in-chrome-safely); vendor, page opened). Step 5 comes from the permissions page: "Use both for defense-in-depth, since sandbox restrictions still apply even if a prompt injection bypasses Claude's decision-making." ([Permissions](https://code.claude.com/docs/en/permissions); vendor, page opened). Steps 6 and 7 are inferences from the Claude Code pages and from the August 2026 attack. Step 8 quotes the advice in the report of that attack. Step 1 is an inference from the mix of three things. Step 9 comes from the MCP page, and step 10 comes from the web fetch page.

The sandbox has limits. The sandbox page says: "The sandbox covers shell commands only. Claude's file tools, MCP servers, and hooks run outside it." ([Sandboxing](https://code.claude.com/docs/en/sandboxing); vendor, page opened). By default, a sandboxed command can also read credential files such as `~/.ssh`. Thus a research machine needs a container or a rule that denies those files (inference).

Privacy depends on the plan. On the consumer plans (Free, Pro, Max), Anthropic uses chats for model training if the "Model Improvement" setting is on ([Privacy Center, 16 March 2026](https://privacy.claude.com/en/articles/10023580-is-my-data-used-for-model-training); vendor, page opened). With the setting on, Anthropic keeps the data for five years, and with the setting off, for 30 days ([Updates to our consumer terms, 28 August 2025](https://www.anthropic.com/news/updates-to-our-consumer-terms); vendor, page opened). The Privacy Center page says that the covered data includes "data collected through Claude for Chrome". The same page says: "Your Incognito chats are not used to improve Claude".

By default, Anthropic does not use data from Team, Enterprise or the API for training ([Privacy Center, 18 August 2026](https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training); vendor, page opened). On each plan, a thumbs up or thumbs down stores the full conversation for a maximum of five years. Thus for a sensitive topic on a consumer plan, turn off "Model Improvement", use an Incognito chat, and give no thumbs rating (inference).

## Sixteen older habits meet the current position

The table puts the commonly repeated advice of 2024 and 2025 next to the position of the current sources. The status column says if the advice is out of date, or if it only has a date limit.

| Older advice | Current position (November 2026) | Status and trust |
|---|---|---|
| Turn on the web search toggle in each chat. | The new experience has no toggle. Claude searches when it helps. The words of the request are the control. | Out of date for the new experience. [Vendor](https://support.claude.com/en/articles/16761823-claude-cowork-and-chat-are-one-claude), model summary. |
| Research is a button in the web interface only. | In claude.ai, the user types `/deep-research` or clicks "+" and "Research". Claude Code has its own bundled `/deep-research` workflow. | Out of date. Vendor. Model summary for claude.ai, [raw read](https://code.claude.com/docs/en/workflows) for Claude Code. |
| Research runs for a maximum of 45 minutes. | No current page gives a run time as a limit. | Not confirmed current. [Vendor, May 2025](https://www.anthropic.com/news/integrations), search only. See the conflicts. |
| Set a thinking budget in the API. | "On Claude 4.7 and later models, setting `budget_tokens` returns a 400 error." The `effort` setting controls depth. | Out of date. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), page opened. |
| Write "think hard" or "think step by step". | The Opus 5.5 guide says to remove such lines, because thinking is always on. | Out of date. [Vendor](https://claude.com/resources/articles/using-claude-opus-5-5-as-your-daily-driver-for-knowledge-work), page opened, 12 November 2026. |
| Tell Claude to show its reasoning in the answer. | On Fable 5 and later, this "might trigger the `reasoning_extraction` refusal category". The user reads the thinking summary. | Out of date. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5), page opened. |
| Use capitals such as CRITICAL and MUST. | "The fix is to soften any aggressive language." Anthropic wrote this for Opus 4.5 and 4.6. Fable 5.2 "treats an instruction that contains "never" as absolute." | Out of date. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), page opened. No measurement for Fable 5.x. |
| Always add "double-check your work". | Opus 5: remove such lines. Fable 5: make the check explicit in long runs. | Depends on the model. See the conflicts. |
| Add lines that push Claude to be more thorough. | "If your prompts previously encouraged the model to be more thorough or use tools more aggressively, reduce that guidance." | Out of date. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), page opened. |
| Write long lists of rigid rules. | Skills for older models "are often too prescriptive for Claude Fable 5 and can degrade output quality." | Out of date for Fable models. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5), page opened. |
| Prefill the assistant turn to force a format. | From Claude 4.6, such requests "return a 400 error." | Out of date. [Vendor](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), page opened. |
| Split research across subagents because the context window holds 200,000 tokens. | "With a 1M-token window on most current models and compaction near the top of it, you'll rarely run out of context." Cost is now the reason to manage context. | Out of date. [Vendor](https://code.claude.com/docs/en/best-practices), page opened. |
| More agents give a better report: the multi-agent system won by 90.2 percent. | The number comes from June 2025, from Claude Opus 4 with Claude Sonnet 4 subagents, on Anthropic's internal test. | Number with a date limit. [Vendor](https://www.anthropic.com/engineering/multi-agent-research-system), page opened. See the conflicts. |
| The API web search and web fetch tools are beta tools. | Since 17 February 2026, the tools need no beta header and support dynamic filtering. | Out of date. [Vendor](https://platform.claude.com/docs/en/release-notes/overview), raw read. |
| A citation link lets the reader trust the claim. | Links from five Claude 4.5 and 4.6 models worked for 97 to 99 percent of citations. The page supported the fact for 52 to 77 percent (May 2026). | An [independent](https://arxiv.org/html/2605.06635v1) measurement corrects this advice. Page opened. |
| A paid tier, a long report or many sources shows quality. | In February 2025, the paid tiers Perplexity Pro and Grok 3 had higher error rates than the free tiers, because they did not decline to answer. DeepTRACE (August 2025): "More sources and longer answers do not translate into reliability." | Independent measurements correct this advice. Page opened: [Tow Center](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php), [DeepTRACE](https://arxiv.org/html/2509.04499v1). Neither study tested Claude. |

One older opinion also has a date limit. In April 2025, in the Claude 3.7 period, Simon Willison wrote that Claude's web search "doesn't feel nearly as good" ([simonwillison.net](https://simonwillison.net/2025/Apr/21/ai-assisted-search/); opinion, page opened). No current independent test compares Claude's search with other tools, so no source confirms or corrects that opinion.

Some older advice stays current. Anthropic still recommends a role: "Setting a role in the system prompt focuses Claude's behavior and tone for your use case." ([Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices); vendor, page opened). The page also keeps the rules for explicit instructions, reasons, XML tags, documents before the question, and quotes before analysis. The hallucination page keeps the permission to say "I don't know". The claude.ai help center also kept the name "extended thinking" for its own feature on 2 June 2026 (vendor, model summary).

## Sources conflict on eight points, and each has a safe action

### Research run time: 45 minutes in 2025, 1 to 3 minutes in 2026

Anthropic's announcement of 1 May 2025 said that Claude can research for a maximum of 45 minutes ([Claude can now connect to your world](https://www.anthropic.com/news/integrations); vendor, search only). The help article of 2 June 2026 describes Research as work of about 1 to 3 minutes ([help article](https://support.claude.com/en/articles/11095361-when-should-i-use-web-search-extended-thinking-and-research); vendor, model summary). One research pass read that article through a model summary, and two other passes saw the number only in search results. A practitioner post reports 5 to 30 minutes for each run, but its year is not visible ([DEV post](https://dev.to/bhaidar/how-to-use-claudeais-research-toggle-inside-claude-code-469d); practitioner report, not confirmed current). No page explains the difference.

Do not plan work around either number. Start Research when you can wait, and read the usage after the run.

### Self-check lines: remove them on Opus 5, keep them explicit on Fable 5

For Opus 5, Anthropic says: "Claude Opus 5 verifies its own work without being told to. If your prompt contains explicit verification instructions ("include a final verification step for any non-trivial task," "use a subagent to verify"), remove them" ([Prompting Claude Opus 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5); vendor, page opened). For Fable 5, Anthropic says: "Make self-verification explicit in long-run prompts." ([Prompting Claude Fable 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5); vendor, page opened). The general page still says: "Ask Claude to verify information across multiple sources."

In claude.ai Research, which runs on Opus 5.5, do not add a general "double-check everything" line. On each model, keep the specific checks: more than one independent source, a quote for each claim, and removal of claims with no quote. For a long run on a Fable model, add a fresh verifier agent. One more caution applies to Fable 5.2 ([Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2); vendor, page opened). Two "treat that answer as done" sentences from the Opus 5.5 guide made Fable 5.2 "far less likely to point out mistakes in its earlier answers." Thus do not copy a prompt from one model to a different model without a test.

### Examples: the general page says 3 to 5, the Claude Code team removed them

The general page says: "Include 3–5 examples for best results." Thariq Shihipar of the Claude Code team said in July 2026 that "removing examples was extremely helpful." ([Simon Willison's notes, 21 July 2026](https://simonwillison.net/2026/Jul/21/cat-and-thariq/); practitioner report of vendor staff, page opened). That remark was about the Claude Code system prompt on the newest models, not about research prompts.

Start a research request with no examples. Add one worked example only when the output has the wrong form. Anthropic uses that fix for Fable 5.1, which "is more likely than Claude Fable 5 to reproduce passages of the source text without marking them as quotations." ([Prompting Claude Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1); vendor, page opened).

### Emphasis words: the guidance says soften, but official samples keep capitals

The general page says to soften strong words. But one sample prompt on the same page still says "you MUST read the file before answering". Anthropic's published research prompt for the Claude 4 generation also uses "**IMPORTANT**" and "MUST" ([research_lead_agent.md](https://raw.githubusercontent.com/anthropics/anthropic-cookbook/main/patterns/agents/prompts/research_lead_agent.md); vendor, page opened). Use normal words with a reason. Take the structure of the 2025 prompt, but do not copy its tone.

### Subagents: a 90.2 percent gain in 2025, no gain from delegation alone in 2026

In June 2025, Anthropic reported that its multi-agent system beat one Claude Opus 4 agent by 90.2 percent on an internal test ([engineering post](https://www.anthropic.com/engineering/multi-agent-research-system); vendor, page opened). The same post gives the cost: "multi-agent systems use about 15× more tokens than chats." The 2026 Claude Code page says that a subagent "doesn't make the answer any better" for the same task ([Best practices](https://code.claude.com/docs/en/best-practices); vendor, page opened). A search summary of one Anthropic article adds that one agent with skills often beat the subagent design ([claude.com article](https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop); vendor, search only). The summary gives 22 October 2026 as the date, but the search tool warned that the date can be wrong.

Use subagents for parallel sub-questions that do not depend on each other, and for a fresh check. Use one agent for a question with one line of work.

### More searches: better answers in 2025 data, weaker citations in 2026 data

In June 2025, Anthropic wrote that "token usage by itself explains 80% of the variance" in performance on the BrowseComp test ([engineering post](https://www.anthropic.com/engineering/multi-agent-research-system); vendor, page opened). In May 2026, an independent benchmark found that support for cited facts fell when tool calls went from 2 to 150 ([Cited but Not Verified](https://arxiv.org/html/2605.06635v1); independent, page opened). The fall was from 80 to 58 percent for Claude Opus 4.6 and from 79 to 17 percent for GPT-5.4. Anthropic wrote the 2025 post about a system of Claude Opus 4 and Claude Sonnet 4. The two sources measure different things: the first measures answer performance, and the second measures citation support. Set the depth to fit the question, and check a long run more than a short run.

### Injection: less than 0.08 percent in vendor tests, 80 percent in one outside attack

Anthropic gives an attack success of "less than 0.08%" for Claude Opus 4.8 on its internal test ([Use Claude in Chrome safely](https://support.claude.com/en/articles/12902428-use-claude-in-chrome-safely); vendor, page opened). In August 2026, a practitioner attack on Claude Code with Opus 5 in auto mode worked "80% of the time" ([Simon Willison](https://simonwillison.net/2026/Aug/27/breaking-claude-code-opus-5-auto-mode); practitioner report, page opened). The two numbers come from different products and different attacks. Simon Willison later agreed that the 2026 case "is more of a confused environment attack" than a classic prompt injection. Do not trust the model to resist: limit what the agent can read, run and send.

### The raw page: the documentation suggests `curl`, and the attack used `curl`

The tools reference tells the user to use `curl` through Bash when the raw page is necessary ([Tools reference](https://code.claude.com/docs/en/tools-reference); vendor, raw read). But the August 2026 attack began after a server error moved the agent from WebFetch to `curl` (researcher's reading of the attack report). The best-practices page adds: "A `WebFetch` deny rule for a domain does nothing about `curl` to that same domain, unless the sandbox is on." ([Best practices](https://code.claude.com/docs/en/best-practices); vendor, page opened). Permit `curl` only with the sandbox on and a short domain list.

## What no source confirmed

- **Accuracy of the current models.** No independent study tested Opus 5.5, Fable 5.2, Sonnet 5.5 or Haiku 5.5 for web research.
- **Accuracy of claude.ai Research.** No independent study tested the product, and Anthropic gives no error rate for it.
- **Research internals.** No current source gives the run time, the number of searches or sources, the regions, or the export options. No 2026 source confirms that the 2025 multi-agent design still runs.
- **claude.ai Research compared with Claude Code `/deep-research`.** No source compared quality, time or usage. No source says if the two use the same engine.
- **Cost of one `/deep-research` run.** Anthropic gives no figure. The practitioner numbers come from one author.
- **Built-in search compared with MCP search tools.** No independent benchmark includes Claude's built-in WebSearch or WebFetch.
- **Effect of the check prompts.** No independent source measured "ask for the exact quote", a balanced prompt, a second AI tool, or notes files in web research.
- **A tested prompt template.** No source gives a tested research template for 2026 models. The example request in this report has no test.
- **Model pages not read.** Nobody fetched the prompt pages for Opus 5.5, Sonnet 5.5 and Haiku 5.5.
- **The older text of Anthropic pages.** Nobody fetched the 2024 and 2025 versions. For some rows, the "older advice" column rests on what current pages tell users to stop.
- **Facts with the level "search only".** These include the 45-minute run time, the BBC and NewsGuard numbers, and the lateral-reading meta-analysis. They also include the Claude court case, the Cowork demonstration, the lethal trifecta post and one 2026 article on subagents.
- **Help-center wording.** A small model summarised most help-center pages, so their wording in this report is a paraphrase unless it is in quotation marks.
- **Numbers that this report left out.** Third-party pages give attack rates of 0 to 0.3 percent for the August 2026 launch of Claude in Chrome. No Anthropic page confirmed them.
- **Prices of add-on search tools.** Roundup pages conflict on the prices and the free limits of Tavily, Brave, Perplexity and Exa. This report gives none of them.
- **Privacy details.** No source says what the search provider receives when Claude searches. No source confirms the default of "Model Improvement" for a new account in November 2026.
- **Controls in claude.ai.** No note records an effort or thinking control for the new claude.ai experience.

## Conclusion

Between 2025 and 2026, the control of research quality moved from strong words in the prompt to settings. The lever that stays with the reader is the check of each source. That check carries more weight than before, because the evidence is older than the product. All independent Claude data comes from models between Claude 3.5 Sonnet and Claude Opus 4.6. In the May 2026 benchmark, Claude Opus 4.6 scored lower than the older Claude Opus 4.5. Thus a new model release is a reason to test again, not a reason to trust more.

A small private test set makes that practical (this report's recommendation, not a sourced fact). Keep some research questions for which you know the correct answer, and run them after each model change. Anthropic gives two reasons for such a repeat. The Fable 5.2 page says to "rerun your effort sweep rather than carrying over a setting you tuned" on an earlier model. The Claude Code team said in July 2026: "We actually have a different system prompt per model now." Score each run with the five criteria of Anthropic's 2025 rubric: factual accuracy, citation accuracy, completeness, source quality and tool efficiency.
