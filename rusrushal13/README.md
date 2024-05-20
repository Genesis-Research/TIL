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
