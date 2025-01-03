# Анализ встроенного пакета dplyr
## Цель работы
1. Развить практические навыки использования языка программирования R для
обработки данных
2. Закрепить знания базовых типов данных языка R
3. Развить практические навыки использования функций обработки данных пакета dplyr – функции select(), filter(), mutate(), arrange(), group_by()

### Просмотр датафрейма

```{r}
starwars
```
### Выведу колонку с именами

```{r}
starwars %>% select(name)
```
### Количество строк в датафрейме

```{r}
starwars %>% nrow()
```
### Количество столбцов в датафрейме

```{r}
starwars %>% ncol()
```
### Примерный вид датафрейма

```{r}
starwars %>% glimpse()
```
### Уникальные расы персонажей

```{r}
length(unique(starwars$species))
```
### Самый высокий персонаж

```{r}
starwars %>%
  arrange(desc(height)) %>%
  slice_head(n=1)

```
### Все персонажи ниже 170

```{r}
 short_characters <- starwars %>%
  filter(height < 170)
print(short_characters)
```

### ИМТ для всех персонажей

```{r}
I <- starwars %>%
  mutate(I = mass / (height ^ 2))
print(I %>% select(name, mass, height, I))
```
### Найти 10 самых "вытянутых" персонажей.

```{r}
Elongation <- starwars %>%
  mutate(elongation = mass / height) %>%
  filter(!is.na(elongation)) %>% 
  arrange(desc(elongation)) %>% 
  head(10)

print(Elongation %>% select(name, mass, height, elongation))
```

### Найти средний возраст персонажей каждой расы вселенной Звездных войн.

```{r}
starwars <- starwars %>%
  mutate(age = 2024 - birth_year)
average_age <- starwars %>%
  group_by(species) %>%
  summarise(average_age = mean(age, na.rm = TRUE))
print(average_age)
```

### Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

```{r}
color <- starwars %>%
  group_by(eye_color) %>%
  summarise(count = n()) %>%
  arrange(desc(count)) %>%
  slice(1)
print(color)
```
### Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

```{r}
average_name_length <- starwars %>%
  mutate(name_length = nchar(name)) %>%  
  group_by(species) %>%
  summarise(average_length = mean(name_length, na.rm = TRUE)) %>%
  arrange(desc(average_length))
print(average_name_length)
```

