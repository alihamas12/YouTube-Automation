# YouTube-Automation
# n8n YouTube Automation for "Growth Daily"

This is an n8n workflow that fully automates the creation and uploading of short motivational videos for the "Growth Daily" YouTube channel. It pulls content ideas from a Google Sheet, uses AI to generate all assets (script, images, voice), compiles them into a video, and publishes it directly to YouTube.



##  workflow Overview

The automation process runs in the following sequence:

1.  **Fetch Idea:** The workflow manually triggers and reads the "Growth Daily" Google Sheet. It looks for the first row in the "Mindset Quotes" tab that has a **"Pending"** status.
2.  **Script Generation:** The "Idea" text from the sheet is sent to a **Google Gemini** AI Agent, which writes a concise 30-second script suitable for a voiceover.
3.  **Image Prompting:** The AI-generated script is passed to a second **Google Gemini** Agent. This agent generates 5 distinct, consistent image prompts that visually tell the story of the script.
4.  **Image Generation:** The workflow loops through these 5 prompts, making a call to the **Fal.ai (Imagen4)** API for each one to generate 5 high-quality images.
5.  **Voice Generation:** The original script is sent to **Deepgram** (using the `aura-2-thalia-en` model) to create a high-quality, realistic voiceover.
6.  **File Hosting:** The generated MP3 audio file is uploaded to **Google Drive**, and its sharing permissions are set to "anyone with the link" to create a public download URL.
7.  **Video Creation:** The 5 image URLs from Fal.ai and the public audio URL from Google Drive are sent to the **json2video.com** API. This service combines them into a single `instagram-story` (9:16) resolution video with zooming effects and subtitles.
8.  **YouTube Upload:** After a 1-minute wait for the video to render, the workflow fetches the final video file from json2video and uploads it directly to **YouTube**. It uses the "Idea" as the video title, the script as the description, and applies pre-defined tags.
9.  **Update Status:** Finally, the workflow updates the original row in the **Google Sheet** status from "Pending" to **"Done"**, completing the loop.

---

## 🚀 Services Used

* **Orchestration:** **n8n**
* **Content Source:** **Google Sheets**
* **AI Scripting & Prompting:** **Google Gemini**
* **Image Generation:** **Fal.ai**
* **Text-to-Speech (TTS):** **Deepgram**
* **File Hosting:** **Google Drive**
* **Video Compilation:** **json2video.com**
* **Publishing:** **YouTube**

---

## ⚙️ Setup Instructions

1.  **Import Workflow:** Import the `workflow.json` file into your n8n instance.
2.  **Create Credentials:** You will need API keys or OAuth2 credentials for all the services listed above.
3.  **Add Credentials in n8n:** Go to the "Credentials" section in your n8n instance and add your credentials for:
    * Google (for Sheets, Drive, and Gemini)
    * Fal.ai
    * Deepgram
    * json2video.com
    * YouTube (OAuth2)
4.  **Connect Nodes:** Open the workflow and connect your new credentials to each of the corresponding nodes (e.g., connect your Google credentials to the Google Sheets nodes, your Fal.ai key to the HTTP Request nodes, etc.).
5.  **Verify Sheet Setup:** Ensure your Google Sheet has the correct tabs ("Mindset Quotes") and columns (`Idea`, `status`, `row_number`).
6.  **Activate & Run:** Activate the workflow and run it manually to test the full automation.

## 📄 License

You can add a license to your repository. [MIT License](https://opensource.org/licenses/MIT) is a common and permissive choice for open-source projects.
