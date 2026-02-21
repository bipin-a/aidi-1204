# Week 5 Assignment — Expectations & Submission Guide

## What You're Submitting

You need to submit **two things**:

1. **The completed notebook** (`.ipynb` file)
2. **A video walkthrough** of your work

Both are required. A missing video means an incomplete submission.

---

## The Notebook

Your completed notebook should have:

- All code cells running without errors (restart kernel and run all before submitting)
- All markdown cells filled in — every scenario description, every four-question table, every written answer
- Your own original real-life scenarios (not the examples from the framing section, not from classmates)
- Side-by-side histogram comparisons for every distribution
- Empirical vs theoretical mean/std comparisons
- All Section 6 (Big Picture) and Section 7 (Scenario Matching) answers completed

If a code cell has an error or a markdown cell is left blank, it will be treated as incomplete.

---

## The Video

**Format:** Screen share with webcam on and audio on. You are walking me through your notebook like you're explaining it to a colleague.

**Length:** 15 minutes maximum. You do not need to use all 15 minutes — just make sure you cover everything below. If your video goes over 15 minutes it will not be reviewed past the 15 minute mark.

### What to cover in the video

- **Your scenarios:** For each distribution, briefly explain the real-life scenario you picked and why it fits that distribution. Walk through your four-question table.
- **Your simulations:** Show your code running. You don't need to explain every line, but explain the logic — what is the loop doing, what are you counting, what gets stored.
- **Your comparisons:** Show the side-by-side plots (Monte Carlo vs numpy/scipy). Point out whether they match and whether your empirical mean/std were close to theoretical.
- **The parameter experiments:** Show what happened when you changed parameters (e.g. increasing n in binomial, increasing λ in Poisson). Explain what you saw and why.
- **The Big Picture (Section 6):** Walk through your "make it normal" results, and your one-paragraph summary. This is the most important part — show me you understand *why* these distributions converge to normal.
- **Scenario Matching (Section 7):** Pick at least 2 of the 5 scenarios and walk through your reasoning using the four-question framework.

You **don't need to cover every single cell**. Focus on demonstrating that you understand the *ideas*, not just that you wrote the code.

---

## Grading

| Component | Weight |
|---|---|
| Notebook — code runs, all cells complete | 30% |
| Notebook — quality of scenarios, answers, and written explanations | 20% |
| Video — clear demonstration of understanding | 50% |

### How the video is graded

The video is where I assess whether you actually understand what you built. Here's how it breaks down:

**Full marks (up to 100%):** You walk through your notebook naturally, explain your thinking in your own words, respond to what's on screen, and show that you understand why things work the way they do. You can glance at notes, scroll through your notebook, and reference your written answers — that's fine and expected.

**Capped at 50%:** If your explanations are unclear, shallow, or suggest you don't understand the material beyond surface level. For example: you can show me the code runs and the plots look right, but you can't explain *why* increasing λ makes Poisson look normal, or you can't explain the difference between what stops in geometric vs negative binomial. Correct code with poor understanding = 50% maximum.

**Automatic 50% penalty:** If you are reading from a script. This means reading pre-written sentences off-screen, reciting memorized explanations, or reading your markdown cells word-for-word as your explanation. I'm not looking for polish — I'm looking for understanding. Stumbling a bit while thinking through an idea is far better than a rehearsed script. You are allowed to look at your notebook, your notes, and your code. You are not allowed to read a prepared speech.

### To be clear

- Looking at your notebook while talking through it → ✅ totally fine
- Glancing at handwritten notes to remember a formula → ✅ fine
- Saying "let me think about this for a sec" → ✅ fine
- Reading sentences from a Google Doc on your other monitor → ❌ script
- Reciting a memorized paragraph without looking at the notebook → ❌ script
- Reading your markdown answers word-for-word as your entire explanation → ❌ script

The goal is a conversation with your notebook, not a presentation.

---

## Summary

| What | Details |
|---|---|
| **Submit** | Completed `.ipynb` notebook + video walkthrough |
| **Video format** | Screen share, webcam on, audio on |
| **Video length** | 15 minutes max |
| **Scripting** | Not allowed — automatic 50% penalty |
| **Notes/notebook reference** | Allowed and encouraged |
| **Key focus** | Show me you understand the *why*, not just the *what* |