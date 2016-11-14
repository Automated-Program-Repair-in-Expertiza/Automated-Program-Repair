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
Expertiza uses Airbrake to capture and track runtime exceptions. I used [Airbrake API](https://airbrake.io/docs/api/) to obtain more than 100 runtime exception errors from Expertiza occured within 1 month. Then I selected 17 exception errors in order to do further automated bug fixing. The main reasons I filtered the rest exception errors are:
 - Already been fixed;
 - Cannot be reproduced;
 - Need to create a new view page to fix the error (missing template);
 - Missing whole methods;
 - Related to database records and migrations;
 - Deprecated functionalities.
 
After checking all 17 exception errors, I found most of them can be reproduced easily. However, others are obscure. Fortunately, Expertiza logs the actions of all users and I can reproduce the rest of exception errors according to the log file. What is more, Airbrake records the backtrace information for each exception error. In this way, it will be easy for me to do fault localization.

Below are details of each exception error, including airbrake group id, occurrences, error message and test cases I wrote:

| Airbrake Group Id   | Occurrences | Error Message                                                   | Test Cases                     |
|---------------------|-------------|-----------------------------------------------------------------|--------------------------------|
| [1806782678925052472](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1806782678925052472) | 932         | Undefined method for nil:NilClass ('can_submit')                | Feature test                   |
| [1775143306379398644](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1775143306379398644) | 790         | Couldn't find Participant without an ID                         | Functional test                |
| [1517247902792549741](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1517247902792549741) | 535         | Undefined method for nil:NilClass ('student?')                  | Feature test + Functional test |
| [1766139777878852159](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1766139777878852159) | 105         | Private method called for nil:NilClass ('select')               | Functional test                |
| [1608029321738848168](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1608029321738848168) | 26          | Undefined method for nil:NilClass ('id')                        | Feature test                   |
| [1781551925379466692](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1781551925379466692) | 16          | Undefined method for nil:NilClass ('split')                     | Unit test                      |
| [1776303046291622084](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1776303046291622084) | 11          | Undefined method for Array ('page')                             | Feature test                   |
| [1805332790232222219](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/aribrake-1805332790232222219) | 8           | Undefined method for nil:NilClass ('id')                        | Unit test                      |
| [1766248124300920137](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1766248124300920137) | 7           | Undefined method for nil:NilClass ('is_waitlisted')             | Unit test                      |
| [1780737855340413304](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1780737855340413304) | 6           | Undefined method for nil:NilClass ('keys')                      | Feature test                   |
| [1774360945974838307](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1774360945974838307) | 5           | Undefined method for nil:NilClass ('tempfile')                  | Functional test                |
| [1804043391875943089](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1804043391875943089) | 2           | No implicit conversion of nil into String                       | Functional test                |
| [1817691804353957801](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1817691804353957801) | 2           | Undefined method for nil:NilClass ('each_pair')                 | Feature test                   |
| [1800902813969550245](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1800902813969550245) | 2           | Couldn't find ReviewResponseMap ('id'=114593)                   | Functional test                |
| [1807465099223895248](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1807465099223895248) | 2           | Couldn't find Team ('id'=27022)                                 | Functional test                |
| [1800240536513675372](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1800240536513675372) | 1           | Uninitialized constant (SignUpSheetController::TopicDependency) | Feature test                   |
| [1784274870078015831](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1784274870078015831) | 1           | Undefined method for nil:NilClass ('user_id')                   | Functional test                |

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
