# pokemon
Pokemon Data - Correlations, PCA, Sankey Diagram and Logistic Regression Analysis

### The entire notebook is already annotated with text so this README is redundant. 
### See the notebook for a smoother read.

## TLDR; Version 

My goal with the project was to discover underlying trends within the dataset.

From types, I wrote a function to determine what is the best Pokemon to play against your opponent, displayed as a Sankey Diagram.

![Sankey Diagram](https://github.com/chewka/pokemon/assets/15725058/c6838596-1452-495c-91bc-cfe83001314b)

Furthermore, I determined that Base Total correlated with 5 other varibles through a Heatmap, seen here as pair plots and a linear regression line.

![heatmap2 300](https://github.com/chewka/pokemon/assets/15725058/e6750502-fd9a-42aa-8f14-1fcd60d79ca5)

![aatack300](https://github.com/chewka/pokemon/assets/15725058/fad543ee-8f7a-4dd1-9126-e26382d047cf)

![defense300](https://github.com/chewka/pokemon/assets/15725058/4c681501-bc3a-4276-b6d8-5bfeb72ede56)

![sp_attack300](https://github.com/chewka/pokemon/assets/15725058/4630a34b-84cc-4384-a19a-725c3a9a06e2)

![sp_defense300](https://github.com/chewka/pokemon/assets/15725058/31d57e69-4c6e-4aeb-9ef8-6efc5a25558b)

![speed300](https://github.com/chewka/pokemon/assets/15725058/bf6f2dac-33f7-4cee-af58-ac54742379c7)

![hp300](https://github.com/chewka/pokemon/assets/15725058/c2b9efed-4352-401e-a158-a1954e60753d)

Highlighting Legendary Pokemon shows they are in a league of their own, all above average.
![shaded kde 300](https://github.com/chewka/pokemon/assets/15725058/c393cbe6-0c7a-4acb-a866-881588b5ff8e)

![Base Total 300](https://github.com/chewka/pokemon/assets/15725058/96b75cee-ce53-4a09-8a7d-5e53494b22ee)

![Legendary Base Total 300](https://github.com/chewka/pokemon/assets/15725058/ad664ac6-d32c-43ca-ac50-fc11ec9597f3)

A similar trend appears with Base Egg Steps, such that you have a 96% chance of getting a Legendary Pokemon after 20,000 steps.

![base egg steps x index](https://github.com/chewka/pokemon/assets/15725058/dfefe366-bc50-4a1c-9b16-b8a7e26e9de2)

![Percentage over 20K](https://github.com/chewka/pokemon/assets/15725058/31c9682a-76e2-4699-b2da-1967d6d8bb1f)

After narrowing our basic features to ‘pokdex number’, ‘base total’, and ‘base egg steps,’ I made a 3D PCA Plot. 

![PCA 3d 20 300](https://github.com/chewka/pokemon/assets/15725058/343980ec-27e0-4f24-9944-12fdc82f91bd)

The Legendary Pokemon are marked by triangles, mostly in the bottom left.

Using a Linear Regression Classifier, I was able to predict a legendary Pokemon from those three variables with around 90% accuracy, modeled with the following two graphs.

![Actual v Predicted 300](https://github.com/chewka/pokemon/assets/15725058/07b80405-008e-499b-b9f4-83e32e6b7eb5)

![actual v predicted all 300](https://github.com/chewka/pokemon/assets/15725058/83e81945-1dc6-441a-8822-8d02dceadd3d)


## Long Version

The first part consists of data validation and cleanup. I started by looking at the variables in the columns and divided them into qualitative (type, name) and quantitative variables. Another distinction would be categorical and numerical. 

Types was the only useful categorical data so I devised an algorithm to determine what is the best Pokémon to play against your opponent. The algorithm simply worked by grouping all of the against_x values of the Pokémon and averaging those numbers by type. It would return the type that had the average maximum. This was then displayed with a Sankey Diagram.

The second part consists of Kernel Density Estimates (which are a smoother version of histograms) as well as Principal Component Analysis. Both of those will be abbreviated to KDE and PCA, respectively.

I started with a Seaborn Heatmap to look at all the correlations of the variables.

Then I plotted the most interesting ones, and in the process discovered that Base_Total is an aggregate of other scores, so we could use that measure as a feature of our PCA instead. 

In general, an aggregate measure is expected to be correlated with its components because the aggregate measure is derived from those components. Because the components contribute to the calculation of the aggregate measure, it is natural for them to exhibit some level of correlation. 

——

### Correlations

Highest Correlations (1.00 > R > 0.8):
* generation and pokedex number
* is_legendary and base_egg_steps
High Correlations (0.8 > R > 0.7):
* base_total and sp_attack
* base_total and sp_defense
* base_total and attack
Strong Correlations (0.7 > R > 0.6):
* base_total and defense
* base_total and hp
* height_m and weight_kg


When you have two variables with a high correlation, you can essentially reduce your variable space to only one of those variables because there would be a redundant effect (on the dependent variables). 

Later on, I will be using PCA for proper dimensionality reduction and to show trends and patterns in the data that would be difficult to see with looking at all the variables at once, that is why this preliminary step of data validation is incredibly important.

——


Furthermore, it became obvious that there was a bimodal distribution  of the Base Total, that was evident in each generation as well.

We were also able to identify that 'pokedex_number' is an index variable, clearly correlating with the 'generation' data (R=0.99), indicating the two variables are interchangable

We can use these three variables in our next part where we do a PCA on the data.

The 'base_egg_steps' column provides information about the number of steps needed for the egg of a particular Pokémon species to hatch.

The entire Base Total column was split by the average into an above-average group and a below-average group. The mean of those two groups is plotted in the final KDE graph.

The final KDE graph indicates that the game is set up to have two groups of Pokémon, an above average group and a below average group (denoted by the two higher density groupings around both the below average and above average demarcations).

It looks like the likelihood of getting an above average Pokémon is slightly higher than the likelihood of getting a below average Pokémon. This assumption is based on the fact that if you were to draw two overlapping bell curves, one for the below-average group and one for the above-average group, it appears that the area under the curve is higher for the above average group. However, the mean of the above average group does not line up with the largest bin of the histogram, so there must be other factors at play or ways to illustrate these relationships.

What further adds to this analysis is the labeling of Legendary Pokémon. You can clearly see that the game developers kept the Legendary Pokémon base_total all in the above-average group, approximately one standard deviation above the above-average mean.

## See Jupyter Notebook for full Discussion of results and Prediction Algorithm

