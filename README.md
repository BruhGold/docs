# DMOJ Documentation

Documentation for the [DMOJ judge](https://github.com/DMOJ/judge-server) system.

Access at <https://docs.dmoj.ca>.

docs is served using docsify

# Install venv
python -m venv .venv

# Activate venv
source .venv/bin/activate # for Linux/MacOS
.\.venv\Scripts\activate.bat # for Windows command line
.\.venv\Scripts\Activate.ps1 # for Windows PowerShell

# install node here
https://nodejs.org/

# install docsify
npx docsify-cli init .

# serve
npx docsify-cli serve ./docs