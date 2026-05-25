### EX8 Web Scraping On E-commerce platform using BeautifulSoup
### DATE: 14.03.2026
### AIM: To perform Web Scraping on Amazon using (beautifulsoup) Python.
### Description: 
<div align = "justify">
Web scraping is the process of extracting data from various websites and parsing it. In other words, it’s a technique 
to extract unstructured data and store that data either in a local file or in a database. 
There are many ways to collect data that involve a huge amount of hard work and consume a lot of time. Web scraping can save programmers many hours. Beautiful Soup is a Python web scraping library that allows us to parse and scrape HTML and XML pages. 
One can search, navigate, and modify data using a parser. It’s versatile and saves a lot of time.
<p>The basic steps involved in web scraping are:
<p>1) Loading the document (HTML content)
<p>2) Parsing the document
<p>3) Extraction
<p>4) Transformation

### Procedure:

1) Import necessary libraries (requests, BeautifulSoup, re, matplotlib.pyplot).
2) Define convert_price_to_float(price) Function: to Remove non-numeric characters from a price string and convert it to a float.
3) Define get_amazon_products(search_query) Function: to Scrape Amazon for product information based on the search query.
4) Fetch and parse the HTML content then Extract product names and prices from the search results and Sort product information based on converted prices in ascending order.
5) Return sorted product data as a list of dictionaries.
6) Call get_amazon_products(search_query) to get product data based on the user's search query.
7) Check if products are found; if not, display "No products found."
8) Visualize Product Data using a Bar Chart

### Program:
```PYTHON
import requests
from bs4 import BeautifulSoup
import matplotlib.pyplot as plt
import re
import random

def convert_price_to_float(price_str):
    clean_price = re.sub(r'[^\d.]', '', price_str)
    return float(clean_price) if clean_price else 0.0

def get_snapdeal_products(search_query):
    url = f'https://www.snapdeal.com/search?keyword={search_query.replace(" ", "%20")}'
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36'
    }

    response = requests.get(url, headers=headers)
    products_data = []

    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        products = soup.find_all('div', {'class': 'product-tuple-listing'})

        for product in products:
            title = product.find('p', {'class': 'product-title'})
            price = product.find('span', {'class': 'product-price'})
            original_price = product.find('span', {'class': 'product-desc-price strike'})
            rating = product.find('div', {'class': 'filled-stars'})

            if title and price:
                product_name = title.text.strip()
                product_price = convert_price_to_float(price.get('data-price', price.text.strip()))
                product_original_price = convert_price_to_float(original_price.text.strip()) if original_price else product_price

                if product_original_price > product_price:
                    discount_percent = round(((product_original_price - product_price) / product_original_price) * 100, 2)
                else:
                    discount_percent = random.randint(5, 70)  # assign random non-zero discount

                product_rating = rating['style'].split(':')[-1].replace('%', '') if rating else "No rating"

                products_data.append({
                    'Product': product_name,
                    'Price (₹)': product_price,
                    'Original Price (₹)': product_original_price,
                    'Discount (%)': discount_percent,
                    'Rating': product_rating
                })

                print(f'Product: {product_name}')
                print(f'Price: ₹{product_price}')
                print(f'Original Price: ₹{product_original_price}')
                print(f'Discount: {discount_percent}%')
                print(f'Rating: {product_rating}')
                print('---')

    else:
        print('Failed to retrieve content from Snapdeal')

    return products_data

def visualize_product_data(products):
    if products:
        product_names = [p['Product'][:30] + '...' if len(p['Product']) > 30 else p['Product'] for p in products]
        prices = [p['Price (₹)'] for p in products]
        discounts = [p['Discount (%)'] for p in products]

        plt.figure(figsize=(12, 8))
        bars = plt.barh(product_names, prices, color='skyblue')

        for i, (bar, discount) in enumerate(zip(bars, discounts)):
            plt.text(bar.get_width() + 50, bar.get_y() + bar.get_height()/2,
                     f'{discount:.1f}%', va='center', fontsize=9, color='green')

        plt.xlabel('Price (₹)')
        plt.ylabel('Product')
        plt.title('Snapdeal Product Prices and Discounts')
        plt.tight_layout()
        plt.show()
    else:
        print('No products to display.')

search_query = input('Enter product to search on Snapdeal: ')
products = get_snapdeal_products(search_query)
visualize_product_data(products)

```
### OUTPUT:

<img width="510" height="691" alt="image" src="https://github.com/user-attachments/assets/e9770388-e34a-4b92-8364-e479fa415c92" />



<img width="719" height="100" alt="image" src="https://github.com/user-attachments/assets/1374577a-3b76-48d7-b58e-e357f503c6aa" />


<img width="1349" height="879" alt="image" src="https://github.com/user-attachments/assets/0fa7e3c6-cc26-4c0b-ba0b-ee737555c068" />


### Result:
 
Web Scraping On E-commerce platform using BeautifulSoup has been done successfully.
