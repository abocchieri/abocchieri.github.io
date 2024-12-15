---
layout: page
title: Projects
subtitle: 
---

## Code Development

### PAOS

<img src="https://abocchieri.github.io/assets/img/paos_logo.png" alt="paos" width="20%" height="20%" style="float:left; padding-right:30px">
[PAOS](https://paos.readthedocs.io/en/latest/) is a reliable, user-friendly, and open-source Physical Optics Propagation code that integrates an implementation of Fourier optics in Python3.8+. It employs the Fresnel approximation for efficient and accurate optical system simulations. By including a flexible configuration file and paraxial ray-tracing, PAOS seamlessly facilitates the study of various optical systems, including non-axial symmetric ones, as long as the Fresnel approximation remains valid. PAOS is publicly available on [GitHub](https://github.com/arielmission-space/PAOS) and [PyPI](https://pypi.org/project/paos/), so you can install it with a simple `pip install paos`.

### ExoRad2.0

<img src="https://abocchieri.github.io/assets/img/exorad_logo.png" alt="exorad" width="20%" height="20%" style="float:left; padding-right:30px">
[ExoRad 2.0](https://exorad2-public.readthedocs.io/en/latest/index.html) is a Python package that can be used as a standalone tool or as a library to be integrated in a larger software framework. In the framework of th√e Ariel Space Mission, we developed ExoRad 2.0, a versatile tool to estimate space instruments’ performance. ExoRad 2.0 (`pip install exorad`) is the core of the second version of the Ariel radiometric simulator, ArielRad. The ArielRad software has been extensively used by the consortium to validate the mission design, optimize the instrument performances, flow down the requirements to the subsystems’ level, and prepare Ariel science.

### ExoSim2.0

<img src="https://abocchieri.github.io/assets/img/exosim_logo.png" alt="exosim" width="20%" height="20%" style="float:left; padding-right:30px">
[ExoSim 2](https://exosim2-public.readthedocs.io/en/latest/) is a time-domain simulator for exoplanet observations. The software can simulate exoplanetary transit, eclipse and phase curve observations from ground and space-based instruments. Such simulation can capture temporal effects, such as correlated noise and systematics on the light curve. The simulator will produce spectral images like those produced by an actual observation. ExoSim 2 (`pip install exosim` for the public version) has been developed for the Ariel Space Mission, to assess the impact of astronomical and instrumental systematic on astrophysical measurement, and to prepare the data reduction pipeline against realistic data sets.

### Alfnoor
<img src="https://abocchieri.github.io/assets/img/alfnoor.png" alt="alfnoor" width="20%" height="20%" style="float:left; padding-right:30px">
Alfnoor (The Thousand Light Simulator) is a project aiming to expand the capabilities of TauREx to populations of atmospheres. It is used in the context of Ariel to simulate the mission performances (see [here](https://arxiv.org/abs/2003.01839), [here](https://arxiv.org/abs/2110.00503), and [here](https://arxiv.org/abs/2309.06817)), and on real data to perform large scale population analyses (see [here](https://arxiv.org/abs/2204.11729)).

### TauREx-emcee

<img src="https://abocchieri.github.io/assets/img/taurex-emcee_logo.png" alt="taurex-emcee" width="20%" height="20%" style="float:left; padding-right:30px">
[taurex-emcee](https://taurex-emcee.readthedocs.io/en/latest/) is a plugin for the [TauREx 3.1](https://taurex3-public.readthedocs.io/en/latest/) atmospheric retrieval framework that extends the choice of sampling methods available to the user. The plugin provides an interface to the [emcee](https://emcee.readthedocs.io/en/stable/) sampler, a popular affine-invariant ensemble sampler widely used in the astronomy community. Running the sampler to convergence is automated through the autoemcee package, which also supports MPI parallelization. Thus, the taurex-emcee plugin allows users to easily launch parallelized retrievals of atmospheric spectra with emcee. This enables reliable, efficient, and fast retrievals. taurex-emcee is publicly available on [GitHub](https://github.com/ExObsSim/taurex-emcee) and [PyPI](https://pypi.org/project/taurex-emcee/), so you can install it with a simple `pip install taurex-emcee`.

## Exoplanet Missions

### Ariel

<img src="https://abocchieri.github.io/assets/img/ariel_preview.png" alt="ariel" width="20%" height="20%" style="float:left; padding-right:30px">
[ESA Ariel](https://arielmission.space/) (Atmospheric Remote-sensing Exoplanet Large-survey) is the M4 space mission of the European Space Agency. Ariel will launch in 2029 and will conduct the first spectroscopic survey of the atmospheres of about 1000 diverse exoplanets. With its 1-m class telescope and simultaneous spectral coverage from the visible to the infrared, Ariel will address questions such as: "What are the physical processes shaping planetary atmospheres?", "What are exoplanets made of?" and "How do planets and planetary systems form and evolve?". As chair of several working groups, including the Ariel M1 and Telescope Assembly Tiger Team, the Ariel Simulators Software, Management and Documentation (S2MD) Working Group, and the Ariel Science Brainstorms Working Group, I am fully committed to the success of this mission.

### EXCITE

<img src="https://abocchieri.github.io/assets/img/excite.jpeg" alt="excite" width="20%" height="20%" style="float:left;padding-right:30px">
[EXCITE](https://www.spiedigitallibrary.org/conference-proceedings-of-spie/12184/2629373/The-EXoplanet-Climate-Infrared-TElescope-EXCITE/10.1117/12.2629373.short?SSO=1) (The EXoplanet Climate Infrared TElescope) is a balloon-borne spectrograph that will address the limitations of current space-based NIR observatories while complementing and maximizing the science return of JWST. With a 0.5 m-class telescope, EXCITE will measure spectroscopic phase curves of bright, short-period extrasolar giant planets (EGPs, or "hot Jupiters") across the 1–4 micron range. EXCITE will continuously observe each target for a full orbital period and use the resulting phase-resolved spectroscopy to map the planet’s temperature profile and chemical composition as a function of longitude. I am member of the EXCITE team and the EXCITE data analysis working group, where I help evaluate the performance of the mission and the target selection strategy.

## Data Challenges

### Ariel Data Challenge

<img src="https://abocchieri.github.io/assets/img/ArielDataChallengeLogo.png" alt="ariel_data_challenge" width="20%" height="20%" style="float:left; padding-right:30px">
The [Ariel Data Challenge](https://www.ariel-datachallenge.space/) calls on the AI global community to investigate solutions for the interpretation and analysis of exoplanet data. After the success of the previous editions (see [here](https://browse.arxiv.org/abs/2010.15996) and [here](https://browse.arxiv.org/abs/2206.14642)), NeurIPS - Ariel Data Challenge 2024 has recently launched on [Kaggle](https://www.kaggle.com/competitions/ariel-data-challenge-2024/overview). I am a key member of the Ariel Data Challenge 2024 team: I helped to develop the challenge, the data to be used, and the state-of-the-art pipeline to benchmark the results of the participants.

## Community Outreach

### ExoClock

<img src="https://abocchieri.github.io/assets/img/exoclock_ariel.png" alt="exoclock" width="20%" height="20%" style="float:left; padding-right:30px">
[ExoClock](http://exoclock.space/) is a very successful international program initiated and managed by Anastasia Kokori (UCL-CSED) and Angelos Tsiaras (Aristotle University of Thessaloniki) to monitor transiting exoplanets in order to keep their ephemerides up-to-date ([ExoClock catalogue](https://www.exoclock.space/database/planets)). With ExoClock, everyone can contribute to real research and become part of a bigger project, such as a space mission. The ExoClock team have created special tools and educational guides for any observatory that would like to support Ariel and exoplanet science. Everyone with some basic equipment, including a telescope and a CCD camera, can participate in the effort of monitoring the planets’ host stars. Read the latest publications of the ExoClock team: [here](https://arxiv.org/abs/2012.07478), [here](https://arxiv.org/abs/2110.13863), and [here](https://arxiv.org/abs/2209.09673). I am the italian contact person for ExoClock, so for any question or doubt, please contact me!
