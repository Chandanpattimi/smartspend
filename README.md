# SmartSpend: AI Personal Finance Manager

Prototype for **Product Design & Development (VR23), Review 1**.
Vignan's Institute of Information Technology, Department of IT.
Presented by P. Chandan (23L31A12C3), guided by Mr. Brajesh Kumar. Supports SDG 8.

Enter a salary and SmartSpend plans the whole month: fixed commitments are paid first,
savings are set aside next, and flexible categories share what is left by weight.

## What the prototype does

| Review 1 feature | Where it is |
| --- | --- |
| Categorized income and expense input | **Setup** tab: fixed, savings and flexible categories |
| Rule-based salary allocation algorithm | `src/engine.js` (`allocate`) |
| Spending caps and bill reminders | Caps on flexible categories; **Track** tab lists bills due in 7 days |
| Allocation breakdown dashboard | Pay slip bar and ledger on the **Plan** tab |
| Manual override with real-time tracking | Tap any amount to override; **Track** tab compares spend with caps |
| Spending insights and alerts | `trackSpending`: over-cap, month-end pace, fixed ratio, savings rate |
| Adapts as spending patterns change | `adaptWeights`: "Learn from this month" in **Setup** |

The SMART objective of allocating across 6 or more categories is met by the sample plan (10 categories).
The currency is configurable (INR, USD, EUR, GBP). Data stays in the browser's local storage.

## How the allocation works

1. **Fixed commitments and manual overrides** are funded first, in list order. If the salary
   cannot cover them, the shortfall is reported per category.
2. **Savings** (a percent of salary or a fixed amount) come next, but never take the money
   that flexible essentials need for their floor.
3. **Flexible categories** split the remainder by weight. Each has an optional floor and cap;
   money a capped category cannot take flows to the others. Anything left once every cap is
   full is shown as unallocated.
4. Rounding uses the largest remainder method, so the lines always add up to the salary exactly.

`adaptWeights` multiplies a category's weight by a factor between 0.8 and 1.25 based on how
last month's projected spending compared with its cap.

## Run it

```bash
npm test          # 15 engine tests, no dependencies (Node 20+)
npm start         # serves index.html locally
```

Or open `index.html` directly in a browser. To publish it, enable **Settings > Pages**
on the repository and serve from the `main` branch root.

## Project layout

```
index.html            mobile-first dashboard (no build step)
src/engine.js         allocation engine, usable in the browser and in Node
test/engine.test.js   unit tests, including 500 randomized salary scenarios
```

## Roadmap (from the Review 1 plan)

- Port the engine to Dart and build the Flutter app (Android first)
- Firebase Authentication and Firestore for categories and transactions
- Reminders as push notifications
- Optional on-device ML model for budget prediction, layered on the rule-based engine
- Pilot with 5-10 real users and refine categories
