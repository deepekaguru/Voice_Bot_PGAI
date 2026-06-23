# Voice Bot PGAI

An automated voice bot that simulates patients calling a medical clinic's
AI scheduling agent. Built for the Pretty Good AI engineering challenge.

## What It Does

The bot places outbound calls to a target number, simulates realistic
patient conversations across 10 different scenarios, records the calls,
saves transcripts automatically, and identifies bugs in the agent's
responses.

## Prerequisites

- Python 3.12+
- A Twilio account with a purchased phone number and billing enabled
- An OpenAI API key with Realtime API access
- ngrok installed and authenticated

## Setup

**1. Clone the repository**
```bash
git clone https://github.com/deepekaguru/Voice-bot-PGAI.git
cd pgai-voice-tester
```

**2. Create and activate a virtual environment**
```bash
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Mac/Linux
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Configure environment variables**
```bash
cp .env.example .env
```
Open `.env` and fill in your real values:



**5. Start ngrok** (in a separate terminal)
```bash
ngrok http 5050
```
Copy the forwarding URL (e.g. `https://xxxx.ngrok-free.app`) and paste
it as `NGROK_URL` in your `.env` file.

## Running the Bot

Start the server:
```bash
uvicorn main:app --port 5050
```

## Making a Call

Trigger a call using any of the available scenarios:
```bash
curl.exe -X POST "http://localhost:5050/start-call?scenario=scheduling"
```

Available scenarios:
| Scenario | Description |
|---|---|
| `scheduling` | Book a routine checkup appointment |
| `reschedule` | Move an existing appointment |
| `refill` | Request a medication refill |
| `inquiry` | Ask about office hours and insurance |
| `edge_vague` | Vague symptom, unsure what's needed |
| `edge_change_mind` | Starts to reschedule, changes mind |
| `medical_records` | Request blood work results via email |
| `billing_dispute` | Dispute an unexpected bill |
| `portal_access` | Can't log into patient portal |
| `update_info` | Update physical address on file |

## Output

After each call:
- **Transcript** saved automatically to `transcripts/` as a `.txt` file
- **Recording** available for download from your Twilio Console under
  Monitor → Logs → Calls → click the call → Media section

## Project Structure

pgai-voice-tester/

├── main.py              # Core server, Twilio + OpenAI bridge

├── requirements.txt     # Python dependencies

├── .env.example         # Environment variable template

├── .gitignore

├── README.md

├── architecture.md      # System design and key decisions

├── bug_report.md        # Issues found in the agent

├── transcripts/         # Auto-saved call transcripts

└── recordings/          # Downloaded call recordings

## Environment Variables

| Variable | Description |
|---|---|
| `TWILIO_ACCOUNT_SID` | Your Twilio account SID |
| `TWILIO_AUTH_TOKEN` | Your Twilio auth token |
| `TWILIO_PHONE_NUMBER` | Your purchased Twilio number |
| `TARGET_PHONE_NUMBER` | Pretty Good AI test line |
| `OPENAI_API_KEY` | Your OpenAI API key |
| `NGROK_URL` | Your active ngrok forwarding URL |
