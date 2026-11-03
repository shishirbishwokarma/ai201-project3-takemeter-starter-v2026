# Milestone 1 — community and what I noticed

## The community

**Fitness Stack Exchange** (fitness.stackexchange.com): the public Q&A site
for training, lifting, running and nutrition. Everything is readable without
an account, and the posts are licensed CC BY-SA.

I wanted r/fitness at first, but Reddit blocks scripted reads now, and Fitness
SE has a public API. I pulled the 300 newest answers and the 300 newest
comments, so the pool has both long posts (answers) and short ones (comments).
That matters because quality varies a lot. Some answers quote a study, some
just say "it's a myth", and some comments only say thanks.

No single show or release, so there's nothing to spoil.

## What I saw reading 40 posts (20 answers, 20 comments)

1. **Some posts point at something you could check, and most don't.** One
   answer quotes a PubMed study on HIIT and heart failure. Another names "the
   IPF's 2023 change" to the bench press elbow rule. A comment gives "135
   degrees knee flexion" for the quads. Most other advice is just stated.

2. **Some advice is confident with nothing behind it.** "It won't affect your
   gains if you run daily for 30 minutes. You don't have to think about it."
   "It's a myth. Women can use heavy dumbbells." "Second-guessing pain
   mechanisms with biomechanical models is so 90's..." There's no reason
   given, only the claim.

3. **Some posts are the person's own training, not advice.** "I've been doing
   this recently and I think it does help... I started running fairly late in
   life." "I've been running the blacknoir version of the program for about
   two months now." The support is "it happened to me", which is different
   from a study and different from no support at all.

4. **A chunk of the comments aren't about fitness at all.** "Thank you."
   "This borders on spam." "You neglect to share that you are the developer
   of this app." "Please consider editing your answer to include the
   reference." These are thanks, site moderation and back-and-forth about the
   thread.

Distinction 1 vs 2 looks like the strongest one, and it's the one the
community itself cares about. A comment literally asks someone to add the
reference. Number 3 might be its own label or might fold into one of the
others; I'll find out in Milestone 2.

# Milestone 2 — stress test of the labels

I gave the three definitions and the rule to Claude and asked for eight posts
that sit on the boundaries. I tried to label each one using only the rule:

| # | Post (made up, for the test) | My label | Clean? |
|---|---|---|---|
| 1 | "Rest 2-3 minutes between heavy sets. Anything less and your bar speed drops." | ? | **No.** Has a number, but it's advice, not a fact |
| 2 | "Your BMR at 80 kg is around 1,800 kcal, so a 500 kcal deficit is about half a kilo a week." | evidence | yes |
| 3 | "I cut from 90 to 78 kg in six months doing exactly 500 under maintenance." | ? | **No.** Numbers, but only about the poster |
| 4 | "Creatine is the most researched supplement there is. Just take it." | claim | yes. "Most researched" isn't specific |
| 5 | "The ACSM recommends 150 minutes of moderate cardio a week." | evidence | yes |
| 6 | "Squats are bad for your knees. Everyone knows that." | claim | yes |
| 7 | "Lol same, my DOMS after leg day lasted 4 days." | personal | yes |
| 8 | "Since 2019 the IPF wants a pause on bench, so touch-and-go won't count at a meet." | evidence | yes |

Two of the eight broke "a checkable fact is enough", so I added two lines
under the rule: a prescription with a number isn't a fact (#1 → claim), and
numbers about yourself are personal (#3 → personal). After that all eight
sorted cleanly.
