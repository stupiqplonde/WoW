# 🎯 **База данных для Dota 2**

## 🏗️ **ОСНОВНЫЕ ТАБЛИЦЫ (5 сущностей)**

### **1. Герои (Heroes)**
| Поле | Тип данных | Описание | Пример |
|------|------------|----------|--------|
| `id` | INT PRIMARY KEY AUTO_INCREMENT | Уникальный идентификатор | 1 |
| `name` | VARCHAR(50) UNIQUE | Имя героя на английском | `antimage` |
| `localized_name` | VARCHAR(100) | Локализованное имя | `Anti-Mage` |
| `primary_attr` | ENUM('str', 'agi', 'int') | Основной атрибут | `agi` |
| `attack_type` | ENUM('Melee', 'Ranged') | Тип атаки | `Melee` |
| `roles` | JSON | Роли героя (массив) | `["Carry", "Escape", "Nuker"]` |
| `base_health` | INT | Базовое здоровье | 200 |
| `base_mana` | INT | Базовая мана | 75 |
| `base_armor` | DECIMAL(3,1) | Базовая броня | 2.0 |
| `base_attack_min` | INT | Мин. урон атаки | 29 |
| `base_attack_max` | INT | Макс. урон атаки | 33 |
| `move_speed` | INT | Скорость движения | 310 |
| `legs` | TINYINT | Количество ног | 2 |

### **2. Игроки (Players)**
| Поле | Тип данных | Описание | Пример |
|------|------------|----------|--------|
| `id` | INT PRIMARY KEY AUTO_INCREMENT | Уникальный ID | 1001 |
| `steam_id` | BIGINT UNIQUE | Steam ID 64-bit | `76561198000000000` |
| `persona_name` | VARCHAR(100) | Отображаемое имя | `Miracle-` |
| `avatar_url` | VARCHAR(255) | Ссылка на аватар | `https://steamcdn-a.akamaihd.net/...` |
| `mmr_estimate` | INT | Примерный MMR | 8500 |
| `solo_mmr` | INT | Соло MMR | 8400 |
| `party_mmr` | INT | Партийный MMR | 8000 |
| `competitive_rank` | INT | Ранг в рейтинговой | 80 |
| `leaderboard_rank` | INT | Позиция в лидерборде | 15 |
| `profile_url` | VARCHAR(255) | Ссылка на профиль | `https://steamcommunity.com/...` |
| `last_login` | DATETIME | Последний вход | `2024-01-15 14:30:00` |
| `country_code` | CHAR(2) | Код страны | `UA` |

### **3. Матчи (Matches)**
| Поле | Тип данных | Описание | Пример |
|------|------------|----------|--------|
| `id` | BIGINT PRIMARY KEY | ID матча | `7144912843` |
| `match_seq_num` | BIGINT | Порядковый номер | `7145000000` |
| `start_time` | INT | Время начала (Unix) | `1642276800` |
| `duration` | INT | Длительность (секунды) | 2456 |
| `game_mode` | TINYINT | Режим игры | 22 |
| `lobby_type` | TINYINT | Тип лобби | 7 |
| `radiant_win` | BOOLEAN | Победа сил света | `true` |
| `cluster` | INT | Кластер сервера | 122 |
| `region` | INT | Регион | 8 |
| `patch` | DECIMAL(3,1) | Версия патча | 7.35 |
| `skill` | TINYINT | Уровень навыка | 3 |
| `league_id` | INT | ID лиги | 15000 |
| `series_id` | INT | ID серии | 12345 |
| `series_type` | TINYINT | Тип серии | 2 |

### **4. Предметы (Items)**
| Поле | Тип данных | Описание | Пример |
|------|------------|----------|--------|
| `id` | INT PRIMARY KEY | ID предмета | 1 |
| `name` | VARCHAR(100) | Внутреннее имя | `item_blink` |
| `localized_name` | VARCHAR(100) | Отображаемое имя | `Blink Dagger` |
| `cost` | INT | Стоимость | 2250 |
| `secret_shop` | BOOLEAN | В секретном магазине | `false` |
| `side_shop` | BOOLEAN | В боковом магазине | `false` |
| `recipe` | BOOLEAN | Является рецептом | `false` |
| `display_color` | CHAR(6) | Цвет отображения | `D2D2D2` |
| `qual` | VARCHAR(50) | Качество предмета | `component` |
| `lore` | TEXT | История предмета | `The fabled dagger used by...` |
| `cooldown` | INT | Время перезарядки | 15 |
| `mana_cost` | INT | Стоимость маны | 0 |

### **5. Способности (Abilities)**
| Поле | Тип данных | Описание | Пример |
|------|------------|----------|--------|
| `id` | INT PRIMARY KEY | ID способности | 5001 |
| `name` | VARCHAR(100) | Внутреннее имя | `antimage_mana_break` |
| `localized_name` | VARCHAR(100) | Отображаемое имя | `Mana Break` |
| `is_ultimate` | BOOLEAN | Ультимативная способность | `false` |
| `ability_slot` | TINYINT | Слот способности | 1 |
| `damage_type` | ENUM('Physical', 'Magical', 'Pure') | Тип урона | `Physical` |
| `bkb_pierce` | BOOLEAN | Пробивает BKB | `true` |
| `spell_immunity` | ENUM('No', 'Yes', 'Pierces') | Пробивает иммунитет | `Pierces` |
| `damage` | VARCHAR(100) | Урон (может быть массив) | `28/40/52/64` |
| `mana_cost` | VARCHAR(100) | Стоимость маны | `0/0/0/0` |
| `cooldown` | VARCHAR(100) | Перезарядка | `0/0/0/0` |

## 🔗 **СВЯЗИ МЕЖДУ ТАБЛИЦАМИ**

### **Один ко многим (One-to-Many):**
1. **Герои → Способности** - У одного героя может быть много способностей
2. **Матчи → Участники матчей** - В одном матче участвует много игроков
3. **Игроки → Статистика по героям** - У одного игрока есть статистика по многим героям

### **Многие ко многим (Many-to-Many):**
1. **Игроки ↔ Матчи** - Игроки участвуют в разных матчах
2. **Герои ↔ Матчи** - Герои участвуют в разных матчах
3. **Игроки ↔ Герои** - Игроки играют разными героями
4. **Герои ↔ Предметы** - Герои используют разные предметы
5. **Игроки ↔ Предметы** - Игроки покупают разные предметы

## 📊 **ТАБЛИЦЫ ВЗАИМОСВЯЗЕЙ (5 таблиц)**

### **1. Участники матчей (Match_Players)**
| Поле | Тип данных | Описание | FK |
|------|------------|----------|----|
| `id` | BIGINT PRIMARY KEY | Уникальный ID | |
| `match_id` | BIGINT | ID матча | → Matches(id) |
| `player_id` | INT | ID игрока | → Players(id) |
| `hero_id` | INT | ID героя | → Heroes(id) |
| `player_slot` | TINYINT | Слот игрока (0-9) | |
| `team` | ENUM('radiant', 'dire') | Команда | |
| `level` | TINYINT | Уровень героя | |
| `kills` | SMALLINT | Убийства | |
| `deaths` | SMALLINT | Смерти | |
| `assists` | SMALLINT | Помощь | |
| `last_hits` | SMALLINT | Добивания крипов | |
| `denies` | SMALLINT | Добивания своих крипов | |
| `gold_per_min` | INT | Золото в минуту | |
| `xp_per_min` | INT | Опыт в минуту | |
| `net_worth` | INT | Общее золото | |
| `hero_damage` | INT | Урон по героям | |
| `tower_damage` | INT | Урон по башням | |
| `hero_healing` | INT | Лечение героев | |
| `gold_spent` | INT | Потраченное золото | |
| `scaled_hero_damage` | INT | Урон с поправкой на время | |
| `scaled_tower_damage` | INT | Урон по башням с поправкой | |
| `scaled_hero_healing` | INT | Лечение с поправкой | |

### **2. Предметы в матче (Match_Items)**
| Поле | Тип данных | Описание | FK |
|------|------------|----------|----|
| `id` | BIGINT PRIMARY KEY | Уникальный ID | |
| `match_player_id` | BIGINT | ID участника матча | → Match_Players(id) |
| `item_id` | INT | ID предмета | → Items(id) |
| `item_slot` | TINYINT | Слот предмета (0-5) | |
| `backpack_slot` | TINYINT | Слот в рюкзаке (0-2) | |
| `purchase_time` | INT | Время покупки (сек) | |
| `neutral_item_tier` | TINYINT | Уровень нейтрального предмета | |
| `is_consumed` | BOOLEAN | Был ли потреблен | |
| `is_dropped` | BOOLEAN | Был ли выброшен | |
| `is_purchased` | BOOLEAN | Был ли куплен | |
| `is_sold` | BOOLEAN | Был ли продан | |

### **3. Способности героев (Hero_Ability_Relations)**
| Поле | Тип данных | Описание | FK |
|------|------------|----------|----|
| `id` | INT PRIMARY KEY | Уникальный ID | |
| `hero_id` | INT | ID героя | → Heroes(id) |
| `ability_id` | INT | ID способности | → Abilities(id) |
| `ability_number` | TINYINT | Номер способности (1-6) | |
| `is_talent` | BOOLEAN | Является талантом | |
| `talent_tier` | TINYINT | Уровень таланта (10/15/20/25) | |
| `is_attribute_bonus` | BOOLEAN | Бонус к атрибутам | |
| `behaviors` | JSON | Поведения способности | |

### **4. Таланты героев (Hero_Talents)**
| Поле | Тип данных | Описание | FK |
|------|------------|----------|----|
| `id` | INT PRIMARY KEY | Уникальный ID | |
| `hero_id` | INT | ID героя | → Heroes(id) |
| `level` | TINYINT | Уровень таланта (10,15,20,25) | |
| `left_talent_ability_id` | INT | Левый талант | → Abilities(id) |
| `right_talent_ability_id` | INT | Правый талант | → Abilities(id) |
| `left_talent_description` | VARCHAR(255) | Описание левого | |
| `right_talent_description` | VARCHAR(255) | Описание правого | |
| `win_rate_left` | DECIMAL(5,2) | Винрейт левого | |
| `win_rate_right` | DECIMAL(5,2) | Винрейт правого | |
| `pick_rate_left` | DECIMAL(5,2) | Частота выбора левого | |
| `pick_rate_right` | DECIMAL(5,2) | Частота выбора правого | |

### **5. Улучшения способностей (Ability_Upgrades)**
| Поле | Тип данных | Описание | FK |
|------|------------|----------|----|
| `id` | BIGINT PRIMARY KEY | Уникальный ID | |
| `match_player_id` | BIGINT | ID участника | → Match_Players(id) |
| `ability_id` | INT | ID способности | → Abilities(id) |
| `time` | INT | Время улучшения (сек) | |
| `level` | TINYINT | Уровень улучшения | |
| `is_ultimate` | BOOLEAN | Улучшение ульты | |
| `is_talent` | BOOLEAN | Выбор таланта | |

## 🎮 **ПРИМЕРЫ ЗАПРОСОВ**

### **1. Топ-10 героев по винрейту в высоком MMR:**
```sql
SELECT 
    h.localized_name,
    COUNT(*) as games_played,
    ROUND(SUM(CASE WHEN (mp.team = 'radiant' AND m.radiant_win) 
                    OR (mp.team = 'dire' AND NOT m.radiant_win) 
               THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) as win_rate
FROM Match_Players mp
JOIN Matches m ON mp.match_id = m.id
JOIN Heroes h ON mp.hero_id = h.id
WHERE m.skill >= 3
GROUP BY h.id
HAVING games_played >= 100
ORDER BY win_rate DESC
LIMIT 10;
```

### **2. Статистика игрока по героям:**
```sql
SELECT 
    h.localized_name,
    COUNT(*) as total_games,
    SUM(CASE WHEN (mp.team = 'radiant' AND m.radiant_win) 
               OR (mp.team = 'dire' AND NOT m.radiant_win) 
          THEN 1 ELSE 0 END) as wins,
    ROUND(AVG(mp.kills), 2) as avg_kills,
    ROUND(AVG(mp.deaths), 2) as avg_deaths,
    ROUND(AVG(mp.assists), 2) as avg_assists,
    ROUND(AVG(mp.gold_per_min), 0) as avg_gpm,
    ROUND(AVG(mp.xp_per_min), 0) as avg_xpm
FROM Match_Players mp
JOIN Matches m ON mp.match_id = m.id
JOIN Heroes h ON mp.hero_id = h.id
WHERE mp.player_id = 1001
GROUP BY h.id
ORDER BY total_games DESC;
```

### **3. Популярные предметы на герое:**
```sql
SELECT 
    i.localized_name,
    COUNT(*) as times_purchased,
    ROUND(COUNT(*) * 100.0 / (
        SELECT COUNT(DISTINCT mp.id) 
        FROM Match_Players mp 
        WHERE mp.hero_id = 1
    ), 2) as purchase_rate
FROM Match_Items mi
JOIN Match_Players mp ON mi.match_player_id = mp.id
JOIN Items i ON mi.item_id = i.id
WHERE mp.hero_id = 1  -- Anti-Mage
    AND mi.is_purchased = true
    AND mi.item_slot BETWEEN 0 AND 5
GROUP BY i.id
ORDER BY times_purchased DESC
LIMIT 15;
```

### **4. История матчей игрока:**
```sql
SELECT 
    m.id,
    FROM_UNIXTIME(m.start_time) as match_time,
    m.duration,
    CASE WHEN (mp.team = 'radiant' AND m.radiant_win) 
          OR (mp.team = 'dire' AND NOT m.radiant_win) 
         THEN 'WIN' ELSE 'LOSS' END as result,
    h.localized_name as hero,
    mp.kills,
    mp.deaths,
    mp.assists,
    mp.gold_per_min as gpm,
    mp.xp_per_min as xpm,
    mp.last_hits as lh,
    mp.hero_damage as hero_dmg
FROM Match_Players mp
JOIN Matches m ON mp.match_id = m.id
JOIN Heroes h ON mp.hero_id = h.id
WHERE mp.player_id = 1001
ORDER BY m.start_time DESC
LIMIT 10;
```

## 📈 **ИНДЕКСЫ ДЛЯ ОПТИМИЗАЦИИ**

```sql
-- Индексы для таблицы матчей
CREATE INDEX idx_matches_start_time ON Matches(start_time);
CREATE INDEX idx_matches_skill ON Matches(skill);
CREATE INDEX idx_matches_patch ON Matches(patch);

-- Индексы для таблицы участников матчей
CREATE INDEX idx_match_players_match_id ON Match_Players(match_id);
CREATE INDEX idx_match_players_player_id ON Match_Players(player_id);
CREATE INDEX idx_match_players_hero_id ON Match_Players(hero_id);
CREATE INDEX idx_match_players_team ON Match_Players(team);

-- Индексы для таблицы предметов в матче
CREATE INDEX idx_match_items_player_id ON Match_Items(match_player_id);
CREATE INDEX idx_match_items_item_id ON Match_Items(item_id);
CREATE INDEX idx_match_items_slot ON Match_Items(item_slot);

-- Индексы для таблицы улучшений способностей
CREATE INDEX idx_ability_upgrades_player_id ON Ability_Upgrades(match_player_id);
CREATE INDEX idx_ability_upgrades_ability_id ON Ability_Upgrades(ability_id);
CREATE INDEX idx_ability_upgrades_time ON Ability_Upgrades(time);
```

## 🗂️ **СХЕМА БАЗЫ ДАННЫХ**

```
Heroes
    ├── Hero_Ability_Relations (1:n)
    ├── Hero_Talents (1:n)
    ├── Match_Players (1:n)
    └── Player_Hero_Stats (через Match_Players)

Players
    ├── Match_Players (1:n)
    └── Ability_Upgrades (через Match_Players)

Matches
    └── Match_Players (1:n)

Items
    └── Match_Items (1:n)

Abilities
    ├── Hero_Ability_Relations (1:n)
    ├── Hero_Talents (1:n) 
    └── Ability_Upgrades (1:n)

Match_Players (таблица связи)
    ├── Match_Items (1:n)
    └── Ability_Upgrades (1:n)
```

## 🎯 **ВОЗМОЖНОСТИ СИСТЕМЫ**

✅ **Хранение полной статистики матчей**  
✅ **Анализ меты и популярности героев**  
✅ **Отслеживание прогресса игроков**  
✅ **Рекомендации по предметам и скиллам**  
✅ **История патчей и изменений баланса**  
✅ **Сравнение статистики между регионами**  
✅ **Выявление синергий героев и предметов**  
✅ **Прогнозирование результатов матчей**
