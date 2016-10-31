# Automated-Program-Repair
This repo contains all the code, artifacts, and submitted documents for the project.

### Purpose
The purpose of this project is to use random search algorithm to fix runtime exceptions in Expertiza automatically

### Research Questions
 - **RQ1:** What percentage of runtime exception errors in Expertiza can be fixed by random search algorithm?
 - **RQ2:** On average, how long will it take random search algorithm to fix one Expertiza bug?
 
### Evaluation Plan
 - **RQ1**
  - Metric: number of  runtime exception errors fixed by automated evolutionary program repair out of the total number of selected errors.
  - Artifact: unresolved Expertiza runtime exception errors caught by Airbrake.
  
 - **RQ2**
  - Metric: average time consumed by random search algorithm to fix one bug.
  - Artifact: unresolved Expertiza  runtime exception errors caught by Airbrake.
  
### Timeline
 - Unresolved bug selection (Oct. 7 to Oct. 13)
 - Random search algorithm implementation (Oct. 14 to Oct. 31)
 - Automated bug fixing (Nov. 1 to Nov. 17)
 - Result analysis (Nov. 18 to Nov. 23)
 - Final paper writing (Nov. 24 to Nov. 31)
 
### Progress
#### Unresolved bug selection
Expertiza uses Airbrake to capture and track runtime exceptions. I used [Airbrake API](https://airbrake.io/docs/api/) to obtain the recent 100 runtime exception errors from Expertiza. Then I selected 25 exception errors out of 100 in order to do further automated bug fixing. The main reasons I filtered the rest 75 exception errors are:
 - Already been fixed;
 - Cannot be reproduced;
 - Need to create a new view page to fix the error (missing template);
 - Missing whole methods;
 - Related to database records and migrations;
 - Deprecated functionalities.
 
After checking all 25 exception errors, I found most of them can be reproduced easily. However, others are obscure. Fortunately, Expertiza logs the actions of all users and I can reproduce the rest of exception errors according to the log file. What is more, Airbrake records the backtrace information for each exception error. In this way, it will be easy for me to do fault localization.

Below are identifications of exception errors I need to check the production file:
 - 1766139777878852159
 - 1770032930415444770
 - 1517247902792549741
 - 1805332790232222219
 - 1780737855340413304
 - 1800317719559012696
 - 1800240536513675372
 - 1781383426363363128
 - 1781341924698360618
 - 1778730510620033769

### References
[1] S. Huang, M. B. Cohen, and A. M. Memon. Repairing GUI
test suites using a genetic algorithm. In Proceedings of the
2010 Third International Conference on Software Testing,
Verification and Validation, ICST ’10, pages 245–254,
Washington, DC, USA, 2010. IEEE Computer Society. <br>
[2] Y. Qi, X. Mao, Y. Lei, Z. Dai, and C. Wang. The
strength of random search on automated program
repair. In Proceedings of the 36th International
Conference on Software Engineering, ICSE 2014,
pages 254–265, New York, NY, USA, 2014. ACM.
