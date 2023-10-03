<p align="center">
  <img width="369" alt="X111" img src="https://user-images.githubusercontent.com/76184559/108545275-99ce1900-72b5-11eb-913f-baa768335c07.png"/>
</p>

<p align="center">
    <a href="https://img.shields.io/badge/version-v0.1.0-orange"><img alt="Version" src="https://img.shields.io/badge/version-v0.1.0-orange"></a>
    <a href="https://github.com/SFL09/PyEmail/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/github/license/SFL09/PyEmail"></a>
    <a href="https://github.com/psf/black/"><img alt="Code style: black" src="https://img.shields.io/badge/code%20style-black-000000.svg"></a>
    <a href="https://pycqa.github.io/isort/"><img alt="Imports: isort" src="https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336"></a>
</p>

PyEmail is a fast, powerful, and easy-to-use open-source Email tool providing a powerfully, user-friendly, and programmatically mail user agent. PyEmail is in a single file with no dependencies other than the [Python Standard Library](https://docs.python.org/3/library/).

## Features
- The defined Email class allows users to set HTML/plain text and reuse the information;
- The defined Server class can automatically manage the connection and send an email.

## Installation from sources
The source code is currently hosted on GitHub at:
https://github.com/SFL09/PyEmail

```bash
$ python -m pip install .
```

```bash
$ python setup.py install
```

## Example
```python
from PyEmail import Email, Server

email = Email(subject="My email title.", text="Hello!")
with Server("user@email.com", "password", "domain.org") as server:
    server.send(email, to=["guest@email.com", "visitor@email.com"])
```

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/SFL09/PyEmail/blob/main/LICENSE) file for details.
