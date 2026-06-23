# Architecture

This project implements an automated voice bot that simulates a patient
calling a medical clinic's AI scheduling agent. Twilio handles the
outbound phone call and streams live audio over a WebSocket connection
to a local FastAPI server. That audio is forwarded in real time to
OpenAI's Realtime API, which handles speech-to-text, language model
reasoning, and text-to-speech in a single unified pipeline. The model's
audio response streams back through Twilio into the live call, creating
a natural two-way voice conversation. ngrok exposes the local server to
the public internet so Twilio can reach it during development. Call
recordings are captured automatically via Twilio, and transcripts are
saved locally after each call ends.

Key design decisions: OpenAI's Realtime API was chosen over a separate
STT + LLM + TTS pipeline because it bundles all three into one streaming
connection, reducing latency and complexity while producing more natural
turn-taking. Semantic VAD with high eagerness controls when the bot
responds, minimizing delay while keeping interrupt_response disabled to
prevent talking over the agent. A 9-second IVR grace period with audio
buffer clear prevents the bot from responding to the automated disclaimer
at the start of each call. Scenario routing is handled server-side via
a dictionary keyed to the Twilio call SID, after finding that URL query
parameters were being dropped by Twilio's infrastructure. Ten distinct
patient personas cover all required test categories including scheduling,
refills, billing disputes, and edge cases.
