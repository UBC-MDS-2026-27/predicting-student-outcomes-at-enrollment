# Predicting academic outcomes from student enrollment records

**DSCI 522 · Group 5**

Bodie (YiChuan) Li, Colby Yurkiw, Ganiyat Adekunle, and Rupesh Patel

How well can information available at enrollment predict whether a student will drop out, remain enrolled, or graduate by the end of their program’s normal duration? We trained a logistic regression model using five enrollment predictors. On 885 students held out for testing, it scored 0.509 on macro F1, a measure that gives each outcome equal weight. Always predicting graduation scored 0.222. The model identified 145 of the 284 students who dropped out. An adviser who relied on its predictions alone would therefore miss nearly half the students they hoped to reach.

To read the report without running the analysis, open the HTML file on GitHub, choose **Download raw file**, and open the downloaded file in your browser. [Report HTML](reports/student-academic-outcomes.html) · [Quarto source](reports/student-academic-outcomes.qmd)

## Reproduce the analysis

1. Install [Git](https://git-scm.com/downloads), [uv](https://docs.astral.sh/uv/getting-started/installation/) and the [Quarto CLI](https://quarto.org/docs/get-started/). Open a terminal and clone the repository.

   ```sh
   git clone https://github.com/UBC-MDS-2026-27/predicting-student-outcomes-at-enrollment.git
   cd predicting-student-outcomes-at-enrollment
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

We used OpenAI Codex to help prepare the enrollment predictors, draft and revise the report and README, and check reported scores against the model output. GitHub Copilot provided feedback on the preprocessing pull request. Teammates reviewed the changes before merging them.

One question we asked was “Why exclude semester performance when predicting at enrollment?” A sample prompt based on our exploratory analysis is “What do the boxplots tell us about how admission grades overlap for students who dropped out, stayed enrolled, or graduated?” Our [AI instructions](AGENT.md) and [contributor policy](CONTRIBUTING.md#ai-use-policy) describe how we use these tools.

## Licenses

The analysis code uses the MIT License. The report text and figures use Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). Both full license texts are in [LICENSE.md](LICENSE.md).
