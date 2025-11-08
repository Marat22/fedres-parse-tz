# Задача
Нужно написать генератор:
- Генератор принимает ссылку на страницу поиска федресурса (прим.: `"https://fedresurs.ru/encumbrances?searchString="`)
- Генератор позволяет итерироваться по карточкам на странице (карточек может быть 1000+)
- Если всё же считаете, что лучше реализовать не через генератор, а как-то по-другому, то проконсультируйтесь сначала со мной

Код должен выглядеть примерно так:
```python
@dataclass
class Card:
    url: str
    """Ссылка на страницу, которая откроется при нажатии на карточку"""
    publish_date: datetime.date
    """Дата публикации"""
    lessor: str
    """Либо название лизингодателя, либо "Сведения скрыты""""
    lessee: str
    """Либо название лизингополучателя, либо "Сведения скрыты""""
    annulled: bool = False
    """Аннулировано сообщение или нет. Пока что пусть всегда будет False"""


def all_cards(link: str) -> Generator[Card, None, None]:
    yield Card(
        url="https://fedresurs.ru/sfactmessages/3bb2d632-5401-4cd5-ae0a-e1a09e5f0ee4",
        publish_date="08.11.2025",
        lessor="Сведения скрыты",
        lessee="ООО \"БИЛД СИСТЕМ ГРУПП\"",
    )
```

## Пример
Например, в all_cards была передана ссылка "https://fedresurs.ru/encumbrances?group=Leasing&period=%7B%22beginJsDate%22:%222022-10-09T19:00:00.000Z%22,%22endJsDate%22:%222022-10-09T19:00:00.000Z%22%7D&limit=15&offset=0"

При парсинге первой карточки:
![alt text](example-image.png)

... получится такой `Card`:
```python
Card(
    url="https://fedresurs.ru/sfactmessages/9bd2fc1b-a979-4cca-8e6b-469c7a69c9c4",
    publish_date="08.11.2025",
    lessor="ООО \"Алина\"",
    lessee="ООО \"Интерлизинг\"",
)
```

# Тестирование
Пройдитесь по всем карточкам из:
https://fedresurs.ru/encumbrances?group=Leasing&period=%7B%22beginJsDate%22:%222023-10-31T19:00:00.000Z%22,%22endJsDate%22:%222023-11-30T19:00:00.000Z%22%7D&limit=15&offset=0

Убедитесь, что действительно получилось больше тысячи

# Требования
- Версия python - 3.11
- Версии библиотек берите из прикрепленного файла [requirements.txt](./requirements.txt)
- Код должен быть задокументирован. Каждая функция должна иметь релевантный docstring (google styleguide)
- В каждой функции должны быть type hints
