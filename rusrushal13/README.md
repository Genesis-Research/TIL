# Notes

## 13 May 2024

- survey countries could only be created and updated by the survey authors (admins, and survey client user)

- for cloned up surveys, we're not creating charges history (weird), need to understand why!

- github actions (checkout, setup-python, setup-node, upload-artifact, cache) updated to latest releases as they're deprecating soon

## 14 May 2024

- `for cloned up surveys, we're not creating charges history (weird), need to understand why!`: the idea is now we store the actions of the charges history using `SurveyChargeHistoryLog` so whenever we perform any action manually then only we get the history of the charges and that's why the cloned survey doesn't have any charges history in the start even though the default charges exist.

- the survey charges (background development, question remainder etc) in the `Survey` model are just there for the default charges and after any action on the charges they get stored in the `SurveyCredit` model

## 15 May 2024

- did one support request, we've two flag in the Country model for the showing the country in the profile of the user for country of expertise and country of residence, `active` and `default`. Both are boolean fields and in order to show them in the country dropdown, we need to make the `default` to be `True` and `active` remains `True` (don't know why), `active` most likely coming Abstract model classes.

- in reports, survey credits are being called using prefetching so for every survey, we're getting all the credits one by one, need to optimize this going forward.

## 22 May 2024

- AWS guard duty is a service that monitors the AWS environment and alerts when it detects suspicious activity. Same like guard duty, AWS WAF is a web application firewall that helps protect web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources. For access control, we can use AWS IAM as well as AWS cognito for authentication which could provide the secure access to the resources. The S3 bucket policies also helps us in securing the bucket and the objects in it. AWS WAF also keep monitoring the traffic and block the malicious traffic.

## 23 May 2024

- Went through a blog today related to [parsing instead of validation statements](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/). It was kind of eye opening in terms where the type system shines, I'm currently refactoring the survey credits tests and we've bunch of validation errors in the codebase for the same so we've tests which tests these validations/bad inputs/edge cases. If we can parse the input instead of validating it then we could have avoided these tests as well as these validations in the codebase itself. Mypy does a nice job. [Found a usecase for the same which `urllib3` library did](https://sethmlarson.dev/tests-arent-enough-case-study-after-adding-types-to-urllib3)

## 24 May 2024

- the survey models tests are mostly around charges being calculated and unit testing of them. After the current changes around manual charging the credits, these calculation are bit futile in terms of the tests as we're not exactly calculating the charges based on the milestones of the survey. I've kept the survey models tests as it is for now as unit tests are always good to have but we need to revisit them in future.

## 27 May 2024

- There was small issue with my terminal around locale and because of that the terminal wasn't acting the way it used to be. I found an issue on the zsh too regarding the same: [Strange issue with Mac OS X terminal](https://github.com/ohmyzsh/ohmyzsh/issues/1602). Even the name suggests it's strange but the solution points to me that it should be more common even though name suggests differently. The issue was with the locale settings and I had to add the locale settings in my `.zshrc` file to fix the issue with `en-us.utf-8` locale.

- the survey model tests are been pushed and started looking at the create authoring survey tests.

## 28 May 2024

- pytest have weird behaviour when it comes ignoring multiple files, instead of passing them together in the command line, we need to pass them one by one. I had to pass them one by one to ignore them. So if you wanna ignore multiple files you've to pass `--ignore` flag along with the file name multiple times.

```shell
pytest --ignore=tests/test_survey_models.py --ignore=tests/test_survey_authoring.py
```

## 30 May 2024

- gone through one talk today related to ORMs and the pg queries behind it (<https://www.youtube.com/watch?v=Ph2hXpTW-Zg&t=52s>). I was expecting bit more from the talk but learnt one two config details around the `postgresql.conf`

```shell
# logs all the statements which are taking more than 0ms to execute
# so we can see the slow queries more than 1000ms in the logs too
log_min_duration_statement = 0

# logs all the statements but there are multiple options in there such as
# none, ddl, mod, all
# by default it's none which means it won't log any statement
# ddl means it will log all the ddl statements that means create, alter, drop etc
# mod means it will log all the ddl and dml statements that means insert, update, delete etc along with ddl
# all means it will log all the statements
log_statment = 'all'
```

- Other than that, I've continued with the test fixes and worked on `test_credit_audit.py` tests today. They mostly consists of survey_charge_history_log tests and the charge_history endpoint tests.

## 31 May 2024

- new weird bug happened today, we usually use `$@` in bash to get the arguments in the script for passing the cron to the elastic beanstalk cron config. But one of our command had a flag along with the command and it was failing because of that. The solution we relied on was to use double quotes but it didn't work. Instead of using the `@*` or `"$@"` we went ahead with just using the simple file name for redirection and then passing the arguments in our docker command. For more info, do check the change: `https://github.com/mat-technology/rpr/pull/3451/files#diff-0e3c6507571f6bb67b93fbe899beb80e2050a5f53d3021489dcb344178d30265R54`

- Also, went through one lambdaconf video today related to learning clojure: `https://www.youtube.com/watch?v=91jmzgW7brc` which was quite interesting as it says it took almost 450 Hours to learn clojure.

## 3 June 2024

- on hold action of the survey, the forecast date of charges is shifted to 3 months ahead from the date it's been forecasted

## 5 June 2024

- Solution stack and most of the module data which keep changing in the terraform modules can be obtained via data sources in terraform. We can use the `aws_elastic_beanstalk_solution_stack` data source to get the solution stack and use it in the `aws_elastic_beanstalk_environment` resource.

## 6 June 2024

- AWS service quota are little bit tricky, there are some quota which are automatic and some are manual. The automatic quota are the one which are managed by AWS and they're automatically increased when needed(need to put on request in sometime they'll be approved more or less in 30 mins). The manual quota are little pesky as you've to wait for them.

- Peering connection of vpcs also requires the route table associations as well so that the traffic can flow between the vpcs, need to keep that in mind.
