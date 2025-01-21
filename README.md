<h3>Подключение модулей</h3>

    Bitrix\Main\Loader::includeModule("catalog");

<h3>Подключение скриптов и стилей</h3>

    use Bitrix\Main\Page\Asset; 

    Asset::getInstance()->addJs(SITE_TEMPLATE_PATH . '/js/.js'); 
    Asset::getInstance()->addCss(SITE_TEMPLATE_PATH . '/css/.css'); 
    Asset::getInstance()->addString('<meta itemprop="name" content="Название сайта"/>');

<h3>GetList элементов ИБ</h3>

use \Bitrix\Iblock\Elements\ElementPhotouploadsTable;

$rs = ElementPhotouploadsTable::getList([
        'order' => ['ID' => 'DESC'],
        'select' => ['ID', 'NAME', 'FILES_LIST', 'OBJECT.VALUE'],
        'filter' => ['=ACTIVE' => 'Y', 'OBJECT.VALUE' => [3589, 44510]],
//        'limit' => 1
    ]);
    while ($ar = $rs->fetch()) {
        p($ar);
    }

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
    $basketItems = $basket->getBasketItems(); // массив объектов Sale\BasketItem
    
Свойства товаров в корзине:
    
    $basketProps = $item->getPropertyCollection(); //Свойства товара в корзине
    $basketProps = $basketProps->getPropertyValues(); //Массив свойств товара
    
Добавить свойство:

    $basketProps->setProperty(array(
        array(
            'NAME' => 'Стиль',
            'CODE' => 'STYLE',
            'VALUE' => 'Модерн',
            'SORT' => 100,
        ),
    ));
    $basketProps->save();
    
Удалить свойство:

    foreach ($basketProps as $item) {
        if ($item->getField('CODE') == 'STYLE') {
            $propertyItem->delete();
            break;
        }
    }
    $basketProps->save();
