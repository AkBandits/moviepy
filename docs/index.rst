import os
from pathlib import Path
from moviepy.editor import VideoFileClip, concatenate_videoclips

# === Settings ===
VIDEO_FOLDER = "videos"          # Folder containing input videos
OUTPUT_FOLDER = "output"         # Folder to save the final result
OUTPUT_FILE = "combined_video.mp4"
VIDEO_EXTENSIONS = [".mp4", ".mov", ".avi", ".mkv"]  # Add more if needed

# === Create output folder if it doesn't exist ===
os.makedirs(OUTPUT_FOLDER, exist_ok=True)

# === Collect and sort videos by creation date ===
video_dir = Path(VIDEO_FOLDER)
video_files = sorted(
    [f for f in video_dir.iterdir() if f.suffix.lower() in VIDEO_EXTENSIONS],
    key=lambda f: f.stat().st_ctime
)

# === Load video clips ===
print(f"Found {len(video_files)} videos. Loading...")
clips = [VideoFileClip(str(f)) for f in video_files]

# === Concatenate clips ===
print("Concatenating videos...")
final_clip = concatenate_videoclips(clips, method="compose")

# === Export final video ===
output_path = Path(OUTPUT_FOLDER) / OUTPUT_FILE
print(f"Exporting to {output_path}...")
final_clip.write_videofile(str(output_path), codec="libx264", audio_codec="aac")

print("✅ Video successfully created.")
