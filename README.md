# Scrapping-Website

from selenium import webdriver
from bs4 import BeautifulSoup as bs
import pandas as pd
import requests
import time, urllib.request

#2 (masukkan keyword sesuai kebutuhan dan paste di driver.get)

driver = webdriver.Chrome("you directory of chromedriver.exe")
driver.get("your url")

#3
driver.find_element_by_css_selector("option[value='100']").click()

#4
content = driver.page_source
soup = bs(content, 'lxml')
table_data = soup.find('table', class_ = 'table table-striped dataTable no-footer')

#5
headers = []
for i in table_data.find_all('th'):
    title = i.text
    headers.append(title)

#6
df = pd.DataFrame(columns = headers)

#7
p = 0
for j in range(1,13):    
    if j > 5 and j < 12 :
        p = 5
    else :
        p += 1
    
    driver.find_element_by_css_selector("a[data-dt-idx='"+str(p)+"']").click()
    time.sleep(60)
    
    content = driver.page_source
    soup = bs(content, 'lxml')
    table_data = soup.find('table', class_ = 'table table-striped dataTable no-footer')
    time.sleep(5)
    
    for k in table_data.find_all('tr')[1:]:
            row_data = k.find_all('td')
            row = [tr.text for tr in row_data]
            length = len(df)
            df.loc[length] = row 
    del k

#8
df.to_csv('your file name.csv', index=False, encoding='utf-8')
