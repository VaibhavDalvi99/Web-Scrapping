# Web Scraping and Data Extraction

- import requests
- #Since we have request to this library we can send requests to grab this web page.

- import pandas as pd
- from bs4 import BeautifulSoup

url = f"https://books.toscrape.com/catalogue/page-1.html"

- #We are sending requests to grab some response from this url.
- response = requests.get(url)

- #Below code is used to see the content of the response but it's a huge binary code and not html. So,this is where beautiful soup comes in. 
- response = response.content

- #It will basically converts the response into HTML code.
- soup = BeautifulSoup(response,'html.parser')

- #It will find the first occurence of ol(order list) in soup.
- ol = soup.find('ol')

- #Find_all will find all occurences if html article tag and if there are other articles tags as well and we want to be more specific we can use article
- #tag along with its class as mentioned below.
- articles = ol.find_all('article',class_='product_pod')

- #Now we have list of all the articles from which we can loop through all the articles and grab all what we need
- articles

- #Parsing all the data needed and putting them inside a list.
- books=[]

- for article in articles:
-        image = article.find('img')
-        title = image.attrs['alt']
-        star = article.find('p')
-       star = star['class'][1]
-        price = article.find('p',class_="price_color").text
-        price = float(price[1:])
-        books.append([title, price, star])
- print(books) #List of lits

- #Above code need to be put inside the loop since we had to get the information from all the 50 pages.
- for i in range(1,4):
-    url = f"https://books.toscrape.com/catalogue/page-{i}.html"
    
-   response = requests.get(url)
    
-    response = response.content
    
-    soup = BeautifulSoup(response,'html.parser')
    
-    ol = soup.find('ol')
    
-    articles = ol.find_all('article',class_='product_pod')

    
-    for article in articles:
-        image = article.find('img')
-        title = image.attrs['alt']
-        star = article.find('p')
-        star = star['class'][1]
-        price = article.find('p',class_="price_color").text
-        price = float(price[1:])
-        books.append([title, price, star])

-  #Using pandas we are converting list of books into pandas dataframe and specifying column names to it. 
- df = pd.DataFrame(books, columns = ["Title","Price","Rating"])
- df

- #Exporting dataframe into csv file (mentioning the path where we want the file to export)
- df.to_csv(r'C:\Users\sneha\Desktop\V.D\Anaconda Python\Web Scarpping\books.csv',index=False)
