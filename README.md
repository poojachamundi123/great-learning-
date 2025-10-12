import streamlit as st
import pandas as pd
import numpy as np
import pydeck as pdk
import matplotlib.pyplot as plt

st.title("📊 Student Performance Dashboard")
st.write("Visualizing student performance and rural learning data")

# ---- SAMPLE DATA ----
data = {
    "Student": ["Asha", "Ravi", "Kiran", "Pooja", "Anita", "Teja"],
    "Math": [78, 64, 90, 45, 85, 52],
    "Science": [72, 58, 95, 50, 80, 60],
    "English": [69, 70, 88, 55, 75, 62],
    "latitude": [17.385, 17.450, 16.506, 17.000, 18.520, 15.828],
    "longitude": [78.486, 78.500, 80.632, 79.600, 73.856, 78.037]
}

df = pd.DataFrame(data)

# ---- BAR CHART ----
st.subheader("📘 Average Marks per Subject")
subject_means = df[["Math", "Science", "English"]].mean()
st.bar_chart(subject_means)

# ---- PIE CHART ----
st.subheader("🎯 Learning Outcome Distribution")
outcomes = ["Excellent", "Good", "Average", "Needs Improvement"]
values = [25, 35, 25, 15]

fig, ax = plt.subplots()
ax.pie(values, labels=outcomes, autopct="%1.1f%%", startangle=90)
ax.axis("equal")
st.pyplot(fig)

# ---- MAP ----
st.subheader("🌍 Student Locations on Map")
st.map(df[["latitude", "longitude"]])

# ---- DECK.GL MAP (Interactive 3D Map) ----
st.subheader("🗺️ Interactive 3D Map (Advanced)")
layer = pdk.Layer(
    "ScatterplotLayer",
    data=df,
    get_position='[longitude, latitude]',
    get_color='[200, 30, 0, 160]',
    get_radius=5000,
)
view_state = pdk.ViewState(latitude=17.5, longitude=78.5, zoom=6, pitch=0)
st.pydeck_chart(pdk.Deck(map_style='mapbox://styles/mapbox/light-v9', layers=[layer], initial_view_state=view_state))


