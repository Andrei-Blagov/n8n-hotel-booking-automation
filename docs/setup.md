# Setup

1. Create one Google Spreadsheet with sheets named `Rooms` and `Bookings`.
2. Use the schemas from `sample-data/rooms.csv` and `sample-data/bookings.csv`.
3. Import both JSON files from `workflows/` into n8n.
4. In every Google Sheets node, select your own spreadsheet and the correct sheet.
5. Connect your Google Sheets OAuth credential.
6. Connect your Gmail OAuth credential.
7. Replace `manager@example.com` with the email that should receive validation alerts and daily reports.
8. Test the New Request workflow with non-overlapping and overlapping date ranges.
9. Test the Daily Report workflow manually.
10. Publish/activate the workflows when ready.

## Important

The public workflow JSON does not contain working credentials or the original spreadsheet ID. These values must be configured in your n8n instance after import.