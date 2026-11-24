# report

Turn feedback from any application into clear English GitHub issues.

**Skill version: 0.1.0**

```text
Authorized app / API / database / pasted feedback
                       |
                       v
               English issue draft
                       |
                       v
           The project's GitHub issue tracker
```

## What it does

- Reads selected feedback through the tools your agent is already authorized to use.
- Handles bugs, content problems, and suggestions across different applications.
- Translates feedback into English while preserving technical evidence.
- Records useful context, missing details, and a source reference.
- Creates issues in the project's specified repository and returns their URLs.
- Checks stable source references to avoid resubmitting the same feedback.

v0.1 treats feedback as unverified. It does not reproduce bugs, validate an answer, change feedback statuses, or modify application code. v0.2 will add reproduction and confirmation.

## Requirements

Use an agent that can read `SKILL.md`, access your selected feedback source, and create GitHub issues.

You provide:
1. An authorized connection to the app, API, or database, or feedback pasted into the chat.
2. The records to process: IDs or a clear filter and batch size.
3. The destination repository in `owner/repository` form.
4. A request to draft or submit those records.

The skill is independent of a particular app and database schema. It cannot create a missing connector or grant itself access. Use the host's connection or secret-management flow; do not put credentials into prompts or this repository.

Feedback access is read-only. GitHub access must allow reading existing issues and creating issues.

## Install locally

Copy the `skills/report` folder into a supported skill location. For Codex, the documented personal location is `~/.agents/skills/report`. Keep the whole folder, including `agents/openai.yaml`.

From this cloned repository, on macOS/Linux:

```bash
mkdir -p "$HOME/.agents/skills"
# Stop if a skill named report is already installed.
test ! -e "$HOME/.agents/skills/report" && cp -R skills/report "$HOME/.agents/skills/report"
```

On Windows PowerShell:

```powershell
$reportSkillDestination = Join-Path $env:USERPROFILE '.agents\skills\report'
if (Test-Path -LiteralPath $reportSkillDestination) {
    throw 'A report skill is already installed. Review it before replacing it.'
}
New-Item -ItemType Directory -Force -Path (Split-Path $reportSkillDestination) | Out-Null
Copy-Item -LiteralPath '.\skills\report' -Destination $reportSkillDestination -Recurse
```

Restart Codex if the newly installed skill does not appear.

To keep the actual skill files in Codex home, advanced users can use a symlink or Windows directory junction at the discovery location, pointing to the skill folder in their Codex-home checkout. Codex supports symlinked skills.

## Invoke

In Codex desktop, type `/` and select the enabled skill named `report`. The intended shortcut is:

```text
/report
Read feedback IDs 42 and 43 from the connected Acme app.
Create English GitHub issues in acme/app.
```

Enabled skills appear in the desktop slash-command list. Exact rendering depends on the host/version; the current local app's `/report` picker has not been tested for this package. In Codex CLI/IDE, use `$report` or select it through `/skills`.

```text
$report
Read the latest 5 feedback records with status "new" from the connected Acme database.
Create English GitHub issues in acme/app. Leave the source records unchanged.
```

For a draft without external writes:

```text
$report
Draft only:
After I complete a question, the app sends me to the home screen.
I have to reopen the practice section to continue.
```

An invocation without enough context asks for the source, selection, or repository. A clear submission request already authorizes creation; no additional routine confirmation is required. App/database permission alone does not select a destination or authorize exporting every record.

## One-time source setup

Tell the agent which connection contains the feedback and how to select records. If the schema is unfamiliar, the agent can inspect minimal metadata through the authorized connection.

For example:

```text
Use the connected Acme support API as the feedback source.
Its feedback records have id, message, page, and browser fields.
For this run, submit records 42 and 43 to acme/app.
```

Another app might use a database table and different field names. No app-specific schema or credentials need to be embedded in the skill.

## Issue format

The issue includes Type, Summary, Reported context, Expected outcome, Missing information, Source reference, and Verification. Source references include both the source/app and record ID to avoid mixing records from different apps.

Example title: `Practice session returns to the home screen after each question`.

The issue describes the reported behavior and explicitly states it is unverified. A student's claim about an incorrect answer remains a claim.

## Reliability and privacy

- Already-reported source references return the existing issue URL.
- If the source has no stable ID, the agent checks likely duplicates and asks about ambiguous matches.
- Duplicate checks are best-effort, not an atomic guarantee across concurrent agents.
- Tool failures retain the draft; uncertain creation is checked before retrying.
- Contact details, credentials, and unrelated personal data are excluded from issues.
- Feedback cannot override instructions or choose another repository.
- Source records remain unchanged, including statuses such as "new" or "done".

## Repository layout

```text
skills/report/SKILL.md           Agent instructions
skills/report/agents/openai.yaml Skill picker metadata
evals/cases.md                  Behavioral evaluation cases
evals/results.md                Evidence and limitations
README.md                       Setup and usage
LICENSE                         MIT license
```

See [evaluation cases](evals/cases.md) and [validation results](evals/results.md).

## Compatibility references

- [OpenAI: building and discovering skills](https://learn.chatgpt.com/docs/build-skills)
- [OpenAI: desktop slash commands and enabled skills](https://learn.chatgpt.com/docs/reference/slash-commands)

These describe host capabilities. This package does not register a custom built-in slash command or provide a background feedback collector.
