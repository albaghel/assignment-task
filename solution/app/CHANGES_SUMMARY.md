# Project Changes Summary

This document outlines the modifications made to the codebase to ensure successful deployment and testing.

## 1. Created `requirements.txt`
- **Change:** Generated a `requirements.txt` file populated with dependencies from `pyproject.toml`.
- **Reason:** The `README.md` instructs users to run `pip install -r requirements.txt`, but the file was missing. This restores consistency between the documentation and the project files.

## 2. Updated `test_api.sh`
- **Change (Port Fix):** Updated `BASE_URL` to use port `8000` instead of `8080`.
  - **Reason:** The `uvicorn` server runs on port `8000` by default. The mismatch prevented the test script from connecting to the API.
  
- **Change (Database Cleanup):** Disabled the automatic deletion of `app.db` at the start of the script.
  - **Reason:** The application initializes database tables only on **startup**. Deleting the database file while the server is running causes "table not found" errors because the server does not recreate tables dynamically. Disabling this step allows tests to run against the active, correctly initialized server.
