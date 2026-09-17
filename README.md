# Experiment 4: Data Wrangling and Data Visualization

**Submitted by: Daphne P. Robleado**

**Section: 2ECE-D**

This experiment serves as a guide to students in filtering multi-condition tabular datasets, constructing specific DataFrames, computing categorical mean summaries, and generating formatted visualizations to analyze ECE board exam performance.



## II. Intended Learning Outcomes
By the end of this activity, the student is expected to:
1. Filter tabular data using several categorical and numerical conditions

2. Construct focused DataFrames by selecting relevant features

3. Summarize the relationship between categorical features and a numerical variable

4. Communicate a data comparison using clear and correctly labeled plots.

## III. Materials Used
1.) ECE2112_PA4.pdf manual  
2.) Jupyter Notebook  
3.) board2.xlsx dataset  

## IV. General Instructions
Use the same ECE Board Exam 2 dataset supplied for Experiment 4. Work in a Jupyter Notebook using Pandas and a Python plotting library used in class. Use the dataset’s existing column labels, including Name, Gender, Track, Hometown, Math, GEAS, Electronics, and Average.

1.) Derive all tables and plot values from the dataset. Do not manually type rows, category means, or plotted values.

2.) When applying more than one condition, make every condition explicit in the filtering expression.  

3.) Keep the original DataFrame unchanged

4.) Every graph must have a title, axis labels, readable category labels, and a consistent scale appropriate to the data.


## V. Programming Problems

### Importing Required Libraries
```
import pandas as pd
import matplotlib.pyplot as plt
```
*Imports the Pandas library for data manipulation, reshaping, and aggregation, and the Matplotlib Pyplot module for generating data visualizations.*


### A. VISAYAS COMMUNICATION DATAFRAME
**Expected Output:** Load `board2.xlsx`, compute the overall `Average` grade per student across Math, GEAS, Electronics, and Communication, filter records where `Hometown` is Visayas and `Track` is Communication, and retain only the columns `Name`, `Gender`, `Math`, `Electronics`, and `Average` in that exact order into `VisComm`. Display the resulting DataFrame and its row count.

**Requirement:** Display the resulting DataFrame and its number of rows. Both filtering conditions must be applied to the source dataset before the columns are selected.

**Solution:**

***i. Load dataset and compute Average***
```
df = pd.read_excel('board2.xlsx')
df['Average'] = df[['Math', 'GEAS', 'Electronics', 'Communication']].mean(axis=1)
df
```
*Loads the Excel dataset into DataFrame `df` and calculates the mean across the four exam subjects using `.mean(axis=1)` into a new column `Average`.*

***Result:***

<img width="508" height="802" alt="image" src="https://github.com/user-attachments/assets/6a25e7ea-5ab6-41fe-87e9-d97fc8c7123a" />


***ii. Filter by Hometown and Track, then select required columns***
```
viscom_filtered = df.loc[(df['Hometown'] == 'Visayas') & (df['Track'] == 'Communication')]
```
*Applies Boolean conditions with bitwise AND (`&`) to isolate examinees from Visayas taking the Communication track before column selection.*

```
VisComm = pd.DataFrame(viscom_filtered, columns=['Name', 'Gender', 'Math', 'Electronics', 'Average'])
VisComm
```
*Shows only `'Name'`, `'Gender'`, `'Math'`, `'Electronics'`, and `'Average'` into `VisComm`.*

```
print("Number of rows:", len(VisComm))
```
*Confirms the row count of the resulting subset.*

***Result:***

<img width="278" height="153" alt="image" src="https://github.com/user-attachments/assets/266ec9df-8b14-4925-a937-5cc776129383" />

```
Number of rows: 5
```

---

### B. VISAYAS FEMALE DATAFRAME
**Expected Output:** Create a DataFrame named `VisFemale` containing students whose `Hometown` is Visayas and whose `Gender` is Female, retaining only `Name`, `Track`, `GEAS`, `Electronics`, and `Average`. Display `VisFemale`, then display only examinees whose `Average` is at least 60 without overwriting `VisFemale`.

**Requirement:** Do not overwrite `VisFemale` when performing the second conditional filter.

**Solution:**

***i. Filter female examinees from Visayas and project features***
```
visfem_filtered = df.loc[(df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female')]
VisFemale = pd.DataFrame(visfem_filtered, columns=['Name', 'Track', 'GEAS', 'Electronics', 'Average'])
VisFemale
```
*Applies Boolean indexing using `(df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female')` and retains only the target features into `VisFemale`.*

***Result:***

<img width="312" height="180" alt="image" src="https://github.com/user-attachments/assets/8ccd77b1-0c55-4bfc-9b9a-4665968c59be" />


***ii. Filter rows where Average is at least 60***
```
VisFemale_Ave_60 = VisFemale.loc[(VisFemale['Average'] >= 60)]
VisFemale_Ave_60
```
*Gets `VisFemale` with the numerical condition `Average >= 60` and stores the resulting 4-row subset into `VisFemale_Ave_60`.*

***Result:***

<img width="316" height="136" alt="image" src="https://github.com/user-attachments/assets/060a2533-ce56-4aa0-82a3-1e360553c895" />

---

### C. CATEGORY-AVERAGE VISUALIZATION
**Expected Output:** Examine how `Average` varies across `Track`, `Gender`, and `Hometown` by computing group means, displaying summary tables, plotting a 3-panel bar chart, and providing concise interpretation statements identifying the category with the highest sample mean for each feature.

**Requirement:** Every graph must include titles, axis labels, and readable category labels. Describe the observed dataset only without asserting causation.

**Solution:**

***i. Compute mean Average across categorical features***
```
track_mean = df.pivot_table(index=['Track'], values='Average').reset_index()
track_mean
```
```
gender_mean = df.pivot_table(index=['Gender'], values='Average').reset_index()
gender_mean
```
```
hometown_mean = df.pivot_table(index=['Hometown'], values='Average').reset_index()
hometown_mean
```
*Gets the arithmetic mean of `Average` for each category using `pivot_table()` and resets the indices to return clean tabular summaries.*

***Result:***

<img width="166" height="98" alt="image" src="https://github.com/user-attachments/assets/db0e582c-a38d-4031-9456-e15bb74aa8ce" />

<img width="158" height="98" alt="image" src="https://github.com/user-attachments/assets/7d8c7072-df5c-48f2-9912-aa8e3291c3bf" />

<img width="162" height="101" alt="image" src="https://github.com/user-attachments/assets/d2f669c3-289c-462c-adf1-c0e6f5018579" />



***ii. Visualize mean Average using Matplotlib bar charts***
```
plt.figure(figsize=(20, 5))

plt.subplot(1, 3, 1)
plt.bar(track_mean['Track'], track_mean['Average'])
plt.title('Average Grades per Track')
plt.xlabel('Track')
plt.ylabel('Mean Average Grade')

plt.subplot(1, 3, 2)
plt.bar(gender_mean['Gender'], gender_mean['Average'])
plt.title('Average Grades per Gender')
plt.xlabel('Gender')
plt.ylabel('Mean Average Grade')

plt.subplot(1, 3, 3)
plt.bar(hometown_mean['Hometown'], hometown_mean['Average'])
plt.title('Average Grades per Hometown')
plt.xlabel('Hometown')
plt.ylabel('Mean Average Grade')

```
*Creates a single figure comprising three subplots displaying the mean `Average` scores by Track, Gender, and Hometown, with titles and axis labels.*

***Result:***

<img width="861" height="247" alt="image" src="https://github.com/user-attachments/assets/5a0a34dc-b00b-4414-8127-7c4156740121" />


***iii. Identify highest category means and provide statements***
```
max_track_df = track_mean.loc[[track_mean['Average'].idxmax()]]
max_gender_df = gender_mean.loc[[gender_mean['Average'].idxmax()]]
max_hometown_df = hometown_mean.loc[[hometown_mean['Average'].idxmax()]]

print('Highest Track Mean')
print(max_track_df)

print('\nHighest Gender Mean')
print(max_gender_df)

print('\nHighest Hometown Mean')
print(max_hometown_df)
```

***Result:***
```
Highest Track Mean
           Track  Average
0  Communication   67.975

Highest Gender Mean
  Gender    Average
1   Male  67.183333

Highest Hometown Mean
  Hometown    Average
0    Luzon  68.083333
```

**Interpretation:**
1. **Track:** In the observed dataset, students in the Communication track achieved the highest sample mean average grade of 67.975.
2. **Gender:** In the observed dataset, Male students achieved the highest sample mean average grade of 67.183.
3. **Hometown:** In the observed dataset, students from Luzon achieved the highest sample mean average grade of 68.083.


---

## VI. See Version History
1. 09/15/2026 - Started the ipynb notebook
2. 09/16/2026 - Finalized ipynb notebook
3. 09/17/2026 - Created GitHub repository for Experiment 4
4. 09/17/2026 - Uploaded the ipynb notebook
5. 09/17/2026 - Generated category-average summary tables, Matplotlib bar charts, and interpretation statements 









