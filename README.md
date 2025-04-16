# Fitness-Tracker-APP
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime

# Title
st.title("🔥 Fitness Tracker App")

# Load existing data or create new
def load_data():
    try:
        return pd.read_csv("fitness_data.csv")
    except FileNotFoundError:
        return pd.DataFrame(columns=["Date", "Workout Type", "Duration (min)", "Calories Burned"])

data = load_data()

# Input Section
st.header("Add New Workout")
workout_type = st.selectbox("Workout Type", ["Running", "Cycling", "Yoga", "Strength", "Swimming", "Other"])
duration = st.number_input("Duration (minutes)", min_value=0, step=1)
calories = st.number_input("Calories Burned", min_value=0, step=1)

if st.button("Add Workout"):
    new_entry = {
        "Date": datetime.now().strftime("%Y-%m-%d %H:%M"),
        "Workout Type": workout_type,
        "Duration (min)": duration,
        "Calories Burned": calories
    }
    data = pd.concat([data, pd.DataFrame([new_entry])], ignore_index=True)
    data.to_csv("fitness_data.csv", index=False)
    st.success("Workout added! You're killin' it.")

# View Logs
st.header("Your Workout Log")
st.dataframe(data)

# Plot Progress
st.header("Progress Chart")
if not data.empty:
    data["Date"] = pd.to_datetime(data["Date"])
    fig, ax = plt.subplots(figsize=(10, 4))
    ax.plot(data["Date"], data["Calories Burned"], marker="o", color="tomato", label="Calories Burned")
    ax.set_xlabel("Date")
    ax.set_ylabel("Calories")
    ax.set_title("Calories Burned Over Time")
    ax.legend()
    st.pyplot(fig)
else:
    st.write("No data to show yet... let’s sweat some more, babe!")

