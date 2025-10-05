n8n-video-trimmer
An n8n workflow for automatically creating and uploading short video clips from a YouTube video.

Overview
This workflow automates the process of downloading a YouTube video, generating subtitles, splitting the video into shorter clips of 40-60 seconds, and uploading them to YouTube. It uses a combination of tools like yt-dlp, whisper, and ffmpeg, and leverages an OpenAI model to intelligently create the clips.

Features
Automated Video Processing: Trigger the workflow with a YouTube URL to automatically process the video.

Subtitle Generation: Uses whisper to generate subtitles for the video.

AI-Powered Clipping: Leverages an OpenAI Chat Model to create a series of 40-60 second clips.

Video Manipulation: Uses ffmpeg to create the final video clips.

YouTube Upload: Automatically uploads the generated clips to YouTube.

Workflow Breakdown
The workflow is composed of the following steps:

Webhook Trigger: The workflow is initiated by a POST request to a webhook, which should contain the URL of the YouTube video to be processed.

Video ID Extraction: The YouTube video ID is extracted from the provided URL.

Directory Creation: A new directory is created to store the video and its related files.

Video Download: The YouTube video is downloaded using yt-dlp.

Audio Extraction: A WAV audio file is created from the downloaded video.

Subtitle Generation: whisper is used to generate subtitles in SRT format from the audio.

Subtitle Processing: The SRT file is read and processed to extract text segments with timestamps.

AI Clipping: The subtitle segments are passed to an OpenAI Chat Model. The model is prompted to create a JSON list of 58–60 second clips, each with a title.

Clip Splitting: The JSON output from the AI is split to process each clip individually.

Video Clipping: ffmpeg is used to create the individual video clips based on the start and end times provided by the AI.

Final Video Creation: Another ffmpeg command is used to create the final video by stacking the generated clip with a background video (subway.mp4).

YouTube Upload: The final video clips are uploaded to YouTube.

Prerequisites
Before using this workflow, ensure you have the following installed and configured:

n8n

yt-dlp

ffmpeg

whisper

An OpenAI API key

YouTube OAuth2 credentials

Setup
Import the Workflow: Import the data.json file into your n8n instance.

Configure Credentials:

OpenAI: Configure the "OpenAI Chat Model" node with your OpenAI API key.

YouTube: Configure the "YouTube" node with your YouTube OAuth2 credentials.

Directory Paths: The workflow uses local directory paths (~/Desktop/video/). You may need to update these paths in the "Execute Command" nodes to match your system's directory structure.

Usage
To use the workflow, send a POST request to the webhook URL provided by the "Webhook" node. The body of the request should be a JSON object with a url key containing the URL of the YouTube video you want to process.

Example:

JSON

{
  "url": "https://www.youtube.com/watch?v=your_video_id"
}
Customization
You can customize the workflow to fit your needs:

Clip Duration: Modify the prompt in the "Video Trimmer" agent node to change the desired duration of the clips.

AI Model: Change the AI model in the "OpenAI Chat Model" node.

ffmpeg Commands: Modify the ffmpeg commands in the "Execute Command" nodes to change the video processing. For example, you can change the background video or the video resolution.

YouTube Upload Settings: Adjust the parameters in the "YouTube" node to change the title, description, privacy status, etc., of the uploaded videos.