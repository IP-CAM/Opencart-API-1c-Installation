## Техническое задание на создание синхронизации сайта c каталогом запчастей с 1С.

Цель: создать api-интерфейс позволяющий синхронизировать каталог запчастей на сайте с 1С.

Задача: разработать api и набор методов для синхронизации каталога запчастей.

### Описание сущностей:
1)	Бренд – сущность, описывающая бренд запчасти, например Higer.
2)	Категория – сущность, описывающая раздел сайта, в которой может находится запчасть. Возможна многоуровневая вложенность категорий.
3)	Карточка – сущность, описывающая непосредственно запчасть.

Каждая сущность обладает определенным набором полей.
### Бренд, набор полей: 
* manufacturer_uid – уникальный идентификатор из 1С
* name – название бренда
* image – логотип бренда
* noindex – управляет запретом на индексирование поисковыми системами
* description – текстовое описание страницы бренда
* meta_description – необходим для seo
* meta_title – необходим для seo
* meta_h1 – необходим для seo (может быть равен названию запчасти)

### Категория, набор полей:
* category_uid – уникальный идентификатор из 1С
* image – изображение категории
* parent_uid - уникальный идентификатор из 1С родительской категории (если нет родительской – устанавливается в ноль)
* sort_order – порядок вывода
* status – управляет отображением, (скрыть вывод категории на сайте)
* noindex – управляет запретом на индексирование поисковыми системами
* name – название категории
* description – текстовое описание категории
* meta_title – необходим для seo
* meta_ description – необходим для seo
* meta_h1 – необходим для seo

### Карточка, набор полей:
* product_uid - уникальный идентификатор из 1С
* model – vin-код запчасти
* sku – каталожный номер (артикул)
* quantity – кол-во данной запчасти
* stock_status_id – статус запчасти (в наличии, нет в наличии, предзаказ, …можно сделать свой статус если необходимо)
* images – массив, содержащий пути к картинкам, где первый элемент массива – основное изображение.
* manufacturer_uid – уникальный идентификатор бренда из 1С
* price – стоимость одной единицы 
* special_price – цена со скидкой
* date_available – дата, начиная с которой, запчасть будет отображена на сайте.
* minimum – значение минимального кол-ва для заказа.
* sort_order порядок вывода в каталоге
* noindex – управляет запретом на индексирование поисковыми системами
* name – название запчасти
* description – текстовое описание запчасти
* meta_title – необходим для seo
* meta_ description – необходим для seo
* meta_h1 – необходим для seo
* status - отображать на сайте или нет
* product_category – массив, содержащий уникальные идентификаторы категорий из 1С, где первый элемент массива – основная категория. Остальные элементы – это в каких еще категориях показывать запчасть.
* product_related – массив, содержащих уникальные идентификаторы карточек запчастей, которые будут выводиться в блоке «похожие товары»

Обсудить, нужны ли поля, характеризующие вес и размер запчасти (длина, ширина, высота)

---
### Описание методов работы с API:
* login – метод позволяющий провести авторизацию в api

* manufacturer/get – метод позволяющий получить бренд по uid
* manufacturer/getAll – метод позволяющий получить список всех брендов
* manufacturer/create – метод позволяющий создать бренд
* manufacturer/update – метод позволяющий обновить информацию о бренде
* manufacturer/delete – метод позволяющий удалить бренд по uid
* manufacturer/deleteAll – метод позволяющий удалить все бренды

* category/get – метод позволяющий получить категорию по uid
* category/getAll – метод позволяющий получить список всех категорий
* category/create – метод позволяющий создать категорию
* category/update – метод позволяющий обновить информацию о категории
* category/delete – метод позволяющий удалить категорию по uid
* category/deleteAll – метод позволяющий удалить все категории

* product/get – метод позволяющий получить карточку по uid
* product/getAll – метод позволяющий получить список всех карточек
* product/create – метод позволяющий создать карточку
* product/update – метод позволяющий обновить информацию о карточке
* product/delete – метод позволяющий удалить карточку по uid
* product/deleteAll – метод позволяющий удалить все карточки

---
---
---

## Метод login
## POST https://test-domen.ru/index.php?route=api/login
### Post-запрос с параметрами:
* username – api username
* key – ключ api
### Результат:
```javascript
{
    api_token: '3krj3n4rhj34brhj34brhj3b4r3hj4rb3hj4rb'
}
```
---
---
---
## Метод manufacturer/get
## GET https://test-domen.ru/index.php?route=api/1c/manufacturer/get&api_token=API_TOKEN&manufacturer_uid=1
### Get-запрос с параметрами:
* manufacturer_uid - уникальный идентификатор бренда из 1С 
### Результат:
```javascript
{
    manufacturer_id: 223,
    manufacturer_uid: 1,
    name: 'Youtong',
    image: 'catalog/manufacturer/UID/1.jpg',
    noindex: 0,
    manufacturer_description: {
        description: 'Описание производителя',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```
---
## Метод manufacturer/getAll
## GET https://test-domen.ru/index.php?route=api/1c/manufacturer/getAll&api_token=API_TOKEN

### Результат:
```javascript
[
    {
        manufacturer_id: 223,
        manufacturer_uid: 1,
        name: 'Youtong',
        image: 'catalog/manufacturer/UID/1.jpg',
        noindex: 0,
        manufacturer_description: {
            description: 'Описание производителя',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```
---
## Метод manufacturer/create
## POST https://test-domen.ru/index.php?route=api/1c/manufacturer/create&api_token=API_TOKEN
### Post-запрос с параметрами:
```javascript
[
    {
        manufacturer_uid: 1,
        name: 'Youtong',
        image: 'catalog/manufacturer/UID/1.jpg',
        noindex: 0,
        manufacturer_description: {
            description: 'Описание производителя',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```
### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Manufacturer's created"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```
---
## Метод manufacturer/update
## POST https://test-domen.ru/index.php?route=api/1c/manufacturer/update&api_token=API_TOKEN&manufacturer_uid=1
### Post-запрос с параметрами:
```javascript
{
    manufacturer_uid: 1,
    name: 'Youtong',
    image: 'catalog/manufacturer/UID/1.jpg',
    noindex: 0,
    manufacturer_description: {
        description: 'Описание производителя',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```
### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Manufacturer updated"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод manufacturer/delete
## GET https://test-domen.ru/index.php?route=api/1c/manufacturer/delete&api_token=API_TOKEN&manufacturer_uid=1

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Manufacturer deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод manufacturer/deleteAll
## GET https://test-domen.ru/index.php?route=api/1c/manufacturer/deleteAll&api_token=API_TOKEN

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Manufacturers deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
---
---
## Метод category/get
## GET https://test-domen.ru/index.php?route=api/1c/category/get&api_token=API_TOKEN&category_uid=2
### Get-запрос с параметрами:
* category_uid - уникальный идентификатор категории из 1С 
### Результат:
```javascript
{
    category_id: 333,
    category_uid: 2,
    parent_uid: 1,
    sort_order: 0,
    status: 1,
    image: 'catalog/category/UID/1.jpg',
    seo_url: '2-yutong',
    category_description: {
        name: 'yutong',
        description: 'Описание категории',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```
---
## Метод category/getAll
## GET https://test-domen.ru/index.php?route=api/1c/category/getAll&api_token=API_TOKEN

### Результат:
```javascript
[
    {
        category_id: 333,
        category_uid: 2,
        parent_uid: 1,
        sort_order: 0,
        status: 1,
        image: 'catalog/category/UID/1.jpg',
        seo_url: '2-yutong',
        category_description: {
            name: 'yutong',
            description: 'Описание категории',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```
---
## Метод category/create
## POST https://test-domen.ru/index.php?route=api/1c/category/create&api_token=API_TOKEN
### Post-запрос с параметрами:
```javascript
[
    {
        category_uid: 2,
        parent_uid: 1,
        sort_order: 0,
        status: 1,
        image: 'catalog/category/UID/1.jpg',
        seo_url: '2-yutong',
        category_description: {
            name: 'yutong',
            description: 'Описание категории',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```
### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Categories created"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```
---
## Метод category/update
## POST https://test-domen.ru/index.php?route=api/1c/category/update&api_token=API_TOKEN&category_uid=1
### Post-запрос с параметрами:
```javascript
{
    category_uid: 2,
    parent_uid: 1,
    sort_order: 0,
    status: 1,
    image: 'catalog/category/UID/1.jpg',
    seo_url: '2-yutong',
    category_description: {
        name: 'yutong',
        description: 'Описание категории',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```
### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Categories updated"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод category/delete
## GET https://test-domen.ru/index.php?route=api/1c/category/delete&api_token=API_TOKEN&category_uid=1

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Category deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод category/deleteAll
## GET https://test-domen.ru/index.php?route=api/1c/category/deleteAll&api_token=API_TOKEN

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Categories deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
---
---
## Метод product/get
## GET https://test-domen.ru/index.php?route=api/1c/product/get&api_token=API_TOKEN&product_uid=1
### Get-запрос с параметрами:
* product_uid - уникальный идентификатор запчасти из 1С 
### Результат:
```javascript
{
    product_id: 332,
    product_uid: 1,
    model: 'VIN код запчасти',
    sku: 'Каталожный номер',
    quantity: 10,
    stock_status_id: 1, // привести к единому стандарту
    images: ['catalog/product/UID/1.jpg','catalog/product/UID/2.jpg'],
    manufacturer_uid: Manufacturer_UID,
    shipping: 1, // под вопросом
    price: 452.50,
    special_price: 450,
    date_available: '2021-06-25',
    minimum: 1, // под вопросом
    sort_order: 0,
    status: 1,
    date_added: '2021-06-25 16:06:50',
    date_modified: '2021-06-25 16:06:50',
    noindex: 0,
    seo_url: '1-bolt-m56',

    product_related: [Product_UID,Product_UID], // под вопросом
    product_category: [Categoty_UID, Categoty_UID, Categoty_UID],
    product_description: {
        name: 'Болт M56',
        description: 'Описание запчасти',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```
## Метод product/getAll
## GET https://test-domen.ru/index.php?route=api/1c/product/getAll&api_token=API_TOKEN

### Результат:
```javascript
[
    {
        product_id: 332,
        product_uid: 1,
        model: 'VIN код запчасти',
        sku: 'Каталожный номер',
        quantity: 10,
        stock_status_id: 1, // привести к единому стандарту
        images: ['catalog/product/UID/1.jpg','catalog/product/UID/2.jpg'],
        manufacturer_uid: Manufacturer_UID,
        shipping: 1, // под вопросом
        price: 452.50,
        special_price: 450,
        date_available: '2021-06-25',
        minimum: 1, // под вопросом
        sort_order: 0,
        status: 1,
        date_added: '2021-06-25 16:06:50',
        date_modified: '2021-06-25 16:06:50',
        noindex: 0,
        seo_url: '1-bolt-m56',

        product_related: [Product_UID,Product_UID], // под вопросом
        product_category: [Categoty_UID, Categoty_UID, Categoty_UID],
        product_description: {
            name: 'Болт M56',
            description: 'Описание запчасти',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```
## Метод product/create
## POST https://test-domen.ru/index.php?route=api/1c/product/create&api_token=API_TOKEN
### Post-запрос с параметрами:
```javascript
[
    {
        product_uid: 1,
        model: 'VIN код запчасти',
        sku: 'Каталожный номер',
        quantity: 10,
        stock_status_id: 1, // привести к единому стандарту
        images: ['catalog/product/UID/1.jpg','catalog/product/UID/2.jpg'],
        manufacturer_uid: Manufacturer_UID,
        shipping: 1, // под вопросом
        price: 452.50,
        special_price: 450,
        date_available: '2021-06-25',
        minimum: 1, // под вопросом
        sort_order: 0,
        status: 1,
        date_added: '2021-06-25 16:06:50',
        date_modified: '2021-06-25 16:06:50',
        noindex: 0,
        seo_url: '1-bolt-m56',

        product_related: [Product_UID,Product_UID], // под вопросом
        product_category: [Categoty_UID, Categoty_UID, Categoty_UID],
        product_description: {
            name: 'Болт M56',
            description: 'Описание запчасти',
            meta_description: 'мета-description',
            meta_title: 'мета-title',
            meta_h1: 'h1 заголовок страницы'
        }
    },
    ...
]
```

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Product's created"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

## Метод product/update
## POST https://test-domen.ru/index.php?route=api/1c/product/update&api_token=API_TOKEN&product_uid=1
### Post-запрос с параметрами:
```javascript
{
    product_uid: 1,
    model: 'VIN код запчасти',
    sku: 'Каталожный номер',
    quantity: 10,
    stock_status_id: 1, // привести к единому стандарту
    images: ['catalog/product/UID/1.jpg','catalog/product/UID/2.jpg'],
    manufacturer_uid: Manufacturer_UID,
    shipping: 1, // под вопросом
    price: 452.50,
    special_price: 450,
    date_available: '2021-06-25',
    minimum: 1, // под вопросом
    sort_order: 0,
    status: 1,
    date_added: '2021-06-25 16:06:50',
    date_modified: '2021-06-25 16:06:50',
    noindex: 0,
    seo_url: '1-bolt-m56',
    product_related: [Product_UID,Product_UID], // под вопросом
    product_category: [Categoty_UID, Categoty_UID, Categoty_UID],
    product_description: {
        name: 'Болт M56',
        description: 'Описание запчасти',
        meta_description: 'мета-description',
        meta_title: 'мета-title',
        meta_h1: 'h1 заголовок страницы'
    }
}
```

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Product updated"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод product/delete
## GET https://test-domen.ru/index.php?route=api/1c/product/delete&api_token=API_TOKEN&product_uid=1

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Product deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```

---
## Метод product/deleteAll
## GET https://test-domen.ru/index.php?route=api/1c/product/deleteAll&api_token=API_TOKEN

### Результат:
```javascript
{
    "statusCode": 200,
    "success": "OK",
    "message": "Products deleted"
}
or error example
{
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query"
}
```
