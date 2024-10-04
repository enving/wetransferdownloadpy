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


import os
import time
from playwright.async_api import async_playwright

class WeTransferDownloader:
    def __init__(self, download_dir="downloads"):
        self.download_dir = download_dir
        os.makedirs(self.download_dir, exist_ok=True)

    async def download_video(self, video_link: str) -> str:
        temp_file_name = f"temp_video_{int(time.time())}.mp4"   #i downloaded a video...but you can transfer any file or make it more general
        temp_file_path = os.path.join(self.download_dir, temp_file_name)

        try:
            print(f"Versuche, Video herunterzuladen von: {video_link}")

            async with async_playwright() as p:
                browser = await p.chromium.launch()
                context = await browser.new_context(
                    locale='de-DE',
                    accept_downloads=True
                )
                page = await context.new_page()
                await page.goto(video_link)
                await page.wait_for_load_state('networkidle')

                # Akzeptieren Sie die Cookies
                accept_cookies_button = await page.wait_for_selector("button[data-testid='Alle akzeptieren-btn']", timeout=30000)
                await accept_cookies_button.click()

                # Akzeptieren Sie die Nutzungsbedingungen
                accept_terms_button = await page.wait_for_selector("button:has-text('Ich akzeptiere')", timeout=60000)
                await accept_terms_button.click()

                # Warten Sie auf den Download-Button
                await page.wait_for_timeout(2000)
                download_button = await page.wait_for_selector("text=Herunterladen", timeout=60000)
                await download_button.click()

                # Warten Sie auf den Download
                download_event = await page.wait_for_event("download", timeout=60000)
                download_path = await download_event.path()
                print(f"Download abgeschlossen: {download_path}")

                # Verschieben Sie die Datei in das gewünschte Verzeichnis
                os.rename(download_path, temp_file_path)
                print(f"Datei verschoben nach: {temp_file_path}")

                return temp_file_path
        except Exception as e:
            print(f"Unerwarteter Fehler beim Herunterladen des Videos: {str(e)}")
            raise

