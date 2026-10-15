# Cortex Drive - 3-Minute Video Pitch Script

**Total: 3:00.** Screen-record the app at `http://localhost:5050` in a clean browser window (use *Reset demo* right before recording). Speak over the screen; cut to a face camera only for the opening line if you like.

Before you hit record: `python app.py`, open the page, click **Reset demo**, and close the right-hand Insights tab so the Activity tab is showing.

---

### 0:00 - 0:20 · The problem (talk over the empty-looking folder list)

> "Google Drive and Dropbox are filing cabinets. You put files in, and they sit there until you go digging. We lose hours searching, filing, and copying numbers between documents.
> We built **Cortex Drive**: a drive where you never talk to folders. You talk to an agent, and the agent runs the files."

*Show: the full screen. Point at the left panel: "These folders were not created by me. The Librarian agent read nine files and organised them by content."*

---

### 0:20 - 0:55 · Semantic retrieval with citations

*Click the first suggestion chip, or type:* **"What was the budget discussed in the Q3 marketing meetings?"**

> "I'll ask a normal question. The router picks the Researcher agent, you can see its trace: it retrieved passages from four files and ranked sentences.
> The answer is built from three meeting transcripts, ordered by date, and every line has a citation to its source.
> And look at this: the agent added something I did not ask for. **The budget moved from a hundred and twenty thousand to a hundred and thirty-five thousand.** That is the Watcher agent noticing a figure changed between files."

*Zoom on the "Heads up" line and the source cards.*

---

### 0:55 - 1:40 · Cross-document synthesis

*Type:* **"Draft a unified requirements document from the Orbit discovery calls"**

> "Now the part that usually takes a product manager two days. Three discovery calls. I did not select anything: the Writer agent found the relevant calls, and noticed a third one in the same project folder and pulled it in.
> It extracts requirements, decisions, constraints, risks and action items, removes duplicates, and cites every line back to its call.
> And here is the aha. At the bottom: **'Direction changed.'** In call one, ops asked for SMS notifications. In call three, finance dropped SMS for push. Anyone who skimmed would have built the wrong thing."

*Scroll the document slowly, stop on the Conflicts section. Click **Save to my drive**.*

> "One click and the document is saved back into the drive, and the Librarian files it next to its sources."

---

### 1:40 - 2:20 · Automated workflows

*Click **Simulate a file drop** three times (or drag in the three invoices). Switch to the Activity tab, then the Outputs tab.*

> "Now automation. I drop an invoice into the drive, and I do nothing else.
> The Librarian files it. The Analyst extracts vendor, invoice number, date and total, categorises it as infrastructure, and appends a row to the expense ledger. Three invoices, three rows, zero typing."

*Type:* **"How much have we spent on invoices so far?"**

> "And I can ask about it in plain English: nine thousand eight hundred dollars, broken down by category."

*Show the bar chart.*

---

### 2:20 - 2:45 · Proactive insights

*Click the **Insights** tab.*

> "The Watcher runs across everything, unprompted. A budget that rose from a hundred and eighty to two hundred thousand after a sponsor email. Two reversed decisions. And a deadline radar: the Q4 proposal is due in five days."

*(If you recorded on a different date, the day counts will differ; that is expected, they're computed live.)*

---

### 2:45 - 3:00 · Close

> "Cortex Drive turns passive storage into an active workspace: it **organises itself, answers with citations, writes from many sources, acts on triggers, and warns you when your documents disagree.**
> It works offline, and with a Claude key the answers and documents are written by Claude. Thank you."

*End card: **Cortex Drive - the drive that reads itself.***

---

## Shot checklist

- [ ] Reset demo, empty chat, Activity tab open
- [ ] Question 1 (budget) shows the "Heads up" line and the source cards
- [ ] Question 2 (requirements) shows the Conflicts section, then **Save to my drive**
- [ ] Three invoice drops, Activity feed visible, Outputs tab shows the ledger
- [ ] Expense question shows bars
- [ ] Insights tab shows figure drift, reversals and deadlines
- [ ] Total length under 3:00 (the video limit); trim pauses, not words

## If something goes wrong on camera

* Click **Reset demo** and retake. State is fully deterministic.
* No internet needed. If you set `ANTHROPIC_API_KEY`, wording will differ slightly from this script.
