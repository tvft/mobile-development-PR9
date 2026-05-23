# Практическая работа №9. Создание меню

Выполнила: Тристан Владислава Дмитриевна ИНС-б-о-24-2 

# Цель работы 

Изучить способы создания и обработки событий от различных типов в меню Android: главного меню и контексного меню. Научиться изменять интерфейс приложения с помощью пунктов меню.

# Ход работы

1. Создание проекта и подготовка интерфейса

1.1. Был открыт Android Studio и создан новый проект с шаблоном Empty Views Activity с названием MenuLab

1.2. В файле activity_main.xml создан интерфейс

activity_main.xml

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="16dp">

    <Button
        android:id="@+id/button1"
        android:layout_width="220dp"
        android:layout_height="wrap_content"
        android:text="Кнопка"
        android:textSize="22sp"/>

</LinearLayout>
```
2. Создание OptionsMenu

2.1. Создана папка res/menu

2.2. Создан в ней файл maun_menu.xml

2.3. Добавлено три пункта меню согласно варианту.

main_menu.xml

```java
<?xml version="1.0" encoding="utf-8"?>

<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/style_red"
        android:title="Красная кнопка"/>

    <item
        android:id="@+id/style_green"
        android:title="Зелёная кнопка"/>

    <item
        android:id="@+id/style_round"
        android:title="Круглая кнопка"/>

</menu>
```
2.4. В MainActivity.java переопределен метод onCreateOptionsMenu для щагрузки меню

2.5. Переопределн метод onOptionsItemSelected для обработки выбора пунктов

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {

    int id = item.getItemId();

    // Красная кнопка
    if (id == R.id.style_red) {

        button.setBackgroundColor(Color.RED);

        button.setText("Красная");

        return true;
    }

    // Зелёная кнопка
    else if (id == R.id.style_green) {

        button.setBackgroundColor(Color.GREEN);

        button.setText("Зелёная");

        return true;
    }

    // Круглая кнопка
    else if (id == R.id.style_round) {

        GradientDrawable shape = new GradientDrawable();

        shape.setColor(Color.BLUE);

        shape.setCornerRadius(100);

        button.setBackground(shape);

        button.setText("Круглая");

        return true;
    }

    return super.onOptionsItemSelected(item);
}
```

3. Создание ContextMenu

3.1. Выбран элемент, для которых будет вызываться контекстное меню. Зарегистрирован в методе onCreate с помощью registernForContextMenu()

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    button = findViewById(R.id.button1);

    registerForContextMenu(button);
}
```

3.2. Переопределен метод onContextItemSelected для создания меню 

```java
@Override
public void onCreateContextMenu(ContextMenu menu,
                                View v,
                                ContextMenu.ContextMenuInfo menuInfo) {

    super.onCreateContextMenu(menu, v, menuInfo);

    menu.setHeaderTitle("Действия");

    menu.add(0, 1, 0, "Вернуть");

    menu.add(0, 2, 0, "Повторить");
}
```
3.3. Преопределен метод onCreateItemSelected для обработки выбора 

```java
@Override
public boolean onContextItemSelected(MenuItem item) {

    // Вернуть стандартную кнопку
    if (item.getItemId() == 1) {

        button.setBackgroundColor(Color.LTGRAY);

        button.setText("Кнопка");

        return true;
    }

    // Повторить последнее действие
    else if (item.getItemId() == 2) {

        button.setBackgroundColor(Color.RED);

        button.setText("Повтор");

        return true;
    }

    return super.onContextItemSelected(item);
}
```

4. Объединение и тестирование

<img width="337" height="636" alt="Снимок экрана 2026-05-23 130101" src="https://github.com/user-attachments/assets/cacf54eb-06bf-4efc-ba42-d9c6b221a200" />

<img width="343" height="641" alt="Снимок экрана 2026-05-23 130054" src="https://github.com/user-attachments/assets/166db7bb-5b64-4a99-8c83-8c2b3e7733ba" />

<img width="337" height="642" alt="Снимок экрана 2026-05-23 130046" src="https://github.com/user-attachments/assets/6b73a3f3-f96a-4751-9fad-adb82e5f731f" />

# Контрольные вопросы

1. Какие типы меню существуют в Android? Опишите их назначение.
   
OptionsMenu (главное меню) — вызывается нажатием на кнопку меню в панели действий (ActionBar), используется для глобальных действий приложения (настройки, поиск, выход)

ContextMenu (контекстное меню) — появляется при долгом нажатии на элементе интерфейса, предоставляет действия, специфичные для выбранного элемента

PopupMenu (всплывающее меню) — привязано к определённому View и появляется по нажатию

2. Как создать главное меню (OptionsMenu)? Какие методы необходимо переопределить в Activity?
3. 
Создать XML-файл в папке res/menu/

Переопределить метод onCreateOptionsMenu() для загрузки меню: getMenuInflater().inflate(R.menu.main_menu, menu)

Переопределить метод onOptionsItemSelected() для обработки выбора пунктов

3. Для чего используется атрибут app:showAsAction? Какие значения он может принимать?
   
Определяет отображение пункта меню в ActionBar или выпадающем меню. Значения:

ifRoom — отображать, если есть место

never — всегда скрывать в меню

always — всегда показывать (не рекомендуется)

4. Как зарегистрировать View для контекстного меню? В каком методе это обычно делается?
   
Вызвать метод registerForContextMenu(View view) для нужного элемента. Обычно делается в методе onCreate().

5. В чём разница между методами onCreateContextMenu и onContextItemSelected?
   
onCreateContextMenu() — создаёт/загружает контекстное меню (аналог onCreateOptionsMenu())
onContextItemSelected() — обрабатывает выбор пункта меню (аналог onOptionsItemSelected())

6. Как создать контекстное меню динамически (программно), без использования XML-ресурса?
   
Использовать метод menu.add(groupId, itemId, order, title) в методе onCreateContextMenu():

```java
menu.add(0, 1, 0, "Пункт 1");
menu.add(0, 2, 1, "Пункт 2");
```

7.Что возвращают методы onOptionsItemSelected и onContextItemSelected? Что означает возврат true?

Оба метода возвращают boolean. Возврат true означает, что обработка выполнена успешно и событие дальше не передаётся.

8. Как определить, для какого именно элемента было вызвано контекстное меню, если зарегистрировано несколько View?
   
Использовать параметр View v в методе onCreateContextMenu() или menuInfo (например, AdapterView.AdapterContextMenuInfo для списков). Можно также проверять v.getId() для идентификации конкретного View.
