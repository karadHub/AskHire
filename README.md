# AskHire

AskHire is a small Streamlit-based chatbot that represents Vaibhav Karad and answers questions about his experience, skills, and projects.

Key features

- Chat UI built with Streamlit
- Uses Google Gemini (via `google-generativeai`) when an API key is provided
- Safe local fallback responder so the app remains usable without Gemini
- Tools for recording contact details and logging unanswered questions (integrates with Pushover if configured)
- Generates follow-up question suggestions to help users continue the conversation

Quickstart

1. Create and activate a virtual environment (recommended):

   On Windows (PowerShell):

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   On Git Bash / WSL (bash):

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies:

   ```bash
   python -m pip install --upgrade pip
   python -m pip install -r requirement.txt
   ```

3. (Optional) Provide environment variables in a `.env` file or your environment. Supported variables:

   - `GEMINI_API_KEY` — API key for Google Generative AI (Gemini). If missing, the app falls back to a local responder.
   - `PUSHOVER_TOKEN` and `PUSHOVER_USER` — optional credentials to receive push notifications when tools are used.
   - `GEMINI_API_KEY` and `PUSHOVER_*` can also be provided via your environment or CI secrets.

4. Run the app:

   ```bash
   streamlit run main.py
   ```

Notes on behavior

- If `GEMINI_API_KEY` is set and `google-generativeai` is installed, the app will attempt to use Gemini and its function-calling features.
- If Gemini is not available (no package or no key), the app uses a lightweight local responder that:
  - Answers simple keyword-based queries from the candidate's summary/CV/LinkedIn
  - Detects contact information and triggers `record_user_details`
  - Logs unknown questions with `record_unknown_question`
- After each reply, the app generates suggested follow-up questions to guide the user. These suggestions are rendered in the UI (buttons) and can be used to prefill or auto-send follow-up messages.

Development notes

- Personal data files are read from the `me/` folder (`me/summary.txt`, `me/cv.pdf`, `me/linkedin.pdf`). Keep sensitive data out of the repo or add `me/` to `.gitignore` if needed.
- The code intentionally keeps the tool functions (recording contact or unknown questions) small and local — you can replace `push()` with other integrations (email, database, analytics).

Contributing

- Open an issue or submit a PR. Keep changes small and include tests or a short demo when changing behavior.

License

- This project is released under the MIT License — see `LICENSE` for details.

Contact

- For quick local testing, run the app and interact through the Streamlit UI at `http://localhost:8501`.
