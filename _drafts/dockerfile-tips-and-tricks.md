---
layout: post
title:  "Dockerfile: tips and tricks"
---

### Dockerfile

TODO: intro
Container images are the foundations of the container ecosystem, but, how do they work ?
To create a container image, the easiest way is through a Dockerfile : a list of instructions on how to build your image.

# Keep your Docker images small

# Layer grouping
Layer grouping is important. Data that is accessed or modified in two different Dockerfile instructions (two `RUN` blocks, for example) will result in a duplicated layer, and the size of the files will double.

Order is important: install your needed packages first, then copy your own files. This allow for better caching and layer reuse

Make your entrypoints extensible
    folder of scripts to run: /docker-entrypoint.d/
        with number at start for ordering

Take a small distro, but flee from the hype
    see debian-slim
    cf. why not alpine

? certificate management ?

subprocess management
    tini ? bash ?

