[models.py](#models.py)  
[views.py](#views.py)  
[urls.py](#urls.py)  
[admin.py](#Название_Ссылки)  

# <a name="models.py">Model.py от Sergay</a> 

## Создание  таблиц в базе данных

#### Импорты
```
from django.db import models
from django.core.validators import MaxValueValidator, MinValueValidator
```

#### Cоздание таблицы  Hotel

```
class Hotel(models.Model):
    name = models.CharField('Название', max_length=50)
    address = models.CharField('Адрес', max_length=50)
    contact_phone = models.CharField('Контактный номер', max_length=11)
    email = models.CharField('Email', max_length=100)
    description = models.CharField('Описание', max_length=100)
    rating = models.PositiveSmallIntegerField(validators=[MinValueValidator(0), MaxValueValidator(5)], default=0)
    price = models.IntegerField('Цена', max_length=50)
```
#### вывод данных в браузер для таблицы Отель
```
    def __str__(self):
        return self.name
```
#### подмена  названий на Отель, Отели
```
    class Meta:
        verbose_name = 'Отель'
        verbose_name_plural = 'Отели'
```
## Выбор комнаты по её типу
```
class Room(models.Model):
    ROOM_TYPE_CHOICES = [
        (0, 'Одноместный'),
        (1, 'Двуместный'),
        (2, 'Люкс'),
    ]
```
#### параметры для комнаты
```
hotel_id = models.ForeignKey(Hotel, on_delete=models.CASCADE)
    type = models.IntegerField('Тип комнат', choices=ROOM_TYPE_CHOICES)
    minbar = models.BooleanField('Мини-Бар', default=True)
    conditioner = models.BooleanField('Кондиционер', default=True)
```
#### вывод данных в браузер для таблицы Room
```
    def __str__(self):
        return self.type
```    
#### подмена  названий на Комната, Комнаты
```  
    class Meta:
        verbose_name = 'Комната'
        verbose_name_plural = 'Комнаты'
```
## Таблица Clients
```
class Clients(models.Model):
    phio = models.CharField('ФИО', max_length=100)
    phone = models.CharField('Телефонный номер', max_length=11)
    email = models.CharField('Email', max_length=100)
    password = models.CharField('Пароль', max_length=200)
    passport_seria = models.IntegerField('Серия паспорта', max_length=100)
    passport_num = models.IntegerField('Номер паспорта', max_length=100)
```
#### вывод данных в браузер для таблицы Clients
```
    def __str__(self):
        return self.phio
```
#### подмена  названий на Клиент, Клиенты
```
    class Meta:
        verbose_name = 'Клиент'
        verbose_name_plural = 'Клиенты'
```

## таблица Reservations
```
class Reservations(models.Model):
    client_id = models.ForeignKey(Clients, on_delete=models.CASCADE)
    room_id = models.ForeignKey(Room, on_delete=models.CASCADE)
    check_in_date = models.DateTimeField('Дата заезда')
    departure_date = models.DateTimeField('Дата выезда')
    total_amount = models.IntegerField('Общая сумма')
```
#### вывод данных в браузер для таблицы Reservations
```
    def __str__(self):
        return self.client_id
```
#### подмена  названий на Отношения
```
    class Meta:
        verbose_name_plural = 'Отношения'
```

## таблица Reviews_and_ratings
```
class Reviews_and_ratings(models.Model):
    client_id = models.ForeignKey(Clients, on_delete=models.CASCADE)
    hotel_id = models.ForeignKey(Hotel, on_delete=models.CASCADE)
    estimation  = models.IntegerField('Оценка')
    comment = models.CharField('Комментарий', max_length=200)
    date  = models.DateTimeField('Дата публикации')
```
#### вывод данных в браузер для таблицы Reviews_and_ratings
```
    def __str__(self):
        return self.client_id
```
#### подмена  названий на Отзывы и оценки
```
    class Meta:
        verbose_name_plural = 'Отзывы и оценки'
```

# <a name="views.py">Views.py от Sergay</a> 

```
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth.decorators import login_required
from django.contrib.auth import login
from .models import Hotel, Room
```
####
```
def hotel(request):
    return render(request, 'hotel/index.html')
```
####
```
def hotel_info(request):
    return render(request, 'hotel/hotel_info.html')
```
####
```
def booking(request):
    return render(request, 'hotel/booking.html')
```
####
```
def booking_info(request):
    return render(request, 'hotel/booking_info.html')
```
####
```
def login(request):
    return render(request, 'registration/login.html')
```
####
```
def register(request):
    return render(request, 'registration/register.html')
```
####
```
def profile(request):
    return render (request, 'profile/user.html')
```
####
```
def hotel(request):
    hotel = Hotel.objects.all()
    return render(request, 'hotel/index.html', {'hotel': hotel})
```
####
```
def hotel_info(request):
    hotel_info = Room.objects.all()
    return render(request, 'hotel/hotel_info.html', {'hotel_info': hotel_info})
```
####
```
def hotel_info(request, id):
    hotel = get_object_or_404(Hotel, id=id)  # Получаем отель по ID
    return render(request, 'hotel/hotel_info.html', {'hotel': hotel})
```
####
```
def booking(request, id):
    hotel = get_object_or_404(Hotel, id=id)  # Получаем отель по ID
    return render(request, 'hotel/booking.html', {'hotel': hotel})
```
####
```
def booking_info(request, id):
    hotel = get_object_or_404(Hotel, id=id)  # Получаем отель по ID
    return render(request, 'hotel/booking_info.html', {'hotel': hotel})
```
# <a name="urls.py">Urls.py от Sergay</a> 

#### импорты 
```
from django.urls import path
from . import views
```
#### ссылки 
```
urlpatterns = [
    path('', views.hotel, name = 'hotel'),
    path('hotel_info/<int:id>/', views.hotel_info, name='hotel_info'),
    path('booking/<int:id>/', views.booking, name = 'booking'),
    path('booking_info/<int:id>/', views.booking_info, name = 'booking_info'),
    path('login/', views.login, name = 'login'),
    path('register/', views.register, name = 'register'),
    path('profile/', views.profile, name = 'profile'),
]
```

# <a name="admin.py">admin.py от Sergay</a> 

