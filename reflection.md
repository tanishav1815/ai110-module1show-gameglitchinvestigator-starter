# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?

--> The game's hints were backwards- guessing too low showed "Go LOWER" and guessing too high showed "Go HIGHER".



- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

  --> inverted comparison messages in check_guess, and the "New Game" button not clearing history, score, or status, leaving stale data from the previous game.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?

--> Claude Code was used throughout.

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).

--> it identified that app.py was ignoring the fixed logic_utils.py entirely and still calling its own buggy check_guess — adding the import fixed it, verified by testing guess=40 with secret=86.


- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

--> it initially fixed the hint logic in logic_utils.py without noticing app.py had its own copy shadowing it, requiring a second fix.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?

--> A bug was fixed when the exact failing scenario produced the right output consistently.

- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.

  --> submitting guess=36 with secret=63 on an even attempt — this exposed a TypeError from the str(secret) conversion that was separate from the inverted-messages bug.

- Did AI help you design or understand any tests? How?

--> AI helped trace it directly from the traceback to the specific line causing the type mismatch.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.

--> The secret kept changing because random.randint() ran unconditionally every rerun, generating a new number on every interaction. 


- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

--> Streamlit reruns the full script on every widget interaction; session_state is a persistent dictionary that survives those reruns.


- What change did you make that finally gave the game a stable secret number?

--> The fix was guarding the assignment with if "secret" not in st.session_state: so it only runs once.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
- This could be a testing habit, a prompting strategy, or a way you used Git.

--> Always verify the function being called is actually the one you fixed imports and local shadowing can silently nullify a fix. 

  



- What is one thing you would do differently next time you work with AI on a coding task?

--> Next time, ask AI to explain what the code is doing before asking it to fix it.

- In one or two sentences, describe how this project changed the way you think about AI generated code.

--> This project showed that AI-generated code needs real testing; it can have multiple compounding bugs that look fine on a quick read.
