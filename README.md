# Docker Learning

Docker Learning Course

Duration: Two weeks, three days per each week, 1 hour per day

## Week 1

Learning Docker

### Day 1

Inception with Docker basics

- Container execution and deatached execution
- Images
  - Layers
  - Tags
  - Docker public registry
- Builds
  - Dockerfile
  - Build, commit, push
- CMD and ENTRYPOINT differences
- Pets vs Cattle
- Docker Development Flow

Reference repositories:

- Daniele Siddi's GitHub - [Curso de introducción a Docker](https://github.com/danielesiddi/docker-course)
- CMD vs ENTRYPOINT in [Stackoverflow](https://stackoverflow.com/questions/21553353/what-is-the-difference-between-cmd-and-entrypoint-in-a-dockerfile)
- Brian DeHamer - Dockerfile [ADD vs COPY](https://www.ctl.io/developers/blog/post/dockerfile-add-vs-copy/)

### Day 2

Docker Elements

- Containers
- Images
- Networks
- Volumes

Refertence repositories:

- Red Panda CI's GitHub - [Jenkins Workshop](https://github.com/red-panda-ci/jenkins-workshop)
- Moncho's Github - [DRY](https://github.com/moncho/dry) (container management CLI)

### Day 3

Docker Compose

- Docker compose file
- Parameters with .env
- Platform definition and usage
- Real examples with live projects

Reference repositories:

- Sergio Ortega Gome's GitHub - [Symfony 4 App Example](https://github.com/sergioortegagomez/red-panda-ci-symfony)

External resources:

- Dockerfile linter resource - [FROM:latest](https://www.fromlatest.io)

## Week 2

Using Docker

### Day 4

Docker compose in real projects

Reference repositories: none that we can share :)

### Day 5

Choose one of the following options

#### Docker in depth

- Docker in Docker
  - Ubuntu DIND
  - Official Docker in Docker
- Quality pieces with Docker
  - Jenkins in Docker
  - Sonar with real live project
  - Zalenium: Selenium Grid with steroids in Docker
- Windows in Docker

Reference repositories:

- Jerôme Petazzoni - [Using Docker in Docker](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/)
- Red Panda CI's GitHub
  - [Ubuntu DIND](https://github.com/red-panda-ci/ubuntu-dind)
- Ayuda Digital
  - [Jenkins DIND](https://github.com/ayudadigital/jenkins-dind)
  - [Jenkins Pipeline Library](https://github.com/ayudadigital/jenkins-pipeline-library)
- Informática Parmadux's GitHub - [Odoo](https://github.com/informaticaph/PXGO_00064_2014_PHA)
- Docker - [Docker in Docker](https://hub.docker.com/_/docker)
- Zalando IT's GitHub - [Zalenium](https://github.com/zalando/zalenium)
- Aerokube's GitHub - [Windows Images](https://github.com/aerokube/windows-images)

#### Dockerize an application

Put Docker on the [Spring Boot Angular4 Heroes](https://github.com/gaoxinwen/spring-boot-angular4-heroes) sample application.

Stages:

1. Follow the instructions of the project and install on your workstation or laptop all the stuff to execute the application.
2. Build a docker-compose platform in two services: database and application.
3. Compare with the [Kairops Fork](https://github.com/kairops/spring-boot-angular4-heroes/tree/feature/dockernice) of the project and the solution proposed here. Don't cheat and look only when you are finished the sedond stage.

Tips:

- Use MySQL 5.x or MariaDB 5.x versions for the database service.
- Use debian based image for the application service.
- Add `--disable-host-check` and `--host 0.0.0.0` parameters for the `ng start` command.

Top goal:

- Try to split the application in two Docker services: backend and frontend.

### Day 6

Choose one of the following options

#### BDD with Docker

Use the project [Duing](https://github.com/ayudadigital/docker-ubuntu-xrdp-mate-custom/tree/master/duing) to dessign and execute a BDD test

Demonstrate how we can use Docker to run a complete linux Desktop environment with all the tools needed to materialize complex tasks.

Reference repositories:

- Ayuda Digital's GitHub - [Docker Ubuntu INstant Gherkin](https://github.com/ayudadigital/docker-ubuntu-xrdp-mate-custom/tree/master/duing)
- Sergio Ortega Gomez's GitHub - [Docker Ubuntu 18.10 Mate Desktop](https://github.com/sergioortegagomez/docker-ubuntu-cucumber)

#### Kubernetes Inception

Take a look at Kubernetes using local minikube and this [Docker Compose Example](https://github.com/aperaltacalvo/docker-compose-example) project.

Thanks to [Álvaro](https://github.com/aperaltacalvo) for this work.

## Further reading

- [General Concepts](https://github.com/ayudadigital/general-concepts/blob/master/es/toc.md) about Software Development (Spanish)
- [Play with Docker](https://training.play-with-docker.com)
- Learning Docker with [Katacoda](https://www.katacoda.com/courses/docker) courses

## Stuff Intallation

You can download the whole Docker images and clone the whole referenced repositories with a script within the project.

Execute the command:

```console
devcontrol/actions/install-stuff.sh exec
```

...or use `devcontrol install-stuff` if you have [Devcontrol](https://github.com/ayudadigital/devcontrol) installed on your system) and wait for the script to finish.

## BDD examples

Find in the `bdd-examples` directory of the repository some examples of BDD test created as part of the day 6 session.

To execute the tests:

1. Start a `duing` container with the repository shared as volume within it.

    ```console
    docker run --name duing \
              -p 3389:3389 \
              --shm-size 1g \
              -dit --restart unless-stopped \
              -v $(pwd):/opt/docker-learning \
              ayudadigital/duing
    ```

2. Copy the `/opt/docker-learning/bdd-examples/test/` directory in `/opt/duingdemo/ci-scripts/test/`. This will add the examples to the `features` directory.

    ```console
    $ docker exec -u ubuntu duing cp -rv /opt/docker-learning/bdd-examples/test/ /opt/duingdemo/ci-scripts/
    '/opt/docker-learning/bdd-examples/test/cucumber/features/step_definitions/bddfire_steps.rb' -> '/opt/duingdemo/ci-scripts/test/cucumber/features/step_definitions/bddfire_steps.rb'
    '/opt/docker-learning/bdd-examples/test/cucumber/features/github_search.feature' -> '/opt/duingdemo/ci-scripts/test/cucumber/features/github_search.feature'
    ```

3. Follow the instructions of the [Duing](https://github.com/ayudadigital/docker-ubuntu-xrdp-mate-custom/tree/master/duing) project.

    Take care that github can't be tested with PhantomJS, so the `rake poltergeist` command will end with an error.

    ```console
    ubuntu@7a0e1ec3060f:/opt/duingdemo/ci-scripts/test/cucumber$ rake poltergeist
    /usr/bin/ruby2.5 -S bundle exec cucumber features -p poltergeist --format pretty --profile html -t ~@api
    {"baseurl"=>"https://www.google.es", "take_screenshots"=>false, "screenshot_delay"=>1, "browser_width"=>1024}
    Using the poltergeist and html profiles...
    Feature: Google Search to explore BDDfire

      Scenario: View home page                      # features/bddfire.feature:4
        Given I am on "http://www.google.com?hl=en" # bddfire-3.0.2/lib/bddfire/web/web_steps.rb:2
        When I fill in "q" with the text "bddfire"  # bddfire-3.0.2/lib/bddfire/web/web_steps.rb:6
        Then I should see "Sign in"                 # bddfire-3.0.2/lib/bddfire/web/web_steps.rb:10

    Feature: Github search for a library

      Scenario: Search library within github                # features/github_search.feature:3
        Given I am on "https://www.github.com"              # bddfire-3.0.2/lib/bddfire/web/web_steps.rb:2
          Request to 'https://www.github.com' failed to reach server, check DNS and/or server status (Capybara::Poltergeist::StatusFailError)
          features/github_search.feature:4:in `Given I am on "https://www.github.com"'
        When I fill in "Search GitHub" with the text "vavr" # bddfire-3.0.2/lib/bddfire/web/web_steps.rb:6
        And I press intro key with my little finger         # features/step_definitions/bddfire_steps.rb:3

    Failing Scenarios:
    cucumber -p poltergeist -p html features/github_search.feature:3 # Scenario: Search library within github

    2 scenarios (1 failed, 1 passed)
    6 steps (1 failed, 2 skipped, 3 passed)
    0m1.410s
    ```
