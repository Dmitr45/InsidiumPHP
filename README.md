# InsidiumPHP
 Тестовое задание на вакансию


# Перенос лендинга Insidium из архива Тильды на сервер (хостинг)

Пример правильной реализации HTML содержимого можно посмотреть по [ссылке](https://lp.insidium.ru/sorokadmitriy/lk/22/sp/). 

# **Задание**

**Вводные данные:** вам передается ссылка на страницу и архив 

[project6447254_1719915827.zip](https://prod-files-secure.s3.us-west-2.amazonaws.com/85c823f8-5c7c-41df-ad7a-eb1b67148bc1/1bf713a4-1f51-4bf5-8157-5571e29d0757/project6447254_1719915827.zip)

Это HTML страница, которую сверстал дизайнер в Tilda. Архив содержит в себе HTML файл, стили и скрипты, т.е. визуальная часть полностью готова. Нужно перенести страницу на хостинг и сделать ряд настроек для корректной работы сайта, визуальная часть должна остаться такой же, как ее сделал дизайнер. 

**Задачи:**

1. настроить HTML по требованиям, которые указаны ниже;
2. настроить отображение содержимого страницы по расписанию, используя php код. Расписание будет указано после ТЗ на HTML.

**Результат:** 

- передать ссылку на страницу, где вы все настроили по ТЗ;
- передать FTP данные для доступа к папке сайта, чтобы можно было проверить ваш разработанный php код.

---

- **Требования к HTML**
    
    
    В начало документа добавляем код защиты от спама (до !Doctype).
    
    ```php
    <?
    if (!(isset($_COOKIE["session_hash"]))) {
    $sessionHash = mt_rand();
    setcookie('session_hash', $sessionHash,time() + (86400 * 3),'/');
    }
    ?>
    ```
    
    ---
    
    ## SEO
    
    **На лендингах обязательно нужно проверить наличие:**
    
    **title** - название мероприятия (Пример - «Тренинг личностного роста»)
    
    ```jsx
    <title>Название страницы</title>
    ```
    
    **description** - УТП, до 200 символов с пробелами
    
    ```jsx
    **<meta name="description" content="Тренинг, который позволит вам ...." />**
    ```
    
    Если в переданном коде данных мета-тегов нет, то нужно их добавить.
    
    ---
    
    Канонический адрес не меняем, оставляем тот, который прописан в переданном файле. 
    
    ```jsx
    **<link rel="canonical" href="https://....">**
    ```
    
    Проверяем заголовок <**H1>** - (пример «Тренинг личностного роста»). Если его нет, то в него оборачиваем заголовок на первом экране
    
    ```jsx
    <h1 class='tn-atom' field='tn_text_...'>Тренинг личностного роста</h1>
    ```
    
    Также, если на лендинге есть h2 и h3, то они идут в строгом хронологическом порядке - h3 вложен в блок с заголовком h2.
    
    ---
    
    Mеняем (или добавляем, если отсутствует) фавикон - берется из корня хостинга (можно взять любой, так как это тестовое задание)
    
    ```jsx
    <link rel="shortcut icon" href="/favicon.ico" type="image/x-icon">
    ```
    
    Вставляем Google Tag Manager (GTM) и скрипт для обработки форм и пр. - landing.js (его можно скачать по [ссылке](https://lp.insidium.ru/sorokadmitriy/lk/22/sp/) и подключить к вашему проекту, либо просто подключить по ссылке). 
    
    **Скрипты подключаем внутри <head>:**
    
    ```jsx
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','GTM-535W5LJ');</script>
    <!-- End Google Tag Manager -->
    <script src="/scripts/landing.js"></script>
    ```
    
    **В самое начало <body> добавляем вторую часть GTM:**
    
    ```jsx
    <!-- Google Tag Manager (noscript) -->
    <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-535W5LJ"
    height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
    <!-- End Google Tag Manager (noscript) -->
    ```
    
    ---
    
    ## Подключение форм на странице
    
    Есть 2 типа форм - форма в попапе и простая на странице в блоке. Попап-форму нужно “очистить” от классов тильды, правил обработки полей тильды, добавить обработчик, добавить атрибуты для полей ввода. Стили менять не нужно. Пример правильной реализации можно посмотреть по [ссылке](https://lp.insidium.ru/sorokadmitriy/lk/22/sp/). 
    
    Форма внутри блока вставлена через скрипты Tilda, она не будет работать в вашей версии (даже визуально не отобразится). Скрипт Tilda для генерации формы нужно удалить, а затем самостоятельно сделать верстку формы и настроить стили так, чтобы ваша форма соответствовала форме на исходной странице, которую верстал дизайнер в Tilda. 
    
    ## Дополнительные скрипты
    
    Перед закрывающим тегом </body> добавляем скрипты
    
    ```jsx
    <script>
    function getCookie(name) {
      var matches = document.cookie.match(new RegExp(
        "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
      ));
      return matches ? decodeURIComponent(matches[1]) : undefined;
    }
    
    $(document).ready(function () {
     if (getCookie('session_hash')) {
    	$("button[type=submit]").before('<input required type="hidden" name="hash" value="' + getCookie('session_hash') + '">');	
     } else {
       let forms = document.getElementsByTagName('form');
    
       if (forms.length > 0) {
         for (let  i = 0; i < forms.length; i++) {
           
          let form = forms[i],
              action = form.getAttribute('action');
    
          if (action == '/send_form_api_def.php') form.setAttribute('action', '/send_form_api.php');   
          if (action == '/send_form_def.php') form.setAttribute('action', '/send_form.php');   
          
         }
       }
     }
    });
    
    $('button[type=submit]').on('click', function(){
    	var btn = $('button[type=submit]');	/*или input[type=submit]*/
    	btn.css("cursor", "not-allowed");
    	btn.css("pointer-events", "none");
    	
    	setTimeout(function() {
    	btn.css("cursor", "pointer");
    	btn.css("pointer-events", "auto");
    	}, 2500);
    });
    </script>
    ```
    
    ---
    
- Настройка расписания цен и отображения блоков
    
    ### Задание 1
    
    Нужно сделать, чтобы с каждой среды с 23:59:59 до каждой субботы  до 23:59:59 были следующие цены
    
    Standart:
    Основная цена- 4 900; Зачеркнутая цена - 27 700
    
    Business:
    
    Основная цена- 9 900; Зачеркнутая цена - 46 300
    
    Vip:
    
    Основная цена- 19 900; Зачеркнутая цена - 66 400
    
    Premium: 
    
    Основная цена- 29 900; Зачеркнутая цена - 99 700
    
    А с каждой субботы с 23:59:59 до каждой среды до 23:59:59 были следующие цены
    
    Standart:
    Основная цена- 9 900; Зачеркнутая цена - 37 700
    
    Business:
    
    Основная цена- 14 900; Зачеркнутая цена - 56 300
    
    Vip:
    
    Основная цена- 29 900; Зачеркнутая цена - 76 400
    
    Premium: 
    
    Основная цена- 39 900; Зачеркнутая цена - 119 700
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/85c823f8-5c7c-41df-ad7a-eb1b67148bc1/bd02493c-39e6-4da9-b25c-d6cf31a56b33/Untitled.png)
    
    ### Задание 2
    
    Нужно, чтобы каждый вторник, четверг и субботу данный блок был скрыт, в остальные дни он должен отображаться на странице