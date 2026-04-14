# Shared Resources

This directory holds files, notebooks, and materials copied from other repositories or external sources. It's a central place to keep reference material that doesn't belong to a specific learning topic.

## What to Put Here

- Entire repos (or subsets) you've cloned for reference
- Boilerplate files and starter templates
- Reference implementations and helper utilities you collect over time
- Course materials and downloaded datasets

## Suggested Structure

```
shared-resources/
├── from-repos/        ← Files copied from specific GitHub repos (credit the source!)
├── datasets/          ← Shared datasets used across multiple projects
├── templates/         ← Reusable templates (notebooks, pyproject.toml, .qmd, etc.)
└── cheat-sheets/      ← Quick reference guides for any topic
```

## Good Repos to Draw From

| Repo | Why It's Useful |
|------|----------------|
| [jakevdp/PythonDataScienceHandbook](https://github.com/jakevdp/PythonDataScienceHandbook) | NumPy, pandas, matplotlib, scikit-learn notebooks |
| [wesm/pydata-book](https://github.com/wesm/pydata-book) | "Python for Data Analysis" exercises |
| [ageron/handson-ml3](https://github.com/ageron/handson-ml3) | "Hands-On Machine Learning" chapter notebooks |
| [dbt-labs/jaffle_shop](https://github.com/dbt-labs/jaffle_shop) | Beginner dbt project |
| [mode/sql-tutorial](https://github.com/modedotla/sql-tutorial) | SQL tutorial scripts |

## Attribution

Always note where you copied a file from! Add a comment at the top of each file or a `SOURCE.md` file in each subfolder:

```
# Source: https://github.com/example/repo
# Copied on: YYYY-MM-DD
# Original license: MIT
```

## Important: Copyright & Licensing

- Before copying code, check the repo's license (MIT, Apache, GPL, etc.)
- If a repo has no license, ask permission before copying
- Never copy code that is under a restrictive or proprietary license without understanding the terms
