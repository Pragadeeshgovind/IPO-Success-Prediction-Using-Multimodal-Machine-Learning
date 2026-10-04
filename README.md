# IPO Signal — Success Prediction Using Multimodal Machine Learning

A responsive research dashboard and FastAPI service demonstrating an end-to-end IPO outcome prediction workflow. The bundled model is a **synthetic-data demonstration**: it is not trained on historical exchange data and must not be used for investment decisions.

## Run locally

### Frontend

```powershell
npm install
npm run dev
```

### API

In a second terminal:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn backend.main:app --reload
```

The Vite development server proxies `/api` to `http://127.0.0.1:8000`. Open the URL printed by Vite (usually `http://localhost:5173`).

### Run both services with Docker Compose

```powershell
docker compose up --build
```

Open `http://localhost:8080`. To use a transformer sentiment model, set `IPO_SENTIMENT_MODEL` to a Hugging Face model identifier before starting Compose and include the optional `transformers` and `torch` packages in the API image build. Model downloads and licenses should be reviewed for the deployment environment.

## Application surfaces

- **Home:** market snapshot, illustrative aggregate KPIs and recent prediction history.
- **IPO Prediction:** structured company, financial, offer and market inputs; optional news and prospectus/document upload; probability, class, return estimate, risk score and factor contribution output.
- **Multimodal Analysis / Model Architecture:** tabular, text, market and optional image channels with a late-fusion pipeline diagram.
- **Dataset & EDA:** synthetic sample provenance, feature catalog, class balance, missingness and distributions; downloadable sample CSV.
- **Performance:** classification, regression, baseline and calibration evaluation surfaces. Dashboard metrics are clearly labelled illustrative; `/api/metrics` evaluates the synthetic demo holdout.
- **Explainability:** local contribution summary, narrative signal and risk profile.
- **Prediction History / Reports:** session-local history, JSON research report export and CSV batch template.

## API

- `GET /api/health`
- `POST /api/predict`
- `POST /api/predict/batch` (CSV, maximum 500 rows / 12 MB)
- `GET /api/metrics`
- `GET /api/dataset`
- `GET /api/dataset/sample.csv`
- `POST /api/documents/extract` (in-memory PDF, DOCX, TXT/MD extraction or image validation; files are not persisted)

The demo's text branch uses an offline lexicon so first run does not download a model. For research deployment, replace it with a versioned Hugging Face transformer, add OCR/vision extraction where appropriate, and record model/data provenance. Uploaded image files are validated but are not OCR-processed in this starter.

## Before real-world research

1. Integrate a verified and licensed historical IPO dataset, document its outcome definition and preserve source-level provenance.
2. Use an event-time split (and point-in-time features) to avoid look-ahead leakage; fit all preprocessing inside training folds.
3. Run nested time-series cross-validation, calibrate on validation data, compare against simple baselines and report uncertainty.
4. Store versioned model artifacts, monitor drift and establish privacy, retention, security and access-control policies.
5. Validate the model independently. This project is decision-support research software, **not financial advice**.
