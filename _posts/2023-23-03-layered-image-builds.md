---
layout: post
title:  "JupyterHub Image Building: A Layered Approach for Customized User Environments"
date:   2023-23-03
categories: stackoverflow jupyterhub docker github-actions github build images configmap k8s jupyter profiles
---

I gave [this answer](https://stackoverflow.com/a/77595025/11422992) about image management for JupyterHub on StackOverflow today and thought I'd turn it into a short blog post, with some code thrown in.

<!--more-->

When things get complex, it is not feasible to have only one image for all users. JupyterHub allows you to specify profiles with different images that a user can choose from when logging in. Check the Customizing User Environment section in the Jupyterhub documentation for that. Here is an example that uses an additional `ConfigMap` in K8s to store profile information:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: jhub-profiles
  namespace: jhub
data:
  values.yaml: |
    singleuser:
      profileList:
        - display_name: "Default environment"
          description: "some description"
          default: True
        - display_name: "Default environment - python 3.8"
          description: "Same environment as the default but ..."
          kubespawner_override:
            image: <custom-image>
```

You can the use e.g. Flux to merge the above when applied:

```yaml
  valuesFrom:
    - kind: ConfigMap
      name: jhub-profiles
      valuesKey: values.yaml
```

One approach to building these images is in layers, starting with a base layer that contains only a base OS image and some configurations and installations that apply to all other images, for example, ca-certificates or tools like wget.

So if you already have an image that contains everything, it might be a good idea to reverse engineer it into several layers of images that you can then stack on top of each other to create specific user images without dependency conflicts. Look at how the [Jupyter community itself manages its images](https://github.com/jupyter/docker-stacks), or [this project](https://github.com/vre-hub/environments) I've been personally involved in. Both use GitHub Actions to build and publish their images and in the second project I linked, we also involved users on GitHub to suggest and implement features in the images.

Depending on how complex you want to get with this process, you can also work with variables, where you specify different versions or packages when building the images. Jinja is also useful in this context as a templating engine.
