# Sports-Team-Performance-Analysis
# In this project, we analyse the performance of a sports team by time scale for a season.This is done by  observing various match statistics like win/loss records, goals scored, and goals conceded. The ultimate  goal here is trying to look at the team as an overall unit in performance, determine patterns and  trends over time and give suggestions based on the performance
# Importing necessary libraries for the project
import pandas as pd
import matplotlib.pyplot as plt

# Step 1: Asking the user to input the data of the match for the analysis and suggestion 
print("Welcome to the Sports Team Performance Analysis!")
print("Please enter match data. To stop entering, type 'done'.")

# Initializing an empty list to store match data
match_data = []

# defining a Loop to collect user inputs
while True:
    date = input("Enter match date (YYYY-MM-DD) or type 'done' to finish: ")
    if date.lower() == 'done':
        break
    opponent = input("Enter opponent name: ")
    goals_scored = int(input("Enter goals scored by your team: "))
    goals_conceded = int(input("Enter goals conceded by your team: "))
    
    # Append the input data as a dictionary to the list
    match_data.append({
        'Date': date,
        'Opponent': opponent,
        'Goals_Scored': goals_scored,
        'Goals_Conceded': goals_conceded
    })

# Convert the list of dictionaries into a DataFrame
df = pd.DataFrame(match_data)

# Step 2: Add a column to classify match results
def classify_match(row):
    if row['Goals_Scored'] > row['Goals_Conceded']:
        return 'Win'
    elif row['Goals_Scored'] < row['Goals_Conceded']:
        return 'Loss'
    else:
        return 'Draw'

df['Result'] = df.apply(classify_match, axis=1)

# Step 3: Calculate summary statistics
total_matches = len(df)
wins = len(df[df['Result'] == 'Win'])
losses = len(df[df['Result'] == 'Loss'])
draws = len(df[df['Result'] == 'Draw'])
win_percentage = (wins / total_matches) * 100
average_goals_scored = df['Goals_Scored'].mean()
average_goals_conceded = df['Goals_Conceded'].mean()

# Display summary
print("\n--- Performance Summary ---")
print("Total Matches:", total_matches)
print("Wins:", wins)
print("Losses:", losses)
print("Draws:", draws)
print(f"Win Percentage: {win_percentage:.2f}%")
print(f"Average Goals Scored Per Match: {average_goals_scored:.2f}")
print(f"Average Goals Conceded Per Match: {average_goals_conceded:.2f}")

# Step 4: Compare with Ideal Performance
ideal_win_percentage = 70  # Ideal win percentage
ideal_average_goals_scored = 3.0  # Ideal average goals scored per match
ideal_average_goals_conceded = 1.0  # Ideal average goals conceded per match

print("\n--- Suggestions for Improvement ---")
if win_percentage < ideal_win_percentage:
    print(f"1. Improve win rate! Target at least {ideal_win_percentage}%.")
if average_goals_scored < ideal_average_goals_scored:
    print(f"2. Work on offense! Target an average of at least {ideal_average_goals_scored} goals per match.")
if average_goals_conceded > ideal_average_goals_conceded:
    print(f"3. Strengthen defense! Reduce goals conceded to less than {ideal_average_goals_conceded} per match.")
if win_percentage >= ideal_win_percentage and \
   average_goals_scored >= ideal_average_goals_scored and \
   average_goals_conceded <= ideal_average_goals_conceded:
    print("Great job! Your team is meeting the ideal performance standards!")

# Step 5: Visualization
# Bar Chart: Results Count
result_counts = df['Result'].value_counts()
plt.figure(figsize=(6, 4))
result_counts.plot(kind='bar', color=['green', 'red', 'blue'])
plt.title('Match Results')
plt.xlabel('Result')
plt.ylabel('Number of Matches')
plt.show()

# Line Chart: Goals Scored vs. Conceded
plt.figure(figsize=(8, 4))
plt.plot(df['Date'], df['Goals_Scored'], marker='o', label='Goals Scored', color='green')
plt.plot(df['Date'], df['Goals_Conceded'], marker='o', label='Goals Conceded', color='red')
plt.axhline(y=ideal_average_goals_scored, color='blue', linestyle='--', label='Ideal Goals Scored')
plt.axhline(y=ideal_average_goals_conceded, color='orange', linestyle='--', label='Ideal Goals Conceded')
plt.title('Goals Scored vs Conceded Over Time')
plt.xlabel('Date')
plt.ylabel('Goals')
plt.legend()
plt.show()

# Step 6: Save the data (optional)
df.to_csv('team_performance.csv', index=False)
print("\nData saved to 'team_performance.csv'.")

