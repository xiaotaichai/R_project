# college-Outcomes Project

As part of an effort to counter rising tuition costs and conflicting incentives in higher education, the U.S. Department of Education has been collecting information for public and private colleges and universities, and assembling this into public 'scorecards'. The data link measures of affordability and ease of access to student outcomes post graduation, and can be summarized to highlight schools providing the best value in education. The current dataset and reports for September 2015 is available below:

Website: https://collegescorecard.ed.gov/
Dataset: https://collegescorecard.ed.gov/data/
Documentation: https://collegescorecard.ed.gov/data/documentation/ (Also see Moodle entries.)

Data are available in yearly reports from 1996 through 2013, and each includes roughly 7,000 observations (colleges) for 1700 variables. For privacy reasons, some data are masked as "PrivacySuppressed", and some values are collected as two-year moving averages to reduce variability. For encoded variables, values are translated in the CollegeScorecardDataDictionary .csv file.


# Goals:
1. **Read the data into a language-native format. Properly treating NAs and other encoded variables.**  

2. **Assess the quality of the data.** This may include:  
 a. Measuring fraction of missing values, or relationships among missing values and other columns.  
 b. Calculating numeric quality scores for several metrics.  

3. **Recode any variables of interest.** For example:  
 a. Create combined categories from several columns.  
 b. Aggregate summary statistics from factor levels in several columns.  
 c. Normalize data between categories, where appropriate.  

4. **Identify and display any relationships of interest.**


# Some variables that we are interested in:
- Region
- Admission rate
- SAT score
- ACT score
- Majors
- Degree types (one-year certificate, associate, award of more than two less than four years, bachelor, distance-education)
- Race (white, black, hispanic, asian, american indian, native hawaiian, two or more races, non-resident aliens, etc.)
- Work status: part time
- Net price (for institutions)
- Income level for students’ family
- Year of institution
- Faculty salary, full-time or not
- Pell grant
- Completion rate
- Adjusted cohort count
- Death
- Transfer
- dependent or independent student
- Gender of student
- Withdrawal rate
- Federal loan
- First-generation students or not



# Questions we try to answer:
- Who completed within 4 years at original institution, men or women (Relative variables are *FEMALE_COMP_ORIG_YR4_RT* and *MALE_COMP_ORIG_YR4_RT*)

- Compare student outcomes of New College of Florida to other colleges

- Compare average net price for school among different family income groups; Who pays more for school (Relative variables are *net price for institutions* and *Income level for students' family*)


# Plan for this project:
- Ask questions where we manipulate data (not a perfect x or y variable) combination of variables.

- Use Heap Map to show the fraction of missing values.

- Build regression models to find relationships between some variables to answer above questions.

