
#Импортирование библиотек

"""
from django.db import models
from django.core.validators import MaxValueValidator, MinValueValidator

"""
#Создание таблиц

"""
class Hotel(models.Model):
    name = models.CharField('Название', max_length=50)
    address = models.CharField('Адрес', max_length=50)
    contact_phone = models.CharField('Контактный номер', max_length=11)
    email = models.CharField('Email', max_length=100)
    description = models.CharField('Описание', max_length=100)
    rating = models.PositiveSmallIntegerField(validators=[MinValueValidator(0), MaxValueValidator(5)], default=0)
    price = models.IntegerField('Цена', max_length=50)
"""

#
def __str__(self):
        return self.name
