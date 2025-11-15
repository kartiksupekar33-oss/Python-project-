# Python-project-
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import os

# --- 1. Define Constants and Setup ---
OUTPUT_FILE = 'project_report_chart.png'
DATA_COUNT = 100

def generate_mock_data():
    """Generates a DataFrame with mock sales and region data."""
    print("Generating mock dataset...")
    
    # Create random data points
    dates = pd.date_range(start='2024-01-01', periods=DATA_COUNT, freq='D')
    sales = np.random.randint(100, 1000, size=DATA_COUNT)
    regions = np.random.choice(['North', 'South', 'East', 'West'], size=DATA_COUNT)
    
    # Create DataFrame
    data = pd.DataFrame({
        'Date': dates,
        'Sales': sales,
        'Region': regions
    })
    
    return data

def analyze_and_visualize(df):
    """
    Performs basic data analysis and generates a visualization.
    
    Args:
        df (pd.DataFrame): The input DataFrame.
    """
    print("\n--- Data Analysis Started ---")
    
    # Calculate key metrics (Feature 1)
    total_sales = df['Sales'].sum()
    print(f"Total Sales recorded: ${total_sales:,.2f}")
    
    # Group and Aggregate Data (Feature 2)
    # Calculate total sales per region
    region_summary = df.groupby('Region')['Sales'].sum().sort_values(ascending=False)
    
    print("\nSales Summary by Region:")
    print(region_summary)
    
    # --- Data Visualization (Project in Action) ---
    print("\nGenerating bar chart visualization...")
    
    # Create the figure and axes for the plot
    fig, ax = plt.subplots(figsize=(10, 6))
    
    # Create a bar chart
    region_summary.plot(kind='bar', ax=ax, color='#4299e1')
    
    # Customize the plot
    ax.set_title('Total Sales by Region (Q1 2024)', fontsize=16, fontweight='bold', color='#1a202c')
    ax.set_xlabel('Region', fontsize=12, color='#4a5568')
    ax.set_ylabel('Total Sales (USD)', fontsize=12, color='#4a5568')
    ax.tick_params(axis='x', rotation=0) # Keep region names horizontal
    
    # Add labels on top of the bars for clarity
    for i, v in enumerate(region_summary):
        ax.text(i, v + 200, f'${v:,.0f}', color='black', ha='center', fontsize=10)
        
    # Set background and grid
    ax.set_facecolor('#f7fafc')
    ax.grid(axis='y', linestyle='--', alpha=0.7)
    
    # Remove plot borders
    for spine in ax.spines.values():
        spine.set_visible(False)

    # Save the chart to a file
    plt.tight_layout()
    try:
        plt.savefig(OUTPUT_FILE)
        print(f"\nSuccessfully saved visualization to: {os.path.abspath(OUTPUT_FILE)}")
    except Exception as e:
        print(f"Error saving file: {e}")
    finally:
        plt.close(fig) # Close the figure to free up memory

if __name__ == "__main__":
    # Check if required libraries are available (common challenge)
    try:
        # 1. Generate Data
        data_frame = generate_mock_data()
        
        # 2. Analyze and Visualize
        analyze_and_visualize(data_frame)
        
        print("\nScript finished. Check your directory for the chart image.")
        
    except ImportError as e:
        print(f"\n--- ERROR ---")
        print(f"Missing required library: {e}")
        print("Please ensure you have pandas, numpy, and matplotlib installed.")
        print("You can install them using: pip install pandas numpy matplotlib")
        print("-------------")
    except Exception as e:
        print(f"\nAn unexpected error occurred: {e}")
