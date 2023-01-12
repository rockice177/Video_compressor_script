# Video_compressor_script
Batch Video compressor

# **Project Explanation**

Using the moviepy library, which can be used to edit and manipulate videos:
Please note that this script won't work with all types of videos, and also some videos maybe unable to be compressed further.

import os
from moviepy.editor import VideoFileClip

# The directory containing the videos to be compressed
input_dir = 'path/to/input/videos'
# The directory where the compressed videos should be saved
output_dir = 'path/to/output/videos'

# The compression settings
fps = 30
bitrate = '2000k'

# Create a text file to log the compression achieved
log_file = open("compression.txt", "w")

# Check and create the input_dir and output_dir directories if they don't exist
if not os.path.exists(input_dir):
    os.makedirs(input_dir)
if not os.path.exists(output_dir):
    os.makedirs(output_dir)

total_saved = 0

for filename in os.listdir(input_dir):
    if filename.endswith('.mp4'):
        try:
            # Open the video using VideoFileClip
            video = VideoFileClip(os.path.join(input_dir, filename))
            # Compress the video
            video = video.fx(video.fps_drop, fps).set_fps(fps).set_bitrate(bitrate)
            # Save the compressed video
            video.write_videofile(os.path.join(output_dir, filename))
            # Get the size of the input and output video
            input_size = os.path.getsize(os.path.join(input_dir, filename))
            output_size = os.path.getsize(os.path.join(output_dir, filename))
            # Calculate the compression achieved and space saved
            compression = (1 - output_size / input_size) * 100
            space_saved = input_size - output_size
            total_saved += space_saved
            # Write the compression achieved and space saved to the log file
            log_file.write(f"{filename} - Compression achieved: {compression:.2f}% - Space saved: {space_saved:,} bytes\n")
        except Exception as e:
            # Write the error to the log file
            log_file.write(f"{filename} - Error: {e}\n")

# Write the total space saved to the log file
log_file.write(f"Total space saved: {total_saved:,} bytes\n")
# Close the log file
log_file.close()

This script keeps track of the space saved by the compression for each file, by subtracting the size of the output file from the size of the input file. At the end of the script, it writes the total space saved in bytes, by adding up all the space saved for each file.
Please note that the {:,} used in the log file is a python string formatter which separates large numbers with commas for better readability.
