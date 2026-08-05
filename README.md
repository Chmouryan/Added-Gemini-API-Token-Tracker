# Gemini Token Usage Tracker for Google Colab

A lightweight Python utility designed for Jupyter Notebooks and Google Colab to automatically track, log, and visualize your token usage when working with the Google Gemini API. It wraps the official `google-genai` SDK and stores usage metrics locally in an SQLite database.

---

## Features

* **Seamless SDK Wrapping:** Functions as a drop-in wrapper for the official `genai.Client`.
* **Automatic Tracking:** Logs prompt tokens, completion tokens, and total tokens directly from the API's `usage_metadata`.
* **Stream Support:** Successfully captures and logs token counts even when using streaming responses.
* **Local Database:** Uses SQLAlchemy to automatically store all request logs locally in a `usage.db` SQLite file.
* **Visual Reporting:** Built-in reporting function that generates Pandas dataframes and interactive Plotly charts to visualize your daily API consumption.

---

## Requirements

Ensure you have the following dependencies installed in your environment:

```bash
pip install google-genai pandas plotly sqlalchemy

```

---

## Setup & Configuration

### 1. Colab Secrets

This tracker is configured to pull your API key securely from Google Colab Secrets.

1. Click on the **Secrets (🔑)** icon in the left sidebar of your Colab notebook.
2. Add a new secret named `GEMINI_API_KEY` and paste your Gemini API key.
3. Toggle the button to enable **Notebook access**.

### 2. Initialization

Import the necessary libraries and set up the database and tracking class as provided in the main notebook script. The script will automatically create `usage.db` in your current working directory.

---

## Usage

### Generating Content

Initialize the `TrackedGemini` client and use the `generate` method just like you would with standard API calls. The default model is set to `gemini-3.5-flash`.

```python
from google.colab import userdata

# Fetch the API key from Colab secrets
api_key = userdata.get('GEMINI_API_KEY')

# Initialize the tracker
client = TrackedGemini(api_key=api_key)

# Generate a response
response = client.generate("Explain artificial intelligence in one short sentence.")
print(response.text)

```

### Viewing Your Usage Report

To see your token usage metrics over a specific number of days, call the `show_report()` function.

```python
# Generate a report for the last 7 days (default)
show_report(days=7)

```

**The report outputs:**

1. **KPIs:** Total requests, total tokens used, and estimated cost (defaults to $0.00 for free tier).
2. **Interactive Chart:** A Plotly line graph plotting daily total token usage.
3. **Data Table:** A Pandas DataFrame breaking down prompt and completion tokens by model.
