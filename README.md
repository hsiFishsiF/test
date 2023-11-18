# test
fish
fish_v
><> fish <><

fish — 10/18/2023 3:31 AM
ok so i tried q7 for aminute and this is what i got
`
def total_points(grades):
    #labs
    lab_totals = lab_total(process_labs(grades))

    #projects
Expand
message.txt
3 KB
but it doesnt work
and im gonna try tmr morn to fix it
quenn — 10/18/2023 9:39 AM
Ahh ok ok
quenn — 10/18/2023 11:00 AM
for 6 did you ever use lowest_scores
quenn — 10/18/2023 11:42 AM
def final_grades(total):
    ans = []
    for i in total:
        if i >= 0.9:
            ans.append('A')
        elif i >= 0.8:
            ans.append('B')
        elif i >= 0.7:
            ans.append('C')
        elif i>= 0.6:
            ans.append('D')
        else:
            ans.append('F')

    result = pd.Series(ans, name='Letter Grades')

    return result



def letter_proportions(total):
    letter_grades = final_grades(total)

    proportions = letter_grades.value_counts()
    count = letter_grades.count()
    ans = proportions/count
    return ans
quenn — 10/18/2023 12:38 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
11 KB
quenn — 10/18/2023 3:13 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
11 KB
quenn — 10/18/2023 4:04 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
13 KB
fish — 10/18/2023 5:22 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
14 KB
fish — 10/18/2023 6:00 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
15 KB
quenn — 10/18/2023 8:11 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
15 KB
fish — 10/19/2023 5:34 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
15 KB
fish — 10/20/2023 1:09 AM
Image
fish — 10/20/2023 2:23 PM
Image
fish — 10/23/2023 12:34 PM
def clean_universities(df):
    mod_df = df.copy()
    mod_df.replace('\n',',', regex=True,inplace=True)
    mod_df['broad_impact'] = mod_df['broad_impact'].astype(int)
    mod_df['national_rank'] = mod_df['national_rank'].str.replace('UK', 'United Kingdom').str.replace('USA', 'United States').str.replace('Czechia', 'Czech Republic')
    mod_df['nation'] = mod_df['national_rank'].str.split(',').str[0]
Expand
message.txt
3 KB
fish — 10/23/2023 5:12 PM
def pet_name_by_owner(owners, pets):
    pets_owners = pd.merge(pets,owners,on = 'OwnerID',how = 'left')
    pets_owners = pets_owners.groupby('OwnerID').agg(lambda x: x.tolist() if len(x) > 1 else x.iloc[0])
    pets_owners = pets_owners[['Name_y','Name_x']]

    def extract_element(lst):
        return lst[0] if isinstance(lst, list) and len(lst) > 0 else lst

    # Apply the function to the 'Values' column
    pets_owners['First Name'] = pets_owners['Name_y'].apply(extract_element)
    pets_owners['Pet Name'] = pets_owners['Name_x']
    pets_owners.drop(['Name_x','Name_y'],inplace=True, axis = 1)
    pets_owners.set_index('First Name', inplace = True)
    return pets_owners['Pet Name']
fish — 10/23/2023 6:05 PM
def total_cost_per_city(owners, pets, procedure_history, procedure_detail):
    pets_price = pd.merge(procedure_history, procedure_detail, on = ['ProcedureType', 'ProcedureSubCode'], how = 'left')[['PetID','Price']]
    pets_price = pets_price.groupby('PetID').sum()
    pet_owner_city = pd.merge(pets,owners,on='OwnerID',how='outer')[['PetID','OwnerID','City']]
    city_price = pd.merge(pet_owner_city,pets_price,on='PetID',how='left').groupby('City').sum()
    city_price.drop('OwnerID',axis=1,inplace=True)
    return city_price['Price']
quenn — 10/23/2023 6:10 PM
def average_seller(sales):
    avgs = sales.groupby('Name')['Total'].sum() / sales.groupby('Name')['Total'].count()

    result_df = pd.DataFrame({'Average Sales': avgs})

    return result_df

def product_name(sales):

    product = sales.groupby(['Name','Product'])['Total'].sum()
    result_df = product.unstack()

    return result_df

def count_product(sales):
    num = sales.groupby(['Product', 'Name', 'Date'])['Name'].count()

    result_df = num.unstack().fillna(0)

    return result_df

def total_by_month(sales):
    sale = sales.copy()
    sale['Date'] = pd.to_datetime(sale['Date'], format='%m.%d.%Y')


    sale['Month'] = sale['Date'].dt.strftime('%B')


    sale = sale.drop(columns=['Date'])

    tot = sale.groupby(['Name', 'Product', 'Month'])['Total'].sum()
    result = tot.unstack().fillna(0)
    return result
quenn — 10/25/2023 11:34 AM
def clean_israel_data(df):
    clean_df = df.copy()
    new_ages = pd.to_numeric(clean_df['Age'], errors = 'coerce')
    clean_df['Age'] = new_ages
    clean_df['Vaccinated'] = clean_df['Vaccinated'].astype(bool)
    clean_df['Severe Sickness'] = clean_df['Severe Sickness'].astype(bool)
    return clean_df
fish — 10/25/2023 11:49 AM
def count_monotonic(arr):
    count = 0
    for i in range(1, len(arr)):
        if arr[i] < arr[i - 1]:
            count += 1
    return count

def monotonic_violations_by_country(vacs):
    vacs.sort_values(['Country_Region', 'Date'], inplace=True)

   # Calculate monotonic violations for 'Doses_admin'
    doses_admin_monotonic = vacs.groupby('Country_Region')['Doses_admin'].diff().lt(0)

   # Calculate monotonic violations for 'People_at_least_one_dose'
    people_at_least_one_dose_monotonic = vacs.groupby('Country_Region')['People_at_least_one_dose'].diff().lt(0)

   # Count the number of violations for each country
    violations_count = {
        'Doses_admin_monotonic': doses_admin_monotonic.groupby(vacs['Country_Region']).sum(),
        'People_at_least_one_dose_monotonic': people_at_least_one_dose_monotonic.groupby(vacs['Country_Region']).sum()
    }

   # Create a DataFrame from the counts
    violations_df = pd.DataFrame(violations_count).fillna(0).astype(int)

    return violations_df



def robust_totals(vacs):
    def get_97th_percentile(series):
        return np.percentile(series, 97)

    doses = vacs.groupby('Country_Region')['Doses_admin'].apply(get_97th_percentile)
    people = vacs.groupby('Country_Region')['People_at_least_one_dose'].apply(get_97th_percentile)

    final_df = pd.merge(doses, people, on = 'Country_Region', how='outer')
    return final_df 
quenn — 10/25/2023 12:48 PM
def effectiveness(df):
    vac_df = df[df['Vaccinated'] == True]
    no_vac_df = df[df['Vaccinated'] == False]
    vac_counts = vac_df.shape[0]
    no_vac_counts = no_vac_df.shape[0]

    vac_sick = vac_df[vac_df['Severe Sickness'] == True].shape[0]
    no_vac_sick = no_vac_df[no_vac_df['Severe Sickness'] == True].shape[0]

    pv = vac_sick / vac_counts
    pu = no_vac_sick / no_vac_counts

    eff = 1 - (pv / pu)
    return eff
quenn — 10/25/2023 4:12 PM
def effectiveness_calculator(
    ,
    young_vaccinated_prop,
    old_vaccinated_prop,
    young_risk_vaccinated,
    young_risk_unvaccinated,
    old_risk_vaccinated,
    old_risk_unvaccinated

):

    young_effectiveness = 1 - (young_risk_vaccinated / young_risk_unvaccinated)
    old_effectiveness = 1 - (old_risk_vaccinated / old_risk_unvaccinated)


    overall_effectiveness = (young_vaccinated_prop young_effectiveness + old_vaccinated_prop * old_effectiveness) / (young_vaccinated_prop + old_vaccinated_prop)

    result = {
        'Overall': overall_effectiveness,
        'Young': young_effectiveness,
        'Old': old_effectiveness
    }

    return result
quenn — 10/27/2023 6:29 PM
def fix_dtypes(pops_raw):
    pops = pops_raw.copy()
    pops['World Percentage'] = pops['World Percentage'].str.replace('%', '').astype(float)/100
    pops['Population in 2023'] = pops['Population in 2023'].astype(str).str.replace('.', '').astype(int)
    pops['Country (or dependency)'] = pops['Country (or dependency)'].astype(str)
    pops['ISO'] = pops['ISO'].astype(str)
    return pops


---------------------------------------------------------------------
QUESTION 4
---------------------------------------------------------------------
def missing_in_pops(tots, pops):
    tots_country_names = set(tots.index)
    pops_country_names = set(pops.reset_index()['Country (or dependency)'])

    missing_countries = tots_country_names - pops_country_names

    return missing_countries


def fix_names(pops):
    popss = pops.copy()
    name_mapping = {
        'Myanmar': 'Burma',
        'Cape Verse': 'Cabo Verde',
        'Republic of the Congo' : 'Congo (Brazzaville)',
        'DR Congo' : 'Congo (Kinshasa)',
        'Ivory Coast' : "Cote d'Ivoire",
        'Czech Republic' : 'Czechia',
        'South Korea' : 'Korea, South',
        'United States' : 'US',
        'Palestine': 'West Bank and Gaza',
    }

    popss['Country (or dependency)'] = popss['Country (or dependency)'].replace(name_mapping)

    return popss
fish — 10/30/2023 4:52 PM
def count_frequency(login):
    df = login.copy(deep = True)
    df['Time'] = pd.to_datetime(df['Time'])

    def days_since_first_login(series):
        today_date = pd.Timestamp('2023-01-31 23:59')
        return len(series)/((today_date - series.min()).days)

    return df.groupby('Login Id')['Time'].agg(days_since_first_login)
fish — 10/30/2023 9:13 PM
for i in skittles.columns[:-1]:
    print(i,pval_color(skittles,i))
def diff_of_means(data, col='orange'):
    x = data.groupby('Factory')[col].mean()
    return abs(x[0]-x[1])


def simulate_null(data, col='orange'):
    with_shuffled = data.assign(Shuffled=np.random.permutation(data[col]))
    return diff_of_means(with_shuffled, 'Shuffled')



def pval_color(data, col='orange'):
    observed_statistic = diff_of_means(data, col)

    num_permutations = 1000
    permuted_statistics = []

    for i in range(num_permutations):
        permuted_statistics.append(simulate_null(data, col))

    p_value = np.mean(np.abs(permuted_statistics) >= np.abs(observed_statistic))

    return p_value
fish — 11/01/2023 7:06 PM
# project.py


import pandas as pd
import numpy as np
import os
Expand
project.py
3 KB
quenn — 11/01/2023 7:06 PM
um its blank
fish — 11/01/2023 7:06 PM
wait
wrong file
quenn — 11/01/2023 7:06 PM
lol
fish — 11/01/2023 7:06 PM
# project.py


import numpy as np
import pandas as pd
import pathlib
Expand
project.py
10 KB
fish — 11/01/2023 7:07 PM
it was for project 1 AHAHA
quenn — 11/01/2023 7:08 PM
loolllll
fish — 11/01/2023 8:18 PM
# project.py


import numpy as np
import pandas as pd
import pathlib
Expand
project.py
10 KB
fish — 11/13/2023 2:35 AM
# lab.py


import os
import pandas as pd
import numpy as np
Expand
lab.py
7 KB
fish — 11/15/2023 1:16 AM
Attachment file type: unknown
template.ipynb
2.33 KB
Attachment file type: unknown
template.ipynb
89.13 KB
quenn — 11/15/2023 7:38 PM
Attachment file type: unknown
template_4.ipynb
199.17 KB
quenn — 11/15/2023 9:56 PM
"Percentage of wins with first blood" typically refers to the proportion of games won when a team secures the first blood. It's calculated as the number of games won when the first blood is obtained, divided by the total number of games where first blood occurs.

On the other hand, "win rate with first blood" is more specific to the team that secures the first blood. It's the proportion of games won by the team that gets the first blood, calculated as the number of games won by the team with the first blood, divided by the total number of games where the team gets the first blood.

In summary, "percentage of wins with first blood" is a general metric that considers all teams, while "win rate with first blood" specifically focuses on the team that achieves the first blood. The calculation methods are similar, but the denominator in the latter case only considers games where the specified team achieves the first blood.
quenn — 11/15/2023 10:08 PM
Attachment file type: unknown
template_4.ipynb
238.75 KB
quenn — Yesterday at 8:10 PM
gender_dist = (
    heights_mcar
    .assign(child_missing=heights_mcar['child'].isna())
    .pivot_table(index='gender', columns='child_missing', aggfunc='size')
)

Added just to make the resulting pivot table easier to read.
gender_dist.columns = ['child_missing = False', 'child_missing = True']

gender_dist = gender_dist / gender_dist.sum()
gender_dist
fish — Yesterday at 11:17 PM
Attachment file type: unknown
template_4_copy.ipynb
542.74 KB
quenn — Today at 1:01 PM
# League Of Legends Early Statistics Analysis 2023

**Name(s)**:  Quennie Zeng and Vaibhav Bommisetty

---
Expand
League_Of_Legends_Early_Stats_Analysis_2023.md
20 KB
to preview i downloaded markdown all in one and they u can ctrl shirt v
fish — Today at 1:47 PM
Attachment file type: unknown
template_4_copy.ipynb
661.67 KB
fish — Today at 4:26 PM
Attachment file type: unknown
template_4_copy.ipynb
535.62 KB
quenn — Today at 4:29 PM
# League Of Legends Early Statistics Analysis 2023

**Name(s)**:  Quennie Zeng and Vaibhav Bommisetty

---

## Introduction

We worked on a dataset of all professional competitive League of Legends matches that took place in 2022. In this dataset, every 12 rows are of the same game, of which 10 are for player statistics on each team and 2 are for team statistics. In total, this dataset contains 149,400 rows, alluding to 12,450 games in total.
Our question is: “Which early game statistic can help us determine the outcome of a game the most?”
To help us find the answer to our question, we used 4 different categories of statistics: 

### 1. Outcome of Game

We needed a column to let us know which team won the game, that column was “result”: 
- **`result`** - The value in this column lets us know if the team won or not. We cleaned the data so that if the team won the value is True, and if the team lost the value is False.

### 2. First to Objective Statistics:

These statistics give us insight into the team objectives in the early game. All of these columns contain booleans, as the objectives are either achieved or not achieved.

- **`firstblood`** - The value is True if the team achieved first blood, and False if the team did not achieve first blood. (A bonus of 150 gold is given to the player who gets first blood)
- **`firstdragon`** - The value is True if the team claimed the first dragon, and False if the team did not claim the first dragon. (The first dragon spawns at exactly 5 minutes into the match)
- **`firstherald`** - The value is True if the team claimed the first herald, and False if the team did not claim the first herald. (The first dragon spawns at exactly 8 minutes into the match)
- **`firsttower`**  - The value is True if the team destroyed their first tower and False if the team did not destroy the first tower. (This statistic is an early game statistic, even though it depends on how the game is going. 11 minutes is a good estimation of when the first tower is taken.)

### 3. Statistics at 10 Minutes:

These statistics are gathered at exactly 10 minutes into the game.

- **`goldat10`** - This gives the gold count of the team at 10 minutes.
- **`xpat10`** - This gives the xp of the team at 10 minutes.
- **`csat10`** - This gives the cs (creep score) of the team at 10 minutes.
- **`golddiffat10`** - This gives the difference in the gold between the teams at 10 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat10`** - This gives the difference in the xp between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat10`** - This gives the difference in the cs between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`killsat10`** - This gives the number of kills that the team has at 10 minutes.
- **`assistsat10`**  - This gives the number of assists that the team has at 10 minutes.

### 4. Statistics at 15 minutes:

Similar to the previous section, this section gives team statistics for 15 minutes.

- **`goldat15`** - This gives the gold count of the team at 15 minutes.
- **`xpat15`** - This gives the xp of the team at 15 minutes.
- **`csat15`** - This gives the cs (creep score) of the team at 15 minutes.
- **`golddiffat15`** - This gives the difference in the gold between the teams at 15 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat15`** - This gives the difference in the xp between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat15`** - This gives the difference in the cs between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`killsat15`** - This gives the number of kills that the team has at 15 minutes.
- **`assistsat15`** - This gives the number of assists that the team has at 15 minutes.

---

## Cleaning and EDA

### Data Cleaning:

We decided to keep the columns above. When we first started looking at the data, we noticed that the number of wins and the number of losses were not the same, there were four more losses than wins, meaning that for two games there was no winner.

This is the head of our df dataframe:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 121.0 | 391.0 | 345.0 | 14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 100.0 | 541.0 | -275.0 | -11.0 | 2.0 | 3.0 | 2.0 | 0.0 | 5.0 | 1.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 119.0 | -475.0 | 153.0 | 1.0 | 0.0 | 3.0 | 0.0 | 3.0 | 3.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 149.0 | -793.0 | -1343.0 | -34.0 | 2.0 | 1.0 | 2.0 | 3.0 | 3.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 21.0 | 443.0 | -497.0 | 7.0 | 1.0 | 2.0 | 2.0 | 0.0 | 6.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 135.0 | -391.0 | -345.0 | -14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |





Games had either both winners or both losers:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| 34918 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 530.0 | 2824.0 | -795.0 | 16.0 | 2.0 | 4.0 | 4.0 | 4.0 | 5.0 | 2.0 |
| 34919 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 546.0 | -2824.0 | 795.0 | -16.0 | 4.0 | 5.0 | 2.0 | 2.0 | 4.0 | 4.0 |
| 87406 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 522.0 | -916.0 | 885.0 | 7.0 | 1.0 | 0.0 | 2.0 | 2.0 | 3.0 | 1.0 |
| 87407 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 529.0 | 916.0 | -885.0 | -7.0 | 2.0 | 3.0 | 1.0 | 1.0 | 0.0 | 2.0 |

We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear<sub>1</sub> And KRC Genk Esports won their game against LowLandLions~We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear.<sub>1</sub> And KRC Genk Esports won their game against LowLandLions <sub>2</sub>

1. [Sector One v mCon LG UltraGear](https://www.youtube.com/watch?v=fm7QNRBEcRI)
2. [KRC Genk Esports v LowLandLions](https://www.strafe.com/match/krc-genk-esports-vs-lowlandlions-regular-regular-summer-2022-f7a887/)

After we fixed the inaccuracy in the data, we then turned all columns that contain 1s and 0s into booleans. These included the columns: `result` and all the first to objective statistics. 

*Note: To keep only relevant columns, we removed the data that included information on late-game statistics or opposing team statistics. Since we were only looking to see what statistics in the early game impacted the outcome of the game, we removed columns such as 'firstbaron', as that is a late-game objective. We also removed opposing team statistics, as that could be found in the other team’s row. The removed columns were focused on late-game objectives and opposing team statistics, which were not relevant to the analysis of early game outcomes.*

So after examining the data and data cleaning, this is what the head of dataframe we would be working with looks like.

In this dataframe, we went through each 10th and 11th row, as that would be the results of each team, not player. Then we took out the relevant columns. When cleaning our data, we found a discreptancy within the results, where there were two games where the result was not recorded. So, we went back to the recording of the games and watched the games to individually fill out the result column. We changed the 1 and 0 to boolean on columns such as `firstdragon` to make it easier to comprehend. 

| gameid              | datacompleteness        | league | playoffs | result | firstblood | firstdragon | firstherald | firsttower | goldat10 | ... | deathsat10 | goldat15 | xpat15 | csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 |
... (191 lines left)
Collapse
League_Of_Legends_Early_Stats_Analysis_2023.md
21 KB
fish — Today at 4:40 PM
Attachment file type: unknown
template_4_copy.ipynb
535.62 KB
﻿
# League Of Legends Early Statistics Analysis 2023

**Name(s)**:  Quennie Zeng and Vaibhav Bommisetty

---

## Introduction

We worked on a dataset of all professional competitive League of Legends matches that took place in 2022. In this dataset, every 12 rows are of the same game, of which 10 are for player statistics on each team and 2 are for team statistics. In total, this dataset contains 149,400 rows, alluding to 12,450 games in total.
Our question is: “Which early game statistic can help us determine the outcome of a game the most?”
To help us find the answer to our question, we used 4 different categories of statistics: 

### 1. Outcome of Game

We needed a column to let us know which team won the game, that column was “result”: 
- **`result`** - The value in this column lets us know if the team won or not. We cleaned the data so that if the team won the value is True, and if the team lost the value is False.

### 2. First to Objective Statistics:

These statistics give us insight into the team objectives in the early game. All of these columns contain booleans, as the objectives are either achieved or not achieved.

- **`firstblood`** - The value is True if the team achieved first blood, and False if the team did not achieve first blood. (A bonus of 150 gold is given to the player who gets first blood)
- **`firstdragon`** - The value is True if the team claimed the first dragon, and False if the team did not claim the first dragon. (The first dragon spawns at exactly 5 minutes into the match)
- **`firstherald`** - The value is True if the team claimed the first herald, and False if the team did not claim the first herald. (The first dragon spawns at exactly 8 minutes into the match)
- **`firsttower`**  - The value is True if the team destroyed their first tower and False if the team did not destroy the first tower. (This statistic is an early game statistic, even though it depends on how the game is going. 11 minutes is a good estimation of when the first tower is taken.)

### 3. Statistics at 10 Minutes:

These statistics are gathered at exactly 10 minutes into the game.

- **`goldat10`** - This gives the gold count of the team at 10 minutes.
- **`xpat10`** - This gives the xp of the team at 10 minutes.
- **`csat10`** - This gives the cs (creep score) of the team at 10 minutes.
- **`golddiffat10`** - This gives the difference in the gold between the teams at 10 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat10`** - This gives the difference in the xp between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat10`** - This gives the difference in the cs between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`killsat10`** - This gives the number of kills that the team has at 10 minutes.
- **`assistsat10`**  - This gives the number of assists that the team has at 10 minutes.

### 4. Statistics at 15 minutes:

Similar to the previous section, this section gives team statistics for 15 minutes.

- **`goldat15`** - This gives the gold count of the team at 15 minutes.
- **`xpat15`** - This gives the xp of the team at 15 minutes.
- **`csat15`** - This gives the cs (creep score) of the team at 15 minutes.
- **`golddiffat15`** - This gives the difference in the gold between the teams at 15 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat15`** - This gives the difference in the xp between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat15`** - This gives the difference in the cs between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`killsat15`** - This gives the number of kills that the team has at 15 minutes.
- **`assistsat15`** - This gives the number of assists that the team has at 15 minutes.

---

## Cleaning and EDA

### Data Cleaning:

We decided to keep the columns above. When we first started looking at the data, we noticed that the number of wins and the number of losses were not the same, there were four more losses than wins, meaning that for two games there was no winner.

This is the head of our df dataframe:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 121.0 | 391.0 | 345.0 | 14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 100.0 | 541.0 | -275.0 | -11.0 | 2.0 | 3.0 | 2.0 | 0.0 | 5.0 | 1.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 119.0 | -475.0 | 153.0 | 1.0 | 0.0 | 3.0 | 0.0 | 3.0 | 3.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 149.0 | -793.0 | -1343.0 | -34.0 | 2.0 | 1.0 | 2.0 | 3.0 | 3.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 21.0 | 443.0 | -497.0 | 7.0 | 1.0 | 2.0 | 2.0 | 0.0 | 6.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 135.0 | -391.0 | -345.0 | -14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |





Games had either both winners or both losers:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| 34918 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 530.0 | 2824.0 | -795.0 | 16.0 | 2.0 | 4.0 | 4.0 | 4.0 | 5.0 | 2.0 |
| 34919 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 546.0 | -2824.0 | 795.0 | -16.0 | 4.0 | 5.0 | 2.0 | 2.0 | 4.0 | 4.0 |
| 87406 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 522.0 | -916.0 | 885.0 | 7.0 | 1.0 | 0.0 | 2.0 | 2.0 | 3.0 | 1.0 |
| 87407 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 529.0 | 916.0 | -885.0 | -7.0 | 2.0 | 3.0 | 1.0 | 1.0 | 0.0 | 2.0 |

We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear<sub>1</sub> And KRC Genk Esports won their game against LowLandLions~We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear.<sub>1</sub> And KRC Genk Esports won their game against LowLandLions <sub>2</sub>

1. [Sector One v mCon LG UltraGear](https://www.youtube.com/watch?v=fm7QNRBEcRI)
2. [KRC Genk Esports v LowLandLions](https://www.strafe.com/match/krc-genk-esports-vs-lowlandlions-regular-regular-summer-2022-f7a887/)

After we fixed the inaccuracy in the data, we then turned all columns that contain 1s and 0s into booleans. These included the columns: `result` and all the first to objective statistics. 

*Note: To keep only relevant columns, we removed the data that included information on late-game statistics or opposing team statistics. Since we were only looking to see what statistics in the early game impacted the outcome of the game, we removed columns such as 'firstbaron', as that is a late-game objective. We also removed opposing team statistics, as that could be found in the other team’s row. The removed columns were focused on late-game objectives and opposing team statistics, which were not relevant to the analysis of early game outcomes.*

So after examining the data and data cleaning, this is what the head of dataframe we would be working with looks like.

In this dataframe, we went through each 10th and 11th row, as that would be the results of each team, not player. Then we took out the relevant columns. When cleaning our data, we found a discreptancy within the results, where there were two games where the result was not recorded. So, we went back to the recording of the games and watched the games to individually fill out the result column. We changed the 1 and 0 to boolean on columns such as `firstdragon` to make it easier to comprehend. 

| gameid              | datacompleteness        | league | playoffs | result | firstblood | firstdragon | firstherald | firsttower | goldat10 | ... | deathsat10 | goldat15 | xpat15 | csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 |
|---------------------|-------------------------|--------|----------|--------|------------|-------------|-------------|------------|----------|-----|------------|----------|--------|--------|--------------|------------|------------|-----------|-------------|------------|
| 10                  | ESPORTSTMNT01_2690210   | complete | LCKC  | False  | False      | True        | False       | True       | 16218.0  | ... | 0.0        | 24806.0  | 28001.0 | 487.0  | -1617.0      | -23.0      | 5.0        | 10.0      | 6.0         | 0.0        |
| 11                  | ESPORTSTMNT01_2690210   | complete | LCKC  | False  | True       | False       | True        | False      | 14695.0  | ... | 3.0        | 24699.0  | 29618.0 | 510.0  | -107.0       | 1617.0     | 23.0       | 6.0       | 18.0        | 5.0        |
| 22                  | ESPORTSTMNT01_2690219   | complete | LCKC  | False  | False      | False       | False       | True       | 14939.0  | ... | 3.0        | 23522.0  | 28848.0 | 533.0  | -1763.0      | -906.0     | -22.0      | 1.0       | 1.0         | 3.0        |
| 23                  | ESPORTSTMNT01_2690219   | complete | LCKC  | False  | True       | True        | True        | False      | 16558.0  | ... | 1.0        | 25285.0  | 29754.0 | 555.0  | 1763.0       | 906.0      | 22.0       | 3.0       | 3.0         | 1.0        |
| 34                  | 8401-8401_game_1        | partial | LPL   | False  | True       | False       | NaN         | NaN        | NaN      | ... | NaN        | NaN      | NaN    | NaN    | NaN          | NaN        | NaN        | NaN       | NaN         | NaN        |




### EDA
#### Univariate Testing:

[insert pie chart]
These pie charts show the proportion of games won when a team secures an early game objective.


=======

These pie charts show the proportion of games won when a team secures an early game objective.


[insert histogram of golddiff for winning teams v gold diff for losing teams]

From these histograms, we can see that winning teams have a higher, more likely positive, gold difference than losing teams.


=======

[insert bar graphs]

These bar graphs show the proportion of teams that secure an early game objective that ends up winning. Different from the pie charts from before, these bar charts focus on the proportion of winning teams that claim objectives, as the pie charts focus on the win rates of teams that get certain early-game objectives.

### Bivariate Testing



This bar chart shows the winning percentages of teams that achieved certain objectives. We can see that getting first blood has a lesser impact on winning than claiming the other objectives, which are roughly the same.

### Interesting Aggregates

This `groupby()` highlights the mean number of early statistics when teams lose or win. The first row is when the team loses and the second row is when the team wins. We can see the distribution between winning or losing with a particular early stat. For instance we are much more likely to win with `firsttower` than win without it. This aggregate lets we see if we got an early stat what are the chances of we winning. 

| result | first_dragon_mean | first_blood_mean | first_herald_mean | first_tower_mean | goldat10 | xpat10 | csat10 | killsat10 | assistsat10 | deathsat10 | goldat15 | xpat15 | csat15 | killsat15 | assistsat15 | deathsat15 |
|--------|-------------------:|------------------:|-------------------:|------------------:|---------:|-------:|-------:|----------:|------------:|------------:|---------:|-------:|-------:|----------:|------------:|------------:|
| False  |            0.423102 |           0.390553 |            0.417458 |           0.316997 | 15343.21 | 17986.81 | 310.04 |    1.77067 |     2.69307 |     2.60380 | 23963.84 | 28893.52 | 493.83 |    3.22754 |      5.20929 |      4.93162 |
| True   |            0.576710 |           0.607920 |            0.582259 |           0.683003 | 16033.79 | 18412.27 | 318.96 |    2.59562 |     3.98128 |     1.77735 | 25688.54 | 29926.60 | 511.14 |    4.92061 |      7.96557 |      3.23657 |


---

## Assessment of Missingness

### NMAR Analysis

Yes, we believe that many columns in our data are Not Missing At Random due to the trends we see in the data. After cleaning the data and seeing how some pieces of data are missing due to the column itself, we can see that statistics are missing for certain losing teams and not missing for certain winning teams. This is also similar the other way around, meaning that the missingness of the rows depend on the type of columns and many other columns as well. This correlation cannot be based on another column's missingness (for the columns we are talking about), so it would not be missing by design. For other columns like `killsat15`, we see that it is MAR, where it is dependent on other columns, where one is `league` as leagues like LPL are missing this stat constantly. 

We also know some information are MAR like `firstdragon` and `golddiffat15` as we can see a correlation with these and `league`. For certain leagues, all the info for these columns are missing, so we can determine that `leagues` play a part in the missingness of other columns. 

### Missingness Dependency:

#### Missingness of ‘firstdragon’ depends on ‘league’:

Missingness of `firstdragon` does depend on `league`. We wanted to determine if `league` and `league` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `firstdragon` was not missing:

| league    | firstdragon |
|-----------|-------------|
| CBLOL      | 0.022858    |
| CBLOLA    | 0.020318    |
| CDF       | 0.006867    |
| CT        | 0.002446    |
| DDH       | 0.019659    |
| EBL       | 0.017402    |
| EL        | 0.012699    |
| ESLOL     | 0.022764    |
| EUM       | 0.025115    |
| GL        | 0.016367    |
| GLL       | 0.019095    |
| HC        | 0.015238    |
| HM        | 0.014392    |
| IC        | 0.007055    |
| LAS       | 0.021353    |
| LCK       | 0.043928    |
| LCKC      | 0.037061    |
| LCL       | 0.001505    |
| LCO       | 0.019942    |
| LCS       | 0.028784    |
| LCSA      | 0.050795    |
| LEC       | 0.022858    |
| LFL       | 0.023234    |
| LFL2      | 0.022670    |
| LHE       | 0.022858    |
| LJL       | 0.020130    |
| LJLA      | 0.003574    |
| LLA       | 0.017590    |
| LMF       | 0.030007    |
| LPLOL     | 0.019659    |
| LVP SL    | 0.023046    |
| MSI       | 0.007525    |
| NEXO      | 0.018154    |
| NLC       | 0.036027    |
| PCS       | 0.025491    |
| PGC       | 0.052864    |
| PGN       | 0.014016    |
| PRM       | 0.034522    |
| SL (LATAM)| 0.015521    |
| TAL       | 0.019283    |
| TCL       | 0.020788    |
| UL        | 0.026150    |
| UPL       | 0.038755    |
| VCS       | 0.030383    |
| VL        | 0.015991    |
| WLDs      | 0.013263    |

Here is the observed distribution when `firstdragon` was missing:

| league | firstdragon |
|--------|-------------|
| DCup   | 0.0         |
| LDL    | 0.0         |
| LPL    | 0.0         |
| WLDs   | 0.0         |


Our observed statistic was: 0.9923034634414514

Our p-value was: 0.0

If the p-value is significant (below the chosen significance level), we may reject the null hypothesis. This suggests that the missingness is not completely random, and there may be a systematic pattern or relationship with observed or unobserved variables. This situation is more indicative of missing at random (MAR) or missing not at random (MNAR).

Here is the empirical distribution of the test statistic:


[insert tvd]


#### Missingness of `firstdragon` does not depend on `firstblood`

We wanted to determine if `firstdragon` and `firstblood` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `firstdragon` and `firstblood` was missing and not missing:

|              | firstdragon_missing = False | firstdragon_missing = True  |
|--------------|-----------------------------|------------------------------|
| **firstblood** |                             |                              |
| False        | 0.500705                    | 0.5011                       |
| True         | 0.499295                    | 0.4989                       |

Our observed statistic was: 0.00039462604900317166

Our p-value was: 0.948
Since the p-value is above the chosen significance level, p-value from the test is not significant and we fail to reject the null hypothesis. In this case, we might conclude that the data is missing completely at random (MCAR).

Here is the empirical distribution of the test statistic:


---

## Hypothesis Testing
**Null Hypothesis**: The distribution of early game statistics where the team won is the same as the distribution of early game statistics where the team lost.

**Alternate Hypothesis**: The distribution of early game statistics where the team won is difference than the distribution of early game statistics where the team lost.

**Test Statistic**: We will be using the Total Variation Distance (TVD).

We decided to use Total Variation Distance because we are using categoricial data, more specifically whether our sample distributions came from the same distribution. Our significance level is going to be 0.05 as it is the standard significance level. 


We found the distribution of early game statistics and how the amount of points can correlate with the result of the game. 

| points | 0.0      | 1.0      | 2.0      | 3.0      | 4.0      | 5.0      | 6.0      | 7.0      | 8.0      | 9.0      | 10.0     |
| ------ | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| result |          |          |          |          |          |          |          |          |          |          |          |
| False  | 0.909385 | 0.84707  | 0.759547 | 0.687318 | 0.589577 | 0.495258 | 0.408747 | 0.311444 | 0.241554 | 0.147168 | 0.09106  |
| True   | 0.090615 | 0.15293  | 0.240453 | 0.312682 | 0.410423 | 0.504742 | 0.591253 | 0.688556 | 0.758446 | 0.852832 | 0.90894  |

Our observed statistic was: 0.05610845677532025
Our p-value: 0.0

Here is the empirical distribution of the test statistic: 
[insert tvd]

For our hypothesis testing, we did 500 simulations and were able to see that the p-value is significant as it is smaller than 0.05. 

Conclusion: We may reject the null hypothesis, meaning we believe that the distributions are not the same.

Since we may reject the null hypothesis this means there is a good amount of evidence that the distribution of early game statistics in games where the team won is different from the distribution of statistics where the team won. 
