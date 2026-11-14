# RYX

### Reverse Engineer Your Ex

You know the feeling. You scroll back through an old chat and you can hear her in it — not her voice, exactly. The way she typed. Four short messages in a row instead of one long one. The same emoji, every time. A typo she never fixed. The way she opened a conversation, and the name she called you, and no one else did.

RYX is planned as an open-source website that keeps that way of writing alive, so you can keep talking in it.

Status: planning phase. No code exists yet. Nothing here is shipped.

License: MIT. For every language.

---

## Contents

- [The idea](#the-idea)
- [Why this exists](#why-this-exists)
- [What it is not](#what-it-is-not)
- [How it is planned to work](#how-it-is-planned-to-work)
- [Planned stack](#planned-stack)
- [Privacy and consent](#privacy-and-consent)
- [Getting the whole chat out](#getting-the-whole-chat-out)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## The idea

People keep years of conversations in screenshots. Not because they planned to. Because deleting them felt wrong.

RYX is planned to read those screenshots, pull out the message turns, and learn the shape of her writing: how long her messages ran, which emojis she reached for, when she mixed languages, how she opened a chat, what she called you.

Then you can talk to an open-weight model that writes in that shape, on your own machine. Your screenshots, your database, your machine. Nothing leaves it unless you say so.

## Why this exists

Sometimes you miss how someone talked to you. Not the whole person, not the whole story. Just the sound of them in text.

RYX is for that feeling. It does not bring anyone back, and it does not pretend to. It just keeps the way they wrote close, so you can talk in that style for a while longer.

## What it is not

It is not her. It copies the texture of the writing, only from the screenshots you give it.

It cannot learn memories that are not in the screenshots, tone of voice, or anything outside the chat.

It is not therapy, and not a substitute for a real relationship.

It must not be used to contact, deceive, or impersonate the real person. Every reply has to say, in the chat itself, that it is AI.

## How it is planned to work

1. Upload screenshots of the conversation.
2. A vision model reads the bubbles and pulls out speaker, text, and order.
3. You correct the speaker mapping once — which side is her, which side is you.
4. RYX builds a style profile from the cleaned turns.
5. The chat engine uses that profile, with an optional trained adapter if you want it.

## What the model is planned to learn

Message length and rhythm, including the habit of sending four short messages in a row. Emoji, sticker, and punctuation habits. Language mixing, slang, and the typos that show up again and again — for every language, not one pair. Openers, nicknames, and running jokes that only make sense if you were there. When a reply goes dry, when it turns playful, when it changes the subject for no reason at all.

## Planned stack

| Layer | Planned choice |
| --- | --- |
| Web app | Next.js, React, TypeScript |
| Screenshot reading | Qwen2.5-VL 7B, local |
| Chat model | Llama 3.1 8B or Qwen2.5 7B |
| Fine-tuning | Unsloth + QLoRA |
| Inference | Ollama for dev, vLLM for serving |
| Storage | SQLite |

These are the planned choices, not tested ones. Model versions will be pinned when code lands.

## Planned setup

No code exists, so this does not work yet. This is the setup we are aiming for:

```bash
git clone https://github.com/coldabs/ryx.git
cd ryx
pnpm install
cp .env.example .env.local
pnpm dev
```

Then open `http://localhost:3000`, upload a few screenshots, and start a chat.

## Privacy and consent

Local by default. Nothing leaves your machine unless you set up a remote server yourself.

Upload only conversations you have permission to use. Do not commit screenshots, databases, or trained adapters to this repo. Deleting a profile has to delete its uploads, its database rows, and its adapter — all of it, or it is a bug.

18+ only.

## Getting the whole chat out

This is the problem to solve first, and it is a stubborn one. No app gives you one clean button for one person's whole history.

WhatsApp lets you export one chat, but caps it at about 40,000 messages. Instagram and Messenger make you export your whole account, then go find that one chat inside it. Screenshots work for a short chat, but not for years of history — and OCR can miss things, or misread them.

So RYX has to make getting the chat out easy, or nothing else matters. The plan is a guided export in the app, step by step, in plain language — what to tap, what file you will get. RYX will take what you can get: a partial export, several exports, screenshots, or a mix of all three, stitch them together, and de-duplicate them. And it will not ask for the whole history. A sample across time — early, middle, recent — is enough to learn the style.

| App | How to get it | What RYX will accept |
| --- | --- | --- |
| WhatsApp | In the chat: Export Chat, Without Media — a `.txt` file, one chat at a time, up to about 40,000 messages | `.txt` |
| Instagram / Messenger | Meta Accounts Center: Export your information, Messages only, All time, JSON — each chat is in its own folder | JSON |

Deleted messages do not appear in any export. Encrypted chats may need a separate download.

## Open questions

A few things are still undecided, and we would rather decide them properly than guess:

- Which vision model handles the most chat apps and languages best?
- How should the speaker-correction step work on a phone, where most of this will happen?
- How much of the style profile should a user be able to edit by hand?

## Roadmap

- [ ] Guided chat download and import — the first milestone, because getting the whole chat out is the main problem
- [ ] Screenshot ingest and speaker correction
- [ ] Style profile viewer and editor
- [ ] Prompt-only chat
- [ ] Optional training and adapter export
- [ ] Memory search, voice notes, and multi-profile support

## Contributing

Issues and pull requests are welcome. Do not include screenshots or personal data in them. For anything that touches training, open an issue first, so the dataset format stays stable.

A human reads every issue and pull request. Nothing merges on its own.

## License

MIT. See [LICENSE](LICENSE). Model weights keep their own licenses.
