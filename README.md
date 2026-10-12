# AI Image Forensics & Deepfake Detection

**Winner, Best Gen AI Project at Chakravyuh 2.0**, a 36-hour national hackathon (SVERI's College of Engineering, Pandharpur, March 2026). Built by a team of 4.

### 🔗 [Live demo: chk-1772545387610-6113.vercel.app](https://chk-1772545387610-6113.vercel.app)

Upload a JPG or PNG and get a manipulation score, a heatmap of suspicious regions, and a downloadable PDF forensic report. Every analysis is recorded in a SHA-256 evidence chain so results can be traced and checked for tampering.

## Screenshots

| Upload | Analysis result | PDF report |
|---|---|---|
| ![Upload page](screenshots/upload.png) | ![Analysis result with heatmap](screenshots/result.png) | ![PDF forensic report](screenshots/report.png) |

## Features

- **Weighted analysis:** combines Error Level Analysis (40%) with a model-based score (60%) into one confidence score.
- **Visual heatmaps:** color-coded overlay that highlights the regions most likely to be manipulated.
- **Evidence chain:** SHA-256 chain of custody for forensic integrity.
- **PDF reports:** downloadable forensic report for each analysis.
- **History:** view and search all analyzed images.
- **File validation:** JPG and PNG only, maximum 10 MB, checked on both frontend and backend.
- **Responsive UI:** works on desktop and mobile.

## How the score works

```
Combined score = (ELA score × 0.4) + (model score × 0.6)
```

| Score | Classification |
|---|---|
| below 30 | Authentic |
| 30 to 70 | Uncertain |
| above 70 | Manipulated |

Scores are probabilistic indicators to support a human reviewer. They are not legal proof.

## Tech stack

- **Backend:** Python, FastAPI, Pillow, NumPy, Matplotlib, ReportLab
- **Frontend:** React 18, Vite, Tailwind CSS, Axios, React Router
- **Data:** Supabase (see `supabase_schema.sql`)
- **Deployment:** Vercel

## Repository structure

```
.
├── api/               # Backend API
├── frontend/          # React web app
├── ml-service/        # Image analysis and model service
├── supabase_schema.sql
├── vercel.json
└── start.bat          # Windows helper script
```

## Run locally

**Frontend**

```bash
cd frontend
npm install
npm run dev
```

The app runs at `http://localhost:5173`.

**Backend**

```bash
cd api
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

API docs are available at `http://localhost:8000/docs`.

Optional frontend environment file (`frontend/.env`):

```
VITE_API_URL=http://localhost:8000
```

## API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/health` | Service status |
| POST | `/analyze` | Analyze an uploaded image (multipart form, field `file`) |
| POST | `/report` | Generate a PDF forensic report |
| GET | `/history` | Paginated analysis history |

Example response from `/analyze`:

```json
{
  "confidence_score": 75.5,
  "classification": "Manipulated",
  "ela_score": 70.0,
  "efficientnet_score": 80.0,
  "heatmap_base64": "iVBORw0KGgo...",
  "file_hash": "abc123...",
  "timestamp": "2026-03-08T12:00:00+05:30",
  "processing_time": 2.5
}
```

## Roadmap

- Additional forensic methods (JPEG ghost analysis, copy-move detection)
- Batch processing for multiple images
- Advanced filtering and search in history
- User authentication and multi-user support

## Troubleshooting

- **Frontend can't reach the API:** check `VITE_API_URL` and the CORS origins in the backend.
- **Analysis fails:** confirm the file is JPG or PNG and under 10 MB, then check the backend logs.

## License

Created for the Chakravyuh 2.0 hackathon.
