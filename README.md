# ScholarLed catalogue

This website is a catalogue of publications from the open access publishers in the [ScholarLed](https://scholarled.org/) consortium (Mattering Press, meson press, Open Book Publishers, punctum books, African Minds, and mediastudies.press). It's an example of constructing a dynamic computational publication using a workflow of Jupyter Notebook files, Python code, and [Quarto](https://quarto.org/) technical publishing software.

All bibliographic metadata is retrieved from [Thoth](https://thoth.pub/), an open metadata management and dissemination system for open access books designed as part of the [COPIM project](https://www.copim.ac.uk/). Thoth has a range of open APIs for metadata retrieval and all metadata is licensed as [Creative Commons Zero (CC0)](https://creativecommons.org/share-your-work/public-domain/cc0/).

## Quarto and Jupyter Notebook

The main publishing workflow uses Quarto to render the output of Jupyter Notebook files. Run the .ipynb files in JupyterLab or Visual Studio Code or any other Jupyter environment and then run:

`quarto render`

for Quarto to render the output into an output directory, in this case, ./docs. 

GitHub Pages then serves the HTML files in the ./docs directory as a website. 

## Docker Compose

This repository also contains Docker Compose and Dockerfiles for running the various applications in Docker containers.

Run `docker-compose up -d --build` to start the containers and `docker-compose down` to stop the containers.

The jupyterlab container runs a stand-alone version of JupyterLab on http://localhost:8888. This can be used to edit any Jupyter Notebook files in the repository. The JupyterLab instance runs with the password 'jupyterlab'.

The nginx container runs Nginx webserver and displays the static site that Quarto renders. This runs at http://localhost:1337.

The quarto container starts a Ubuntu 22.04 container, installs various things like Python, downloads Quarto and installs it, and then adds Python modules like jupyter, matplotlib, and panda. It then runs in the background so Quarto can be called on to render the qmd and ipynb files into the site/book like so:

`docker exec -it quarto quarto render` 

There's a known issue with Quarto running in the Docker container in macOS due to the amd64 emulation of Docker Desktop for arm64 MacOS. See discussion at https://github.com/quarto-dev/quarto-cli/discussions/3308. This shouldn't occur in any other environment running Docker.