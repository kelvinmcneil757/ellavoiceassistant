# 🤖 Ella AI – Conversational Scheduling Agent

Ella is your always-on AI voice assistant, built to handle inbound phone calls, respond to FAQs, check real-time calendar availability, and book meetings — all without human intervention. Powered by **n8n**, **OpenAI GPT-4o-mini**, and **Google Calendar**, this smart system makes your voice interface feel like a concierge-level experience.

---

## 🧠 System Overview

Ella is powered by two key n8n workflows:

1. **`EllagetAvailability`** – Checks if a user-requested time is free in your calendar, then intelligently responds with availability or alternatives.
2. **`EllaScheduleBooking`** – Books the meeting directly into your Google Calendar and confirms it conversationally with the caller.



These workflows combine AI reasoning, smart date handling, and calendar tooling into a seamless voice-first UX.

---

## 🔗 Workflow 1: `EllagetAvailability` – 📅 Check Calendar Availability

### 💡 Flow Summary:
This workflow handles the **first step** in the user interaction. When a user requests a date/time, it confirms availability or suggests alternate slots.

### 🧩 Key Nodes:
- `Webhook` – Receives incoming request with a desired `date` and `time`.
- `Date & Time` – Standardizes timestamps.
- `Google Calendar Tool` – Queries the calendar for availability.
- `OpenAI Chat Model + AI Agent` – Uses GPT-4o-mini to respond naturally with:
  - ✅ Confirmation of requested time if available
  - 🔄 Up to 3 smart alternatives if not
  - ⏭️ Next available weekday if no times exist
- `Respond to Webhook` – Sends response back to the voice system.

### 🗣 Example:
> "Unfortunately, 2:00 PM on Friday is booked. But I can do 3:15 PM, 4:00 PM, or 4:45 PM. What works best for you?"

---

## 🔗 Workflow 2: `EllaScheduleBooking` – 📆 Finalize the Booking

### 💡 Flow Summary:
This workflow **completes the interaction** by scheduling the confirmed meeting time and logging it to Google Calendar.

### 🧩 Key Nodes:
- `Webhook` – Collects user’s name, email, date, and time.
- `Date & Time` – Ensures clean, timezone-aware timestamps.
- `AI Agent` (LangChain) – Reformats the time to **RFC3339**, confirms details, and responds with a personalized wrap-up.
- `Output Parser + Fixer` – Autocorrects formatting errors in AI output to ensure proper Google Calendar compatibility.
- `Google Calendar` – Creates a 30-minute event with:
  - 📧 Attendee email
  - 📝 Summary like: `FIGS AI Demo Scheduled by Ella w/ Mike`
- `Respond to Webhook` – Returns a conversational confirmation to the voice assistant.

### 🗣 Example:
> "You're all set, Mike! I’ve scheduled your demo for June 13th at 2:00 PM. Let me know if there's anything else I can help with."

---

## 🔍 Tech Stack

- **n8n** – Low-code orchestration engine
- **OpenAI GPT-4o-mini** – Conversational intelligence
- **Google Calendar API** – Real-time scheduling & availability
- **LangChain Output Parsers** – Clean, robust AI responses
- **Webhook Triggers** – Compatible with voice agents (e.g., ElevenLabs, Twilio, etc.)

---

## 🔄 Workflow Integration

These workflows are designed to **work in sequence**:
1. `EllagetAvailability` is triggered first during the phone conversation.
2. If the user confirms, `EllaScheduleBooking` is triggered with the chosen time.

Both return human-like responses and are optimized for **real-time, natural dialogue**.

---

## 🌐 Potentially Coming Soon (we will see if I continue working on this) 

- ✨ Vector database support for contextual FAQs via RAG (Retrieval-Augmented Generation)
- 📁 Supabase or Airtable integration for enterprise-level knowledge bases
- 📊 CRM sync & post-call analytics

---

## 🚀 Deploy This

1. Deploy both workflows in your **n8n instance**.
2. Connect them to your **voice agent’s webhook callbacks**.
3. Set up your **Google Calendar OAuth** credentials.
4. Sit back and let Ella handle your scheduling like a pro.

---

Want help scaling Ella for your business or turning it into a branded voice assistant? [Let’s talk.](mailto:info@figsai.com)
