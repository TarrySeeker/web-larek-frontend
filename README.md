#Веб-приложение для интернет-магазина
Это веб-приложение разработано для интернет-магазина "WebLarek". Оно позволяет пользователям просматривать каталог товаров, добавлять их в корзину, оформлять заказы и многое другое.

#Основные функции
Просмотр каталога товаров.
Добавление товаров в корзину.
Управление корзиной (удаление товаров, изменение количества).
Оформление заказа с указанием адреса доставки и контактной информации.
Уведомление пользователя о результате заказа.
-

# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/scss/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

## Об архитектуре 

Взаимодействия внутри приложения происходят через события. Модели инициализируют события, слушатели событий в основном коде выполняют передачу данных компонентам отображения, а также вычислениями между этой передачей, и еще они меняют значения в моделях.

# Основные типы данных

## Интерфейс IPage.
Этот интерфейс определяет основные характеристики главной страницы:
```
interface IPage {
    catalogItems: HTMLElement[]; // Массив элементов каталога на странице
    isLocked: boolean; // Флаг, указывающий, заблокирована ли страница
}

```

## Интерфейс IProductItem.
Этот интерфейс содержит информацию о конкретном товаре:
```
interface IProductItem {
    id: string; // Уникальный идентификатор товара
    description: string; // Описание товара
    image: string; // Путь к изображению товара
    title: string; // Наименование товара
    category: string; // Категория товара
    price: number | null; // Цена товара (может быть null, если цена не определена)
}

```

## Интерфейс ICardActions.
Этот интерфейс определяет действия, которые могут быть выполнены с карточкой товара:
```
interface ICardActions {
    onClick: (event: MouseEvent) => void; // Функция, вызываемая при клике на карточку товара
}

```

## Интерфейс ICard.
Этот интерфейс описывает карточку товара:
```
interface ICard {
    buttonText: string; // Текст на кнопке карточки
    itemCount: number | null; // Количество товаров в карточке (может быть null, если количество неизвестно)
}

```

## Интерфейс IModalData.
Этот интерфейс описывает содержимое модального окна:
```
interface IModalData {
    content: HTMLElement; // Содержимое модального окна в виде элемента HTML
}

```

## Интерфейс IBasketView.
Этот интерфейс описывает содержимое корзины:
```
interface IBasketView {
    items: HTMLElement[]; // Элементы, представляющие товары в корзине
    total: number | string; // Общая сумма стоимости товаров в корзине (может быть числом или строкой)
    selected: string[]; // Идентификаторы выбранных товаров в корзине
}

```

## Интерфейс IFormState.
Этот интерфейс описывает состояние полей формы:
```
interface IFormState {
    valid: boolean; // Флаг, указывающий на корректность заполнения формы
    errors: string[]; // Массив сообщений об ошибках заполнения формы
}

```

## Интерфейс IOrderForm.
Описывает адрес доставки.
```
interface IOrderForm {
  payment: string;
  address: string;
}

```

## Интерфейс IContactsForm.
Этот интерфейс описывает данные о заказе, включая адрес доставки и способ оплаты:
```
interface IOrderForm {
    payment: string; // Способ оплаты заказа
    address: string; // Адрес доставки заказа
}

```

## Интерфейс IOrder расширяет интерфейс IOrderForm.
Этот интерфейс расширяет интерфейсы IOrderForm и IContactsForm и добавляет дополнительные поля для описания заказа:
```
interface IOrder extends IOrderForm, IContactsForm {
    total: number | string; // Общая сумма заказа (может быть числом или строкой)
    items: string[]; // Массив идентификаторов товаров в заказе
}

```

## Интерфейс IContacts расширяет интерфейс IContactsForm.
Этот интерфейс расширяет интерфейс IContactsForm и добавляет дополнительное поле для описания контактной информации:
```
interface IContacts extends IContactsForm {
    items: string[]; // Массив элементов контактной информации
}

```

## Интерфейс ISuccess.
Этот интерфейс описывает информацию об успешном завершении заказа:
```
interface ISuccess {
    id: string; // Уникальный идентификатор завершенного заказа
    total: number; // Общая сумма завершенного заказа
}

```

---

# Базовый код

## Класс Model<T>
Абстрактный базовый класс, для управления данными и их взаимодействия с системой событий.
```
- isModel - функция для проверки на модель.
```
Содержит метод:
- emitChanges(event: string, payload?: object) - cообщить всем что модель поменялась.

## Класс Component<T>
Абстрактный базовый класс, предоставляет инструментарий для работы с DOM в дочерних компонентах.
container: HTMLElement: Это элемент DOM, который будет использоваться как контейнер для компонента. В этот контейнер будут добавляться создаваемые элементы, а также применяться различные операции DOM-манипуляции.
```
const containerElement = document.getElementById('container');
const component = new MyComponent(containerElement);
```
element: HTMLElement - элемент DOM, у которого нужно переключить класс.
className: string - имя класса, которое нужно переключить.
force?: boolean - необязательный параметр, указывающий, нужно ли явно добавить или удалить класс (true - добавить, false - удалить). Если не указан, то класс будет переключен.

- toggleClass(element: HTMLElement, className: string, force?: boolean) - переключить класс.
- setText(element: HTMLElement, value: unknown) - установить текстовое содержимое.
- setDisabled(element: HTMLElement, state: boolean) - сменить статус блокировки.
- setHidden(element: HTMLElement) - скрыть элемент.
- setVisible(element: HTMLElement) - показать элемент.
- setImage(element: HTMLImageElement, src: string, alt?: string) - установить изображение с алтернативным текстом.
- render(data?: Partial<T>): HTMLElement - вернуть корневой DOM-элемент.

## Класс EventEmitter
Базовый класс, центральный брокер событий. Позволяет компонентам подписываться на события и реагировать на них.
В конструктор класса EventEmitter приходит один параметр:
container: HTMLElement: Это элемент DOM, который будет использоваться как контейнер для управления событиями. В этот контейнер будут добавляться обработчики событий, и именно он будет являться точкой входа для взаимодействия с событиями в рамках экземпляра класса EventEmitter.
```
const eventContainer = document.getElementById('event-container');
const eventEmitter = new EventEmitter(eventContainer);
```
- on<T extends object>(eventName: EventName, callback: (event: T) => void) - установить обработчик на событие.
- off(eventName: EventName, callback: Subscriber) - снять обработчик с события.
- emit<T extends object>(eventName: string, data?: T) - инициировать событие с данными.
- onAll(callback: (event: EmitterEvent) => void) - слушать все события.
- offAll() - сбросить все обработчики.
- trigger<T extends object>(eventName: string, context?: Partial<T>) - сделать коллбек триггер, генерирующий событие при вызове.

## Класс Api
```
const api = new Api();
```
Базовый класс - клиент для взаимодействия с внешними API и сервером. Содержит методы:
- handleResponse(response: Response): Promise<object> - обработчик ответа с сервера.
- get(uri: string) - получить ответ с сервера.
- post(uri: string, data: object, method: ApiPostMethods = 'POST') - отправить данные на сервер.

---

# View - компоненты представления

## Класс Card
Класс для создания карточки товара. Наследует класс Component. Содержит сеттеры и геттеры:
- set id(value: string) - установить id товара.
- get id(): string - получить id товара.
- set title(value: string) - установить название товара.
- get title(): string - получить название товара.
- set image(value: string) - установить url изображения товара.
- set category(value: string) - установить категорию товара.
- set price(value: number | null) - установить цену товара.
- set buttonText(status: string) - установить текст кнопки в карточке товара.
- set description(value: string) - установить описание товара.
- set index(value: string) - установить индекс товара.
- get index(): string - получить индекс товара.

## Класс Form<T>
Класс для управления формами. Наследует класс Component. Содержит методы:
- onInputChange(field: keyof T, value: string) - вызывается при изменении значений полей формы.
- render(state: Partial<T> & IFormState) - возвращает форму с новым состоянием.

Сеттеры:
- set valid(value: boolean) - установить валидацию полей.
- set errors(value: string) - установить вывод информации об ошибках.

## Класс Order
Класс предназначен для выбора способа оплаты и ввода адреса доставки. Наследует класс Form. Содержит метод:
- selectPaymentMethod(name: HTMLElement) - выбрать способ оплаты.

Сеттер:
- set address(value: string) - адрес доставки.

## Класс Contacts
Класс предназначен для управления формой контактных данных пользователя. Наследует класс Form. Содержит сеттеры:
- set phone(value: string) - телефон пользователя
- set email(value: string) - эл. почта пользователя.

## Класс Page
Класс управления элементами интерфейса главной страницы. Наследует базовый класс Component. Содержит сеттеры:
- set counter(value: number | null) - обновить счётчик количества товаров в корзине.
- set catalog(items: HTMLElement[]) - вывести каталог товаров.
- set locked(value: boolean) - установить или снять блокировку прокрутки страницы.

## Класс Basket
Класс управляет отображением корзины. Наследует класс Component. Содержит сеттеры:
- set items(items: HTMLElement[]) - вставить данные в корзину.
- set selected(items: ProductItem[]) - установить наличие товаров в корзине.
- set total(total: number | string) - установить общую сумму товаров в корзине.

## Класс Modal
Класс управления поведением модальных окон. Наследует класс Component. Содержит сеттер:
- set content(value: HTMLElement) - установить содержимое модального окна.

Методы:
- open() - открыть окно.
- close() - закрыть окно.
- render(data: IModalData): HTMLElement - вывести данные.

## Класс Success
Класс отображения успешного завершения процесса оплаты.
- set id() - id заказа.
- set total(total: number | string) - отобразить общую сумму товаров.

---

# Model - компоненты данных

## Класс ProductItem
Наследует класс Model. Реализует экземпляр товара.
```

class ProductItem extends Model<IProductItem> {
	id: string;
	description: string;
	image: string;
	title: string;
	category: string;
	price: number | null;
	status: ProductStatus = 'sell';
	itemCount: number = 0;
}

```

## Класс AppState
Центральный класс для управления данными и событиями. Наследует класс ProductItem. Содержит методы:
- clearBasket() - очистить корзину.
- getTotal() - получить общую сумму заказа.
- setCatalog(items: IProductItem[]) - установить список товаров.
- setOrderField(field: keyof IOrderForm, value: string) - отслеживать изменения полей заказа.
- validateOrder() - валидация полей заказа.
- setContactsField(field: keyof IContactsForm, value: string) - отслеживать изменения полей контактной информации.
- validateContacts() - валидация полей контактной информации.
- toggleBasketList(item: ProductItem) - удалить или добавить товар в список корзины.
- getBasketList(): ProductItem[] - получить список корзины.

## Класс WebLarekAPI.
Класс для связи и получения информации с сервера. Наследует базовый класс Api.
Содержит метод:
- getProductList(): Promise<IProductItem[]> - получить список товаров с сервера.
- orderResult(order: IOrder): Promise<ISuccess> - отправить результат заказа.