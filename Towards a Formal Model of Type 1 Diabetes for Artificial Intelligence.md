# [Towards a Formal Model of Type 1 Diabetes for Artificial Intelligence](https://radar.brookes.ac.uk/radar/file/1c739bf4-e995-4d8e-b63b-fa0936980210/1/fulltext.pdf)

## Introduction

* T1D patients have to compute multiple times a day their dose, which is error prone.
* Need for systems to be really robust before they can be deployed.
* Recommender system for bolus dose is proposed.

## Formal specification
* Using Event-B formal modelling language - provides verification guarantees using set theory as a language.
* Event B is limited to natural and integer numeric types.
* Providing invariants and limits in the specification allows for a simple model to ensure a proper bolus dose.

* Variables:
  - maxBolus
  - targetRangeUpper - target blood glucose upper value
  - targetRangeLower - target blood glucose lower value
* Invariants:
  - maxBolus is a natural number <= 500 IU
  - targetRangeUpper is a natural number and >= 55 and <= 150
  - targetRangeLower is a natural number and >= 30 and <= 80
  - targetRangeLower <= targetRangeUpper

* These examples illustrate critical safety constraints for a T1D bolus calculator involving AI
  can be captured formally. Note there are difficult challenges with verifying AI systems that modify
  themselves.
