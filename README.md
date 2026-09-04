# Basic Companies, Prestige Goods & New Goods

Мод для **Victoria 3 1.13.x**. Версия `0.1.0` расширяет систему стандартных компаний: для товарных цепочек, которым не хватает generic prestige-варианта через стандартную компанию, создается отдельная копия подходящей ванильной компании. Права на здания и prosperity-бонусы исходной компании сохраняются.

## A. Generic prestige goods через стандартные компании

Основной принцип:

1. Для товара создается отдельная компания `company_basic_vicmod_*`.
2. За основу берется существующая ванильная `company_basic_*`, связанная с производством этого товара.
3. `building_types`, `extension_building_types`, `possible`, `ai_will_do` и `prosperity_modifier` повторяют исходную компанию.
4. Копия получает свой generic prestige good.
5. Существующие исторические prestige goods ванили не заменяются и продолжают работать параллельно.

### Ресурсы

| Товар | Компания мода | Ванильная основа |
|---|---|---|
| Wood | `company_basic_vicmod_wood` | `company_basic_forestry` |
| Hardwood | `company_basic_vicmod_hardwood` | `company_basic_forestry` |
| Rubber | `company_basic_vicmod_rubber` | `company_basic_forestry` |
| Coal | `company_basic_vicmod_coal` | `company_basic_mineral_mining` |
| Sulfur | `company_basic_vicmod_sulfur` | `company_basic_mineral_mining` |
| Lead | `company_basic_vicmod_lead` | `company_basic_metal_mining` |
| Iron | `company_basic_vicmod_iron` | `company_basic_metal_mining` |
| Gold | `company_basic_vicmod_gold` | `company_basic_gold_mining` |
| Oil | `company_basic_vicmod_oil` | `company_basic_oil` |

### Промышленность и военные товары

| Товар | Компания мода | Ванильная основа |
|---|---|---|
| Tanks | `company_basic_vicmod_tanks` | `company_basic_motors` |
| Aeroplanes | `company_basic_vicmod_aeroplanes` | `company_basic_motors` |
| Ammunition | `company_basic_vicmod_ammunition` | `company_basic_munitions` |
| Engines | `company_basic_vicmod_engines` | `company_basic_motors` |
| Merchant Marine | `company_basic_vicmod_merchant_marine` | см. исключение ниже |

### Сырье и потребительские товары

| Товар | Компания мода | Ванильная основа |
|---|---|---|
| Fabric | `company_basic_vicmod_fabric` | `company_basic_fabrics` |
| Fruit | `company_basic_vicmod_fruit` | `company_basic_wine_and_fruit` |
| Wine | `company_basic_vicmod_wine` | `company_basic_wine_and_fruit` |
| Dye | `company_basic_vicmod_dye` | `company_basic_silk_and_dye` |
| Silk | `company_basic_vicmod_silk` | `company_basic_silk_and_dye` |
| Tea | `company_basic_vicmod_tea` | `company_basic_colonial_plantations_1` |
| Tobacco | `company_basic_vicmod_tobacco` | `company_basic_colonial_plantations_2` |
| Sugar | `company_basic_vicmod_sugar` | `company_basic_colonial_plantations_2` |
| Liquor | `company_basic_vicmod_liquor` | `company_basic_food` |
| Luxury Furniture | `company_basic_vicmod_luxury_furniture` | `company_basic_home_goods` |
| Luxury Clothes | `company_basic_vicmod_luxury_clothes` | `company_basic_textiles` |
| Porcelain | `company_basic_vicmod_porcelain` | `company_basic_home_goods` |
| Fine Art | `company_basic_vicmod_fine_art` | см. исключение ниже |

### Исключения

#### Merchant Marine

Ванильный `prestige_good_generic_merchant_marine` уже существует, поэтому мод **не создает его дубль**. Добавляется только generic standard company, которая использует существующий prestige good и ванильный unlock trigger.

В `99_basic_companies.txt` нет стандартной `company_basic_*`, владеющей Ports. Поэтому профиль сделан по существующим ванильным компаниям Merchant Marine: основной тип здания `building_port`, расширение `building_shipyard`, а prosperity-бонус остается отраслевым.

#### Fine Art

В `99_basic_companies.txt` нет стандартной компании для `building_art_academy`. Поэтому `company_basic_vicmod_fine_art` основана на ванильной `company_ricordi`, но региональная привязка к Ломбардии убрана, чтобы компания была generic. Профиль зданий и prosperity-бонусы сохранены:

- `building_art_academy`;
- extension `building_vineyard`;
- `country_prestige_mult = 0.15`;
- `state_loyalists_from_political_movements_mult = 0.05`.

### Удалено по уточнению ТЗ

Из блока generic prestige companies полностью убраны:

- `manowars`;
- `clippers`.

Мод не добавляет для них ни prestige goods, ни отдельные standard companies.

## B. Новые товары

Исходный блок новых товаров сохранен.

### Spices

- производятся чайными и кофейными плантациями;
- потребляются населением как luxury food;
- используются Food Industries;
- standard company: `company_basic_spices`;
- prestige good: `prestige_good_vicmod_generic_spices`.

### Horses

- производятся Livestock Ranches;
- используются населением в `popneed_free_movement`;
- standard company: `company_basic_horses`;
- prestige good: `prestige_good_vicmod_generic_horses`.

### Machine Tools

- производятся Tooling Workshops;
- требуют Steel и обычных Tools;
- используются Steel, Motor и Electrics Industries;
- standard company: `company_basic_machine_tools`;
- prestige good: `prestige_good_vicmod_generic_machine_tools`.

### Copper

- пока производится как попутный продукт Iron и Lead Mines;
- используется Motor и Electrics Industries;
- standard company: `company_basic_copper`;
- prestige good: `prestige_good_vicmod_generic_copper`.

## Структура файлов

```text
.metadata/metadata.json
common/
  buildings/vicmod_building_injections.txt
  company_types/
    vicmod_new_goods_companies.txt
    vicmod_prestige_companies_resource.txt
    vicmod_prestige_companies_industry.txt
    vicmod_prestige_companies_consumer.txt
  goods/vicmod_new_goods.txt
  modifier_type_definitions/vicmod_goods_modifiers.txt
  pop_needs/vicmod_pop_needs.txt
  prestige_goods/vicmod_generic_prestige_goods.txt
  production_method_groups/vicmod_new_pmg.txt
  production_methods/vicmod_new_pm.txt
localization/
  english/vicmod_l_english.yml
  russian/vicmod_l_russian.yml
README.md
```

## Совместимость и ограничения

- Целевая версия: Victoria 3 `1.13.*`.
- `services`, `transportation` и `electricity` остаются вне этой системы. Это local goods, для которых обычная схема prestige goods не работает корректно.
- Новые generic prestige goods используют временно существующие ванильные DDS соответствующих товаров. Иконки можно заменить позже без изменения экономики.
- Баланс новых товаров из блока B остается стартовым и требует игрового теста.

## Что проверить в игре

1. Игра запускается без ошибок парсинга в `error.log`.
2. Все `company_basic_vicmod_*` отображаются среди доступных типов компаний при выполнении условий.
3. Копии сохраняют те же права на здания, что их ванильные основы.
4. Prosperity-бонусы копий совпадают с исходными компаниями.
5. При доступности prestige goods компания может перейти на соответствующий generic prestige good.
6. `company_basic_vicmod_merchant_marine` использует ванильный `prestige_good_generic_merchant_marine`.
7. `manowars` и `clippers` отсутствуют среди добавленных модом prestige goods и компаний.
8. Spices, Horses, Machine Tools и Copper продолжают производиться и создавать спрос по своим цепочкам.

## Статус

`0.1.0`: второй проход по ТЗ с расширенным покрытием generic prestige goods и переходом от `INJECT` к отдельным копиям компаний.
