---
layout: post
title:  "TIL: Automated Python Script Reloading with Watchdog"
date:   2023-11-28
categories: python watchdog watchmedo pip docker compose bash file system update
---

[What is a TIL?](2023-11-28-til-today-i-learned.md)

Today I learnt about [watchdog](https://pypi.org/project/watchdog/), a Python utility that allows you to monitor file system events, useful for example if you want to restart a script when changes are made to it. I used it with Docker Compose to run a Python program every time I changed the file.

Create a small bash script to run *watchmedo*, specifying the directory, pattern or single file you want to watch (in this case using the `$1` variable to pass the specific file from the Docker Compose later):

```bash
#!/bin/bash
echo "Starting '$1' with auto-restart on file change ..."
watchmedo auto-restart --directory=./ --pattern=*.py --recursive -- python3 $1
```

Add the file and pip package to your *Dockerfile* (make sure it has the right permissions):

```bash
# install pip packages
RUN pip install watchdog
# setup scripts and entrypoint
COPY ./watchdog.sh /
RUN chmod +x watchdog.sh
```

In your Docker Compose, you can now run the script and specify a file to track:

```yaml
services:
  svc:
    build: .
    command: /watchdog.sh test.py # replace this with any file you want to update and restart
```

That's it!
