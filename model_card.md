# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name

**VibeFinder 1.0**

---

## 2. Intended Use

VibeFinder is designed to suggest songs from a small catalog based on what a user says they want right now — their favorite genre, mood, energy level, and whether they like acoustic sound.

It is built for classroom exploration, not for real users. It assumes the user can describe their taste in simple terms. It does not learn from listening history or adapt over time.

**Not intended for:** real music apps, large catalogs, or users with complex or mixed tastes.

---

## 3. How the Model Works

Every song in the catalog gets a score between 0 and 1. The score is built from six things the system checks:

- **Genre** — does the song's genre match what the user likes? This is the most important factor.
- **Mood** — does the song feel like the right vibe? Happy, chill, intense, sad, etc.
- **Energy** — is the song as fast and intense as the user wants?
- **Acoustic feel** — does the song sound acoustic or electronic, and does that match the user's preference?
- **Tempo** — is the speed of the song close to what the user's energy level suggests?
- **Emotional tone** — does the song feel positive or dark in the right way?

Each check gives a small number. The system adds them all up with different weights — genre counts the most, tempo and emotional tone count the least. The songs with the highest totals are recommended.

---

## 4. Data

- The catalog has **20 songs** stored in a CSV file.
- Songs cover a wide range of genres: lofi, pop, rock, jazz, metal, EDM, classical, hip hop, soul, reggae, funk, and more.
- Moods include: happy, chill, intense, sad, relaxed, focused, romantic, dreamy, angry, and others.
- The dataset was built by hand for this project — it is not based on real streaming data.
- **Limits:** 20 songs is very small. Some genres only have one song, so users with niche tastes will always get weak results. Lyrics, artist popularity, and listening history are not included at all.

---

## 5. Strengths

- Works well for users with clear, common preferences like "chill lofi" or "high-energy pop" — those profiles get strong, obvious matches at the top.
- The reason labels ("matches your mood", "fits your acoustic preference") make it easy to understand why a song was recommended.
- After fixing the genre and mood lookup tables, cross-genre suggestions improved — for example, indie pop now shows up for pop fans.

---

## 6. Limitations and Bias

The biggest weakness found during testing is that genre has too much power at 35%. When testing the Deep Intense Rock profile, `Storm Runner` scored 0.98 but every other song dropped to 0.67 or lower — not because they sound bad, but because their genre label is different. This means a rock fan will rarely see a high-energy EDM or hip hop song in their results, even if it would feel just as good. Genre works more like a wall than a preference, which keeps users stuck in the same genre every time. A better system would mix in some songs from nearby genres to keep things interesting.

---

## 7. Evaluation

- **Which user profiles I tested:** Three profiles were tested — High-Energy Pop, Chill Lofi, and Deep Intense Rock.

- **What I looked for in the recommendations:** Whether the top 5 songs felt like a natural fit for each profile, and whether the reasons made sense.

- **What surprised me:** For Deep Intense Rock, `Storm Runner` scored 0.98 but the next song dropped to 0.67. One song dominated everything else, leaving very little variety. For Chill Lofi, a quiet acoustic jazz song scored only 0.50 even though it would feel right in a lofi playlist — it was hurt by the genre not matching.

- **Any simple tests or comparisons I ran:** After fixing `RELATED_GENRES` to include more genre connections, `Rooftop Lights` jumped from #5 to #2 for the High-Energy Pop profile. This confirmed the fix worked and made results feel more realistic.

---

## 8. Future Work

- **Lower the genre weight** from 35% to around 25% and spread the rest to mood and energy. This would stop genre from acting like a hard filter and let good-sounding songs from other genres show up.
- **Add more songs to the catalog.** With only 20 songs, users with uncommon tastes like jazz or classical almost never get a strong match. A bigger catalog would make every profile feel more fairly served.
- **Ask users to rate results.** Right now the system has no way to learn if its suggestions were good or bad. Even a simple thumbs up or down could help improve future recommendations.

---

## 9. Personal Reflection

**Biggest learning moment:** The weights matter more than the code. Writing the scoring function was easy. Deciding how much genre should count compared to mood was the hard part. When genre was too high, one song would win by a lot and the rest felt like bad guesses. Getting the numbers right took more thinking than writing the actual code.

**How AI tools helped, and when I needed to check:** AI helped build the lookup tables fast and caught the bug where moods like "sad" and "romantic" were in the CSV but the scoring logic did not know about them. But the results still needed a manual check. The numbers looked fine, but when I ran the profiles the results felt off. I had to look at the output and say "this does not feel right" before the problem became clear.

**What surprised me about simple algorithms:** Just a few comparisons and some addition can feel like a real recommendation. When the Chill Lofi profile returned quiet acoustic tracks at the top, it almost felt personal — even though the system knows nothing about me. It was surprising how something so simple could feel smart.

**What I would try next:** Let users give a thumbs up or down on each song, and use that to slowly adjust the weights over time. I would also try turning off the genre weight completely for one test to see if mood and energy alone are enough — that would show whether genre is actually helping or just getting in the way.
