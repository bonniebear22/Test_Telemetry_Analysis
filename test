import streamlit as st
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
import matplotlib.pyplot as plt
import io

# Mock Data (Replace with real telemetry data)
def load_mock_data():
    data = {
        'LapDistPct': np.linspace(0, 100, 100),
        'Speed': np.random.uniform(50, 200, 100),
        'Throttle': np.random.uniform(0, 100, 100),
        'Brake': np.random.uniform(0, 100, 100),
        'PosX': np.cumsum(np.random.uniform(-1, 1, 100)),
        'PosY': np.cumsum(np.random.uniform(-1, 1, 100)),
    }
    return pd.DataFrame(data)

# Sidebar Configuration
st.sidebar.title("Telemetry Dashboard")
selected_metric = st.sidebar.selectbox("Select Metric", options=["Speed", "Throttle", "Brake"])
uploaded_file = st.sidebar.file_uploader("Upload Telemetry File", type=["ibt", "csv"])

# Main Title
st.title("iRacing Telemetry Dashboard")

# Mock telemetry data
data = load_mock_data()

# Function to Plot Event Markers
def plot_event_markers(df):
    fig = px.scatter(df, x='PosX', y='PosY', title='Track Layout with Event Markers')

    # Braking zones
    brake_events = df[df['Brake'] > 50]
    fig.add_trace(go.Scatter(
        x=brake_events['PosX'], 
        y=brake_events['PosY'], 
        mode='markers',
        marker=dict(size=8, color='red', symbol='x'),
        name='Braking Zone'
    ))

    # Apexes
    apex_events = df[df['Throttle'] < 20]
    fig.add_trace(go.Scatter(
        x=apex_events['PosX'], 
        y=apex_events['PosY'], 
        mode='markers',
        marker=dict(size=8, color='green', symbol='triangle-up'),
        name='Apex'
    ))

    fig.update_layout(showlegend=True)
    st.plotly_chart(fig)

# Function to Analyze Sectors
def sector_analysis(df, sectors=5):
    df['Sector'] = pd.qcut(df['LapDistPct'], sectors, labels=[f'Sector {i+1}' for i in range(sectors)])
    sector_data = df.groupby('Sector').mean()[['Speed', 'Throttle', 'Brake']]
    
    st.bar_chart(sector_data)

# Function to Plot Lap Time Comparison
def lap_time_comparison(lap_times):
    df = pd.DataFrame({
        'Lap': [f'Lap {i+1}' for i in range(len(lap_times))],
        'Lap Time (s)': lap_times
    })
    st.bar_chart(df.set_index('Lap'))

# Function to Export Data and Visualizations
def export_to_csv(df):
    buffer = io.StringIO()
    df.to_csv(buffer, index=False)
    buffer.seek(0)
    st.download_button(label="Download Telemetry Data as CSV", data=buffer, file_name="telemetry_data.csv", mime="text/csv")

# Interactive Visuals
st.subheader("Track Layout with Event Markers")
plot_event_markers(data)

st.subheader("Sector-Specific Analysis")
sector_analysis(data)

st.subheader("Lap Time Comparison")
lap_time_comparison([75.3, 73.9, 74.5])  # Mock lap times

# Export Section
st.subheader("Export Telemetry Data")
export_to_csv(data)

