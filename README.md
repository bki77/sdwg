# Создание  таблиц в базе данных

## Cоздание таблицы  Отель

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
#### вывод данных в браузер
```
    def __str__(self):
        return self.name
```
#### подмена  названий для отеля
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
#### вывод данных в браузер
```
    def __str__(self):
        return self.type
```    
#### подмена  названий для Комнат
```  
    class Meta:
        verbose_name = 'Комната'
        verbose_name_plural = 'Комнаты'
```
## Таблица клиентов
```
class Clients(models.Model):
    phio = models.CharField('ФИО', max_length=100)
    phone = models.CharField('Телефонный номер', max_length=11)
    email = models.CharField('Email', max_length=100)
    password = models.CharField('Пароль', max_length=200)
    passport_seria = models.IntegerField('Серия паспорта', max_length=100)
    passport_num = models.IntegerField('Номер паспорта', max_length=100)
```
#### вывод данных в браузер
```
    def __str__(self):
        return self.phio
```
#### подмена  названий для 
    class Meta:
        verbose_name = 'Клиент'
        verbose_name_plural = 'Клиенты'
```