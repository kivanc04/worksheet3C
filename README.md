# worksheet3C
"""
Stage: Development -01
@author: Ahmet Kıvanç Demirkıran, 120200133 
@author: E. Esin Yılmaz, 120200059
"""
import requests
from bs4 import BeautifulSoup

URL = "https://www.airbnb.com.tr/?_set_bev_on_new_domain=1669973802_NTQ0MTg1YmUyYmM2"
page = requests.get(URL)

soup = BeautifulSoup(page.content, "html.parser")
def scrape_page(page_url):
    """Extracts HTML from a webpage"""
    
    answer = requests.get(URL)
    content = answer.content
    soup = BeautifulSoup(content, features='html.parser')
    
    return soup
    def extract_listing(URL):
    """Extracts listings from an Airbnb search page"""
    
    page_soup = scrape_page(URL)
    listings = page_soup.findAll("div", {"class": "_8s3ctt"})

    return listings
    def extract_element_data(soup, params):
	    """Extracts data from a specified HTML element"""
	    
	    # 1. Find the right tag
	    if 'class' in params:
	        elements_found = soup.find_all(params['tag'], params['class'])
	    else:
	        elements_found = soup.find_all(params['tag'])
	        
	    # 2. Extract text from these tags
	    if 'get' in params:
	        element_texts = [el.get(params['get']) for el in elements_found]
	    else:
	        element_texts = [el.get_text() for el in elements_found]
	        
	    # 3. Select a particular text or concatenate all of them
	    tag_order = params.get('order', 0)
	    if tag_order == -1:
	        output = '**__**'.join(element_texts)
	    else:
	        output = element_texts[tag_order]
	    
	    return output
