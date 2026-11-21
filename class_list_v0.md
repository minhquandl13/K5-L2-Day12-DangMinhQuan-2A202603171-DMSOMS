# Class list v0 — Đặng Minh Quân · 2A202603171 · G01

Written from Cabin Guideline v1.0 (§4.1–4.8), extending the class list on lecture slide 54.
Every row answers one question: **what does the model need this class for?**

## 1. DMS — Driver Monitoring System

| # | Class / attribute | Values | Purpose (what the DMS does with it) | Visible on IR? |
|---|---|---|---|---|
| 1 | `person` (`seat` = driver) | driver | Driver present in the seat; anchors the belt and hand logic. No driver → no monitoring. | Yes |
| 2 | `person` (`seatbelt`) | worn_correct / worn_incorrect / not_worn / unknown | Belt-compliance alarm; `worn_incorrect` is the dangerous case (belt behind the back). | Partly — dark belt on dark clothing goes `unknown` |
| 3 | `face` (box) | — | Face localisation; every read below (eyes, landmarks, gaze) is cropped from this box. | Yes |
| 4 | `face.eye_left` / `face.eye_right` | open / closed / unknown | Drowsiness and microsleep: eyes closed over time (PERCLOS-style) and blink rate. Per-eye, because one eye can be occluded. | Yes, except under LED glare or dark lenses |
| 5 | `face.glasses` | none / clear / sun | Decides whether the eye signal is trustworthy. `sun` means the eyes are effectively unobservable, so the system must fall back to head pose. | Partly — sunglasses can look transparent on NIR |
| 6 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Visual distraction: eyes off the road while driving. `lap` + `phone` is the classic phone-while-driving pattern. | Partly — needs head pose, and NIR detail is low |
| 7 | `face13` | 13 landmarks, each with `occluded` / `outside` | Head pose (pitch/yaw/roll), blink and yawn geometry, gaze direction — this is what makes gaze measurable rather than guessed. | Yes, with the occluded points flagged |
| 8 | `hand` | `whose` driver/passenger · `side` left/right · `on_wheel` yes/no/unknown | Hands-on-wheel compliance and takeover readiness; also anchors `phone` (`held`). | Yes |
| 9 | `phone` | held / mounted / lap | Phone-use detection — a safety and legal violation class, not an object class. | Yes by shape; a dark screen shows no glow on NIR |
| 10 | tag `frame_meta` | modality · light · camera_blocked | Frame gate: the model must know whether this frame is usable (ir vs rgb, night, blocked lens) before trusting any prediction. | n/a (tag, not an object) |

## 2. OMS — Occupant Monitoring System

| # | Class / attribute | Values | Purpose (what the OMS does with it) | Visible on IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | rear_left / rear_middle / rear_right / front_passenger | Per-seat occupancy — the base state of the whole rear cabin. | Yes |
| 2 | `person` (`seatbelt`) | not_worn / unknown *(SVIRO has no worn case)* | Rear belt-compliance reminder. | Often not — dark belt, low contrast |
| 3 | `face` (box only) | — | Confirms the occupant is a person (adult) rather than luggage, and pairs a face with a seat for belt warnings. | Yes |
| 4 | `child_seat` | `seat` · `occupied` no / unknown | Child-restraint presence and emptiness; drives the "child left in car" reminder and airbag suppression. | Yes (shape) |
| 5 | `infant_seat` | `seat` · `facing` rear / front | Rear-facing carrier detection — the most dangerous forgotten-child case — plus airbag suppression. | Yes (shape) |
| 6 | `object` | `seat` · `type` bag / box / cup / other | Separates belongings from occupants so luggage is not counted as a child; also flags loose objects. | Shape yes, `type` often degrades to `other` |
| 7 | tag `frame_meta` | modality · light · camera_blocked | Same frame gate as DMS: a blocked or very dark rear camera must not raise a false "empty cabin". | n/a (tag) |

Empty seats carry **no box**; they are counted in the frame note instead (§4.8).

## 3. Rules applied while building this list

1. Seat names follow the **vehicle**, not the picture: on OMS rear-seat frames, image-left = `rear_right`.
2. Eye and hand left/right follow the **person**; on rotated D## frames the driver's left eye has the larger y.
3. `unknown` whenever the visual evidence is insufficient — never inferred from the other eye, the paired frame, or the filename.
4. Boxes hug the visible extent, tight to the edge.
5. An object inside a child seat takes **two boxes** (`child_seat` + `object`) on the same `seat`.
6. `face13` only on D## frames and only when the face is at least 60 px wide; every hidden point gets `occluded` or `outside`.
7. `seatbelt = worn_correct` cannot occur on SVIRO; only `not_worn` or `unknown`.
8. SVIRO `_ir` is **simulated** IR: LED glare, bright pupils and distance falloff are absent, so those phenomena must never be logged from an S## frame.

## 4. Escalation questions

**Q1. How should an annotator bound an object or a facial feature that is only partly visible, when the hidden part is the one that decides the class?**
- Example frames: `G01 D01` — a `face13` corner point sits behind an LED reflection on the lens, so its position can be estimated but not seen; `G01 S03_ir` — the cup sits on the `rear_right` / `rear_middle` boundary with its lower edge hidden by the seat.
- Provisional answer: attribute fields go `unknown`; `face13` points go `occluded` when the location can be estimated with confidence and `outside` when it cannot. No point is ever placed "to make 13".

**Q2. When a person occupies the same bench position as a mounted infant carrier or child seat, which box wins?**
- Example frames: `G01 S02_rgb`, `G01 S07_rgb`, `G01 S08_rgb` — `person` and `infant_seat` / `child_seat` overlap on the same seat.
- Provisional answer: `§4.8` says one box per seat, but `§4.2` lists both classes without saying which survives when they coincide. I kept the `person` box and set the carrier to `occupied = unknown` where its interior is not visible, and raised it with the coach. Needs a ruling in v1.1.

## 5. Reference

Cabin Guideline v1.0, §4.1–4.8 (labels, attributes, six general rules, face / eyes / gaze, hands / phone / belt, child seats and objects). Day 21 (DMS) and Day 22 (OMS) will extend this list to v1.x.
