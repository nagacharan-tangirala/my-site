---
layout: post
title: Reproducibility report for the article accepted to PADS 2024
date: 2024-05-30 10:00:00
inline: false
related_posts: false
---

For the [article](https://dl.acm.org/doi/pdf/10.1145/3615979.3656062) on Disolv simulator that is submitted to PADS, I opted for an optional artefact evaluation. This involves an additional review of the code and methodologies to assess how reproducible the results in the paper are. A [docker image](https://hub.docker.com/repository/docker/tnagacharan/pads-2024-disolv/general) was designed to assist the reviewer in conducting this evaluation. Thanks to [Fabrizio Fornari](https://scholar.google.com/citations?user=1JEa1w4AAAAJ) for reviewing the artefact.

After some back and forth, the reviewer was successfully able to reproduce the results and concluded that the article deserves the following badges - Functional, Artifacts Available, and Results Reproduced. These badges are allocated based on the [conditions defined by ACM](https://www.acm.org/publications/policies/artifact-review-and-badging-current). The only badge that was not awarded is the _Reusable badge_, which requires an extensive documentation supporting the reusability of the simulator for other experiments. While the [docker image](https://hub.docker.com/repository/docker/tnagacharan/pads-2024-disolv/general) has instructions to regenerate the paper results, the [documentation for the simulator](https://disolv.dev/) itself is a work in progress. It should get better with more users experimenting with Disolv.

[Report is available](https://dl.acm.org/doi/10.1145/3615979.3665108) along with the proceedings. The badges are also visible on the top-right of the [article](https://dl.acm.org/doi/pdf/10.1145/3615979.3656062).
