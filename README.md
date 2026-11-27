# Predicting academic outcomes from student enrollment records

**DSCI 522 · Group 5**

Bodie (YiChuan) Li, Colby Yurkiw, Ganiyat Adekunle, and Rupesh Patel

How well can information available at enrollment predict whether a student will drop out, remain enrolled, or graduate by the end of their program’s normal duration? We trained a logistic regression model using five enrollment predictors. On 885 students held out for testing, it scored 0.509 on macro F1, a measure that gives each outcome equal weight. Always predicting graduation scored 0.222. The model identified 145 of the 284 students who dropped out. An adviser who relied on its predictions alone would therefore miss nearly half the students they hoped to reach.

Read the report online without installing anything:

[Read the published report](https://ubc-mds-2026-27.github.io/predicting-student-outcomes-at-enrollment/) · [Quarto source](reports/student-academic-outcomes.qmd) · [Report HTML](reports/student-academic-outcomes.html)

## Usage

To keep the analysis reproducible, we use a Docker container with the same software and package versions for everyone. You can find the image on Docker Hub at [rjcrystal786/predicting-student-outcomes-at-enrollment](https://hub.docker.com/r/rjcrystal786/predicting-student-outcomes-at-enrollment)

## Reproduce the analysis

### 1. Install prerequisites

Install [Git](https://git-scm.com/downloads) and [Docker Desktop](https://www.docker.com/products/docker-desktop/). Start Docker Desktop before continuing. On Windows, make sure Docker Desktop uses Linux containers. Python, uv, Quarto, and the project dependencies run inside the container, so you do not need to install them separately on your computer.

### 2. Clone the repository

On Windows, open Git Bash. On macOS, open Terminal. Clone the repository and enter its directory:

```sh
git clone https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment.git
cd predicting-student-outcomes-at-enrollment
```

Run the remaining commands from this directory.

### 3. Check your SSH key

The container setup requires an SSH public key on your computer. Check whether the default Ed25519 public key exists:

```sh
ls ~/.ssh/id_ed25519.pub
```

If the command prints the file path, your public key already exists. Skip Step 3.1 and continue to **Step 4: Pull the Docker image**.

If the command reports “No such file or directory,” complete Step 3.1 before continuing.

#### 3.1. Create a key if needed

Create an SSH key:

```sh
ssh-keygen -t ed25519
```

Accept the default file location and follow the prompts. Do not overwrite an existing key. Confirm that the public key now exists:

```sh
ls ~/.ssh/id_ed25519.pub
```

Once the command prints the file path, continue to **Step 4: Pull the Docker image**.

### 4. Pull the Docker image

From the repository’s root directory, download the Docker image pinned in `docker-compose.yml`:

```sh
docker compose pull
```

Wait for the download to finish successfully before continuing to Step 5.

### 5. Render the analysis

Render the analysis inside the container:

```sh
docker compose run --rm analysis-env uv run quarto render
```

Rendering requires internet access to download the dataset from UCI. The `--rm` option automatically removes the container after the command finishes. Generated files remain in your local repository because the project directory is mounted into the container.

After rendering finishes successfully, continue to Step 6.

### 6. Open the report

Open `reports/student-academic-outcomes.html` in your browser to read the report. Rendering also writes the original CSV to `data/raw/data.csv` and the training and test files to `data/processed/`.

For instructions on developing inside the container or changing project dependencies, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Dependencies

Install [Docker](https://www.docker.com/) to run the analysis in a container.

The repository’s [Dockerfile](Dockerfile) builds the image with Quarto, uv, Python, and the Python packages listed in [pyproject.toml](pyproject.toml). The [uv.lock](uv.lock) file records the exact package versions.

## Dataset

Realinho, V., Vieira Martins, M., Machado, J., & Baptista, L. (2021). *Predict Students’ Dropout and Academic Success* [Dataset]. UCI Machine Learning Repository. [DOI 10.24432/C5MC89](https://doi.org/10.24432/C5MC89).

The original data contain 4,424 student records across 17 undergraduate programs at the Polytechnic Institute of Portalegre in Portugal. We randomly assigned 3,539 students to the training set and 885 to the test set, preserving the proportions of the three outcomes. We removed 12 first- and second-semester columns from both files because those observations occur after enrollment, and trimmed whitespace from column names. The source data and these derived files are available under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## AI use

We used AI tools for different tasks during Milestone 1. For example, we used OpenAI Codex to discuss which predictors would be available at enrollment, troubleshoot a Matplotlib warning, and explore ways to present the figures in a way that effectively communicate the findings without overwhelming the audience. Codex also suggested plotting code and wording for parts of the report and README, and checked reported scores against the model output.

Our team decided which predictors to include, how to interpret the results, and which suggested changes to keep. Team members opened the issues and pull requests, reviewed one another’s changes, and made the merge decisions. Our [AI instructions](AGENT.md) describe the tutoring approach we adopted, and our [contributor policy](CONTRIBUTING.md#ai-use-policy) explains how contributors disclose and verify AI-assisted work.

Examples of questions we discussed with AI included:

- Why should we exclude semester performance when predicting outcomes at enrollment?
- What is causing the Matplotlib deprecation warning in the age and admission-grade boxplots?

## Licenses

The analysis code uses the MIT License. The report text and figures use Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). Both full license texts are in [LICENSE.md](LICENSE.md).
