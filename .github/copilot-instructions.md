<!-- Copilot instructions for workspace: CSC8111 ML Coursework -->
# Quick Guide for AI coding agents

This repository is a small, notebook-driven ML coursework project. The single authoritative entrypoint is the Jupyter notebook `CSC8111_2025.ipynb` and the dataset files in the repository root (`fars.csv`, `fitting-results2.csv`). Use the notebook as the source of truth for data flows, variable names and model pipelines.

Key facts (discoverable in `CSC8111_2025.ipynb`):
- Project entrypoint: `CSC8111_2025.ipynb` (single notebook contains EDA, preprocessing, models).
- Data files: `fars.csv` (loaded with `pd.read_csv("fars.csv")` in some cells and occasionally with absolute path).
- Typical imports: `pandas`, `numpy`, `matplotlib`, `seaborn`, `sklearn` (modeling + preprocessing).
- Preprocessing pattern: a `ColumnTransformer` named `preprocessor` combines `StandardScaler` for numeric columns and `OneHotEncoder(handle_unknown="ignore")` for categorical columns.
- Pipelines to target: `pipe_lr` (LogisticRegression) and `pipe_rf` (RandomForestClassifier); both include the `preprocessor` step and use `class_weight="balanced"`.
- Train/test split: `train_test_split(..., stratify=y, test_size=0.2, random_state=42)` and upstream uses `StratifiedKFold` for CV.

What to do when editing or suggesting code:
- Keep changes minimal and notebook-centric. If you add helper .py files, update the notebook to import them and demonstrate usage in a code cell.
- Preserve cell outputs where possible when making non-functional changes; keep narrative text blocks (markdown) intact.
- When altering data paths, prefer relative paths (`fars.csv`). Note that some cells use a full Windows path — prefer converting to relative paths for portability.

Patterns & conventions to follow (concrete examples):
- Use the existing `preprocessor` ColumnTransformer pattern. Example: modify or extend the transformer rather than replacing it wholesale.
- When tuning models, perform parameter grids via `GridSearchCV` applied to the pipeline object (e.g., `GridSearchCV(pipe_rf, param_grid, cv=StratifiedKFold(...))`).
- Preserve `class_weight="balanced"` for classifiers unless a clear, dataset-specific reason is documented in the notebook text.
- Keep plotting code using `matplotlib`/`seaborn` in notebook cells; prefer inline plots instead of external scripts.

Developer workflows (how humans run this project):
- Open and run the notebook in Jupyter (recommended):
  - PowerShell:
    ```powershell
    jupyter notebook CSC8111_2025.ipynb
    # or
    jupyter lab
    ```
- To reproduce a single cell or block programmatically, convert the cell to a `.py` script and run with the same working directory containing `fars.csv`.

Integration and external dependencies:
- The project depends on commonly used Python data/ML libraries. Derive a requirements list from imports: at minimum `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`.
- No CI/tests are present in the repository. Changes that add tests should include a short example notebook cell showing test invocation or a minimal `pytest` target.

Search hints for maintainers / agents:
- To find model definitions: search for `pipe_lr`, `pipe_rf`, and `preprocessor` in `CSC8111_2025.ipynb`.
- To find data loading: search for `read_csv("fars.csv")` (some cells use `r"C:\\Users\\...\\fars.csv"`).

When in doubt ask the user about runtime environment (Python version, virtualenv) before adding dependency files. After edits, request that the user run the notebook to validate outputs and plots.

If you update this file: merge content rather than overwriting. Preserve any user notes already present in `.github/copilot-instructions.md`.

— End of file
