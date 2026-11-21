# AI usage declaration — Đặng Minh Quân · 2A202603171

Reminder that governed every row below: no cabin image or driver image was handed to any AI tool, and no AI produced a label for me.

| # | Tool | What I asked (summary) | How I used the answer | Did I verify it? Against what? |
|---|---|---|---|---|
| 1 | Doubao AI assistant (desktop) | Which files the submission must contain and where they go, read from `SUBMISSION.md` / `RUBRIC.md` | Copied the template forms to the submission root under the exact required filenames | Re-read `SUBMISSION.md` myself; the filenames match the tree in it |
| 2 | Doubao AI assistant | Step-by-step of CVAT task creation (project → task → 22 files, labels inherited from the project) | Used it as a checklist for Task 0; the task ended up with 22 frames and 1 job | CVAT's own task page showed Frames: 22; `cabin_check` reads 22 image entries |
| 3 | Doubao AI assistant | How to name seats and eye/hand sides (car vs person convention, rotated D## frames) | Used it to build my own one-page mapping sheet; every decision still taken on the frame itself | Cross-read `GUIDE §3`, `§4.4`, `COMMON_ERRORS`; then confirmed numerically in the export (`left_eye_*` y > `right_eye_*` y on all 6 D frames) |
| 4 | Doubao AI assistant | Whether a finished `export_final.zip` complied with the guideline (tool parsed the XML and reported box coordinates and attribute values, no images involved) | Used the audit to locate silent problems the script cannot see: 4 adults without `face`, seats carrying both a `person` and a carrier, eyes set `unknown` on 5 of 6 driver frames | Re-checked each flagged frame myself in CVAT before accepting it |
| 5 | Doubao AI assistant | Wording review of `class_list_v0.md` (structure, missing columns, whether an escalation question was already answered by the guideline) | Rewrote the list myself using the template's columns; replaced one question that §4.4 rule 3 already answers | Compared against `templates/class_list_v0.md` and `GUIDE §4.2/§4.8` |
| 6 | Doubao AI assistant | One D## frame screenshot was shared while asking which axis left/right applied to (breach of RULES §3, recorded here for transparency) | No label was taken from that answer; it was corrected to read the guideline text only | The rule is restated in `GUIDE §4.4` and `COMMON_ERRORS`, which I then followed |

Nothing above produced an annotation. Every box, attribute and `face13` point in `export_final.zip` was drawn by me in CVAT.
