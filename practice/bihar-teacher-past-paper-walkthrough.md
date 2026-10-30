# BPSC Higher Secondary 2023 Paper — Selected Solved Questions

This walkthrough uses selected questions from the official 26 August 2023 Higher Secondary Paper 4. The source booklet had 40 General Studies and 80 Computer Science questions, 120 total, with two hours and a one-fourth penalty for a wrong response. Those are historical rules; follow the current BPSC notice for an actual exam.

- [Official question booklet](https://bpsc.bihar.gov.in/Archive/2023/NB-2023-08-26-37.pdf)
- [Official provisional Series A key](https://bpsc.bihar.gov.in/Archive/2023/NB-2023-09-01-40.pdf)

The solutions below paraphrase selected items; question numbers refer to the Series A booklet.

## General Studies examples

### Q1 — Count three-digit multiples of 5

A three-digit number has hundreds digit 1–9 (9 choices), tens digit 0–9 (10 choices), and units digit 0 or 5 (2 choices). Count = 9×10×2 = 180. The answer is 180. Do not multiply by 10 for the hundreds place because zero would produce a two-digit number.

### Q3 — Average speed over two segments

The trip is 200 km: 120 km at 90 km/h and 80 km at 40 km/h. Time is 120/90 + 80/40 = 4/3 + 2 = 10/3 hours, or 3 hours 20 minutes. Average speed is total distance/total time = 200/(10/3)=60 km/h. Averaging 90 and 40 directly is wrong because the segment times differ.

### Q4 — Loss measured as a percentage of selling price

Let selling price be 100 units. A loss equal to 10% of selling price is 10 units, so cost price is 110. Loss as a percentage of cost price = 10/110×100 = 9 1/11%. The base named in the question changes the denominator.

### Q6 — Carpet area

At ₹4.50 per metre, ₹810 buys 180 metres of carpet. Its width is 0.75 m, so carpeted area is 180×0.75=135 m². The room is 18 m long, so breadth = 135/18=7.5 m.

### Q7 — Combined work rates

A and B together complete 1/15 of the job per day. A alone completes 1/20 per day. B's rate is 1/15−1/20=(4−3)/60=1/60 job/day, so B alone needs 60 days. Subtract rates, not days.

## Computer Science examples

### Q44 — Recognize multiple true compiler statements

The item tests the compiler's input and output and whether translation happens in stages. A compiler receives a source program and produces target/object code; it commonly performs analysis and synthesis phases rather than one indivisible step. The source paper's provisional key marks “more than one” for this item. The exam lesson is to test every option separately: if two statements are true, do not select only one true option.

### Q45 — Memory compaction

Compaction relocates processes/regions so scattered free holes are combined into one larger free block. It addresses external fragmentation, though relocation costs time and requires movable allocations. It does not remove internal fragmentation inside an allocated block.

## Use the official key correctly

The linked key is provisional and Series A. Question order/options may differ by set. First solve the official booklet under its stated rules, then check the corresponding set key; do not copy a Series A letter onto another series. For an item later revised in a final key, use the final BPSC key if available.

### Review table

| Miss | Concept | Why I missed it | One new question to retry |
|---|---|---|---|
| Example | Percent base | Used selling price as denominator for cost-price loss | Create a reverse-percentage problem |
| Example | Work rate | Subtracted completion days instead of rates | Recompute with different times |