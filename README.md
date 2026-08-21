# Oniro Documentation

Welcome to the Oniro Project documentation repository. This repository hosts the documentation for the Oniro Project, an Eclipse Foundation initiative dedicated to developing an open-source, vendor-neutral operating system (OS) platform. Oniro builds on OpenHarmony and extends its capabilities to provide a versatile platform for smart devices across many industries.

## Documentation Structure

This documentation uses [MkDocs](https://www.mkdocs.org) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme. GitHub Pages hosts the generated site, which provides clear and accessible guidance for Oniro developers.


### View the Generated Documentation

The latest version of the Oniro Project documentation is available at: [docs.oniroproject.org](https://docs.oniroproject.org/).

## Building the Documentation Locally
### Prerequisites

To build and preview the documentation locally on Windows, ensure that Python 3.12 or later and `pip` are installed, then follow these steps:

1. Clone this repository:
   ```sh
   git clone https://github.com/eclipse-oniro4openharmony/eclipse-oniro4openharmony.github.io.git
   cd oniro-docs
   ```

2. Create a virtual environment:
   ```python
   python -m venv venv
   ```

3. Activate the virtual environment:
   ```bash
   .\venv\Scripts\activate
   ```

4. Install Material for MkDocs:
   ```sh
   pip install mkdocs-material
   ```
5. Build and serve the documentation:
   ```sh
   mkdocs serve
   ```
6. Open `http://localhost:8000/` in your web browser to view the documentation.

## Publishing on GitHub Pages

This repository is configured to use GitHub Actions for building and deploying documentation. To publish updates:

1. Push changes to the `main` branch.
2. GitHub Actions will automatically build and deploy the documentation to GitHub Pages.

## Using Material for MkDocs

This documentation is based on the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme, which provides a minimal yet functional interface. The theme and its features are documented in the [MkDocs Material repository](https://github.com/squidfunk/mkdocs-material).

## Contributions

We welcome contributions to improve this documentation. If you would like to contribute:

1. Fork this repository.
2. Create a feature branch.
3. Submit a pull request with your changes.

For more details, refer to our contribution guidelines.

## License and Attribution

This repository is licensed under the [CC BY 4.0 License](https://creativecommons.org/licenses/by/4.0/). 

---

For more details about the Oniro Project, visit the [Eclipse Oniro website](https://projects.eclipse.org/projects/oniro).

