# yaml-parsing

import requests 
import sys
import csv 
 
from bs4 import BeautifulSoup as bs
 
#Java Buildpack Version
version = '4.32.1'
 
str = 'https://github.com/cloudfoundry/java-buildpack/releases/tag/v{}'
 
# Combining the URL + version strings together
url = str.format(version)
 
# For display 
JavaBuildpackVersion = "java Buildpack Version" + " "  + version
print(JavaBuildpackVersion)
print("Github URL" + ":" + url)
 
req = requests.get(url)
 
soup = bs(req.content, 'html.parser')
 
#Exporting data to csv file
 
filename = 'JavaBP.csv'
 
csv_writer = csv.writer(open(filename,'w', newline=''))
 
heading = soup.find('h2')
 
print(heading.text)
 
# Run a for llop to extract the table data and store it in csv file
 
for tr in soup.find_all('tr'):
    data = []
 
    # For extraction the table heading this execute only once
    
    for th in tr.find_all('th'):
        data.append(th.text.strip())
 
    if data:
        print("Inserting headers : {}".format(','.join(data)))
        csv_writer.writerow(data)
        continue
 
    # For scraping the actual table data values
 
    for td in tr.find_all("td"):
        data.append(td.text.strip())
 
    if data:
        print("Inserting Data : {}".format(','.join(data)))
        csv_writer.writerow(data)
