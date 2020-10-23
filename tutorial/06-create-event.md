<!-- markdownlint-disable MD002 MD041 -->

В этом разделе мы добавим возможность создания событий в календаре пользователя.

1. Откройте **./СРК/АПП/граф.сервице.ТС** и добавьте в класс следующую функцию `GraphService` .

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a>Создание новой формы события

1. Создайте угловой компонент для отображения формы и вызова этой новой функции. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate component new-event
    ```

1. После завершения команды добавьте компонент в `routes` массив в файле **./СРК/АПП/АПП-раутинг.модуле.ТС**.

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. Создайте новый файл в каталоге **./СРК/АПП/Нев-Евент** с именем **New – Event. TS** и добавьте следующий код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    Этот класс будет выступать в качестве модели для новой формы события.

1. Откройте **/СРК/АПП/Нев-Евент/Нев-Евент.компонент.ТС** и замените его содержимое приведенным ниже кодом.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. Откройте **./срк/апп/нев-евент/new-event.component.html** и замените его содержимое приведенным ниже кодом.

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. Сохраните изменения и обновите приложение. Нажмите кнопку **создать событие** на странице календаря, а затем используйте форму, чтобы создать событие в календаре пользователя.

    ![Снимок экрана с формой создания события](images/create-event.png)
