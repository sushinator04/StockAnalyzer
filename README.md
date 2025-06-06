## Backend (FastAPI + Neo4j)

### 1. Overview
This service provides three endpoints:
- `POST /scrape_summary`  
  • Takes `{ "url": "..." }`, scrapes a short bio from the given investor/hedge-fund page, and returns `{ "summary": "…" }`.
- `GET /investors/{name}/investments`  
  • Calls an external holdings API (configured via `.env`) and returns that investor’s portfolio as JSON.
- `POST /investors/{name}/persist`  
  • Scrapes the bio, fetches holdings, and writes an `:Investor` node, `:Stock` nodes, and `:OWNS` relationships into Neo4j.

### 2. Prerequisites
- Python 3.9+ (we recommend installing via Homebrew on macOS)
- Neo4j (community or Aura Free)
- Git 
- (Optional) Postman or `curl` for testing

### 3. Setup

```bash
# 1. Clone the repo and cd into it
git clone https://github.com/<your-username>/investor-portfolio-graph.git
cd investor-portfolio-graph/backend

# 2. Create a Python virtual environment and activate it
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Create a .env file at backend/.env with:
#    NEO4J_URI=bolt://localhost:7687
#    NEO4J_USER=neo4j
#    NEO4J_PASSWORD=<your_neo4j_password>
#    HOLDINGS_API_URL=https://api.example.com/holdings
#    HOLDINGS_API_KEY=<your_api_key>

# 5. Start Neo4j (if not already running):
brew services start neo4j

# 6. Run the FastAPI server
uvicorn app.main:app --reload
