# Vic3 MP Mod

Версия **0.2.0** для Victoria 3 `1.13.*`.

`0.2.0` основана на `0.1.0`: весь ранее добавленный блок standard companies, prestige goods и новых товаров сохранен. Главная новая часть версии - стартовый сетап зданий для `UKR`, `BYE` и `POL` по issue #1, а также отдельная польская армия.

## Что изменилось в 0.2.0

### Общие правила реализации

- Для `UKR` и `BYE` существующие стартовые здания **не удаляются**. Требуемые уровни добавляются поверх существующей истории, как прямо указано в issue.
- Для `POL` сельское хозяйство и здания развития добавляются, а текущая **обрабатывающая промышленность** в целевых польских регионах удаляется на старте и заменяется заданным набором.
- Под формулировкой «2 метод производства» используется второй нормальный/доступный вариант основного PMG, а не скрытый региональный вариант. Для Wheat Farms используется `pm_soil_enriching_farming`, потому что формально второй элемент текущего vanilla-списка `pm_herring_meal_farming` доступен только в Японии.
- «Литва» из блока `BYE` сопоставлена с `STATE_KAUNAS`, так как именно этот state локализован в vanilla как `Lithuania`.
- «Вармия» из блока `POL` сопоставлена с `STATE_EAST_PRUSSIA`, который при польском динамическом названии отображается как `Warmia`.
- `STATE_BESSARABIA` включена в общий сельскохозяйственный пакет `UKR`, потому что issue отдельно относит Бессарабию к этому тегу.

## UKR

Для общего сельскохозяйственного блока диапазоны из issue (`25-45` Wheat Farms и `20-40` Livestock Ranches) зафиксированы внутри заданных границ:

| Регион | Wheat Farms | Livestock Ranches | Vineyards | Fishing Wharves |
|---|---:|---:|---:|---:|
| Kyiv | 45 | 40 | 10 | 15 |
| East Galicia | 45 | 35 | 10 | 15 |
| Taurida | 40 | 30 | 10 | 15 |
| Cherson | 40 | 30 | 10 | 15 |
| Kharkov | 35 | 30 | 10 | 15 |
| Volhynia | 35 | 25 | 10 | 15 |
| Crimea | 25 | 20 | 10 | 15 |
| Chernihiv | 30 | 25 | - | 15 |
| Bessarabia | 30 | 25 | 10 | 15 |

Виноградники добавляются только там, где vanilla state resources разрешают `building_vineyard`. В Chernihiv виноградник не добавлен.

Дополнительно:

- **Kyiv**: Coal Mine 5, Logging Camp 10, Iron Mine 3, Paper Mill 2, Furniture Manufactory 1, Tooling Workshop 2, Glassworks 1 + Ceramics, Trade Center 6, University 2, Construction Sector 3.
- **Kharkov**: Logging Camp 5, Iron Mine 1, Construction Sector 1.
- **Chernihiv**: Logging Camp 3.
- **Bessarabia**: Textile Mill 1.
- **Crimea**: Trade Center 6.

## BYE

Для столицы Minsk:

- Rye Farms 20, второй основной PM;
- Livestock Ranches 15, второй основной PM;
- Iron Mine 2;
- Logging Camp 7;
- Glassworks 3 + Ceramics;
- Tooling Workshop 1;
- University 5;
- Art Academy 5;
- Construction Sector 1.

Для остальных поддержанных регионов BYE:

- Mogilev: Rye Farms 10, Livestock Ranches 5, Food Industry 1;
- Brest: Rye Farms 10, Livestock Ranches 5;
- Vilnius: Rye Farms 10, Livestock Ranches 5;
- Kaunas / Lithuania: Rye Farms 10, Livestock Ranches 5, Shipyard 1, Trade Center 9.

## POL

Сельское хозяйство повторяет схему BYE:

- Warsaw / Mazovia: Rye Farms 20, Livestock Ranches 15;
- остальные польские регионы и Warmia: Rye Farms 10, Livestock Ranches 5.

В столице (`STATE_MAZOVIA`) добавлены University 2, Trade Center 5, Government Administration 4.

В Warmia (`STATE_EAST_PRUSSIA`) добавлены Trade Center 7 и Shipyard 2.

### Обрабатывающая промышленность POL

На `on_game_started` существующие manufacturing buildings в целевых польских регионах удаляются, после чего создается новый набор. В каждом регионе есть минимум три разных типа по 2-4 уровня:

| Регион | Новый набор |
|---|---|
| Mazovia | Food Industry 4, Textile Mill 3, Tooling Workshop 3 |
| Greater Poland | Food Industry 3, Furniture 3, Textile Mill 2 |
| Lesser Poland | Glassworks 3, Paper Mill 2, Food Industry 3 |
| West Galicia | Textile Mill 3, Food Industry 3, Glassworks 2 |
| Posen | Tooling Workshop 3, Furniture 2, Paper Mill 2 |
| West Prussia | Food Industry 3, Textile Mill 2, Paper Mill 2 |
| Upper Silesia | Steel Mill 4, Tooling Workshop 4, Glassworks 3 |
| Warmia / East Prussia | Food Industry 2, Textile Mill 2, Furniture 2, Shipyard 2 |

## Польская армия

Для `POL` добавлена отдельная армия:

- 25 Line Infantry;
- 10 Mobile Artillery;
- 7 Lancers;
- все части привязаны к `STATE_MAZOVIA` (Warsaw);
- HQ: `sr:region_eastern_europe`.

## Новые файлы 0.2.0

```text
common/history/buildings/vicmod_issue1_ukr_buildings_1.txt
common/history/buildings/vicmod_issue1_ukr_buildings_2.txt
common/history/buildings/vicmod_issue1_bye_buildings.txt
common/history/buildings/vicmod_issue1_pol_buildings.txt
common/history/military_formations/vicmod_issue1_pol_army.txt
common/on_actions/vicmod_issue1_on_actions.txt
localization/russian/vicmod_issue1_l_russian.yml
localization/english/vicmod_issue1_l_english.yml
```

## Сохранено из 0.1.0

Версия по-прежнему содержит:

- generic prestige-company copies для расширенного списка vanilla goods;
- prestige goods, включая hardwood, iron, liquor, fabric, fruit, luxury furniture, luxury clothes, dyes, tobacco, silk, sugar, tea, sulfur, engines, porcelain, oil, wine и fine art;
- отсутствие mod-added вариантов `manowars` и `clippers`;
- новые товары `spices`, `horses`, `machine_tools`, `copper` с производством, спросом и standard companies.

## Проверка в игре

Перед слиянием `0.2.0` в основную ветку нужно проверить:

1. отсутствие parser errors в `error.log`;
2. стартовые уровни UKR и BYE без удаления существующих зданий;
3. корректный cleanup manufacturing buildings POL и создание новых 2-4 level наборов;
4. отображение Kaunas как целевого региона блока «Литва» и East Prussia как Warmia при польской локализации;
5. production methods для ферм, шахт, лесопилок, стекольных и строительных зданий;
6. появление польской армии 25/10/7 в Eastern Europe;
7. отсутствие регрессий функциональности `0.1.0`.

### Примечание по Fishing Wharves

Issue требует по 15 Fishing Wharves в подходящих регионах UKR. В нескольких vanilla state regions исходный `capped_resources` для рыболовства ниже 15. В history-файле сохранено требуемое значение 15, поэтому этот пункт особенно важно проверить непосредственно в игре. Если движок ограничит стартовый уровень cap-ом, потребуется отдельно поднять соответствующие resource caps.

## Статус

`0.2.0`: реализация запросов из issue #1, подготовлена для игрового smoke-test.
