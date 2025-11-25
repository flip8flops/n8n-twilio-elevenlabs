# n8n-twilio-elevenlabs

Showcase workflow for an AI voice agent that collects a phone number via an n8n form, triggers an outbound call through Twilio, and uses ElevenLabs Conversational AI to greet the caller in Indonesian while retelling the name they provided.

---

## Highlights

- **Interactive form** – A public n8n form titled `Call Me` asks for the caller’s name and phone number.
- **Outbound call** – Submissions trigger ElevenLabs’ `convai/twilio/outbound-call` API, which dials the provided number using a Twilio-connected agent line.
- **Voice storytelling** – The ElevenLabs agent interprets the caller’s name and narrates it back in Bahasa Indonesia, creating a friendly localized experience.
- **Seamless integration** – ElevenLabs’ call agent API is connected to a Twilio phone number, so no extra telephony wiring is required.

---

## Workflow Breakdown

| Step | Node | Purpose |
| --- | --- | --- |
| 1 | `On form submission` (`formTrigger`) | Presents the `Call Me` form and captures **name** + **phone number** (both required). |
| 2 | `HTTP Request` | Issues a POST request to `https://api.elevenlabs.io/v1/convai/twilio/outbound-call`, passing the `agent_id`, `agent_phone_number_id`, and `to_number` (mapped from the submitted phone field). |

All HTTP calls use custom credentials (`Custom Auth account 11Labs call agent`) plus the required `xi-api-key` header. Twilio phone connectivity is handled inside ElevenLabs’ API, so n8n only needs to supply the target number.

---

## Repository Contents

- `main.json` – Primary workflow (form trigger → ElevenLabs outbound call).
- `sub01.json` – Placeholder workflow for future automation ideas.

Import these JSON files into n8n via **Import from file** to recreate the setup.

---

## Getting Started

1. **Prerequisites**
   - n8n instance (self-hosted or cloud).
   - ElevenLabs account with access to the Conversational AI (ConvAI) Twilio agent endpoint.
   - Twilio number already linked inside ElevenLabs (the API handles dialing).

2. **Configure Credentials**
   - HTTP Custom Auth: store your ElevenLabs call-agent token (`xi-api-key`).
   - (Optional) HTTP Basic Auth if you route through an authenticated proxy/webhook.

3. **Deploy the Form**
   - Activate `main` workflow.
   - Share the generated public form URL so users can submit their name and phone number.

4. **Test the Call**
   - Submit the form with your number.
   - The agent should call you, ask for confirmation, and retell your name in Indonesian using ElevenLabs’ synthesized voice.

---

## Customization Ideas

- Personalize the Indonesian narrative or add more contextual prompts in ElevenLabs.
-- Extend the form with additional questions (preferred pronunciation, occasion, etc.).
- Chain post-call actions (e.g., log to Google Sheets, send a recap message via Twilio SMS).

---

## Status & Roadmap

- ✅ Prototype workflow wired to ElevenLabs + Twilio.
- 🚧 Next: enrich post-call follow-ups, add multi-language support, and publish a live demo link.

Contributions and suggestions are welcome—open an issue or submit a PR!