# Notes

## 13 May 2024

- survey countries could only be created and updated by the survey authors (admins, and survey client user)

- for cloned up surveys, we're not creating charges history (weird), need to understand why!

- github actions (checkout, setup-python, setup-node, upload-artifact, cache) updated to latest releases as they're deprecating soon

## 14 May 2024

- `for cloned up surveys, we're not creating charges history (weird), need to understand why!`: the idea is now we store the actions of the charges history using `SurveyChargeHistoryLog` so whenever we perform any action manually then only we get the history of the charges and that's why the cloned survey doesn't have any charges history in the start even though the default charges exist.

- the survey charges (background development, question remainder etc) in the `Survey` model are just there for the default charges and after any action on the charges they get stored in the `SurveyCredit` model
