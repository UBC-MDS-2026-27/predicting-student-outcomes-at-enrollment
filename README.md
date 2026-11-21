# Predicting academic outcomes from student enrollment records

**DSCI 522 · Group 5**

Bodie (YiChuan) Li, Colby Yurkiw, Ganiyat Adekunle, and Rupesh Patel

How well can information available at enrollment predict whether a student will drop out, remain enrolled, or graduate by the end of their program’s normal duration? We trained a logistic regression model using five enrollment predictors. On 885 students held out for testing, it scored 0.509 on macro F1, a measure that gives each outcome equal weight. Always predicting graduation scored 0.222. The model identified 145 of the 284 students who dropped out. An adviser who relied on its predictions alone would therefore miss nearly half the students they hoped to reach.

To read the report without running the analysis, open the HTML file on GitHub, choose **Download raw file**, and open the downloaded file in your browser. [Report HTML](reports/student-academic-outcomes.html) · [Quarto source](reports/student-academic-outcomes.qmd)

## Reproduce the analysis

1. Install [Git](https://git-scm.com/downloads), [uv](https://docs.astral.sh/uv/getting-started/installation/) and the [Quarto CLI](https://quarto.org/docs/get-started/). Open a terminal and clone the repository.

   ```sh
   git clone https://github.com/UBC-MDS-2026-27/DSCI-522-Group-5.git
   cd DSCI-522-Group-5
   ```

2. Set up the Python 3.12 environment. The project dependencies are listed in [pyproject.toml](pyproject.toml), and [uv.lock](uv.lock) records the exact versions.

   ```sh
   uv sync --locked
   ```

3. From the same directory, render the report. This step needs internet access to download the dataset from UCI.

   ```sh
   uv run quarto render
   ```

4. Open `reports/student-academic-outcomes.html` in your browser. The render also writes the original CSV to `data/raw/data.csv` and the training and test files to `data/processed/`.

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
