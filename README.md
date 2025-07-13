# urban-air-quality

To create a Python application for monitoring and analyzing real-time urban air quality data using IoT sensors, we'll make use of several libraries and tools. Below is a simple example program illustrating how you might set up such a project. This program will simulate data retrieval from IoT sensors, process the data, and provide a simple analysis with error handling built in.

```python
import random
import time
import pandas as pd
import matplotlib.pyplot as plt

# Simulate IoT sensor data retrieval
def get_sensor_data():
    """
    Simulates the retrieval of air quality data from IoT sensors.
    Returns a dictionary with simulated sensor data for PM2.5, PM10, and NO2.
    """
    try:
        data = {
            'PM2.5': random.uniform(5, 150),  # Particulate matter < 2.5 micrometers
            'PM10': random.uniform(10, 200),  # Particulate matter < 10 micrometers
            'NO2': random.uniform(10, 100)    # Nitrogen dioxide
        }
        return data
    except Exception as e:
        print(f"Error retrieving sensor data: {e}")
        return None

# Analyze the air quality data
def analyze_air_quality(data):
    """
    Analyzes the air quality data.
    Prints a simple analysis and creates a plot.
    """
    try:
        pm25 = data['PM2.5']
        pm10 = data['PM10']
        no2 = data['NO2']

        # Simple thresholds for demonstration purposes
        pm25_threshold = 35
        pm10_threshold = 50
        no2_threshold = 40

        print("Air Quality Analysis:")
        if pm25 > pm25_threshold:
            print(f"Warning: PM2.5 levels ({pm25:.1f}) exceed the safe threshold ({pm25_threshold})!")
        else:
            print(f"PM2.5 levels ({pm25:.1f}) are within safe limits.")

        if pm10 > pm10_threshold:
            print(f"Warning: PM10 levels ({pm10:.1f}) exceed the safe threshold ({pm10_threshold})!")
        else:
            print(f"PM10 levels ({pm10:.1f}) are within safe limits.")

        if no2 > no2_threshold:
            print(f"Warning: NO2 levels ({no2:.1f}) exceed the safe threshold ({no2_threshold})!")
        else:
            print(f"NO2 levels ({no2:.1f}) are within safe limits.")

        # Generate a plot
        plt.figure(figsize=(10, 5))
        labels = ['PM2.5', 'PM10', 'NO2']
        values = [pm25, pm10, no2]
        plt.bar(labels, values, color=['green', 'blue', 'red'])
        plt.xlabel('Pollutants')
        plt.ylabel('Concentration (ug/m3)')
        plt.title('Current Air Quality Levels')
        plt.show()

    except Exception as e:
        print(f"Error analyzing air quality data: {e}")

# Store the data in a DataFrame
def store_data(data, df):
    """
    Stores new sensor data into a pandas DataFrame.
    Handles exception if data is invalid.
    """
    try:
        current_time = pd.to_datetime('now')
        row = {'Timestamp': current_time, 'PM2.5': data['PM2.5'], 'PM10': data['PM10'], 'NO2': data['NO2']}
        df = df.append(row, ignore_index=True)
        return df
    except Exception as e:
        print(f"Error storing sensor data: {e}")
        return df

# Main function to run the application
def main():
    """
    Main function to run the air quality monitoring application.
    """
    # DataFrame to store historical data
    df = pd.DataFrame(columns=['Timestamp', 'PM2.5', 'PM10', 'NO2'])

    while True:
        print("\nRetrieving new sensor data...")
        sensor_data = get_sensor_data()

        if sensor_data is not None:
            print(f"Retrieved data: {sensor_data}")
            df = store_data(sensor_data, df)
            analyze_air_quality(sensor_data)

        # Simulate real-time data retrieval by pausing for a few seconds
        time.sleep(5)

# If this script is being run directly, execute the main function
if __name__ == "__main__":
    main()
```

### Explanation:

- **Data Simulation**: Since we don't have actual IoT sensors, `get_sensor_data` simulates sensor readings with random values.
  
- **Data Analysis**: The `analyze_air_quality` function checks the data against set thresholds and generates a plot of current pollutant levels.

- **Data Storage**: Data is stored using a `pandas DataFrame`, allowing you to analyze historical trends when the data collection is continuous.

- **Real-time Simulation**: The `main` function loops, simulating real-time data collection every 5 seconds.

- **Error Handling**: Each function contains basic error handling to print errors and maintain the program’s functionality in case unexpected data is encountered.

Ensure Python, pandas, and matplotlib are installed in your environment before running this program. You can refine and extend this program with real sensor data, more advanced analyses, and integrations with machine learning models for predictive capabilities.