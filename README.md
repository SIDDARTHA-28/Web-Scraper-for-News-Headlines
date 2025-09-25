# Web-Scraper-for-News-Headlines
Scrape top headlines from a news website
import requests
from bs4 import BeautifulSoup

def scrape_headlines(url, output_file):
    try:
        # Send request to the website
        response = requests.get(url, headers={'User-Agent': 'Mozilla/5.0'})
        response.raise_for_status()  # raise error if request failed

        # Parse the content
        soup = BeautifulSoup(response.text, 'html.parser')

        # Find headlines (BBC headlines are in <h3> tags with specific classes)
        headlines = soup.find_all('h3')

        # Open file to save headlines
        with open(output_file, "w", encoding="utf-8") as f:
            for i, headline in enumerate(headlines, start=1):
                text = headline.get_text(strip=True)
                if text:
                    f.write(f"{i}. {text}\n")
        
        print(f"✅ Headlines saved to {output_file}")

    except Exception as e:
        print(f"❌ Error: {e}")

if __name__ == "__main__":
    url = "https://www.bbc.com/news"   # You can replace with any news site
    output_file = "headlines.txt"
    scrape_headlines(url, output_file)
