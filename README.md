# A/B Testing With SQL

## Introduction
- We would like to test whether a change of the layout of item pages will affect item orders/views.
- An experiment was run at an item-level. All users who visit will see the same page, but the layout of different item pages may differ.
- For test assignments, 0 = control and 1 = treatment.
- Testing was limited for `test_number` = 'item_test_2' in this project.

## Metrics
- Two metrics were used:
  - 30-day orders (binary) : whether an item was ordered 30 days within the treatment.
  - 30-day views (binary) : whether an item was viewed 30 days within the treatment.

## Hypothesis
- H0: There is no effect of treatment on 30-day orders/views.
- H1: There is an effect of treatment on 30-day orders/views.

## Results
- View results [here](https://app.mode.com/lullaby1024/reports/2779da93b019).
