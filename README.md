# kaiju-quiz-packs
Quiz Packs for Kaiju Quiz

## Background
This repository contains quiz packs that can be imported into Kaiju Quiz for educational purposes, similar to flash cards.  Anyone can contribute a quiz pack of any topic.

## Quiz Pack - Users
Since these quiz packs can be contributed by anybody and is often created with the help of AI, the accuracy and quality of these quiz packs vary.  **Users must not blindly accept the answers as authoritative.** Always indepndently verify the answers to the questions.

## Quiz Pack - Contributors

You’ll get the most accurate answers by:
1. Constraining what the AI can do
2. Telling it exactly how to format output
3. asking it to justify or cross-check each correct option. 

### Guidelines

- Define scope and difficulty clearly  
  State topic, subtopics, and target level (e.g., “intro college biology, basic recall + application”). This reduces off-topic or overly hard questions.

- Require one clearly correct answer  
  Tell the AI: “Each question must have exactly one unambiguously correct option.” This matches best practices in assessment design. 

- Ask for evidence-based keys  
  Instruct it to add a short explanation and (if relevant) mention the concept, rule, or data that makes the correct answer right. This surfaces mistakes so you can spot them. 

- Make distractors plausible, not random  
  Ask for distractors based on common mistakes or misconceptions, and require them to be similar in length, style, and level of detail to the correct answer.
  
- Use simple, direct question stems  
  Request clear, positive wording (avoid “Which of the following is NOT…?” unless truly needed) and one idea per question.

- Fix the structure and format
  Refer to the quiz schema section below.
  
- Limit number of options  
  Ask for 3–4 options per question; this is usually enough to discriminate knowledge without clutter.
  
- Add a built-in self-check  
  You can add: “After generating each question, internally re-check that the correct answer matches the explanation and that no distractor could be argued correct.” This reduces obvious inconsistencies.
  
- Use few-shot examples  
  Include one or two of your own “gold-standard” questions plus answers/explanations in the prompt, and say “Follow this style.” This is a strong prompt-engineering technique for quality.

## Quiz Pack - Schema
Packs are plain JSON files.

### Top-level structure

```json
{
  "id": "unique-pack-id",
  "name": "Display Name",
  "description": "Short description shown on the pack card.",
  "questions": [ ... ]
}
```

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | `string` | yes | Must be unique across loaded packs |
| `name` | `string` | yes | Shown in the pack list and results screen |
| `description` | `string` | yes | One-line summary |
| `questions` | `QuizQuestion[]` | yes | At least 1 question required |

### Question structure

```json
{
  "id": "q1",
  "question": "The question text displayed to the player.",
  "answers": ["Option A", "Option B", "Option C", "Option D"],
  "correctAnswers": [1],
  "type": "single",
  "defaultTimeSeconds": 30
}
```

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | `string` | yes | Unique within the pack |
| `question` | `string` | yes | The question prompt |
| `answers` | `string[]` | yes | 2–4 answer choices (exactly 2 for `true-false`) |
| `correctAnswers` | `number[]` | yes | Zero-based indices into `answers`; one index for `single`/`true-false`, multiple for `multiple` |
| `type` | `"single"` \| `"multiple"` \| `"true-false"` | yes | Controls layout and submit behaviour |
| `defaultTimeSeconds` | `number` | yes | Per-question countdown in seconds when no override is set |
| `references` | `string[]` | no | Optional list of sources shown in Review Answers (plain text or URLs — URLs are rendered as clickable links) |

### Single-answer example

```json
{
  "id": "capitals-1",
  "question": "What is the capital of Japan?",
  "answers": ["Osaka", "Tokyo", "Kyoto", "Hiroshima"],
  "correctAnswers": [1],
  "type": "single",
  "defaultTimeSeconds": 20,
  "references": ["https://www.britannica.com/place/Tokyo"]
}
```

`correctAnswers: [1]` means index 1 — "Tokyo" — is correct. Auto-submits on selection.

### Multiple-answer example

```json
{
  "id": "planets-1",
  "question": "Which of the following are gas giants in our solar system?",
  "answers": ["Mars", "Jupiter", "Saturn", "Venus"],
  "correctAnswers": [1, 2],
  "type": "multiple",
  "defaultTimeSeconds": 45
}
```

`correctAnswers: [1, 2]` means "Jupiter" and "Saturn" must both be selected (and nothing else) to score the question correct.

### True/False example

```json
{
  "id": "tf-1",
  "question": "The Great Wall of China is visible from space with the naked eye.",
  "answers": ["True", "False"],
  "correctAnswers": [1],
  "type": "true-false",
  "defaultTimeSeconds": 15,
  "references": ["NASA, 'The Great Wall', https://www.nasa.gov/vision/space/workinginspace/great_wall.html"]
}
```

`answers` must always be `["True", "False"]` in that order. `correctAnswers: [0]` = True, `correctAnswers: [1]` = False. Renders as two large side-by-side buttons; auto-submits on selection.

### Minimal complete pack

```json
{
  "id": "my-pack-001",
  "name": "My First Pack",
  "description": "A starter pack with one question.",
  "questions": [
    {
      "id": "q1",
      "question": "Which planet is closest to the Sun?",
      "answers": ["Venus", "Earth", "Mercury", "Mars"],
      "correctAnswers": [2],
      "type": "single",
      "defaultTimeSeconds": 30
    }
  ]
}
```
