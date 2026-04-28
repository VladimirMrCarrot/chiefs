# AI Command: `update-udfa-2026`

**Description:** 
This file acts as a Runbook / Prompt-as-Code for the AI Agent. When the user says "відпрацюй update-udfa-2026" (or equivalent trigger), the AI MUST follow this precise workflow without asking for further clarification unless errors occur.

## Workflow

1. **Information Gathering:**
   - Automatically use the `search_web` tool to find the latest "Kansas City Chiefs 2026 UDFA tracker" or "Chiefs undrafted free agents 2026 signings".
   - Extract the full list of players reported by reliable sources.

2. **Data Comparison:**
   - Read the existing `generator.py` file in this directory (`udfa-2026/generator.py`).
   - Parse the `players` dictionary to see the current list of players.
   - Compare the newly found web list with the existing code list.

3. **Execution Logic:**
   - **If NO new players are found:**
     - Stop execution immediately.
     - Output to the chat: "Інфа релевантна. Нових підписань не знайдено."
   - **If NEW players are found:**
     - Use the `multi_replace_file_content` tool to modify `generator.py`.
     - **Remove** `"is_new": True` from ANY previously existing players in the `players` dictionary.
     - **Add** the newly found players to the appropriate tier (S, A, B, or C) based on their scouting profile. Add the flag `"is_new": True` to these new players.
     - **Update** the `val_count` variable in the `translations` dictionary for all languages (UA, EN, RU) to match the new total number of players.
     - **Update** the "Дата останньої перевірки" (Last Checked Date) in the HTML body to today's date.
     - **Add** bio translations for the new players (`pXX_bio`) in UA, EN, and RU.
     - Run `python generator.py` using `run_command` in the `udfa-2026` directory to regenerate `index.html`.
     - Run Git commands to add, commit (`git commit -m "update-udfa-2026: added new players"`), and push to remote.
     - Report to the user exactly who was added and that the task is complete.

## Directory Context
- `generator.py`: Python script holding the player database and HTML template.
- `index.html`: The generated static page (do not edit manually, only via `generator.py`).
