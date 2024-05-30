# Instructor instructions

## Preparation instructions

1. Get an account on https://github.com
2. Make sure you have the git version control system setup:
   https://help.github.com/en/github/getting-started-with-github/set-up-git
3. Clone the repository ``git@github.com:jhale/computational-workflows.git``.
4. Install Docker on your computer
   https://www.docker.com/products/docker-desktop or
   https://docs.docker.com/install/
5. Please read the paper
   https://journals.plos.org/ploscompbiol/article/file?id=10.1371/journal.pcbi.1005510&type=printable

## Technical setup

1. Switch to ``/bin/bash`` shell.
2. Run ``swc-shell-split-window``.
3. Instruct students to go to: https://pad.carpentries.org/comp-workflows
4. Print copies of paper and git cheat sheet
   https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf

## Overview

* 1 hour: Discussion. "Good enough practices in scientific computing"
* ~2 hours: Docker. *Specifying and running software anywhere*.
* ~1 hours: Unit testing with Python and pytest.
* ~1 hours: Continuous integration on github.

* We will learn together by *doing* things.
* We will use a top-down learning style (broad picture first, details last).
* There will be time for questions at all difficulty levels.

## Good enough practices in scientific computing

https://journals.plos.org/ploscompbiol/article/file?id=10.1371/journal.pcbi.1005510&type=printable

# Points to discuss amongst the group

1. Box 1. As an individual, within each section (1-6), tick the practices that
   you already undertake in your scientific computing work.

2. Box 1. In a group three, within each section (1-6), rank (1 through N, most
   to least important) each practice. If you do not know anything about a
   practice, put a star next to it. Justify your decisions.

3. Scenario 1: You are working on a small software project that will only be
   used by you to produce the results for one paper. Which of the practices
   will you *not* use?

4. Scenario 2: You are working on a large software project that will be used by
   many people and will exist beyond your tenure in the laboratory. Which of
   the best practices will you *not* use?

3. Question: Does your research group have any guidelines on scientific
   software good practice?

5. Question: What is the difference between *scientific software good
   practice*, *open science* and *reproducible computing science*?

# Docker

Reference material:

* The docker cheat sheet: https://github.com/wsargent/docker-cheat-sheet
* https://ieeexplore.ieee.org/document/7933304 Containers for Portable,
  Productive, and Performant Scientific Computing (Hale et al.)
* https://docs.docker.com/
* https://fenics.readthedocs.io/projects/containers/en/latest/

Points to cover:

* What is Docker?
  * Docker is a software tool designed to make it easy to create, deploy and
    run applications by using containers.
* What is a container.
  * Package up an application and all of the parts it needs (libraries,
    configuration) into a *single* package.
* Ensures that the application runs *identically* on any machine, regardless of
  any customisations on that machine.

* What *problems* does Docker solve *in a scientific computing context*?
  * *Portability, collaboration.*
    * My colleague wants to run my code, but:
      * he is running a different operating system.
      * she has installed a different version of software package x.
  * *Reproducibility.*
    * I want to publish my code and software so that someone else can run them.
    * I want to be able to reproduce my results in 5 years.
  * *High-performance computers (HPC)*
    * I won't cover this in this course, see Hale et al. and
      https://sylabs.io/docs/

## Installing Docker

Docker is available for Linux, Mac OS and Windows.

https://www.docker.com/products/docker-desktop
https://docs.docker.com/install/linux/docker-ce/ubuntu/
https://docs.docker.com/install/linux/docker-ce/debian/
https://docs.docker.com/install/linux/docker-ce/fedora/

## Testing Docker for the first time

    docker run hello-world

## Getting a bit more interesting

    docker run -it ubuntu bash

* `docker` the docker *client*.
* `run` Run a *container*.
* `-it`: Interactive (e.g. terminal) session.
* `ubuntu`: Name of the image (https://hub.docker.com/).
* `bash`: Program to execute inside the container.

    exit

## Exercises

1. Run the executable `python` within the `python` image.

        docker run -ti <> <>

   To exit:

       >>> exit()

2. Tags. Run the executable `python` within the `python:3.8` image.

        docker run -ti <> <>

   To exit:

       >>> exit()

## Sharing files from the host (e.g. your laptop) to the container

* Key point: By default, Docker containers are *completely isolated* from your
own system.
* You must *manually* specify which directories you want to share
into a container.

    docker run -ti -v $(pwd):/root/shared ubuntu bash

Inside the running container:

    # cd /root/shared
    # ls
    # exit

* `-v /host/path:/container/path` share the path at `/host/path` on the host,
to the path `/container/path` inside the container.

## Exercise: Running a `python` script

There is a python script at ``python/hello_world.py``. Modify it to read:

```
print("Hello world!")
```

Make sure you are in the root of the git repository.

```
# pwd
/Users/jack.hale/code/computational-workflows
```

Now run the script using a Docker container:

```
docker run -ti -v $(pwd):/root/shared <> "<> <>"
```

* <> Image name.
* <> Command to run in container.
* <> Path (container!) for the Python script to run.

## Exercise: `bash` then `python`

Instead, run ``bash`` inside a container and then run ``python
/root/shared/python/hello_world.py``.

## Making your own Docker image

Full documentation: https://docs.docker.com/engine/reference/builder/

Docker can build *new* images by writing a `Dockerfile`.

A `Dockerfile` contains a list of instructions (recipe) explaing how the image
should be built.

A very simple `Dockerfile` can be found at `docker/example1/Dockerfile`.

# Exercise: Building your own image

Open `docker/example1/Dockerfile`. What does it do?

You can *build* a docker image following the recipe in the `Dockerfile` by
running:

```
cd docker/example1/Dockerfile
docker build .
```

You should see some output like:

```
Successfully built e97420b64647
```

This unique *hash* can be used to run the image.

```
docker run -ti e97420b64647 ipython
```

Note: You need to substitute *your* unique hash.

## Exercise: Changing the default command

Try running:

```
docker run -ti e97420b64647
```

What happens?

We can change the default command to `ipython` using the `CMD` instruction at
the bottom of the `Dockerfile`.

Try adding:

```
CMD ["ipython"]
```

At the bottom of the `docker/example1/Dockerfile` and rebuilding. Then `docker
run` the new container. What do you see?

## Exercise: Tagging an image

*Tagging* (e.g. naming) an image gives us an easier way to refer it to than the
*hash* (e.g. e97420b64647).

```
cd docker/example1/Dockerfile
docker build --help
docker build -t ipython .
docker run -it <>
```

## Exercise: Sharing images with a colleague.

Scenarios:
* You want to share an image with your colleague.
* You want to release the software environment you used for the results in a
  paper as supplementary material.

Please read the documentation here:
https://docs.docker.com/engine/reference/commandline/save/ and construct 
a command that saves the image `ipython` to a file `ipython-image.tar.gz`.

Take a look at https://docs.docker.com/engine/reference/commandline/load/
and find how to load a `*.tar.gz` file back into Docker.

## Extended exercise.

Write your answer in the `docker/exercise1` directory.

A more convienient way to store Docker images is on a *Docker registry*.  In
this exercise we will *push* (like `git`) to the Docker Hub
(https://hub.docker.com) and your colleague will *pull* (like `git`) your image
to his/her computer.

1. Sign up for an account at https://hub.docker.com.
2. You need to login using:

    docker login

3. Write a new `Dockerfile` to build an image tagged with `pytest` and `numpy`
   installed (via `pip`). `pytest` is a Python package for writing unit tests
   (more later).

```
FROM python:3.12

RUN pip install numpy pytest
```

4. Build the image and tag it `<yourdockerhubusername>/pytest`.
5. Push the image with tag `<yourdockerhubusername>/pytest` to the Docker Hub
   (https://docs.docker.com/engine/reference/commandline/push/).

## Dissecting a more complex `Dockerfile`

https://github.com/FEniCS/dolfinx/blob/master/Dockerfile

## 15 mins: Ask me anything (about Docker).

## Break.

# Unit testing and Test Driven Development

## References

https://en.wikipedia.org/wiki/Test-driven_development

* Question: What is testing?

* Unit testing: Running *experiments* on classes and methods in your
  program and confirming that the code behaves in the expected way.

## Easier to see than to explain.

Go to the directory `python/unit_testing/`.

1. There is a file `add.py` with a single function. What does the function do?
2. There is a file `test_add.py` with a single function. What does the function
   do?
3. Run a docker container using the image e.g. `jhale/pytest` with the
   directory `computational-workflows` shared into the container at
   `/root/shared`.
4. Inside the container, go to the directory
   `/root/shared/python/unit_testing`.
5. Run the program `py.test`. What happens?

## Exercise

1. Add a new method `subtract` in `subtract.py` that takes two arguments `a`
   and `b` and subtracts `b` from `a`. 

2. Write two tests in `test_subtract.py` your function.

3. Run the tests using `py.test`.

## Parameterising tests

It is useful to run parameterise tests, i.e. run the same test over multiple
values of inputs. This can be done easily using the `@pytest.mark.parametrize`
*decorator*.

https://docs.pytest.org/en/latest/parametrize.html

1. Modify the `test_add.py` file as follows:

```
import pytest
from add import add

@pytest.mark.parametrize("a,b,result", [(3, 5, 8), (2, 4, 6), (-3, -4, -7)])
def test_add(a, b, result):
    c = add(a, b)
    assert(c == result)
```

2. Run `py.test -v` inside the container. What do you see?

## Exercise: `pytest.mark.parametrize`

Parametrize the `test_subtract` test.

## Expected failures.

Sometimes it is useful to test whether our function *fails* in the right way.

For example, our `subtract` function should `raise` an `Exception` if we try
and subtract a string from an integer.

Add the following test to the bottom of `test_subtract.py`.
```
import pytest

def test_subtract_type_error():
    with pytest.raises(TypeError):
        c = subtract(4, "b")
```

## Marking slow running tests

Particularly in scientific codes, certain tests can take a long time to execute.

A key aspect of unit testing is that the tests are run frequently (on every git
commit). Having slow tests can break this cycle. Either you commit less often,
or you do not run the tests often.

It is possible to mark certain tests as being slow, and therefore automatically
skip them.

Try making a long running function called `slow()` using the Python `time.sleep()`.
Write a test called `test_slow`. Use the decorator `@pytest.mark.slow` to mark
the test as slow. To run the slow test:

    py.test -v -k "slow" .

## Expected failures

Sometimes it is simply not possible to get a piece of code working for all possible
inputs. Perhaps it contains a bug, or perhaps it is not clear why it fails.

In this case, it can be useful to write a test *that is expected to fail*. Why?

Write a test in `python/unit_testing/basic/test_add.py` that is expected to
fail.  Mark that it is expected to fail using the `@pytest.mark.xfail`
decorator.

## Extended exercise: Triangle

1. In the file `python/unit_testing/triangle/triangle.py` there is a function that
   takes in the length of the two shorter sides of a right angled triangle
   and computes the three angles of the corresponding right angled triangle.

2. What tests could we use to verify the correctness of our function?

* Angles add to 180 degrees.
* Isoceles triangle.
* One angle is always 90 degrees. If not, the program should throw an Exception.
* 30-60-90 triangle.
* Small angle approximation

3. Write some tests. Discuss floating point comparisons. Bonus points for using
   `pytest.mark.parametrize` on one test.

## Extended exercise: Wallet

1. In the directory `python/unit_testing/wallet` there is a complete set of tests
   in `test_wallet.py` for a program `wallet.py`. Get the tests to pass by finishing
   the code in `wallet.py`.

2. Add a method `add_interest` to the class `Wallet` which adds compound interest to the
   wallet at a certain rate and over a certain period.  Write a test for the
   new functionality.

```
# test_wallet.py

import pytest
from wallet import Wallet, InsufficientAmount

def test_default_initial_amount():
    wallet = Wallet()
    assert wallet.balance == 0

def test_setting_initial_amount():
    wallet = Wallet(100)
    assert wallet.balance == 100

def test_wallet_add_cash():
    wallet = Wallet(10)
    wallet.add_cash(90)
    assert wallet.balance == 100

def test_wallet_spend_cash():
    wallet = Wallet(20)
    wallet.spend_cash(10)
    assert wallet.balance == 10

def test_wallet_spend_cash_raises_exception_on_insufficient_amount():
    wallet = Wallet()
    with pytest.raises(InsufficientAmount):
        wallet.spend_cash(100)
```

and then:

```
# wallet.py

class InsufficientAmount(Exception):
    pass


class Wallet(object):
    def __init__(self, initial_amount=0):
        raise NotImplementedError

    def spend_cash(self, amount):
        raise NotImplementedError

    def add_cash(self, amount):
        raise NotImplementedError
```

## Ask me anything (about unit testing)

# Putting it all together (github + docker container + unit testing + running on circleci)

1. Create new repository on github called `triangle`.
2. `git clone` it.
2. Copy across `docker/Dockerfile` (based on python image with pip sympy and numpy)
3. `git add`, `git commit` and `git push` the `Dockerfile`.
3. Build the Docker image. Push it to the dockerhub with tag `<username>/triangle`.
6. Create the file `.github/workflows/test.yml`, and push it up to github.
8. Github. Branches > Branch Protection Rules.
8. Create a branch in the git repository. Add a new test.
9. Make a new pull request.
10. The pull request can only be merged when the tests pass.

```
name: triangle CI
on: [push]
jobs:
  tests:
    runs-on: ubuntu-20.04
    container: <username>/triangle:latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run tests
        run: |
          python3 -m pytest
```
