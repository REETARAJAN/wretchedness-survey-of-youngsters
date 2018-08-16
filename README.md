# wretchedness-survey-of-youngsters




Wretchedness survey of youngsters

   
				 							
Introduction:
   For this project, our initial motivation was to explore general trends of preferences and feelings across different activities and perspectives for young people. One of our group mates felt lonely from time to time, so we are especially concerned with issue. Our goal was to predict if someone is lonely around us so that we can take care of them. Taking it from there, we were able to narrow down our question of interests to loneliness of young people. First, we would like to figure out if specific groups demonstrate loneliness. Furthermore, we wanted to investigate what factors correlate with loneliness. How can we predict if someone is lonely was our next step. Finally, we wanted to establish a simple application to test loneliness. 


Algorithm	:

In this section, we only cover the major functions that are essential to our analysis. Details can be found in the actual codes and comments. Following the data preprocessing steps, we produced two functions to calculate the confidence intervals. The first function “CI” calculates the 95% confidence interval for mean for the group of interest. The second function “twosampCI” returns the 95% confidence interval for difference in sample means for two groups. With these two functions, scipy library was used to calculate the critical values for confidence intervals. These two functions can be applied anytime when considering 95% Confidence Intervals for specific groups or across different groups.

Next, we wrote three functions that conducts hypothesis test. The first function “check_values_in_varibale” returns all unique values of a variable as a list. The second function “divide_groups” divides the variable of interest into the unique values subsets. The third function “hypothesis_test” calls the previous functions, using a stats built-in function of scipy library, to return the p-values of the t-test. This function can be used to compare any two numerical data lists, and users can use this function if they are interested in any other variables other than loneliness.

The central piece of our program is a logistic regression model and a loneliness test. The logistic regression model uses 71 independent variables, using K-fold cross validation to get a 70%  prediction accuracy for our test set. The function “calculateloneliness” is based on the logistic regression model. Lastly, the function “areyoulonely” takes user input and calls “calculateloneliness”, outputting the loneliness level of the user.
 
Our program also includes several functions for plotting graphs such as the correlation matrix (Appendix 1). Overall, our program has user interaction with the user, taking inputs from the user and producing outputs, their loneliness level, that users might find interesting. We believe that our CI function, hypothesis test function and logistic regression go beyond our basic queries and can be used for other datasets (and deserve some extra credits :))
			 							
Results:
In this section, we present the major findings for the queries we raised at the beginning of the report.

1. Are there any gender differences in loneliness?
We produced 95% Confidence Interval for the mean of loneliness level for male group and female group. 
95% CI for male is [2.63, 2.90] shown in Fig.1.
95% CI for female is [2.87, 3.09] shown in Fig.2. 
95% CI for the difference between two groups was also calculated: [-0.39, -0.04]. 
Hypothesis test suggested that the difference is statistically significant. So we are able to make the conclusion that young females are generally more lonely than young males. 


2. Are people without any siblings more lonely than those who have?
We produced 95% Confidence Interval for the mean of loneliness level for people who have siblings and those don’t have siblings.
95% CI for single child: [2.72, 3.10] shown in Fig.3.
95% CI for those have siblings: [2.79, 2.98] shown in Fig.4.
95% CI for the difference between two groups was also calculated: [-0.19, 0.24]. 
Hypothesis test for these two groups generated p-value of 0.81 that is too large.
Hypothesis test suggested that there’s no statistically significant difference between two groups. Thus we could not conclude that single children are more lonely than people who have siblings. 




3. What traits are associated with loneliness?
Since numerical variables can’t be directly compared with categorical variables, we did two correlation analyses, one with numerical variables(Appendix 2) and another with categorical variables (Appendix 3). As shown in Fig.5, not surprisingly, happiness, energy level and friends are negatively correlated with loneliness level. As illustrated in Fig.6, people who rarely use internet also seem to demonstrate low loneliness level. Interestingly, a person who likes branded clothing is also unlikely to be lonely. On the other end of the spectrum, shown in Fig.7 and Fig.8, people who have mood swings and have an education degree of primary school tend to be lonely, probably because the college students are too busy with coursework to feel lonely. 


4. How to predict a young person’s loneliness?
We firstly transformed our original loneliness variable to a dummy variable. Loneliness > 3 are considered as lonely. Loneliness<=3 are considered not lonely. Using our logistic regression model with 71 variables, we can predict a person’s loneliness level at an accuracy rate of 72% with (K-fold) test sets. The variables with largest coefficients can be found in Fig.9. In short, imagine a person likes writing, fear public speaking, enjoys using PC and spends a lot a gadgets, If he also happens to not like spending time with friends, hates geography and economy, doesn’t have problems with snakes, then chances are that he is feeling pretty lonely.


5. How can these findings be used?
As mentioned in the algorithm section above, we built a simple loneliness test based on the top 4 positive features and the top 4 negative features in our logistic regression model. The loneliness test takes user input, calculates the user’s loneliness level and displays it. Users can test their own or anyone else’s loneliness level, if interested.


Testing
    In total, we wrote three classes of unit tests, each consisting two unit tests. The first class tests the “CI” and “twosampCI” function. We tested if these two function return the correct confidence intervals using the test case list1 and list2, as shown in the sample run1. The second class tests the “calculateloneliness” and “areyoulonely” functions. We tested if these functions calculated and displayed loneliness correctly, as shown in the sample run2. Our last class tests the hypothesis test functions, testing if it takes the correct variable of interest and returns the correct p-values.

[Sample Run 1]
import unittest
class confidenceIntervalTestCase(unittest.TestCase):
    def test_CI_correct(self):
        list1 = [1,2,3,4,5,6]
        
        self.assertEqual(CI(pd.Series(list1)),[2.0030527802429905, 4.9969472197570095])
        
    def test_twosampCI(self):
        list1 = [1,2,3,4,5,6]
        list2 = [2,3,4,5,6,7]
        
        self.assertEqual(twosampCI(pd.Series(list1),pd.Series(list2)),[-3.1170030603370606, 1.1170030603370606])

if __name__ == '__main__':
    unittest.main()   


import unittest
class confidenceIntervalTestCase(unittest.TestCase):
    def test_CI_correct(self):
        list1 = [1,2,3,4,5,6]
        
        self.assertEqual(CI(pd.Series(list1)),[2.0030527802429905, 4.9969472197570095])
    
    def test_twosampCI(self):
        list1 = [1,2,3,4,5,6]
        list2 = [2,3,4,5,6,7]
        
        self.assertEqual(twosampCI(pd.Series(list1),pd.Series(list2)),[-3.1170030603370606, 1.1170030603370606])


if __name__ == '__main__':
    unittest.main()   


..
----------------------------------------------------------------------
Ran 2 tests in 0.020s

OK

[Sample Run 2]
import unittest
class AreyoulonelyTestCase(unittest.TestCase):
    def test_calculate_correct(self):
        self.assertEqual(calculateloneliness(1,1,1,1,1,1,1,1),0)
    def test_areyoulonely(self):
        print('************** enter 1 for all questions *****************')
        print(areyoulonely())
        self.assertEqual(areyoulonely(),(1,1,1,1,1,1,1,1))


if __name__ == '__main__':
    unittest.main()   


************** enter 1 for all questions *****************
 ====== LONELINESS TEST  =======
Do you think you are lonely?

1

Doesn't matter what you think.
We will see after 8 questions.

1. Public speaking: Not afraid at all 1-2-3-4-5 Very afraid of (integer)1

2. Poetry writing: Not interested 1-2-3-4-5 Very interested (integer)1

3. Internet: Not interested 1-2-3-4-5 Very interested (integer)1

4. PC Software, Hardware: Not interested 1-2-3-4-5 Very interested (integer)1

5. Socializing: Not interested 1-2-3-4-5 Very interested (integer)1

6. Economy, Management: Not interested 1-2-3-4-5 Very interested (integer)1

7. Cars: Not interested 1-2-3-4-5 Very interested (integer)1

8. I spend a lot of money on partying and socializing.: Strongly disagree 1-2-3-4-5 Strongly agree (integer)1

 ==========  RESULT  =========
You must be a joyful person!
You are only-1.61% likely to be a lonely person.
(1, 1, 1, 1, 1, 1, 1, 1)
 ====== LONELINESS TEST  =======
Do you think you are lonely?

1

Doesn't matter what you think.
We will see after 8 questions.

1. Public speaking: Not afraid at all 1-2-3-4-5 Very afraid of (integer)1

2. Poetry writing: Not interested 1-2-3-4-5 Very interested (integer)1

3. Internet: Not interested 1-2-3-4-5 Very interested (integer)1

4. PC Software, Hardware: Not interested 1-2-3-4-5 Very interested (integer)1

5. Socializing: Not interested 1-2-3-4-5 Very interested (integer)1

6. Economy, Management: Not interested 1-2-3-4-5 Very interested (integer)1

7. Cars: Not interested 1-2-3-4-5 Very interested (integer)1

8. I spend a lot of money on partying and socializing.: Strongly disagree 1-2-3-4-5 Strongly agree (integer)1
..
 ==========  RESULT  =========
You must be a joyful person!
You are only-1.61% likely to be a lonely person.

 ==========  RESULT  =========
You must be a joyful person!
You are only-1.61% likely to be a lonely person.

----------------------------------------------------------------------
Ran 2 tests in 19.027s

OK



