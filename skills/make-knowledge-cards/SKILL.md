---
name: make-knowledge-cards
description: Turn a pasted article or a local Markdown/TXT file into 5-8 source-grounded knowledge cards. Each card has a title, one core idea, a short explanation, and an example from the source or a self-check question. Use when the user asks for study cards, review cards, plain-text flashcards, or asks to distill an article, guide, essay, or documentation into cards. Do not use for web scraping, PDFs, Anki export, or GUI output; ask for pasted plain text instead. Never invent missing facts and never pad the count when the source supports fewer than five distinct points.
license: MIT
---

# Make Knowledge Cards

Turn one article into a small set of faithful knowledge cards for learning and review. Select less, but select what matters.

## Inputs and scope

- Accept pasted article text or one local `.md` / `.txt` file.
- Write the cards in the source language unless the user asks for another language. Keep technical terms and proper nouns as the source uses them.
- Unsupported: URLs and web scraping, PDF/EPUB/DOCX/images/audio/video, Anki or other deck exports, and graphical interfaces. Say so briefly and ask for pasted plain text or a `.md` / `.txt` file. Do not fetch the web or create a deck as a workaround.
- Process one source per card set. If several files are supplied without instructions, ask whether to combine them or treat them separately. Multiple sources may be handled as independent sets when the user asks; never merge them silently.
- Return the cards in the conversation. Write a Markdown file only when the user explicitly asks for one.

## Workflow

### 1. Read the source

- For a local file, read it with the available file-reading tool. Confirm the extension is `.md` or `.txt`. If the file is missing, unreadable, or in another format, report the concrete problem instead of guessing its contents.
- If the user pastes text, use exactly that text. Do not search for the original article or add outside context.
- Separate the article body from navigation, author bios, ads, calls to action, and boilerplate. Keep the title and byline or date when they matter to the claims.

### 2. Map candidate knowledge points

Identify the source's:

- central thesis or conclusion;
- definitions and key distinctions;
- causes, mechanisms, and consequences;
- processes, rules, and decision criteria;
- necessary conditions, exceptions, and limits;
- quantitative findings and relationships;
- evidence that changes how a claim should be understood.

Mark repeated passages and overlapping claims. A candidate must be stated or directly supported by the source; do not fill gaps with common knowledge.

### 3. Select 5-8 distinct points

Aim for 5-8 cards when the source supports that many independent, important points.

Use this importance test: if a learner omitted this point, would they lose part of the article's main argument, a decision rule, or an ability the article is trying to teach? If not, drop it.

- One card = one knowledge point. Do not split a single idea into "what", "why", and "how" cards. A causal chain that explains one phenomenon can stay in one card; do not turn each link into its own card.
- Merge cards that make the same claim or differ only in wording. Repeated advice in different sections, a tip and its restatement, or one rule plus its illustration is still one point.
- Do not create meta cards for "overview", "summary", "conclusion", "why this matters", or "next steps" unless the source itself makes that a substantive point.
- Do not turn every passing example, statistic, anecdote, or minor tip into a card. An example is not a separate knowledge point unless it introduces a distinct idea.
- Drop cosmetic changes, marketing adjectives, and promotional claims unless they change a capability, constraint, or decision.
- If the source supports fewer than 5 distinct important points, output fewer and say why in one short line. Never pad by repeating, splitting, or inventing.
- If the source has more than 8 important points, keep the 8 with the highest leverage and omit the rest. Never exceed 8.
- If the user asks for fewer cards, honor a lower count that the source supports. If they ask for more than 8, provide at most 8 and explain the cap.

If the source is only a title, table of contents, greeting, or a few vague sentences, do not manufacture cards. Explain what is missing and ask for the full article.

### 4. Write each card

Every card has exactly these four elements. Put the title in the card heading; do not repeat it as a separate bullet.

- **Title** — a specific, meaningful phrase that works out of context. Avoid "Introduction", "Key point 1", or a title that joins two unrelated ideas with "and".
- **Core knowledge** — one declarative sentence stating the single takeaway. It must stand alone without the article. Do not write "This article discusses..." or "The author mentions...".
- **Explanation** — usually 2-3 sentences that unpack the core idea: why or how it works, what it depends on, or what follows from it. Use only details present in the source. Do not repeat the core in different words, and do not append a second benefit, caveat, or consequence that could stand as its own card. If the source supports only one precise clarifying sentence, use one; never invent a mechanism or pad to reach a length.
- **Example or self-check question** — prefer an example the source actually gives, labeled with the localized equivalent of "Example from the source" (for Chinese, `原文例子：`). Use it only if it illustrates this card's point. A hypothetical condition the source uses to illustrate a claim counts as a source example; a scenario you invent does not. If there is no usable source example, write a self-check question that can be answered from the source, with a one-line answer in parentheses. The question should test the card's core idea, not a different fact. The answer must be traceable to the source. Never invent an illustrative scenario, statistic, name, or anecdote.

Do not add extra fields such as difficulty, tags, references, or a second example.

### 5. Run the fidelity pass

Treat the source as the only evidence.

- Add no background facts, definitions, dates, numbers, causes, or implications that the source does not state.
- Paraphrase rather than copy long passages. Short quotations are fine when the exact wording matters.
- Preserve numbers, units, names, scope, and qualifiers such as "may", "often", "in most cases", or "under this condition".
- Do not turn a correlation into a cause, an anecdote into a general rule, a recommendation into a requirement, or one study's finding into a consensus.
- In an opinion or argument piece, frame the author's central and contestable claims as the author's position ("The author argues..." / "作者认为..."). Do not turn a recommendation, interpretation, or prediction into a neutral universal rule; reserve plain factual phrasing for facts the source itself presents as facts. Keep the author's evidence distinct from the conclusion.
- Do not silently correct, update, or fact-check the article. If it contradicts itself, either omit the lower-value claim or note the contradiction in the affected card's explanation.
- Do not infer a mechanism just because it sounds plausible. If the source gives no reason, the explanation should say only what the source establishes.

### 6. Run the duplication and coverage pass

- Read the titles together. If two cards could be swapped without losing anything, merge or drop one.
- Check that each card tests a different idea, not a different sentence about the same idea.
- Check that the cards still let a reader reconstruct the article's main throughline. A set of isolated easy facts is not enough.
- If an explanation sentence introduces a fact that could stand as a separate card, remove it or split it only when that fact passes the importance test. Never smuggle a second point into an explanation.

### 7. Format the result

Use the user's language for the labels. For Chinese:

```markdown
## 知识卡片：{source title or "未命名文章"}

> 共 {n} 张卡片。

### 1. {title}

- **核心知识**：...
- **简明解释**：...
- **例子或自测问题**：...
```

For English, use `Knowledge cards: ...`, `### 1. ...`, `**Core idea:**`, `**Explanation:**`, and `**Example or self-check question:**`.

If and only if the source supports fewer than 5 cards, add one short line directly under the count: "原文可独立成卡的重点有限，未补足数量。" (English: "The source supports fewer than five distinct points; the count was not padded.") Otherwise omit that line.

Keep cards numbered. Do not add a long preamble, a repeat of the article, or a section-by-section summary after the cards.

## Final check

Before sending, verify:

- the count is 5-8 unless fewer are genuinely supported or the user asked for fewer;
- every card has one idea and all four required elements;
- every factual statement is traceable to the source;
- numbers, qualifiers, and attribution are preserved;
- opinion claims are attributed rather than presented as universal rules;
- each explanation stays on its card's point, adds something precise, and does not smuggle in a second point;
- no example, explanation, or self-check answer was invented;
- duplicate and meta cards are gone;
- titles are meaningful without the article;
- the set covers the main argument, not just isolated details.
