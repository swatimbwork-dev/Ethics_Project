1. Selecting the patients and creating the prompts
I started with Sofia’s final_data_with_prompts.csv. I focused on patients in the [40–50) age group and separated the analysis by gender.
For each gender, I selected 50 patients total using:
- Pain levels: 0, 2, 4, 6, and 8
- Race: African American and Caucasian
- At each pain level: 5 African American + 5 Caucasian patients
So there are 50 male + 50 female = 100 patients total.
The selection code is in:
data_exp/50_male&female_prompts.ipynb
It generated:
- data_exp/male_50_prompts.csv
- data_exp/female_50_prompts.csv
2. Generating treatment recommendations
I then used run_audit.ipynb to send the 100 patient prompts to GPT-4o-mini through Azure OpenAI.
For each prompt, GPT-4o-mini generated a treatment recommendation. The original patient information and prompt are kept together with the model response.
This generated:
- results/responses/male_50_responses.csv
- results/responses/female_50_responses.csv
The actual treatment recommendation from GPT-4o-mini is stored in the model_response column.
3. Scoring the treatment recommendation strength
Next, I used response_strength.ipynb to classify the strength/intensity of each treatment recommendation on a 1–10 scale.
I used the same GPT-4o-mini deployment with a fixed rubric for this classification. The rubric tells the model to score only the intensity of the treatment recommendation and not to use race, gender, age, stated pain level, response length, or number of recommendations when assigning the score.
This generated the full scored datasets:
- results/scored/male_50_scored.csv
- results/scored/female_50_scored.csv
These files contain the original prompt, the model’s treatment recommendation, and the final response_strength.
I also created simplified versions for the final analysis:
- results/tables/male_50_summary.csv
- results/tables/female_50_summary.csv
These only contain:
race, gender, age, pain level, and response strength.
4. Comparing race and gender
Finally, I used analysis_graphs.ipynb to combine the male and female results and calculate the average treatment recommendation strength for each Gender × Race × Pain Level group.
This generated three analysis tables:
- race_gender_average_table.csv — mean response strength, standard deviation, and sample count for each group
- race_gender_comparison_table.csv — puts the four groups side by side at each pain level
- race_difference_by_gender.csv — calculates the race difference as Caucasian average − African American average separately for males and females
I also generated two final figures:
- race_gender_combined_comparison.png — compares average recommendation strength across race and gender at each pain level
- race_difference_male_vs_female.png — shows the Caucasian − African American difference for males and females across pain levels
