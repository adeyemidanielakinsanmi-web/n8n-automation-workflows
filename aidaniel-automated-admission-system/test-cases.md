# Test Cases

All applicant data below is fictional.

| # | UTME Score | Pre-Degree Score | Expected Status | Actual Result |
|---|------------|------------------|-----------------|---------------|
| 1 | 65 | 70 | First Degree | First Degree |
| 2 | 40 | 35 | Not Admitted | Not Admitted |
| 3 | 60 | 45 | University School Diploma | University School Diploma |
| 4 | 45 | 60 | University School Diploma | University School Diploma |
| 5 | 50 | 50 | First Degree | First Degree |

## What was verified
- Correct admission status assigned
- Applicant ID generated
- Applicant routed to the right Switch branch
- Correct Gmail notification sent

## Notes
Test 5 checks the boundary rule: a score of exactly 50 counts as meeting the threshold.
