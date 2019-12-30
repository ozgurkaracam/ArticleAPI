Sample Laravel API & Vue APP..

Installation

git clone https://github.com/ozgurkaracam/ArticleAPI.git

php artisan make:migrate --seed

npm install

php artisan serve


**API**

URL | METHOD | PAYLOAD | DESCRIPTION
--- | --- | --- | ---
api/articles | GET | | get all article. 
api/article/{id} | GET |  | get singular article
api/article | POST |{title:title , body:body} | Post article
api/article | PUT |{article_id:int, title:string , body:string} | Edit article


