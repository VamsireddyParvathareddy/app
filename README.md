# app
import tkinter as tk
from tkinter import ttk
import time
from datetime import datetime
from playsound import playsound
import threading

class ReminderApp:
    def init(self, root):
        self.root = root
        self.root.title("Reminder App")

        # List of days, times, and activities
        self.days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
        self.activities = ["Wake up", "Go to gym", "Breakfast", "Meetings", "Lunch", "Quick nap", 
                           "Go to library", "Dinner", "Go to sleep"]

        # Day selection
        self.day_label = ttk.Label(root, text="Day of the Week:")
        self.day_label.pack(pady=5)
        self.day_var = tk.StringVar()
        self.day_dropdown = ttk.Combobox(root, textvariable=self.day_var, values=self.days)
        self.day_dropdown.pack(pady=5)

        # Time selection
        self.time_label = ttk.Label(root, text="Choose Time:")
        self.time_label.pack(pady=5)
        self.time_var = tk.StringVar()
        self.time_entry = ttk.Entry(root, textvariable=self.time_var)
        self.time_entry.insert(0, "HH:MM:SS")
        self.time_entry.pack(pady=5)

        # Activity selection
        self.activity_label = ttk.Label(root, text="Select Activity:")
        self.activity_label.pack(pady=5)
        self.activity_var = tk.StringVar()
        self.activity_dropdown = ttk.Combobox(root, textvariable=self.activity_var, values=self.activities)
        self.activity_dropdown.pack(pady=5)

        # Set reminder button
        self.set_reminder_button = ttk.Button(root, text="Set Reminder", command=self.set_reminder)
        self.set_reminder_button.pack(pady=20)

        # Status Label
        self.status_label = ttk.Label(root, text="", foreground="green")
        self.status_label.pack(pady=5)

    def set_reminder(self):
        selected_day = self.day_var.get()
        selected_time = self.time_var.get()
        selected_activity = self.activity_var.get()

        if not selected_day or not selected_time or not selected_activity:
            self.status_label.config(text="Please select all fields!", foreground="red")
            return

        self.status_label.config(text=f"Reminder set for {selected_day} at {selected_time} for {selected_activity}.", foreground="green")
        
        # Start a new thread to wait for the time and then play the sound
        threading.Thread(target=self.wait_for_time, args=(selected_time,)).start()

    def wait_for_time(self, reminder_time):
        while True:
            current_time = datetime.now().strftime("%H:%M:%S")
            if current_time == reminder_time:
                playsound("chime.mp3")  # Path to your sound file
                break
            time.sleep(1)

if name == "main":
    root = tk.Tk()
    app = ReminderApp(root)
    root.mainloop()
