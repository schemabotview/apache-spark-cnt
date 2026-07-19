## Lazy evaluation — transformations vs actions

Spark's execution model rests on one idea, and everything in this module builds on it: **Spark is lazy.** Operations split into two kinds, and only one of them actually runs anything.

- **Transformations** — `filter`, `select`, `groupBy`, `join`, `withColumn`. They build the recipe. **Nothing runs.** Each one just returns a new DataFrame with the step appended to a plan.
- **Actions** — `count`, `show`, `collect`, `write`. They trigger execution against that recipe.

Why does this matter? By the time you call an action, Spark sees the *entire* chain of transformations at once and can plan the most efficient execution — reordering, combining, and dropping work. If transformations ran eagerly, each step would commit before the next, and the optimizer would have nothing to optimize.

Hold that distinction — a transformation builds, an action triggers — because the next several sections are all about *what Spark does with that plan* the instant an action fires.
