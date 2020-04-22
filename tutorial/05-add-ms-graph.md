<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы добавите Microsoft Graph в приложение. В этом приложении для совершения вызовов в Microsoft Graph будет использоваться [Клиентская библиотека Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Создайте новый файл в `./src/app` каталоге `event.ts` и добавьте приведенный ниже код.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. Добавление новой службы для хранения всех вызовов графов. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate service graph
    ```

    Как и при работе со службой проверки подлинности, созданной ранее, создание службы позволяет вставить ее в любой компонент, которому необходим доступ к Microsoft Graph.

1. После выполнения команды откройте `./src/app/graph.service.ts` файл и замените его содержимое на следующее:

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    Рассмотрите, что делает этот код.

    - Он инициализирует клиент Graph в конструкторе для службы.
    - Он реализует `getEvents` функцию, использующую клиент Graph, следующим образом:
      - URL-адрес, который будет вызываться — это `/me/events`.
      - `select` Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.
      - `orderby` Метод сортирует результаты по дате и времени создания, начиная с самого последнего элемента.

1. Создайте угловой компонент, чтобы вызвать этот новый метод и отобразить результаты вызова. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate component calendar
    ```

1. После выполнения команды добавьте компонент в `routes` массив. `./src/app/app-routing.module.ts`

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Откройте `./src/app/calendar/calendar.component.ts` файл и замените его содержимое на приведенный ниже код.

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

Теперь это просто отрисовывает массив событий в JSON на странице. Сохраните изменения и перезапустите приложение. Войдите и щелкните ссылку **Календарь** на панели навигации. Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете обновить `CalendarComponent` компонент, чтобы отображать события более удобным для пользователя способом.

1. Удалите временный код, который добавляет оповещение из `ngOnInit` функции. Обновленная функция должна выглядеть так, как показано ниже.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Добавьте функцию в `CalendarComponent` класс, чтобы отформатировать `DateTimeTimeZone` объект в строку ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Откройте `./src/app/calendar/calendar.component.html` файл и замените его содержимое на приведенный ниже код.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

В результате получается цикл по коллекции событий и добавляется строка таблицы для каждой из них. Сохраните изменения и перезапустите приложение. Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
