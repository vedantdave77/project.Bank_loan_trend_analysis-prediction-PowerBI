# Microsoft-Power-Bi

### My Project Working Module:

![](powerbi.gif)






==================================================================================
.
<!-- wp:heading {"level":3} -->
<h3><strong>Step 1: &nbsp;Business Problem</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Identify the Current quarterly based loan data and compile them for Analysis.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Calculate detail return for the 15- and 30-year duration on given interest rate.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Arrange dashboard with filter option of individual client id with particular date analysis.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Consider frontline comfort during creation of Dashboard with graphical representation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Tool </strong><strong></strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Microsoft power Bi (public – desktop)</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Step 2: Data Understanding</strong><strong></strong></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>We have quarterly based data in form of excel spread sheets.</li><li>Used datasets:</li><li>Q1 Interest rate (15Y, 30Y)</li><li>Q2 Interest rate (15Y, 20Y)</li><li>Q3 Interest rate (10Y,20Y,30Y)</li><li>The whole data is based on past year (2018)’s client loan achievers record</li><li>Calculation is based on 4 percent interest rate and as per bank policy there are two long term repay durations of 15 and 30 years.</li><li>Bank will give opportunity of equal interest payment amount during whole duration.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3><strong>Step 3: Data Process</strong></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>Merge quarterly based spread sheet into yearly record.</li><li>Check the missing data and if so remove them from calculation</li><li>Make the following calculations for required result.</li><li>We will generate date table for the future forecasting (up to 2050)</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3><strong>Calculations</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here is the calculation for each equation for understanding flow of data processes</p>
<!-- /wp:paragraph -->

<!-- wp:group {"backgroundColor":"very-light-gray"} -->
<div class="wp-block-group has-very-light-gray-background-color has-background"><div class="wp-block-group__inner-container"><!-- wp:paragraph -->
<p>1. Loan Period = GENERATESERIES(1,360,1)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>2. Loan Amount(PV) = 100000</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>3. Annual Interest Rate (30 year loan) = 0.04</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>4. Monthly Interest rate (30 year loan) = POWER(1+ [3. Annual Interest Rate (30 year loan)],1/12)-1</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>5. Total Periods in loan duration = CALCULATE(MAX('1. Loan Period'[Loan Period]),ALL('1. Loan Period'[Loan Period]))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>6. Payment Amount = DIVIDE([4. Monthly Interest rate (30 year loan)]*[2. Loan Amount(PV)],1-POWER(1+[4. Monthly Interest rate (30 year loan)],-[5. Total Periods in loan duration]),0)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>7. Period in loan = CALCULATE(MAX('1. Loan Period'[Loan Period]))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>8. Periods remaining in loan = [5. Total Periods in loan duration]-[7. Period in loan]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>9. Rmaining loan balance = [2. Loan Amount(PV)]*POWER(1+[4. Monthly Interest rate (30 year loan)],[7. Period in loan]-1)-[6. Payment Amount]*DIVIDE(POWER(1+[4. Monthly Interest rate (30 year loan)],[7. Period in loan]-1)-1,[4. Monthly Interest rate (30 year loan)],0)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>10. Interest paid for loan period = [4. Monthly Interest rate (30 year loan)]*[9. Rmaining loan balance]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>11. Principal paid for loan period = [6. Payment Amount]-[10. Interest paid for loan period]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>12. Total payment = [5. Total Periods in loan duration]*[6. Payment Amount]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>13. Total interest paid = SUMX('1. Loan Period',[10. Interest paid for loan period])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>14. Total principal paid = SUMX(ALL('1. Loan Period'[Loan Period]),[11. Principal paid for loan period])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>15. Total Payments (running total) = [7. Period in loan]*[6. Payment Amount]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>16. Interest paid (running total ) = SUMX(FILTER(ALL('1. Loan Period'[Loan Period]),[7. Period in loan]&lt;=MAX('1. Loan Period'[Loan Period])),[10. Interest paid for loan period])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>17. Principal paid (running total ) = SUMX(FILTER(ALL('1. Loan Period'[Loan Period]),[7. Period in loan]&lt;=MAX('1. Loan Period'[Loan Period])),[11. Principal paid for loan period])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>18. Principal compared to the loan balance = [9. Rmaining loan balance]+[17. Principal paid (running total )]-[11. Principal paid for loan period]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>19. Dates = GENERATESERIES(DATE(2019,1,1),DATE(2050,9,30))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>20. Day = DAY('19. Dates'[19. Dates])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>21. Date interested rate selected = SELECTEDVALUE('Interest Rate'[Date])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>22. Loan Duration (in month) = IF(SELECTEDVALUE('Selected Loan Duration'[Duration Length]) = "15 years",180,360)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>23. Forecast Date = IF(MONTH([21. Date interested rate selected])&lt;12, DATE(YEAR([21. Date interested rate selected]),MONTH([21. Date interested rate selected])+1,1),DATE(YEAR([21. Date interested rate selected])+1,1,1))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>24. Interest Rate (for selected loan duration) = IF(SELECTEDVALUE('Selected Loan Duration'[Duration Length]) = "15 years", CALCULATE(MAX('Interest Rate'[15 year rate]),FILTER('Interest Rate','Interest Rate'[Date]= [21. Date interested rate selected])),CALCULATE(MAX('Interest Rate'[30 year rate]),FILTER('Interest Rate','Interest Rate'[Date]= [21. Date interested rate selected])))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>25. Monthly interest rate (depending on loan duration) = POWER(1+ [24. Interest Rate (for selected loan duration)],1/12)-1</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>26. Loan Period = DATEDIFF([23. Forecast Date],MAX('32. Dates (with variable)'[Dates]),MONTH)+1</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>27. Loan period (month into loan duration) = CALCULATE(MAX('1. Loan Period'[Loan Period]),FILTER('1. Loan Period','1. Loan Period'[Loan Period] = [26. Loan Period]))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>28. Payment Amount (for selected loan duration) = IF([26. Loan Period]&gt;=1&amp;&amp;[26. Loan Period]&lt;=[22. Loan Duration (in month)],divide([25. Monthly interest rate (depending on loan duration)]*[2. Loan Amount(PV)],1-POWER(1+[25. Monthly interest rate (depending on loan duration)],-[22. Loan Duration (in month)]),0))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>29. Loan blance (for selected loan duration) =</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>VAR AccrualFactor = POWER(1+[25. Monthly interest rate (depending on loan duration)],[27. Loan period (month into loan duration)]-1)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Return</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>IF([26. Loan Period]&gt;=1&amp;&amp;[26. Loan Period]&lt;=[22. Loan Duration (in month)],[2. Loan Amount(PV)]*AccrualFactor-[28. Payment Amount (for selected loan duration)]*DIVIDE(AccrualFactor-1,[25. Monthly interest rate (depending on loan duration)],0),BLANK())</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>30. Interest paid (for selected loan duration) = [25. Monthly interest rate (depending on loan duration)] * [29. Loan blance (for selected loan duration)]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>31. Principal paid (for selected loan duration) = [28. Payment Amount (for selected loan duration)] - [30. Interest paid (for selected loan duration)]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>33. Interest paid(running total for loan durattion) = SUMX(DATESBETWEEN('32. Dates (with variable)'[Dates],[23. Forecast Date],MAX('32. Dates (with variable)'[Dates])),[30. Interest paid (for selected loan duration)])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>34. Principal paid(running total for loan durattion) = SUMX(DATESBETWEEN('32. Dates (with variable)'[Dates],[23. Forecast Date],MAX('32. Dates (with variable)'[Dates])),[31. Principal paid (for selected loan duration)])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>35. Ending loan banlance (for selected loan duration) = [2. Loan Amount(PV)]- [34. Principal paid(running total for loan durattion)]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>36. Loan Balannce end of period = IF([26. Loan Period]&gt;=1,CALCULATE([29. Loan blance (for selected loan duration)],DATEADD('32. Dates (with variable)'[Dates],1,MONTH)))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>37. Interest paid QTD = SUMX(DATESQTD('32. Dates (with variable)'[Dates]),[30. Interest paid (for selected loan duration)])</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>38. Interest paid YTD = SUMX(DATESYTD('32. Dates (with variable)'[Dates]),[30. Interest paid (for selected loan duration)])</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:group -->

<!-- wp:heading {"level":3} -->
<h3><strong>Step 4: Modeling:</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here, we use Microsoft Power Bi as tool for business analysis and create the dashboard for the required presentations with graphical representation using bar graph.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The “Loan Distribution graph will show the distribution of loan, here we have equal distribution of paying amount during period. So the distribution is mostly normalize ( distributed equally) and the rate of changing is also smooth throughout the period.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Step 5: Evaluation</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Evaluation is required process in banking due to real public interaction and trustful relationship with the clients.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We will use verification method to verify our calculations as an examples …</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Interest paid for loan must be equal to Quick testing measures example</li></ul>
<!-- /wp:list -->

<!-- wp:group {"backgroundColor":"very-light-gray"} -->
<div class="wp-block-group has-very-light-gray-background-color has-background"><div class="wp-block-group__inner-container"><!-- wp:paragraph -->
<p><strong>LHS</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>10. Interest paid for loan period = [4. Monthly Interest rate (30 year loan)]*[9. Rmaining loan balance]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>RHS</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Quick measure testing =</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>CALCULATE(</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;[10. Interest paid for loan period],</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;FILTER(</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ALLSELECTED('1. Loan Period'[Loan Period]),</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ISONORAFTER('1. Loan Period'[Loan Period], MAX('1. Loan Period'[Loan Period]), DESC)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>)</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:group -->

<!-- wp:list -->
<ul><li>The amount paid and amount remain to paid must be equal to total principle amount + total duration interest rates.</li></ul>
<!-- /wp:list -->

<!-- wp:group -->
<div class="wp-block-group"><div class="wp-block-group__inner-container"><!-- wp:group {"backgroundColor":"very-light-gray"} -->
<div class="wp-block-group has-very-light-gray-background-color has-background"><div class="wp-block-group__inner-container"><!-- wp:paragraph -->
<p><strong>LHS</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>12. Total payment = [5. Total Periods in loan duration]*[6. Payment Amount]</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>RHS</strong><strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>9. Rmaining loan balance = [2. Loan Amount(PV)]*POWER(1+[4. Monthly Interest rate (30 year loan)],[7. Period in loan]-1)-[6. Payment Amount]*DIVIDE(POWER(1+[4. Monthly Interest rate (30 year loan)],[7. Period in loan]-1)-1,[4. Monthly Interest rate (30 year loan)],0)</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:group --></div></div>
<!-- /wp:group -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <strong>+</strong><strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>15. Total Payments (running total) = [7. Period in loan]*[6. Payment Amount]</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Step 6: Development:</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>It is the stage which contains tour steps Plan Deployment, Plan monitoring and Maintenance for the frontline system implementations, Product Final and Reviewing our project</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Dashboard Figures</strong></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li><strong>With loan amount 10000 CAD (medium loan payer)</strong></li><li><strong>Interest rate 4 percent (simple)</strong></li></ul>
<!-- /wp:list -->


### Pre-Data Analysis

![](https://github.com/vedantdave77/project.Bank_loan_trend_analysis-prediction-PowerBI/blob/master/Dashboard_Photos%20(Jan%2C01%2C2018)/Data_Pre-analysis.png)


---

### Feature Creation

![](https://github.com/vedantdave77/project.Bank_loan_trend_analysis-prediction-PowerBI/blob/master/Dashboard_Photos%20(Jan%2C01%2C2018)/Analysis_Feature_Creation%20(Calculation).png)

### Loan Forcasting 

### for, 15 years 
![](https://github.com/vedantdave77/project.Bank_loan_trend_analysis-prediction-PowerBI/blob/master/Dashboard_Photos%20(Jan%2C01%2C2018)/Loan_Data_Analysis%20(forecasting).png)

### Loan Distribution Graph 
### for, 15 years
![](https://github.com/vedantdave77/project.Bank_loan_trend_analysis-prediction-PowerBI/blob/master/Dashboard_Photos%20(Jan%2C01%2C2018)/Loan_distribution_graph.png)

![]()
