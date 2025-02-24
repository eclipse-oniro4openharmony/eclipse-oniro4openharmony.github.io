# Oniro Documentation

Welcome to the Oniro Project documentation repository. This repository hosts the documentation for the Oniro Project, an Eclipse Foundation initiative dedicated to developing an open-source, vendor-neutral Operating System (OS) platform. Oniro builds upon OpenHarmony, extending its capabilities to provide a versatile platform for smart devices across various industries.

## Documentation Structure

This documentation is powered by [Jekyll](https://jekyllrb.com) and uses the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme. It is hosted on GitHub Pages and follows a structured approach to provide clear and accessible guidance for Oniro developers.

### View the Generated Documentation

The latest version of the Oniro Project documentation is available at: [docs.oniroproject.org](https://docs.oniroproject.org/).

## Building the Documentation Locally

To build and preview the documentation on your local machine, ensure that [Jekyll](https://jekyllrb.com) and [Bundler](https://bundler.io) are installed, then follow these steps:

1. Clone this repository:
   ```sh
   git clone https://github.com/eclipse-oniro4openharmony/eclipse-oniro4openharmony.github.io.git oniro-docs
   cd oniro-docs
   ```
2. Install dependencies:
   ```sh
   bundle install
   ```
3. Build and serve the documentation:
   ```sh
   bundle exec jekyll serve
   ```
4. Open `http://localhost:4000/` in your web browser to view the documentation.

## Publishing on GitHub Pages

This repository is configured to use GitHub Actions for building and deploying documentation. To publish updates:

1. Push changes to the `main` branch.
2. GitHub Actions will automatically build and deploy the documentation to GitHub Pages.

## Using the Just the Docs Theme

This documentation is based on the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme, which provides a minimal yet functional interface. The theme and its features are documented in the [Just the Docs repository](https://github.com/just-the-docs/just-the-docs).

## Contributions

We welcome contributions to improve this documentation. If you would like to contribute:

1. Fork this repository.
2. Create a feature branch.
3. Submit a pull request with your changes.

For more details, refer to our contribution guidelines.

## License and Attribution

This repository is licensed under the [MIT License](https://opensource.org/licenses/MIT). The documentation framework is adapted from the [Just the Docs Template](https://github.com/just-the-docs/just-the-docs-template), ensuring a robust and customizable documentation experience.

---

For more details about the Oniro Project, visit the [Eclipse Oniro website](https://projects.eclipse.org/projects/oniro).

