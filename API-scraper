from browsermobproxy import Server
from selenium import webdriver
import pandas as pd

# Path to browsermob-proxy
bmp_path = 'path/to/browsermob-proxy/bin/browsermob-proxy'
server = Server(bmp_path)
server.start()
proxy = server.create_proxy()

# Selenium setup
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument(f'--proxy-server={proxy.proxy}')
driver = webdriver.Chrome(options=chrome_options)

# Start capturing
proxy.new_har("omgevingswet", options={'captureHeaders': True, 'captureContent': True})

# Open the website
url = "https://omgevingswet.overheid.nl/regels-op-de-kaart/zoeken/locatie?locatie=Prins%20Bernhardplein%2017,%205391AR%20Nuland&regelsandere=regels&session=80590de5-e12a-484d-a7da-8526a2943cde"
driver.get(url)

# Wait for the page to load (adjust as needed)
import time
time.sleep(10)

# Get HAR data
har_data = proxy.har

# Extract API URLs
api_calls = []
for entry in har_data['log']['entries']:
    url = entry['request']['url']
    if any(api in url for api in ['api', 'rest', 'service']):  # Adjust filter as needed
        api_calls.append({'url': url})

# Save to CSV
df = pd.DataFrame(api_calls)
df.to_csv('api_calls.csv', index=False)

# Cleanup
driver.quit()
server.stop()