# Fearlessly Refactoring Legacy Ruby (Surgical Refactors)

Early success: making it easy to make new things :happy:

later success: very different. people are more critical, more serious, more and more money is involved
making it easy to maintain old things

Can we make it easier to maintain old ruby code?

---

Legacy Code : "Old code, code that doesn't have tests, code that we don't like ..."

Refactoring legacy code is very hard
Legacy refactors are hard to sell (to managers / sales people)

(top: business priority / right: cost/risk)
Bug fixes / New features
Testing   / Refactoring

hard to estimate
business perspective: it is invisible

---

"Make Refactors Great Again"

"Make Refactors Great for the 1st time"

---

Selling refactoring to business people

(1) Scare them! "If we don't refactor, then ..."

(2) Absorb the cost
... but requires discipline, collapses under pressure

(3) take hostages
It is blaming business for rushing, erodes trust in the team

---

Too much pressure

not many tools for refactoring
refactors are scary!

"The Frightened Programmer" (a fake book)

---

(1) Refactoring patterns
(2) Characterization testing
blackbox test harness -> refactor -> delete the test
tempting to quit halfway through
(3) A/B testing / experiments
heavy monitoring is required

---

TDD (Talk-Driven Development)

Gem : suture

* plan
* cut
* record interactions
* validate
* refactor
* verify
* compare
* fallback
* delete

----

(live demo)
