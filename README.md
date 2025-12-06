# Wine Market Analysis

This repository provides tools and code for collecting, cleaning, and analyzing data from the [Vino&Vino online wine catalogue](https://vinovino.am/en/catalog/wine). The goal is to explore trends in wine pricing, producers, origins, and other key attributes, leveraging real-world data scraped from an Armenian online wine retailer.

## Features

- **Automated Web Scraping**: Uses `requests`, `Selenium`, and `BeautifulSoup` to collect data even from dynamic web pages requiring scrolling or Javascript rendering.
- **Comprehensive Data Fields**: Extracts wine producer, name, vintage, sweetness, color, volume, country, region, and price for each wine card.
- **Data Analysis-Ready**: Stores data in a pandas DataFrame and exports it as a CSV for further cleaning and analysis.

## Getting Started

### Prerequisites

- Python 3.8+
- Google Chrome browser (for Selenium)
- [chromedriver](https://sites.google.com/a/chromium.org/chromedriver/) (handled automatically if using `webdriver-manager`)

#### Key Python Packages
- `selenium`
- `webdriver-manager`
- `requests`
- `beautifulsoup4`
- `pandas`

Install the requirements:
```bash
pip install selenium webdriver-manager requests beautifulsoup4 pandas
```

### Usage

You can run the workflow in Jupyter Notebook or Google Colab.

#### Example workflow:

1. **System Setup**  
   Install dependencies and (on Colab) set up Chrome:

   ```python
   pip install selenium webdriver-manager
   # For Google Colab, also:
   !apt-get update
   !wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
   !dpkg -i google-chrome-stable_current_amd64.deb
   !apt-get install -f
   ```

2. **Scraping Logic**  
   - Use `requests` for basic connectivity checks.
   - Use Selenium to control a headless Chrome browser, load content, and scroll.
   - Once the full page is loaded, parse with BeautifulSoup.
   - Extract wine attributes and save as a CSV.

See [`wine_market_analysis.ipynb`](wine_market_analysis.ipynb) for the full code and documentation.

## Project Structure

```
wine_market_analysis.ipynb   # Main notebook with scraping & analysis workflow
wine_data.csv                # Output dataset (not tracked by default)
README.md                    # This file
```

## Notable Code Snippets

**Initialize Selenium Webdriver**

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--headless=new')
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--disable-dev-shm-usage')
chrome_options.binary_location = '/usr/bin/google-chrome' # for Colab

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()),
                          options=chrome_options)
driver.get(url)
```

**Scroll to Load Dynamic Content**
```python
stopScrolling = 0
while True:
    stopScrolling += 1
    driver.execute_script("window.scrollBy(0,40)")
    time.sleep(0.05)
    if stopScrolling > 5000:
        break
    time.sleep(0.1)
```

**Data Extraction with BeautifulSoup**
```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(driver.page_source, 'html.parser')
# ... your code for finding and extracting wine cards ...
```

## Data Fields

- `Producer`
- `Wine Name`
- `Vintage`
- `Sweetness`
- `Color`
- `Volume`
- `Country`
- `Region`
- `Price`

## Outputs

- `wine_data.csv`: Cleaned dataset for analysis.


**For detailed workflow and explanations, see [wine_market_analysis.ipynb](wine_market_analysis.ipynb).**
