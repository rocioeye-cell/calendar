name: Update Free Events Calendar
on:
  schedule:
    - cron: "0 1 * * *" # 每天台北時間09:00執行
  workflow_dispatch:

jobs:
  update-calendar:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: pip install requests beautifulsoup4 google-api-python-client google-auth

      - name: Create credentials.json
        run: echo "${{ secrets.GOOGLE_CREDENTIALS_JSON }}" > credentials.json

      - name: Run script
        env:
          GOOGLE_CALENDAR_ID: ${{ secrets.GOOGLE_CALENDAR_ID }}
        run: python beclass_to_calendar.py
