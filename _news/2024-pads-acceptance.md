---
layout: post
title: Regular Article accepted at PADS 2024
date: 2024-03-30 10:00:00
inline: false
related_posts: false
---

The regular article titled **"Simulating Data Flows of Very Large Scale Intelligent Transportation Systems"** is accepted for presentation at the ACM Sigsim PADS (Principles of Advanced Discrete Simulation) 2024 conference. In this article, an open-source, highly scalable, and customizable VANET simulator called [Disolv](https://github.com/nagacharan-tangirala/disolv) is described. The simulator is developed in Rust and incorporates several key design decisions aimed at solving the scalability issue of the current state-of-the-art VANET simulators.

The key benefit of the simulator is that it provides a performant backend of VANET(Vehicular Ad-hoc Networks) communication structure, allowing the researcher/user to focus on the VANET application that is under study. For instance, a researcher interested in RSU placement problem does not have to implement the entire application in a simulator modeling the entire protocol-stack (ns-3, Veins etc.). Instead, they can use Disolv and achieve immense scalability for evaluations at city-scale. For a stable state condition, Disolv ensures a statistically negligible loss of fidelity at the benefit of manifold scalability.

The article contains the design architecture and the experiments carried out to validate the usefulness of the simulator. Additionally, we signed up for the optional [ACM reproducibility initiative](https://www.acm.org/publications/policies/artifact-review-and-badging-current) which assesses the reproducibility capabilities of the results presented in the paper. A key goal that we are trying to tackle with Disolv is the reproducibility crisis. It is imperative that we participate in this initiative.
