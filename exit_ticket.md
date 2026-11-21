# Exit ticket — Day 12

**Full name:** Đặng Minh Quân · **Code:** G01

---

**Question 1 — choose one.** A DMS camera on many cars projects ~940 nm near-infrared LED light onto the driver's face. What is the main reason?

Answer: **B** — it produces a stable face image day and night without dazzling the driver. 940 nm sits outside visible light, so the illuminator is invisible to the eye; it is not for skin temperature (A), not for prettier colour video (C), and IR does not pass through every sunglasses lens (D) — dark lenses still block the eye signal.

**Question 2 — number, 2 decimals.** A DMS camera runs at 30 fps. The model estimates head yaw > 30 degrees continuously from frame 240 to frame 314, **counting both frame 240 and frame 314**. For how many seconds is the driver not looking at the road?

Answer: **2.50 s** — frames 240…314 inclusive = 314 − 240 + 1 = 75 frames; 75 / 30 = 2.50 s.

**Question 3 — choose all correct statements.** Which are true about cabin data compared with exterior camera data?

Answer: **A, B, C**

- A true — a DMS model needs the actual face, so the driver's face cannot be blurred the way a pedestrian's face can.
- B true — at night the cabin camera is near-infrared and monochrome, so a shirt-colour attribute cannot be annotated.
- C true — the OMS camera looks toward the rear of the car, so the seat on the left of the picture is the car's right seat.
- D false — cabin objects are close to the camera (roughly 0.5–1.5 m), not "far and small"; what is true is that many objects share one frame.
- E false — a fixed cabin scene still needs its own guideline and QC: that is exactly why Guideline Cabin v1.0 exists, and why 22 frames produced 75 boxes to audit.
