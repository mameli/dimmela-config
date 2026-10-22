# Prompts for Dimmela

[Dimmela](https://dimmela.app) is a macOS menu-bar app for dictation. It transcribes your speech locally and pastes the text where you are typing. Optionally, an LLM rewrites each Dictation with a Mode: a prompt file or a skill that you choose.

This repository contains the prompt files I use as Modes. They remove false starts and repetitions, resolve spoken corrections, fix likely recognition errors and improve punctuation, without summarizing or changing what you meant.

Five prompts are available:

- [`prompt.md`](prompt.md) cleans up a Transcript in its original language.
- [`translate-to-english.md`](translate-to-english.md) cleans up the Transcript and translates it into natural English.
- [`translate-to-x.md`](translate-to-x.md) turns an Italian dictation into a natural English post or reply for X.
- [`compose-x-post.md`](compose-x-post.md) turns a rambling spoken brief about what you want to say into a finished X post. It captures the intent instead of transcribing the words.
- [`compose-text.md`](compose-text.md) does the same for any kind of text, such as an email, a chat message or a note, and adapts the tone to the recipient.

## Setup

1. Install Dimmela:

   ```sh
   brew install --cask mameli/dimmela/dimmela
   ```

2. Clone this repository.
3. In **Settings › Post-processing**, set the provider. Dimmela works with any OpenAI-compatible provider. I use [OpenRouter](https://openrouter.ai/) with the model `anthropic/claude-haiku-5.5` and **Reasoning** set to **Low**. Paste your API key and click **Test Connection**.
4. In **Settings › Modes**, click **Add Folder…** under **Mode sources** and select your clone.
5. Under **Available**, enable the prompts you want. Drag them to set their order: the first nine get the Mode digits 1–9. Select the **Default Mode**.
6. Hold or tap `fn+Space` and speak. While you dictate, press a digit to use another Mode, `0` for no Post-processing, or `shift` to show the list.

## How Dimmela uses these files

Dimmela reads the prompt file at every Dictation, so your edits apply to the next one. It sends the file as the system prompt, followed by the terms in your Vocabulary, and the Transcript as the user message.

The `${output}` line at the end of each file comes from Handy, where these prompts started. Dimmela removes it before it sends the prompt.

## What the prompts handle

- Filler words, stuttering, repetitions and abandoned sentences
- Spoken corrections such as “five tests, actually six”
- Dictation commands such as “delete this part” or “write this instead”
- Likely speech-recognition errors inferred from context
- Grammar, punctuation, paragraphing and natural sentence structure
- Numbers, dates, measurements, names and negations
- Dictated filenames containing words such as “underscore” and “dot”
- Incomplete time references, without inventing “today,” “tomorrow” or similar details

## Improving the prompts

In **Settings › Post-processing**, turn on **Keep a Post-processing log**. Dimmela then writes each Transcript, Mode, model and LLM answer to `~/.dimmela/post-processing.jsonl`. Add a `"comment"` where the answer was not what you wanted, and give the log to a model to improve the prompt.

## Privacy and cost

Dimmela transcribes your speech on your Mac. When a Dictation uses a Mode, Dimmela sends the Transcript, the prompt and your Vocabulary to your provider. Your audio is never sent.

The cost depends on the model and on the length of the prompt and Transcript. Provider prices and model availability can change.

## References

- [Dimmela](https://dimmela.app)
- [Dimmela repository](https://github.com/mameli/Dimmela)
- [OpenRouter](https://openrouter.ai/)
