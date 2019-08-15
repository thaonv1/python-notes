# Khởi tạo project trong django

- Tạo thư mục

```
mkdir django_projects
cd django_projects
```

- Trong thư mục này ta khởi tạo tiếp thư mục chứa source

```
mkdir src
```

- Tại thư mục chứa source, ta khởi tạo mtr


`python3.6 -m venv env`

- Lúc này sẽ có một thư mục được tạo, ta kích hoạt mtr bằng câu lệnh

`source env/bin/activate`

- Ta sẽ tiến hành khởi tạo project django

```
django-admin startproject locallibrary
cd locallibrary
```

- Khởi tạo app bằng câu lệnh

`python manage.py startapp catalog`

- Sau khi tạo, cần đăng kí app với catalog

Sửa file `settings.py` và thêm vào phần `INSTALLED_APPS`

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'catalog',
]
```

## Kết nối db MYSQL

```
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {
            'read_default_file': '/path/to/my.cnf',
        },
    }
}


# my.cnf
[client]
database = NAME
user = USER
password = PASSWORD
default-character-set = utf8
```

Hoặc có thể thêm các option luôn

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME':,
        'USER':,
        'PASSWORD':,
        'HOST':,
        'PORT':
    }
}
```

Để tạo migration

`python manage.py makemigrations`

Để tạo db  

`python manage.py migrate`

Run server

`python manage.py runserver`
