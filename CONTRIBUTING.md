# Contributing

We welcome bug reports, suggestions, and contributions to our analysis of student academic outcomes. The [README](README.md) describes the project question, dataset, and report. All contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## Prerequisites

Before starting a substantial change, search the [existing issues](https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment/issues) to see whether someone has already reported the problem or proposed a similar idea. If a relevant issue exists, join that discussion. Otherwise, open an issue and wait for a team member to agree on the scope before beginning the work.

For a **bug report**, explain what you expected to happen, what actually happened, and the smallest set of steps that reproduces the problem. If the report fails to render, include the command you ran, the error message, and your operating system. A short code example or screenshot can help someone reproduce the issue. See the [guide to minimal reproducible examples](https://www.tidyverse.org/help/#reprex).

For a **suggested change**, identify the affected part of the project and explain why the change would help. For example, if a figure is difficult to interpret, give its number or section heading and describe the comparison you want readers to be able to make.

## Setting up the project
*Instructions adapted from [Milestone 2, step 6](https://ubc-mds.github.io/DSCI_522_dsci-workflows_students/assignments/milestone2.html#positron-ssh)*

1. Follow [Reproduce the analysis](README.md#reproduce-the-analysis) to install Docker, create an SSH key and download the image, then return to these steps.

2. Before your first connection, install Positron and check your Git settings:
   
   a. Install [Positron](https://positron.posit.co/download.html) and follow the set up steps. The **Remote SSH** allows you to connect to the container and is built in to the download.

   b. Check that Git knows who you are. Run the following command:
   ```sh
   git config --global user.email
   ```

   Your GitHub account email should be printed by the above. If it prints nothing, set it with the following commands. Otherwise, continue to the next section.
   ```sh
   git config --global user.name "Your Name" &&
   git config --global user.email "you@example.com"
   ```

The project is now set up. Continue to the next section for steps that must be completed each time you work on the project.

## Each time you work on the project

To connect and work within the container, follow these steps:

**1.** Start the container in the background:
```sh
docker compose up -d
```
*(Optional)* To verify the container started successfully, running the following should list analysis-env as running.
```sh
docker compose ps
```

**2.** Open Positron, then open the Command Palette (`Cmd+Shift+P` on Mac, `Ctrl+Shift+P` on Windows). It will appear at the top of the IDE.

**3.** Type and run **Remote SSH: Show Remote Menu**, then pick **Connect to Host.**

**4.** Type `root@localhost:2222` and press Enter. A new Positron window will open. You may see these prompts:

   - If it asks for the platform of the remote host, pick *Linux*.
   - If it asks whether you trust the host (a “fingerprint”), click *Continue*.
   - The first time, Positron installs its server inside the container. This takes a few minutes. Be patient.

**5.** Check that the bottom left corner shows `SSH: localhost:2222`, this means you are connected. Explorer should show **No folder opened** and a terminal should start in `/root`. Proceed with the following steps:

   **a.** Click **Open Folder** in the Explorer. A path box appears at the top of the window.

   **b.** Delete what is in it, type `/workspace`, and press Enter (or click OK, you may need to click “ok” a few times before this works).

   **c.** The window reloads. The Explorer now shows WORKSPACE [SSH: LOCALHOST:2222] with your project files. A file you change here changes on your computer too.

**6.** Pick the interpreter by clicking the interpreter button at the **top right** of the window (it will show a Python version). Click **New Console Session**, then:

   **a.** *Python*: pick the **Python 3.12** interpreter with the path `/opt/venv/bin/python`, which is the environment built from the project's `uv.lock`. The top right button then shows that name. If it isn’t in the list, open the Command Palette (see step 2 for shortcut), run **Python: Select Interpreter**, choose **Enter interpreter path…** and type `/opt/venv/bin/python`. Don’t pick `/workspace/.venv`: that is the environment from your own computer, and it doesn’t run in the container.

   **b.** To check, run `import sys; print(sys.executable)` in the Console. It should print `/opt/venv/bin/python`.

**7.** Complete work as usual. The Terminal, Console and Render button all run **inside** the container.

**8.** When you are finished working, close the Positron window, then stop the container in a terminal on your computer:

```sh
docker compose down
```

## Troubleshooting set up
*Troubleshooting adapted from [Milestone 2, step 6](https://ubc-mds.github.io/DSCI_522_dsci-workflows_students/assignments/milestone2.html#positron-ssh)*

- **`~/.ssh/id_ed25519.pub` is a folder.** Docker created it because the key didn't exist when the container started. Run `docker compose down`, delete that folder, create the key by following the [README](README.md#reproduce-the-analysis), and start again.
- **`~/.gitconfig` is a folder, or Git in the container doesn't know your name.** The same thing happened with your Git settings. Run `docker compose down`, delete that folder, set your Git name and email as described above, and start again.
- **GitHub asks you to sign in when you push from the container.** Sign in once in the browser window that Positron opens. Positron remembers it after that.
- **It asks for a password.** The container can't see your SSH key. Check that your public key is named `id_ed25519.pub`, or change the key path in `docker-compose.yml`.
- **"Remote host identification has changed."** You're using a new image, for example after pulling an updated tag. Run `ssh-keygen -R "[localhost]:2222"`, then connect again.
- **Linux laptops: SSH refuses the key.** The mounted key file belongs to you, not `root`. Remove the `#` from the `StrictModes no` line in the `Dockerfile` and push, so the workflow publishes a fixed image.
- **Port 2222 is already in use.** Change the first `2222` in `docker-compose.yml` to another number, such as `2223`, and connect to `root@localhost:2223` instead. Don't commit this change, since it only applies to your computer.

## Adding or updating a package

If your work required a new or updated package, use the following steps to update `pyproject.toml` and `uv.lock`:

1. Create a new branch from an up-to-date `main` and add the package from the Positron terminal (inside the container)
```sh
git switch main
git pull
git switch -c <branch-name>
```

```sh
uv add <package>
```

2. Commit the changes, then push to GitHub
```sh
git add pyproject.toml uv.lock
git commit -m "Add <package> for <reason>"
git push -u origin <branch-name>
```

3. Wait for the [Publish image to Docker Hub](https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment/actions/workflows/publish-dockerhub.yml) workflow to finish on your branch. When it completes, you will see a green checkmark.

4. Close Positron, then from a terminal on your computer, from the project folder, download the updated `docker-compose.yml` and the new image. Then, restart the container so it uses the new image:
```sh
git pull
docker compose pull
docker compose down
docker compose up -d
```

5. Reconnect with Positron as in [Each time you work on the project](#each-time-you-work-on-the-project).

6. Open a pull request. After it is merged, everyone updates their local copy and image:
```sh
git switch main
git pull
docker compose pull
```

## Publishing the report

The published report on [GitHub Pages](https://ubc-mds-2026-27.github.io/predicting-student-outcomes-at-enrollment/) is a copy of the rendered report stored at `docs/index.html`. Each time you change the analysis or report, update that copy so the published version matches.

1. Render the report inside of the container. Open the Positron terminal and run:
```sh
uv run quarto render
```

Alternatively, from a terminal in the project folder on your computer run:
```sh
docker compose run --rm analysis-env uv run quarto render
```

2. Open `reports/student-academic-outcomes.html` and check that the figures, tables and text display correctly.

3. Copy the rendered report over to `docs/index.html`:
```sh
cp reports/student-academic-outcomes.html docs/index.html
```

4. Commit the report source `.qmd`, the rendered report `.html` and the published copy, then push your branch:
```sh
git add reports/student-academic-outcomes.qmd reports/student-academic-outcomes.html docs/index.html
git commit -m "Update <section/part of report> and republished report"
git push origin <branch-name>
```

5. Open a pull request on GitHub. The report will publish from main so you must merge before changes become live.

6. A few minutes after the merge, open the [published report](https://ubc-mds-2026-27.github.io/predicting-student-outcomes-at-enrollment/) and check that it shows your changes, including the figures.

## Pull request process

1. **Create a branch from an up-to-date `main`.** Use one branch per independent change and choose a short, descriptive name. For example, to work on a new EDA figure, run the following commands from the repository root.

   ```sh
   git switch main
   git pull --ff-only
   git switch -c add-eda-figure
   ```

   Team members should work on branches in this repository. External contributors can follow the same process in their own fork.

2. **Make and check your changes.** Follow [PEP 8](https://peps.python.org/pep-0008/) for Python code. If you change the analysis or report, render it and republish it by following [Publishing the report](#publishing-the-report). Before committing, check that the affected figures, tables and cross-references display correctly and that the written interpretation still agrees with the output. For a Markdown-only change, preview the file and check its links.

3. **Review and commit your work.** Use `git status` and `git diff` to check which files and lines changed. Stage the files relevant to the issue and write a commit message that describes the change. Push your branch to GitHub. For the example branch above, use `git push -u origin add-eda-figure`.

4. **Open a pull request into `main`.** Explain what changed, why it was needed, and how you checked it. Link the related issue using a phrase such as `Closes #12` if the PR resolves it. For a visual change, identify the figure or report section the reviewer should inspect.

5. **Request review from another team member.** Address their feedback and push any revisions to the same branch. At least one other team member must approve the PR before it is merged. The approving reviewer merges the PR into `main`.

## AI use policy

Contributors may use generative AI tools to brainstorm approaches, explain errors, suggest code or wording, and double-check their work. The contributor is responsible for understanding and verifying anything they submit. Run suggested code, check numerical claims against the analysis output, and verify citations against the original sources.

If you used AI assistance, briefly state in the PR description which tool you used, what it helped with, and how you checked the result. For example, “I used ChatGPT to suggest tooltip code, rendered the report, and compared the displayed counts with the training data.” Document project-level AI use in the README, including representative prompts and links to any AI instruction files used.

Contributors must personally open GitHub issues and pull requests. Human teammates review the changes, make approval decisions, and merge approved pull requests.

## Acknowledgments

The setup and pull request guidance was adapted for this project from the [UBC DSCI 522 Breast Cancer Predictor project template](https://github.com/UBC-MDS/DSCI_522_project-template-breast-cancer-predictor/tree/1.0.0).