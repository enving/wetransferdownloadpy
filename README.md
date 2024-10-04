# wetransferdownloadpy
Is using a crawler without need to install a webbrowser driver which makes is easy and in a box...locally to download automatically a file sent over wetransfer. The other wetransfer download automations are not working I have tested them all which brought me to upload this one here for you.  Is working on 04.10.2024 have fun :)

##see full code in main.py


**wetransferdownloadpy** is a simple Python package designed to facilitate the downloading of videos from WeTransfer using Playwright. This package abstracts the complexities of interacting with the WeTransfer website, allowing users to easily download files with just a few lines of code.

## Features

- Download videos from WeTransfer links.
- Utilizes Playwright for browser automation.
- Supports asynchronous operations for efficient downloading.

## Installation

To install Wetransferpy, you need to have Python 3.7 or higher installed on your system. You can install the package using pip:

pip install Wetransferpy


### Dependencies

Wetransferdownloadpy requires the following packages:

- `playwright`: For browser automation.
- `aiohttp`: For asynchronous HTTP requests.
- `asyncio`: For managing asynchronous operations.
- `requests`: For making HTTP requests.


## Important Notes

- **Button Texts**: Depending on the language settings of your browser (your IP adress location) or the WeTransfer website, the button texts may vary. You might need to adjust the selectors in the code to match the button texts in your preferred language.
- **Playwright Installation**: After installing the package, you may need to install the Playwright browsers. You can do this by running:

playwright install


## Contributing

Contributions are welcome! If you have suggestions for improvements or new features, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

