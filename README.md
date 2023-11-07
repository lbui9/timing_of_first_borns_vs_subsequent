# Assignment 8: The National Survey of Family Growth dataset

## Instructions

In this assignment you will use statistical inference to answer a question the timing of first-born vs. subsequent children, using the National Survey of Family Growth, Cycle 6 dataset.

**Make sure to answer these questions using full sentences and paragraphs, and proper spelling and punctuation.**

Read the [About the dataset](#about-the-dataset) section to get some background information on the dataset that you'll be working with.
Each of the below [exercises](#exercises) are to be completed in the provided spaces within your starter file `birthtimes.Rmd`.
Then, when you're ready to submit, follow the directions in the **[How to submit](#how-to-submit)** section below.


## About the dataset

For this dataset, you will be working with the *National Survey of Family Growth, Cycle 6* dataset published by the National Center for Health Statistics.
Your analysis will revolve around answering the following question,

> Do first born children either arrive early or late when compared with non-first-borns?

The questions in this assignment will guide you through the process of answering this question using statistical inference.

The dataset contains 244 variables collected from 13,593 women.
We only require a few variables to answer the above question, which are listed below.
Complete descriptions of all the variables can be found in the [NSFG Cycle 6: Female Pregnancy File Codebook][nsfg-cycle-6].

| Variable               | Description                                                                                                                                     |
| ---------              | ---------------------------------------------------------------------------------------                                                         |
| `caseid`   | integer ID of the respondent                                                                                                                    |
| `prglngth` | integer duration of the pregnancy in weeks                                                                                                      |
| `outcome`  | integer code for the outcome of the pregnancy, with a 1 indicating a live birth                                                                 |
| `birthord` | serial number for live births; the code for a respondent's first child is 1, and so on. For outcomes other than live birth, this field is blank |

## Exercises

**Refer back to our earlier inference assignment (Assignment 6 on the yawning Mythbusters) if you don't remember how to write particular pieces of code.**

1.  
    Addressing the question *"do first born children either arrive early or late when compared with non-first-borns?"* means that we should only consider live births in the dataset.
    Filter the dataset so that it only contains outcomes with live births and assign this result to a new variable `live_births`.
    
    Next, we need to label all births in `live_births` as either "first" or "other" so that we can easily find the children that are first borns and the ones that are not. We will do this by creating a new column called `birth_order`. 
    There are a couple of different ways that you can create the `birth_order` column (you should pick one or the other):
    
    *   Split the dataset into two parts, a `first_births` dataset and an `other_births` dataset.
        Do this by applying a filter to extract the first births, then use `mutate()` to create a new column called `birth_order` that labels these rows as "first".
        Then repeat this process, except apply a filter to extract all other births and label those as "other" in `birth_order`.
        To recombine the datasets into one, use `bind_rows()`.
        
    *   Use `if_else()` with `mutate()` to create the `birth_order` column and the "first" and "other" labels. See the explanation box below for how to use the `if_else()` function. (If you do it this way, you won't need to use `bind_rows()`).
  
    After labeling the births, remove the extraneous variables from the data frame leaving just the `prglngth` and `birth_order` columns.
    Assign the resulting data frame to a variable named `pregnancy_length`.
    
    Commit your work.
        
    > #### The `if_else()` function
    >
    > The if_else() function is a way of applying an if/else statement to an entire column or vector at once! Just like an if/else statement, it needs a conditional expression (which evaluates to `TRUE` or `FALSE`), and a value to insert in that position of the new output vector for each of those conditions. These are the three arguments of the function: 
    >
    > ```r
    > if_else(
    >   <SOME BOOLEAN CONDITION>,
    >   <value to insert if TRUE>,
    >   <value to insert if FALSE>
    > )
    > ```
    >
    > For example (note that we need to use the `if_else()` function inside the `mutate()` function):
    >
    > ```r
    > input_dataframe %>%
    > mutate(
    >   new_column_name = if_else(
    >     column_1 == 5,
    >     "column 1 equals five",
    >     "it is not 5"
    >   )
    > )
    > ```
    >
    > You can also try using `case_when()` instead of `if_else()` to accomplish this. Try looking up the documentation for both functions to find out more about them (either within RStudio in the Help tab, or using Google).
    
2. 
    Take the `pregnancy_length` dataset in Exercise 1 and plot 2 probability mass function (PMF) histograms of the pregnancy length variable for (1) "first" birth rows and (2) "other" births. You must show both of these histograms on the same plot (i.e. what parameter of the aes function will allow you to create different colored histograms on the same plot?).
    
    Also, since you are plotting multiple colored histograms, make sure that both bars start at the x-axis (i.e. you do not want the bars of one color to be stacked on top of the other color). Since this means that the bars will be overlapping, you should also make sure that they are transparent (so that you can see the lower layer). Hint: we have done something similar to this in Assignment 4, Exercise 8.
    
    Use a `binwidth` of 1, and add the function `xlim(27, 46)` to your plot so that the window focuses where most of the data is. Remember to give your graph an appropriate tile and axis labels using the `labs` function.
    
    Then use your plot to answer the following questions:
    * Where is the mode (most common value) of the distributions?
    
    * By observing the visualization only, is it possible to confirm the statement that "first born children either arrive early or arrive late when compared with non-first-borns", or should we use a hypothesis test? Remember to use full sentences and paragraphs when answering.
    
    Commit your work.

    
3. 
    Group the `pregnancy_length` dataframe by the `birth_order` column and compute the summary statistics of the `prglngth` column using the `summarize` function (mean, median, standard deviation (with the `sd()` function), inter-quartile range, minimum, maximum). As we have grouped by `birth_order`, these statistics should automatically be calculated for for both "first" and "other" births.
    
    How do the different summary statistics compare between the two distributions?
    Does it look like there may be a notable difference between the two distributions?
    Explain (remember to use full sentences and paragraphs).
    
    Commit your work.

4. 
    If we want to determine whether or not the difference between two distributions is statistically significant, we need to run a hypothesis test.
    
    What is the **test statistic** in this experiment? (This is the number you will be calculating for each of the permutations in the hypothesis test, and creating a PMF histogram of. I.e. it is what we will compute in the `calculate` function using the `stat` parameter.)
    
    Formalize your analysis by writing down the null and alternative hypotheses for the question of whether first babies arrive early or arrive late when compared with non-first-borns.
    
    You should also indicate whether you're conducting a **one-sided** or **two-sided** hypothesis test.
    
    Commit your work.
    

5. 
    Use a simulation to generate the null distribution so that you can perform the hypothesis test.
    Do this using the functions provided in the `infer` package, `specify()`, `hypothesize()`, `generate()`, and `calculate()`.
    To collect enough statistics, you should set the simulation to repeat 10,000 times.
    
    Once you've obtained the null distribution, calculate the observed statistic. Then get the *p*-value for your hypothesis test using `get_p_value()`. To find the correct value of the `direction` parameter to use for this type of test, take a look at the *documentation* for the `get_p_value` function: in the bottom right pane of RStudio, go to the Help tab and type `get_p_value` into the search bar.
    
    Assuming a significance level of &alpha; = 0.05, can we reject the null hypothesis?
    
    Finally, visualize the simulated null distribution and the p-value of the onserved statistic using `visualize()` and `shade_p_value()`.
    
    Remember to give your graph a title, and replace the x-axis label with the actual test statistic that you are calculating (you identified this in the previous exercise).
    
    Commit your work.
    
6. 
    Use a bootstrap simulation to calculate the 95% confidence interval for your **observed statistic**.
    
    We can do this using the using the `specify()`, `generate()`, and `calculate()` functions from the `infer` package. Fill in the ellipses, making sure that generate **bootstrap** samples rather than permutations:
    
    ```r
    birth_bootstraps <- pregnancy_length %>%
      specify(prglngth ~ birth_order) %>%
      generate(10000, type = "...") %>%
      calculate(stat = "...", order = c(..., ...))
    ```
    
    To collect enough statistics, we have set the bootstrap simulation to repeat 10,000 times.
    
    Once you've obtained the bootstrap distribution, use `get_confidence_interval()` to find the upper and lower bounds of the 95% confidence interval:
    
    ```r
    bootstrap_ci <- birth_bootstraps %>%
      get_confidence_interval()
    bootstrap_ci
    ```
    
    Does the observed statistic fall within the range of the 95% confidence interval?
    
    Finally, visualize the bootstrap distribution using `visualize()` and show the 95% confidence interval using `shade_confidence_interval()`:
    
    ```r
    birth_bootstraps %>%
      visualize() +
      shade_confidence_interval(bootstrap_ci)
    ```
    
    Use `labs()` to set the title and x-axis label.
    
    Commit your work.
    
7. 
    In addition to hypothesis tests and confidence intervals, we should also consider the **effect size**, which measures the relative difference between two distributions.
    The effect size helps us better know how important a given result actually is, not just whether or not we can reject the null hypothesis.
    
    One measure of the effect size is called [Cohen's *d*][wiki-cohens-d], which we will use to compute the effect size between the pregnancy lengths for "first" and "other" births.
    The different ranges of *d* can be interpreted using the following table:
    
    | Effect size | d    |
    | ----------- | ---- |
    | Very small  | 0.01 |
    | Small       | 0.20 |
    | Medium      | 0.50 |
    | Large       | 0.80 |
    | Very large  | 1.20 |
    | Huge        | 2.00 |
    
    The following set of functions should be preloaded for you in the set-up chunk: `cohens_d_bootstrap()`, `bootstrap_report()`, and `plot_ci()`.
    These functions will use bootstrap simulations to compute the confidence interval for the Cohen's *d* parameter.
    
    Run the bootstrap simulation as follows (you can just copy and paste this code):
    
    ```r
    bootstrap_results <- cohens_d_bootstrap(data = pregnancy_length, model = prglngth ~ birth_order)
    ```
    
    To print a report for the bootstrap simulation, run:
    
    ```r
    bootstrap_report(bootstrap_results)
    ```
    
    To visualize the bootstrap distribution and confidence interval, run:
    
    ```r
    plot_ci(bootstrap_results)
    ```
    
    Using the provided table, report how large the effect size is for the difference in pregnancy lengths for "first" and "other" births (using full sentences). You should find level that your value of *d* is *greater* than, e.g. if your calculated *d* is 0.6, then the effect size is medium (i.e. >0.5).
    
    Commit your work.


## Submitting

To submit your assignment, follow the two steps below.

1.  Save, commit, and push your completed R Markdown file so that everything is synchronized to GitHub.
    If you do this right, then you will be able to view your completed file on the GitHub website.

2.  Knit your R Markdown document to the PDF format, export (download) the PDF file from RStudio Server, and then upload it to *Assignment 8* posting on Blackboard.


## Cheatsheets

You are encouraged to review and keep the following cheatsheets handy while working on this homework:

*   [Data transformation cheatsheet][data-transformation-cheatsheet]

*   [Data import cheatsheet][data-import-cheatsheet]

*   [ggplot2 cheatsheet][ggplot2-cheatsheet]

*   [RStudio cheatsheet][rstudio-cheatsheet]

*   [RMarkdown cheatsheet][rmarkdown-cheatsheet]

*   [RMarkdown reference][rmarkdown-reference]

[nsfg-cycle-6]:                   https://doi.org/10.3886/ICPSR04157.v1
[wiki-cohens-d]:                  https://en.wikipedia.org/wiki/Effect_size#Cohen.27s_d
[ggplot2-cheatsheet]:             https://github.com/rstudio/cheatsheets/raw/master/data-visualization-2.1.pdf
[rstudio-cheatsheet]:             https://github.com/rstudio/cheatsheets/raw/master/rstudio-ide.pdf
[rmarkdown-reference]:            https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf
[rmarkdown-cheatsheet]:           https://github.com/rstudio/cheatsheets/raw/master/rmarkdown-2.0.pdf
[data-import-cheatsheet]:         https://github.com/rstudio/cheatsheets/raw/master/data-import.pdf
[data-transformation-cheatsheet]: https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf
