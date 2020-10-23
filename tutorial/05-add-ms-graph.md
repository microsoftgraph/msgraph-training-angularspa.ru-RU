<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы добавите Microsoft Graph в приложение. В этом приложении для совершения вызовов в Microsoft Graph будет использоваться [Клиентская библиотека Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Добавление новой службы для хранения всех вызовов графов. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate service graph
    ```

    Как и при работе со службой проверки подлинности, созданной ранее, создание службы позволяет вставить ее в любой компонент, которому необходим доступ к Microsoft Graph.

1. После выполнения команды откройте **./СРК/АПП/граф.сервице.ТС** и замените ее содержимое на следующий.

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            let token = await this.authService.getAccessToken()
              .catch((reason) => {
                done(reason, null);
              });

            if (token)
            {
              done(null, token);
            } else {
              done("Could not get an access token", null);
            }
          }
        });
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[]> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          let result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
      }
    }
    ```

    Рассмотрите, что делает этот код.

    - Он инициализирует клиент Graph в конструкторе для службы.
    - Он реализует `getCalendarView` функцию, использующую клиент Graph, следующим образом:
      - URL-адрес, который будет вызываться — это `/me/calendarview` .
      - `header`Метод включает `Prefer: outlook.timezone` заголовок, который приводит к тому, что время начала и окончания возвращаемых событий будет в предпочтительном часовом поясе пользователя.
      - `query`Метод добавляет `startDateTime` `endDateTime` Параметры и определяет окно представления календаря.
      - `select`Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.
      - `orderby`Метод сортирует результаты по времени начала.

1. Создайте угловой компонент, чтобы вызвать этот новый метод и отобразить результаты вызова. Выполните следующую команду в командной панели CLI.

    ```Shell
    ng generate component calendar
    ```

1. После завершения команды добавьте компонент в `routes` массив в файле **./СРК/АПП/АПП-раутинг.модуле.ТС**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Откройте **/СРК/АПП/календар/календар.компонент.ТС** и замените его содержимое приведенным ниже.

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findOneIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findOneIana(this.authService.user.timeZone);
        const timeZone = ianaName!.valueOf() || this.authService.user.timeZone;

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user.timeZone)
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

Теперь это просто отрисовывает массив событий в JSON на странице. Сохраните изменения и перезапустите приложение. Войдите и щелкните ссылку **Календарь** на панели навигации. Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете обновить `CalendarComponent` компонент, чтобы отображать события более удобным для пользователя способом.

1. Удалите временный код, который добавляет оповещение из `ngOnInit` функции. Обновленная функция должна выглядеть так, как показано ниже.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Добавьте функцию в класс, `CalendarComponent` чтобы отформатировать `DateTimeTimeZone` объект в строку ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Откройте **./срк/апп/календар/calendar.component.html** и замените его содержимое приведенным ниже.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

В результате получается цикл по коллекции событий и добавляется строка таблицы для каждой из них. Сохраните изменения и перезапустите приложение. Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
