import tkinter as tk
from tkinter import ttk
from pytube import YouTube

def download_video_audio():
    url = url_entry.get()
    choice = choice_var.get()

    try:
        yt_object = YouTube(url)

        if choice == '1':
            file_name = audio_name_entry.get()
            audio = yt_object.streams.filter(only_audio=True).first()
            audio.download(output_path=path, filename=file_name)
            status_label.config(text="Download completed successfully.")
        elif choice == '2':
            quality = quality_var.get()
            video = yt_object.streams.filter(res=quality, file_extension='mp4').first()
            video.download(output_path=path)
            status_label.config(text="Download completed successfully.")
        else:
            status_label.config(text="Invalid choice.")
    except Exception as e:
        status_label.config(text="Error: Invalid URL or download failed.")

root = tk.Tk()
root.title("YouTube Video Downloader") 
url_label = ttk.Label(root, text="Enter URL of the YouTube Video:")
url_entry = ttk.Entry(root, width=40)
choice_label = ttk.Label(root, text="Choose an option:")
choice_var = tk.StringVar()
choice_var.set('1')
audio_radio = ttk.Radiobutton(root, text="Audio only", variable=choice_var, value='1')
video_radio = ttk.Radiobutton(root, text="Video and Audio", variable=choice_var, value='2')

audio_name_label = ttk.Label(root, text="Enter Name for Your Audio File:")
audio_name_entry = ttk.Entry(root, width=40)

quality_label = ttk.Label(root, text="Choose Video Quality:")
quality_var = tk.StringVar()
quality_var.set('360p')
quality_combobox = ttk.Combobox(root, textvariable=quality_var, values=['360p', '480p', '720p'])

download_button = ttk.Button(root, text="Download", command=download_video_audio)
status_label = ttk.Label(root, text="")

url_label.grid(row=0, column=0, padx=10, pady=5, sticky=tk.W)
url_entry.grid(row=0, column=1, padx=10, pady=5, columnspan=2)
choice_label.grid(row=1, column=0, padx=10, pady=5, sticky=tk.W)
audio_radio.grid(row=1, column=1, padx=10, pady=5)
video_radio.grid(row=1, column=2, padx=10, pady=5)
audio_name_label.grid(row=2, column=0, padx=10, pady=5, sticky=tk.W)
audio_name_entry.grid(row=2, column=1, padx=10, pady=5, columnspan=2)
quality_label.grid(row=3, column=0, padx=10, pady=5, sticky=tk.W)
quality_combobox.grid(row=3, column=1, padx=10, pady=5, columnspan=2)
download_button.grid(row=4, column=0, columnspan=3, padx=10, pady=10)
status_label.grid(row=5, column=0, columnspan=3, padx=10, pady=5)

root.mainloop()
