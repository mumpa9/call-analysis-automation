Call Analysis Automation
An AI-powered system for analyzing phone calls and extracting structured information.
Project status: In development
Overview
This project automates the analysis of phone conversations using AI.
The system processes audio recordings, extracts important information about the conversation, and saves the results in a structured format.
Workflow
1.	A phone call recording is uploaded to Google Drive.
2.	n8n detects and processes the recording.
3.	An AI model analyzes the audio.
4.	Important information is extracted into structured fields.
5.	The results are saved to Google Sheets.
Technologies Used
•	n8n - workflow automation
•	Google Drive - audio file storage
•	Google Gemini - AI-powered audio analysis
•	Google Sheets - structured data storage
Extracted Information
The system is designed to identify:
•	Client name and phone number
•	Company and product
•	Budget and interest level
•	Customer questions and objections
•	Manager promises
•	Conversation summary
•	Recommended next steps
Current Limitations
•	Audio recordings are uploaded manually.
•	Some workflow steps still require improvement.
•	The system is not yet fully automated.
Future Improvements
•	Fully automate audio recording processing.
•	Improve data validation and error handling.
•	Add a Python backend using FastAPI.
•	Introduce a database for storing call information.
•	Develop a web interface for viewing analytics.
Privacy
Demonstrations use synthetic data. Real phone recordings, personal information, API keys, and access tokens are not published.
________________________________________
This project is being developed as a practical learning project in AI Engineering and backend development.
