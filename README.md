# Vic3 MP Mod - Basic Companies, Prestige Goods и новые товары

Мод для **Victoria 3 1.13.x**. Ветка `0.1.0` содержит реализацию двух частей ТЗ:

1. полный аудит ванильных товаров на наличие prestige goods и создание недостающих вариантов через копии ванильных basic companies;
2. четыре новых товара (`spices`, `horses`, `machine_tools`, `copper`) с производством, спросом и стандартными компаниями.

## A. Полный аудит prestige goods

Аудит выполнен по ванильному `00_goods.txt` из ТЗ и ванильному `00_prestige_goods.txt`.

Всего в `00_goods.txt` найдено **53 товара**. У **40** уже есть хотя бы один prestige good. Без prestige-варианта оставались **13 товаров**:

- `ammunition`;
- `tanks`;
- `aeroplanes`;
- `manowars`;
- `wood`;
- `services`;
- `transportation`;
- `electricity`;
- `clippers`;
- `coal`;
- `lead`;
- `rubber`;
- `gold`.

Из этих 13 **10 являются обычными/нелокальными товарами и реализованы как новые prestige goods**. Три оставшихся (`services`, `transportation`, `electricity`) имеют `local = yes`; prestige goods для local goods не работают в текущей механике Victoria 3, поэтому создавать неработающие определения ради формальной полноты не стали.

### Копии ванильных базовых компаний

Для 10 товаров существует ванильная `company_basic_*`, которая владеет зданием, способным производить соответствующий базовый товар. Для каждого такого товара создана **отдельная копия** исходной компании.

| Товар | Новая компания | Копия vanilla |
|---|---|---|
| `wood` | `company_basic_vicmod_wood` | `company_basic_forestry` |
| `rubber` | `company_basic_vicmod_rubber` | `company_basic_forestry` |
| `coal` | `company_basic_vicmod_coal` | `company_basic_mineral_mining` |
| `lead` | `company_basic_vicmod_lead` | `company_basic_metal_mining` |
| `gold` | `company_basic_vicmod_gold` | `company_basic_gold_mining` |
| `clippers` | `company_basic_vicmod_clippers` | `company_basic_shipyards` |
| `manowars` | `company_basic_vicmod_manowars` | `company_basic_shipyards` |
| `tanks` | `company_basic_vicmod_tanks` | `company_basic_motors` |
| `aeroplanes` | `company_basic_vicmod_aeroplanes` | `company_basic_motors` |
| `ammunition` | `company_basic_vicmod_ammunition` | `company_basic_munitions` |

У копий **не менялись**:

- `building_types`;
- `extension_building_types`;
- `possible`;
- `ai_will_do`;
- `prosperity_modifier`;
- category, icon, background и набор dynamic naming исходной компании.

То есть права на постройки и prosperity-бонусы буквально повторяют vanilla. Изменены только `possible_prestige_goods` и `prestige_goods_trigger`, потому что для новых prestige goods не существует ванильных Journal Entry variables.

### Три локальных товара без vanilla basic company

`services`, `transportation` и `electricity` **не получили prestige-good definitions и компании-копии**.

Причина техническая:

- `services` производятся Urban Center;
- `transportation` производятся Railway;
- `electricity` производится Power Plant;
- в ванильном `99_basic_companies.txt` нет `company_basic_*`, имеющей права соответственно на Urban Center, Railway или Power Plant;
- главное: эти три товара имеют `local = yes`, а prestige goods не работают для local goods.

Поэтому даже создание новой компании с измененными building rights не решает исходную задачу без более глубокой переделки самих goods. В текущей версии они честно отмечены как исключение движка, а не изображены "реализованными".

### Deprecated `manowars`

`manowars` помечен в `00_goods.txt` как deprecated, но включен в аудит намеренно. Формулировка ТЗ требует пройти **все товары**, а vanilla уже содержит prestige good для другого deprecated-товара `ironclads`. Поэтому исключать `manowars` только по пометке deprecated было бы произвольным решением.

## B. Новые товары

### Специи (`spices`)

- производятся чайными и кофейными плантациями;
- потребляются населением как luxury food;
- используются Food Industries;
- компания: `company_basic_spices`;
- prestige good: `prestige_good_vicmod_generic_spices`.

### Лошади (`horses`)

- производятся Livestock Ranch;
- входят в `popneed_free_movement`;
- компания: `company_basic_horses`;
- prestige good: `prestige_good_vicmod_generic_horses`.

### Станки (`machine_tools`)

- производятся Tooling Workshops;
- требуют steel и tools;
- используются Steel, Motor и Electrics Industries;
- компания: `company_basic_machine_tools`;
- prestige good: `prestige_good_vicmod_generic_machine_tools`.

### Медь (`copper`)

- реализована как попутный выпуск Iron/Lead Mines;
- используется Motor и Electrics Industries;
- компания: `company_basic_copper`;
- prestige good: `prestige_good_vicmod_generic_copper`.

## Почему медь пока не отдельная шахта

Отдельный `building_copper_mine` требует распределить resource caps по state regions. Без этого здание формально существует, но строить его негде. Поэтому в первой версии медь выделена в отдельный PMG попутного извлечения. Позже это можно заменить полноценными месторождениями без переделки товара и потребителей.

## Структура

```text
.metadata/metadata.json
common/
  buildings/vicmod_building_injections.txt
  company_types/vicmod_prestige_companies_resource.txt
  company_types/vicmod_prestige_companies_industry.txt
  company_types/vicmod_new_goods_companies.txt
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

## Проверка в игре

1. Игра запускается без ошибок парсинга в `error.log`.
2. Все 10 новых copies отображаются в доступных generic companies при выполнении vanilla-условий исходной компании.
3. Каждая copy сохраняет те же building rights и prosperity bonus, что ее vanilla-источник.
4. При prosperity соответствующая copy может выпускать свой prestige good.
5. `manowars` и `gold` не вызывают ошибок системы prestige goods.
6. `services`, `transportation`, `electricity` не затронуты, поскольку являются local goods и не поддерживают рабочую prestige-good механику.
7. Новые товары `Spices`, `Horses`, `Machine Tools`, `Copper` присутствуют и имеют устойчивые источники спроса и предложения.

## Статус

`0.1.0` - исправленный проход по уточненному ТЗ. Баланс новых товаров и окончательное решение по трем локальным товарам требуют игрового smoke-test и, при необходимости, отдельного согласования.
