Mobility-Based Disaster Preparedness Analysis
This project analyzes disaster preparedness using human mobility data, with a focus on three major disaster events: Hurricane Ian (2022), Kentucky Flood (2022), and Hurricane Sally (2020). The study aims to measure baseline mobility patterns and how they change during the preparedness phase before these disasters, providing insights into community readiness and response.

Table of Contents
Project Overview
Data Processing Pipeline
1. Data Download
2. File Extraction
3. Data Filtering
4. Merging with Population Data
5. Normalization
6. Preparedness Measurement
7. Data Cleaning
8. T-Tests
9. Visualization


Project Overview
This project downloads mobility data for specific disaster-affected counties, processes it, and analyzes changes in mobility during preparedness periods. The goal is to measure how mobility changes in response to disaster warnings and compare preparedness levels across different events.

The analysis is performed for three disaster areas:

Breathitt County, Kentucky (Flood, 2022)
Lee County, Florida (Hurricane Ian, 2022)
Baldwin County, Alabama (Hurricane Sally, 2020)
Mobility data is filtered for each county and analyzed to determine the baseline and preparedness period mobility. The project also compares disaster preparedness across events using statistical methods.

Data Processing Pipeline
1. Data Download
The data is downloaded from a specified API, which provides mobility data for the whole United States. The script extracts data for a specific time period relevant to the disasters under study.

python
Copy code
# Download data for specific counties
while True:
    results = requests.get(...)
    # Save each downloaded file
2. File Extraction
The downloaded files are compressed in .gz format. The script extracts these files and stores them in a specified output directory.


# Extract files from .gz format
extract_all_gz_files(input_directory, output_directory)
3. Data Filtering
For each disaster, mobility data is filtered using FIPS codes to select only the block groups relevant to the county being studied.


# Filter data by FIPS code
filtered_df = filter_rows_by_fips(df, fips_code='12071')  # Example for Lee County
4. Merging with Population Data
The mobility data is merged with population data to normalize the number of visits based on the population of each block group. This is done to ensure that the mobility measures account for differences in population size.


# Merge population data with mobility data
merged_df = pd.merge(file1_df, file2_df, left_on='visitor_home_cbgs', right_on='ORIGIN_BG', how='inner')
5. Normalization
Mobility data is normalized using scaling factors derived from the population and the number of devices detected in each block group. This provides more accurate mobility metrics for analysis.


# Normalize mobility data
df['scaling_factor'] = df['Population_total'] / df["NUMBER_DEVICES_RESIDING"]
df["visitor_scaled"] = df["count"] * df['scaling_factor']
6. Preparedness Measurement
Preparedness is measured by calculating the change in mobility from the baseline period to the preparedness period (typically the two weeks leading up to the disaster).


# Calculate baseline and preparedness mobility
df['Base_Mo'] = baseline_mobility_value
df['Prepared_Mo'] = preparedness_mobility_value
df['Measure_Mo'] = ((preparedness_mobility_value - baseline_mobility_value) / baseline_mobility_value) * 100
7. Data Cleaning
The dataset is cleaned by removing duplicates, unnecessary columns, and rows with null values. This ensures the integrity of the analysis.


# Remove duplicates and clean up columns
df_cleaned = df.drop_duplicates(subset=['raw_visit_counts', 'raw_visitor_counts', ...])
8. T-Tests
Statistical tests are performed to compare preparedness across the different disaster events (e.g., Hurricane Ian vs. Flood). T-tests help determine if the differences in preparedness are statistically significant.


# Perform T-tests for Preparedness Period Mobility
t_test_preparedness_hf = ttest_ind(preparedness_hurricane_Ian, preparedness_flood, equal_var=False)
9. Visualization
The project generates time series plots of mobility for each disaster event, highlighting key periods such as the preparedness phase.


# Plot mobility trends over time
plt.plot(data['Week'], data['visit_counts_cbg_scaled'], label='Mobility over the weeks')

Run the scripts in the following order:

Download data from the API.
Extract .gz files.
Filter the data for specific counties.
Merge the data with population data.
Normalize and analyze mobility patterns.
Perform statistical tests and generate plots.
