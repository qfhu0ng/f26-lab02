# Lab 2 Starter: Availability Calculator

A small reservation component. Given a room's bookings and the day's business hours,
`AvailabilityCalculator.freeSlots` computes when the room is free. It is the code you
work in for Lab 2.

It ships with a generated test suite that passes, and a property-based test harness
(jqwik) with one example property. Everything is green. Your job in Lab 2 is to decide
whether green actually means correct.

**Read `ARCHITECTURE.md` before the code.**

## Build and test

```
mvn test
```

`mvn test` runs both files, the ordinary example-based tests (`AvailabilityCalculatorTest`)
and the property-based tests (`AvailabilityProperties`). A code-coverage report is written
to `target/site/jacoco/index.html`.

## Continuous integration

This repository has CI configured in `.github/workflows/ci.yml`. GitHub disables workflows on a
fresh fork, so enable them once on your fork (the handout shows where). After that, every
push runs `mvn test`. You will watch the gate go red when your new property finds the bug, then
green once you fix it.

## Where things are

- Component: `src/main/java/edu/cmu/cs214/availability/`
- Example-based tests: `src/test/java/edu/cmu/cs214/availability/AvailabilityCalculatorTest.java`
- Property-based tests: `src/test/java/edu/cmu/cs214/availability/AvailabilityProperties.java`
- Setup: `SETUP.md`

See the Lab 2 handout on the course page for the three milestones you show a TA.

## Milestone 3: Audit of the Generated Test Suite

1. **No trailing-only free-time case (controllability gap).** The suite never supplies
   a non-empty booking list where a booking starts at `DAY_START` and the last booking
   ends before `DAY_END`. That input would make the trailing interval the only free
   time and directly expose its omission.
2. **No empty-bookings case (controllability gap).** The suite never supplies an empty
   booking list, where the entire business day should be free.
3. **The no-overlap test has a safety-only oracle (observability gap).**
   `returnedSlotsNeverOverlapABooking` checks only that returned slots are valid, not
   whether any free time was omitted.

High coverage did not save the suite because coverage reports which existing lines and
branches executed, not whether the assertions fully specify the result. In particular,
there was no line for emitting the trailing gap to cover, and the test that exercised
the relevant input did not observe the missing output.

## AI Assistance

Tool: OpenAI Codex desktop app. Model: `gpt-5.6-sol`.
