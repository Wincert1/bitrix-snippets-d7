<h3>Подключение модулей</h3>

    Bitrix\Main\Loader::includeModule("catalog");

<h3>Работа с корзиной</h3>

Корзина это экземпляр класса Bitrix\Sale\Basket.

    use Bitrix\Sale;
    
Получить корзину текущего пользователя на текущем сайте:

    $basket = Sale\Basket::loadItemsForFUser(Sale\Fuser::getId(), Bitrix\Main\Context::getCurrent()->getSite());
    foreach ($basket as $item) {
         echo $item->getField('NAME') . ' - ' . $item->getQuantity() . '<br />';
    }
    
Перебирать товары можно с помощь foreach
Некоторые методы для работы с товарами корзины:


    $item->getId();         // ID записи в корзине
    $item->getField('NAME');// Любое поле товара в корзине
    $item->getProductId();  // ID товара
    $item->getPrice();      // Цена за единицу
    $item->getQuantity();   // Количество
    $item->getFinalPrice(); // Сумма
    $item->getWeight();     // Вес
    $item->getPropertyCollection(); // Свойства товара в корзине
