# Automated-Program-Repair
This repo contains all the code, artifacts, and submitted documents for the project.

### Slides
 - [Link](https://goo.gl/LMmwy1)

### Purpose
The purpose of this project is to use random search algorithm to fix runtime exceptions in Expertiza automatically

### Research Questions
 - **RQ1:** What percentage of runtime exception errors in Expertiza can be fixed by random search algorithm?
 - **RQ2:** On average, how long will it take random search algorithm to fix one Expertiza runtime exception error?
 
### Evaluation Plan
 - **RQ1**
  - Metric: number of  runtime exception errors fixed by automated evolutionary program repair out of the total number of selected errors.
  - Artifact: unresolved Expertiza runtime exception errors caught by Airbrake.
  
 - **RQ2**
  - Metric: average time consumed by random search algorithm to fix one runtime exception error.
  - Artifact: unresolved Expertiza runtime exception errors caught by Airbrake.
  
### Timeline
 - Unresolved runtime exception erro selection (Oct. 7 to Oct. 13)
 - Random search algorithm implementation (Oct. 14 to Oct. 31)
 - Automated runtime exception erro fixing (Nov. 1 to Nov. 17)
 - Result analysis (Nov. 18 to Nov. 23)
 - Final paper writing (Nov. 24 to Nov. 31)
 
### Progress
#### Runtime exception error selection
Expertiza uses Airbrake to capture and track runtime exceptions. I used [Airbrake API](https://airbrake.io/docs/api/) to obtain more than 100 runtime exception errors from Expertiza occured within 1 month. Then I selected 17 exception errors in order to do further automated runtime exception error fixing. The main reasons I filtered the rest exception errors are:
 - Already been fixed;
 - Cannot be reproduced;
 - Need to create a new view page to fix the error (missing template);
 - Missing whole methods;
 - Related to database records and migrations;
 - Deprecated functionalities.
 
After checking all 17 exception errors, I found most of them can be reproduced easily. However, others are obscure. Fortunately, Expertiza logs the actions of all users and I can reproduce the rest of exception errors according to the log file. What is more, Airbrake records the backtrace information for each exception error. In this way, it will be easy for me to do fault localization.

#### Test case creation and developer patch generation
##### Details of selected runtime exception errors
Below are details of each exception error, including airbrake group id, occurrences, error message and test cases I wrote (if you click each airbrake group id, it will redirect to the corresponding branch including developer patch and test case):

| Airbrake Group Id   | Occurrences | Error Message                                                   | Test Cases                     | Num of Mutants
|---------------------|-------------|-----------------------------------------------------------------|--------------------------------|--------------|
| [2472](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1806782678925052472) | 932         | Undefined method for nil:NilClass ('can_submit')                | [Feature test#L32](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L32)                   |134    |
| [8644](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1775143306379398644) | 790         | Couldn't find Participant without an ID                         | [Functional test#L63](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L63)                |97   |
| [9741](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1517247902792549741) | 535         | Undefined method for nil:NilClass ('student?')                  | [Feature test#L116](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L116) <br/> [Functional test](https://github.com/expertiza/expertiza/blob/master/spec/controllers/tree_display_controller_spec.rb#L5) |32   |
| [2159](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1766139777878852159) | 105         | Private method called for nil:NilClass ('select')               | [Functional test#L97](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L97)                |76   |
| [8168](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1608029321738848168) | 26          | Undefined method for nil:NilClass ('id')                        | [Feature test#L83](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L83)                   |290    |
| [6692](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1781551925379466692) | 16          | Undefined method for nil:NilClass ('split')                     | [Unit test#L4](https://github.com/expertiza/expertiza/blob/master/spec/models/airbrake_expection_errors_unit_tests_spec.rb#L4)                      |126   |
| [2084](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1776303046291622084) | 11          | Undefined method for Array ('page')                             | [Feature test#L170](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L170)                   |160
| [2219](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/aribrake-1805332790232222219) | 8           | Undefined method for nil:NilClass ('id')                        | [Unit test#L37](https://github.com/expertiza/expertiza/blob/master/spec/models/airbrake_expection_errors_unit_tests_spec.rb#L37)                      |64  |
| [0137](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1766248124300920137) | 7           | Undefined method for nil:NilClass ('is_waitlisted')             | [Unit test#L66](https://github.com/expertiza/expertiza/blob/master/spec/models/airbrake_expection_errors_unit_tests_spec.rb#L66)                      |263 |
| [3304](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1780737855340413304) | 6           | Undefined method for nil:NilClass ('keys')                      | [Feature testL#41](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L41)                   |968    |
| [8307](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1774360945974838307) | 5           | Undefined method for nil:NilClass ('tempfile')                  | [Functional test#L44](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L44)                |476  |
| [3089](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1804043391875943089) | 2           | No implicit conversion of nil into String                       | [Feature test#L157](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L157)                |39  |
| [7801](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1817691804353957801) | 2           | Undefined method for nil:NilClass ('each_pair')                 | [Feature test#L99](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L99)                   |286    |
| [0245](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1800902813969550245) | 2           | Couldn't find ReviewResponseMap ('id'=114593)                   | [Functional test#L147](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L147)                |142    |
| [5248](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1807465099223895248) | 2           | Couldn't find Team ('id'=27022)                                 | [Functional test#L6](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L6)                |277    |
| [5372](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1800240536513675372) | 1           | Uninitialized constant (SignUpSheetController::TopicDependency) | [Feature test#L64](https://github.com/expertiza/expertiza/blob/master/spec/features/airbrake_expection_errors_feature_tests_spec.rb#L64)                   |181  |
| [5831](https://github.com/Automated-Program-Repair-in-Expertiza/expertiza/tree/airbrake-1784274870078015831) | 1           | Undefined method for nil:NilClass ('user_id')                   | [Functional test#L116](https://github.com/expertiza/expertiza/blob/master/spec/controllers/airbrake_exception_errors_controller_tests_spec.rb#L116)                |157

##### Example of developer patch and test case
I choose the runtime exception error which has the most occurrences to describe the test case creation and developer patch generation.

According to airbrake error message, the buggy code is at line 388 of `	app/controllers/sign_up_sheet_controller.rb` file. And the url to trigger this bug is `	
https://expertiza.ncsu.edu/sign_up_sheet/list?id=811`. Here is the buggy functionality.
```
387:    def are_needed_authorizations_present?
388:        @participant = Participant.where('user_id = ? and parent_id = ?', session[:user].id, params[:assignment_id]).first
389:        authorization = Participant.get_authorization(@participant.can_submit, @participant.can_review, @participant.can_take_quiz)
390:        if authorization == 'reader' or authorization == 'submitter' or authorization == 'reviewer'
391:            return false
392:        else
393:            return true
394:        end
395:    end
```
In buggy code, it reads the value from `params[:assignment_id]`. However, the url offers `id` as parameter. Consequently, instance variable `@participant` will be nil. And then, when line 389 tried to read the `can_submit` attribute from `@participant`, the system will raise `nil:NilClass` error.

So for developer patch, I added one new line (`params[:assignment_id] = params[:id] if params[:assignment_id].nil?`) above line 388. The new line means if `params[:assignment_id]` is nil, system will assign `params[:id]` to `params[:assignment_id]`. In this case, either `params[:id]` or `params[:assignment_id]` has the correct value will make the code work.

For test case, I wrote a feature test using Rspec and Capybara. Test code is shown below. Basically I tries to visit 2 different urls and make sure the system does not raise `nil:NilClass` error.
```
it "Airbrake-1806782678925052472", js: true do
    	login_as 'student2066'
    	visit '/sign_up_sheet/list?assignment_id=1'
    	expect(page).to have_content('Signup sheet for')
    	expect(page).to have_content('Hello world!')
    	expect(page).to have_content('TestReview')
	
    	visit '/sign_up_sheet/list?id=1'
    	expect(page).to have_content('Signup sheet for')
    	expect(page).to have_content('Hello world!')
    	expect(page).to have_content('TestReview')
	end
```




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
