# Coursework 2

## Practicalities

Submission by email to [jack.hale@uni.lu](mailto:jack.hale@uni.lu), Subject:
Sustainable Scientific Software Coursework 2.

Please ask questions on the course Moodle.

*Deadline: 2nd July end of day. No extensions.*

The email should contain a link to a GitHub repository (shared with me `jhale`
and `mail@jackhale.co.uk`) with:

* a modified version of this file `coursework2.md` file containing the specific
  commands that you used to complete the exercises. You do not have to include
  everything, just the key commands. I have left placeholders where these
  commands should go:

    ```
    # Add your commands here
    ```
* A `wallet.py` file that passes the unit tests.
* A `Dockerfile`.
* A working GitHub Actions file `.github/workflows/test.yml` that runs the unit
  tests inside the built image.

## High level overview

At the end of this homework, you will have a computational workflow consisting
of:

1. A git repository hosted at https://github.com.

1. A `Dockerfile` describing an environment containing `python` and the Python module
   `pytest`.

2. A Docker image built from the `Dockerfile` and uploaded to the Dockerhub.

3. A simple Python program describing a simple Wallet class and a set of unit tests.

4. A continuous integration setup using GitHub Actions.

For reference, you can use my notes that I used during the course
[here](../../notes/README_instructor.md). 

## Setting up a git version control repository

1. Go to [GitHub](https://github.com). Click on the plus icon in the top right
   hand corner of the screen. Then click on New Repository.

2. Create a *private* repository called e.g.
   `jhale/sustainable-scientific-software-homework`.

3. Clone your repository. In a terminal run, e.g.:

       git clone git@github.com:jhale/sustainable-scientific-software-homework.git

   substituting with the correct location of your repository.

4. Copy this `coursework2.md` text file to your new repository (download from
   [here](https://raw.githubusercontent.com/jhale/sustainable-scientific-software/main/courseworks/coursework2/coursework2.md).
   `git add`, `git commit` and `git push` it to your repository.

## Dockerfile

1. Create a file `Dockerfile` in the repository containing the following text.

```
FROM python:3.12

RUN apt-get update && apt-get install git
RUN pip install pytest numpy

CMD ["/bin/bash", "-l"]
```

2. `git add` and `git push` the file `Dockerfile` to the repository.

```
# Add your commands here
```

## Build and push Docker image

1. `docker build`, `docker login` and `docker push` the image described by the
   `Dockerfile` to the Dockerhub. Tag your Docker image
   `<yourdockerhubusername>/sss.

```
# Add your commands here
```

## Run a container, and share in files from the host.

1. `docker run` the image. Use the `-v` flag to share the root of the git
   repository to `/root/shared` inside the container. Use the `-ti` flag to get
   an interactive prompt inside the running container.

```
# Add your commands here
```

## Setup a simple Python test suite

1. In a terminal running on the host (outside the container), copy across the
   files ``wallet.py`` and
   ``test_wallet.py`` to the root of your homework
   repository.  ``git add``, ``git commit`` and ``git push`` them.

```
# Add your commands here
```

2. Start a Docker container using your image and share your repository into a
   directory `/root/shared` into the container.

```
# Add your commands here
```
3. Run the tests inside the container by going to `/root/shared` and running the
   command `py.test`. The tests should fail.

3. In a terminal on the host modify ``wallet.py`` until the tests in
   ``test_wallet.py`` all pass.

4. ``git add``, ``git commit`` and ``git push`` the working ``wallet.py`` file.

## GitHub Actions for Continuous Integration

1. Using the example in the class notes make a `.github/workflows/test.yml`
   file that checks out your repository and runs the unit tests inside the
   Docker image that you pushed to the DockerHub.

3. Push the `.github/workflows/test.yml` file to GitHub. Check that you get the
   green tick showing that your tests pass.

## Submission

1. Ensure that all of your work is pushed to the GitHub repository.

2. Share the repository with me
