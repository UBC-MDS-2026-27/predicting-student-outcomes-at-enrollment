# Contributing

We welcome bug reports, suggestions, and contributions to our analysis of student academic outcomes. The [README](README.md) describes the project question, dataset, and report. All contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## Prerequisites

Before starting a substantial change, search the [existing issues](https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment/issues) to see whether someone has already reported the problem or proposed a similar idea. If a relevant issue exists, join that discussion. Otherwise, open an issue and wait for a team member to agree on the scope before beginning the work.

For a **bug report**, explain what you expected to happen, what actually happened, and the smallest set of steps that reproduces the problem. If the report fails to render, include the command you ran, the error message, and your operating system. A short code example or screenshot can help someone reproduce the issue. See the [guide to minimal reproducible examples](https://www.tidyverse.org/help/#reprex).

For a **suggested change**, identify the affected part of the project and explain why the change would help. For example, if a figure is difficult to interpret, give its number or section heading and describe the comparison you want readers to be able to make.

## Setting up the project

Install [Git](https://git-scm.com/downloads), [uv](https://docs.astral.sh/uv/getting-started/installation/), and the [Quarto CLI](https://quarto.org/docs/get-started/). Team members can then clone the repository and create the project environment by running the following commands in a terminal.

```sh
git clone https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment.git
cd predicting-student-outcomes-at-enrollment
uv sync --locked
```

Run the following command from the repository root to execute the analysis and render the report. Internet access is required because the analysis downloads the original dataset from the UCI Machine Learning Repository.

```sh
uv run quarto render
```

Open `reports/student-academic-outcomes.html` in your browser to inspect the result. The analysis source is `reports/student-academic-outcomes.qmd`. Make report edits in that source file and render it again to update the HTML.

If your change introduces a Python dependency, run `uv add <package>`, replacing `<package>` with its name. This project is configured to pin direct dependencies to exact versions in `pyproject.toml`. Commit the updated `pyproject.toml` and `uv.lock` together so others can reproduce your environment.

## Pull request process

1. **Create a branch from an up-to-date `main`.** Use one branch per independent change and choose a short, descriptive name. For example, to work on a new EDA figure, run the following commands from the repository root.

   ```sh
   git switch main
   git pull --ff-only
   git switch -c add-eda-figure
   ```

   Team members should work on branches in this repository. External contributors can follow the same process in their own fork.

2. **Make and check your changes.** Follow [PEP 8](https://peps.python.org/pep-0008/) for Python code. If you change the analysis or report, run `uv run quarto render` and open the resulting HTML. Check that the affected figures, tables, and cross-references display correctly and that the written interpretation agrees with the output. Include both the updated `.qmd` and rendered `.html` in your commit. For a Markdown-only change, preview the file and check its links.

3. **Review and commit your work.** Use `git status` and `git diff` to check which files and lines changed. Stage the files relevant to the issue and write a commit message that describes the change. Push your branch to GitHub. For the example branch above, use `git push -u origin add-eda-figure`.

4. **Open a pull request into `main`.** Explain what changed, why it was needed, and how you checked it. Link the related issue using a phrase such as `Closes #12` if the PR resolves it. For a visual change, identify the figure or report section the reviewer should inspect.

5. **Request review from another team member.** Address their feedback and push any revisions to the same branch. At least one other team member must approve the PR before it is merged. The approving reviewer merges the PR into `main`.

## AI use policy

Contributors may use generative AI tools to brainstorm approaches, explain errors, suggest code or wording, and double-check their work. The contributor is responsible for understanding and verifying anything they submit. Run suggested code, check numerical claims against the analysis output, and verify citations against the original sources.

If you used AI assistance, briefly state in the PR description which tool you used, what it helped with, and how you checked the result. For example, “I used ChatGPT to suggest tooltip code, rendered the report, and compared the displayed counts with the training data.” Document project-level AI use in the README, including representative prompts and links to any AI instruction files used.

Contributors must personally open GitHub issues and pull requests. Human teammates review the changes, make approval decisions, and merge approved pull requests.

## Acknowledgments

The setup and pull request guidance was adapted for this project from the [UBC DSCI 522 Breast Cancer Predictor project template](https://github.com/UBC-MDS/DSCI_522_project-template-breast-cancer-predictor/tree/0.0.1).