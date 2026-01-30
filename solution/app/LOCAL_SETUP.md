# Local Setup and Running Guide

Follow these steps to set up and run the User and Post API application on your local machine.

## Prerequisites
- Linux/Ubuntu environment
- Python 3.12+ installed
- `curl` (for testing)

## 1. Environment Setup

Open your terminal in the project directory (`nebula-aurora-assignment`).

### Create and Activate Virtual Environment
It is recommended to use a virtual environment to isolate dependencies.

```bash
# Clean up any previous broken environment
rm -rf .venv

# Create a new virtual environment
python3 -m venv .venv

# Activate the virtual environment
source .venv/bin/activate
```
*Note: If you run into an error creating the venv on Ubuntu, you might need to run `sudo apt install python3.12-venv`.*

### Install Dependencies
Install the required packages using the generated `requirements.txt`.

```bash
pip install -r requirements.txt
```

## 2. Running the Application

Start the FastAPI server using `uvicorn`. This command will start the server on port `8000` with hot-reloading enabled.

```bash
uvicorn app.main:app --reload
```
You should see output indicating the server is running at `http://127.0.0.1:8000`.

## 3. Verifying the Deployment

Open a **new terminal window** (keep the server running in the first one), navigate to the project directory, and run the test script.

```bash
# Make sure the script is executable
chmod +x test_api.sh

# Run the automated tests
./test_api.sh
```

**Expected Output:**
The script will run through several tests (creating users, posts, checking error handling) and should finish with:
```
✓ Created 3 users
✓ Retrieved users by ID
✓ Created 3 posts
✓ Retrieved posts by ID
✓ Tested error handling (404s)
✓ Verified Prometheus metrics

All tests completed!
```

## 4. Accessing Documentation & Metrics

Once functionality is verified, you can access the interactive API docs and metrics in your browser:

- **Swagger UI:** [http://localhost:8000/docs](http://localhost:8000/docs)
- **ReDoc:** [http://localhost:8000/redoc](http://localhost:8000/redoc)
- **Prometheus Metrics:** [http://localhost:8000/metrics](http://localhost:8000/metrics)
