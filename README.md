# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [ ] Describe the game's purpose.

--> A number guessing game where the player tries to guess a secret number within a limited number of attempts, receiving higher/lower hints after each guess while a score is tracked.

- [ ] Detail which bugs you found.

--> 
Hint messages were inverted — guess > secret showed "Go HIGHER!" and guess < secret showed "Go LOWER!"

app.py defined its own check_guess shadowing the fixed version in logic_utils.py

On even-numbered attempts, secret was cast to str, causing a TypeError on comparison

"New Game" didn't reset history, score, or status

Out-of-range guesses counted as valid attempts

Score could go negative with no floor on the -5 penalty

Developer Debug Info rendered before the submit block, always showing stale score

Show hint checkbox had no effect after submit — message wasn't persisted in session_state

Pressing Enter didn't submit the form

- [ ] Explain what fixes you applied.

--> Corrected check_guess in logic_utils.py: guess < secret → "Too Low / Go HIGHER!", guess > secret → "Too High / Go LOWER!"

Added from logic_utils import check_guess to app.py and removed the local duplicate

Removed the even-attempt str(secret) conversion so both sides are always int

Added history, score, status, and last_hint resets to the New Game handler

Moved attempts += 1 inside the valid in-range branch with a range check guard

Wrapped deductions with max(0, current_score - 5)

Moved the Debug Info expander to after the submit block

Stored hint in session_state.last_hint, rendered outside submit block via show_hint checkbox

Wrapped input and button in st.form so Enter triggers submission


## 📸 Demo

- [ ] [Insert a screenshot of your fixed, winning game here]
- [ ] 



## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
