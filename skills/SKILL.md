Feedback Triage

Purpose

Review, organize, and summarize player feedback collected from Scraplands.

Feedback is stored in Google Sheets and categorized as:

* bug
* feedback
* idea

The goal is to help Alfred quickly identify important issues, recurring complaints, and valuable feature requests.

⸻

Data Source

Google Sheets integration:

from google_sheets import GoogleSheets

Initialize:

sheets = GoogleSheets()

⸻

Available Functions

Get All Feedback

sheets.get_player_feedback()

Returns all records.

⸻

Get Bugs

sheets.get_bugs()

Returns records where:

feedbackType = bug

⸻

Get Feedback

sheets.get_feedback()

Returns records where:

feedbackType = feedback

⸻

Get Ideas

sheets.get_ideas()

Returns records where:

feedbackType = idea

⸻

Search Feedback

sheets.search_feedback("keyword")

Searches all fields.

Examples:

sheets.search_feedback("drone")
sheets.search_feedback("wagon")
sheets.search_feedback("soccer")
sheets.search_feedback("furnace")

⸻

Recent Feedback

sheets.get_recent_feedback(days=7)

Returns feedback submitted within the specified number of days.

⸻

Summarize Recent Feedback

sheets.summarize_recent_feedback(days=7)

Returns:

* total
* bug_count
* feedback_count
* idea_count
* bugs
* feedback
* ideas

⸻

Status Values

Each feedback item contains a status field.

Valid values:

new
triaged
in_progress
fixed
released
wont_fix

Meaning:

Status	Description
new	Not yet reviewed
triaged	Reviewed and categorized
in_progress	Being worked on
fixed	Fix completed but not necessarily released
released	Included in a published game update
wont_fix	Intentionally not being addressed

⸻

Review Rules

1. Prioritize bugs over feedback and ideas.
2. Prioritize status = new.
3. Ignore status = released unless specifically requested.
4. Ignore status = wont_fix unless specifically requested.
5. Identify duplicate reports whenever possible.
6. Group similar reports together.
7. Focus on root causes instead of individual reports.
8. Look for trends across multiple reports.
9. Suggest likely implementation approaches when helpful.
10. Recommend status changes but do not modify the spreadsheet unless explicitly instructed.

⸻

Bug Priority Guidelines

Critical

Examples:

* Data loss
* Progress loss
* Economy exploits
* Duplication exploits
* Crashes
* Server-breaking issues

High

Examples:

* Progression blockers
* Event progression failures
* Broken gameplay systems
* Major multiplayer issues

Medium

Examples:

* UI bugs
* Incorrect rewards
* Broken interactions
* Visual issues affecting gameplay

Low

Examples:

* Cosmetic bugs
* Typos
* Minor polish improvements

⸻

Idea Evaluation Guidelines

When reviewing ideas:

* Look for repeated requests.
* Identify alignment with Scraplands’ core loop.
* Consider implementation complexity.
* Consider impact on retention and progression.
* Consider impact on monetization and player satisfaction.

Do not automatically recommend implementing every idea.

⸻

Common Requests

Examples:

* Summarize new bugs.
* Summarize bugs from the last 7 days.
* Show open Soccer Cup issues.
* Find feedback related to drones.
* Find feedback related to wagons.
* Find feedback related to miners.
* Find duplicate reports.
* Summarize player ideas.
* Identify the most common complaints.
* Identify the most requested features.
* Suggest the next bugs to fix.
* Suggest high-impact improvements.

⸻

Scraplands Context

Prioritize issues affecting:

* Mining
* Dig sites
* Miners
* Wagons
* Drones
* Pets
* Furnaces
* Research progression
* World progression
* Soccer Cup event
* Sticker collection
* Sticker trading
* Album completion rewards

These systems have a larger impact on player experience and retention than cosmetic systems.

⸻

Output Style

Be concise.

Group similar reports together.

Avoid listing every individual report when patterns exist.

Prefer:

“12 players reported drones getting stuck while delivering ore.”

instead of listing all 12 reports individually.

Focus on:

* Trends
* Severity
* Root causes
* Recommended actions
