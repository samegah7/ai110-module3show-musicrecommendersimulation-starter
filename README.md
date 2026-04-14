# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

This project is a simplified version of how real music apps figure out what to play next. I built a scoring system that looks at a user's taste profile and compares it against a small catalog of songs, giving each song a score based on how well it matches. The top-scoring songs are what get recommended.

---

## How The System Works

Real-world recommenders like Spotify or YouTube use two main strategies: collaborative filtering (looking at what similar users listened to) and content-based filtering (looking at the actual features of a song like its tempo or mood). My version focuses on content-based filtering because it's more transparent — you can actually explain *why* a song got recommended instead of just saying "other people liked it." The system prioritizes mood and energy above everything else, because those feel like the most immediate signals for what someone actually wants to hear in a given moment. Genre matters too, but I weighted it slightly lower since people often cross genres depending on their vibe.

### Song Features

Each `Song` in the catalog stores:

- `genre` — the musical category (e.g. lofi, pop, rock, ambient, jazz, synthwave, indie pop)
- `mood` — the emotional tone (e.g. chill, happy, intense, focused, relaxed, moody)
- `energy` — a 0.0 to 1.0 score for how high-energy the track feels
- `tempo_bpm` — beats per minute
- `valence` — a 0.0 to 1.0 score for how positive or upbeat the song sounds
- `danceability` — a 0.0 to 1.0 score for how easy it is to dance to
- `acousticness` — a 0.0 to 1.0 score for how acoustic vs. electronic the song is

### UserProfile Features

Each `UserProfile` stores:

- `favorite_genre` — the genre they tend to gravitate toward
- `favorite_mood` — the mood they're looking for right now
- `target_energy` — the energy level they want (0.0 to 1.0)
- `likes_acoustic` — a true/false flag for whether they prefer acoustic over electronic sounds

### How Scoring Works

For each song, the system calculates a score out of 1.0:

- Mood match → worth **0.35** (highest weight — most immediate signal)
- Energy proximity → worth **0.30** (uses `1 - |song.energy - user.target_energy|` so closer = higher score)
- Genre match → worth **0.25** (strong identity signal but not the whole picture)
- Acoustic preference → worth **0.10** (tiebreaker based on the `likes_acoustic` flag)

All songs get scored, then sorted highest to lowest. The top `k` results (default 5) are returned as the recommendations.

---

## Sample Output

![Terminal output showing recommendations](terminal_output.png)

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this


---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card - Music Recommender Simulation

## 1. Model Name

Give your recommender a name, for example:

> VibeFinder 1.0

---

## 2. Intended Use

- What is this system trying to do
- Who is it for

Example:

> This model suggests 3 to 5 songs from a small catalog based on a user's preferred genre, mood, and energy level. It is for classroom exploration only, not for real users.

---

## 3. How It Works (Short Explanation)

Describe your scoring logic in plain language.

- What features of each song does it consider
- What information about the user does it use
- How does it turn those into a number

Try to avoid code in this section, treat it like an explanation to a non programmer.

---

## 4. Data

Describe your dataset.

- How many songs are in `data/songs.csv`
- Did you add or remove any songs
- What kinds of genres or moods are represented
- Whose taste does this data mostly reflect

---

## 5. Strengths

Where does your recommender work well

You can think about:
- Situations where the top results "felt right"
- Particular user profiles it served well
- Simplicity or transparency benefits

---

## 6. Limitations and Bias

Where does your recommender struggle

Some prompts:
- Does it ignore some genres or moods
- Does it treat all users as if they have the same taste shape
- Is it biased toward high energy or one genre by default
- How could this be unfair if used in a real product

---

## 7. Evaluation

How did you check your system

Examples:
- You tried multiple user profiles and wrote down whether the results matched your expectations
- You compared your simulation to what a real app like Spotify or YouTube tends to recommend
- You wrote tests for your scoring logic

You do not need a numeric metric, but if you used one, explain what it measures.

---

## 8. Future Work

If you had more time, how would you improve this recommender

Examples:

- Add support for multiple users and "group vibe" recommendations
- Balance diversity of songs instead of always picking the closest match
- Use more features, like tempo ranges or lyric themes

---

## 9. Personal Reflection

A few sentences about what you learned:

- What surprised you about how your system behaved
- How did building this change how you think about real music recommenders
- Where do you think human judgment still matters, even if the model seems "smart"

