# Lung
lung-repair-dashboard/ │ ├── app.py                      # Your Streamlit code ├── lung_repair_protocol_langchain.json  # Your JSON protocol └── requirements.txt            # Dependencies
import streamlit as st
import json
from datetime import datetime

# Load protocol JSON
with open("lung_repair_protocol_langchain.json") as f:
    protocol = json.load(f)

st.title(protocol["protocol_name"])
st.caption(f"Start Date: {protocol['start_date']} | Duration: {protocol['duration_days']} Days")

# Day selector
selected_day = st.slider("Select Day", 1, protocol["duration_days"], 1)

st.subheader(f"Tasks for Day {selected_day}")

for task in protocol["tasks"]:
    if task["active_days"] == "daily" or selected_day in task["active_days"]:
        st.markdown(f"**{task['time']} – {task['name']}**")
        st.write(task["instructions"])
        st.checkbox(f"Mark '{task['name']}' as done", key=f"{task['name']}_{selected_day}")

st.markdown("---")
st.subheader("Protocol Notes")
for note in protocol["notes"]:
    st.markdown(f"- {note}")
