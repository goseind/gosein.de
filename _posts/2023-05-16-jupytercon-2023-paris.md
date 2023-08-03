---
layout: post
title:  "Insights from JupyterCon 2023 in Paris"
date:   2023-05-16
categories: openscience datascience jupyter jupyternotebooks jupyterhub jupyterlab paris tech event community cern eosc
---

***This articel has also appeared in the [CERN Computing Blog](https://computing-blog.web.cern.ch/2023/05/insights-from-jupytercon-2023-in-paris/)***

Project Jupyter and IPython have no doubt revolutionized the way scientific computing is done. Today CERN and many other major organisations around the globe are using them to do their research. This article gives you the newest insights into Project Jupyter from its biggest community gathering in 2023 in Paris - the JupyterCon!

![](/assets/2023-05-16-jupytercon-2023-paris/JupyterCon2023_Talk.jpeg)

Project Jupyter is a large open-source project born out of the IPython project for interactive data science and scientific computing with an international community. Jupyter products are used by many large institutes and corporations like NYU, MIT, Harvard, Berkeley, NASA, Netflix, AWS, and Google (and of course CERN) for their scientific and educational needs as well as many science experiments from various disciplines across the globe. Since its start in 2014, it has grown bigger by the year and revolutionized the way science is done.

![](/assets/2023-05-16-jupytercon-2023-paris/labpreview.jpg)

*Source: https://jupyter.org/, 05/2023*

At JupyterCon this international community comes together once a year to present and discuss their advancements, use cases and products in and around the expanding Jupyter Ecosystem. The experience is enriched by insightful keynotes, in-depth training and networking events. The conference had several tracks ranging from Community Tools and Practices to Enterprise Jupyter Infrastructure, Data Science, Education and Research and Scientific Computing. This year's conference took place at Cité des sciences et de l’industrie - an amazing venue in Paris - with around 300 attendees.

![](/assets/2023-05-16-jupytercon-2023-paris/JupyterCon2023_Venue.jpeg)

Throughout the conference, there were many opportunities to network with other attendees, many of whom are facing the same challenges in their domains as we at CERN. Open science, data preservation and reproducibility are pressing topics in science and the conference provided a good opportunity to discuss these with researchers from MIT,  NASA, the Memorial University of Newfoundland working with NASA TOPS on open science as well as the Leibniz Institute for Information Infrastructure in Germany.
Apart from the above, there were many more fruitful conversations with maintainers, admins and users of the Jupyter Ecosystem.

The conference was packed with interesting talks, here are just some of the key takeaways I found particularly interesting also in the context of CERN:
The new major release of the JupyterHub subproject brings with itself:
* Advanced testing against K8s with CI/CD workflows
    * Ready to use Grafana Dashboards for monitoring of Jupyter services
    * Advances build in user management with groups for e. g. resources limitation
    * Real-time collaboration on Jupyter Notebooks
    * pytest-jupyterhub package for lightweight local JupyterHub development
* Paul Romer, a University Professor at NYU and co-recipient of the 2018 Nobel Prize in Economics Sciences talked in his keynote about scientific publications, reproducibility and signing and preservation of Jupyter Notebooks. [Link to this talk](https://cfp.jupytercon.com/2023/talk/T8HH8K/)
* 2i2c is a non-profit organisation which designs, develops, and operates JupyterHubs in the cloud for communities of practice in research & education. It builds and supports open-source infrastructure that serves these communities and has a lot of experience and infrastructure code to discuss and explore. [Link to their GitHub Orga](https://github.com/2i2c-org) as well as their [talk](https://cfp.jupytercon.com/2023/talk/GESWGK/)
* Alyssa Goodman who is the Robert Wheeler Willson Professor of Applied Astronomy at Harvard University and a Research Associate at the Smithsonian Institution talked in her keynote about glue, a package which combines data, graphs and tools for the context of astronomy and other fields, with an upcoming package for Jupyter called glupyter. [Find more info here](https://scholar.harvard.edu/agoodman/presentations/seeing-universe-more-clearly-glue-glupyter)
* Github presented its integration of Jupyter into GitHub Codespaces as a native environment for users to open notebooks with, Jupyter has its own sponsored service for that called BinderHub, more info can be found [here](https://docs.google.com/presentation/d/e/2PACX-1vShtgk4eq1FmGvrAjaCNI72QJXCwYAXzJsEE0FTlE3s-vGrV_mCJzTLCk20KvkFc6yVD54CHBqGBDcH/pub?start=false&loop=false&delayms=5000#slide=id.geb5b66dfde_2_11) and [here](https://jupyter.org/binder)
* New JupyterLite, a Jupyter Notebook running entirely in the browser, making it easier to set up and share notebooks and comes with a dedicated repository spawner called repo2jupyterlite. More info can be found [here](https://jupyterlite.readthedocs.io/en/latest/)
* Sliver is a hacking tool to explore vulnerabilities in software systems and a talk at the conference gave a brief introduction to it, more details [here](https://github.com/BishopFox/sliver)
* A Dask tutorial for distributed computing can be found [here](https://github.com/mrocklin/dask-tutorial)
Real-time collaboration for Jupyter users, more info [here](https://jupyterhub.readthedocs.io/en/stable/tutorial/collaboration-users.html)
* Effective Practices for Community Management from an experienced user experience researcher for sustainable software. Her talk can be found [here](https://cfp.jupytercon.com/2023/talk/MZPLPL/)

These are just an excerpt of the many presentations, lightning talks and poster sessions given, the full schedule and soon the recording should be available on the JupyterCon website [here](https://cfp.jupytercon.com/2023/schedule/)

During the conference I had the chance to present our project, the Virtual Research Environment/Data Lake, a European effort under the EOSC Future project with a team of maintainers here at CERN.

<iframe width="560" height="315" src="https://www.youtube.com/embed/JGQpdivaNtI?start=1030" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Additionally, development sprints took place where developers and other project members could sit together to solve issues and think about new features and general ideas on how to move the project forward. I attended one of these sprints to help resolve issues and get help and feedback on some of the issues we have in our project.
These sprints are super useful and are the perfect place to have in-depth technical discussions and find solutions for problems, but also to connect to the community.

![](/assets/2023-05-16-jupytercon-2023-paris/JupyterCon2023_Sprint.jpg)

CERN with the EOSC Future project is a big user and beneficiary of the IPython and Jupyter products and is planning to get more involved with the upstream project. In case you are interested in collaborating or simply exploring ideas feel free to reach out to me!
