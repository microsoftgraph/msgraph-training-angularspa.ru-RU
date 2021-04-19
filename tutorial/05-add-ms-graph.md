<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете включать Microsoft Graph в приложение. Для этого приложения вы будете использовать [библиотеку microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Получение событий календаря из Outlook

1. Добавьте новую службу для проведения всех вызовов Graph. Запустите следующую команду в CLI.

    ```Shell
    ng generate service graph
    ```

    Как и в созданной ранее службе проверки подлинности, создание службы позволяет вводить ее в любые компоненты, которые нуждаются в доступе к Microsoft Graph.

1. После завершения команды откройте **./src/app/graph.service.ts** и замените содержимое следующим.

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
            const token = await this.authService.getAccessToken()
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

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[] | undefined> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          const result =  await this.graphClient
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
        return undefined;
      }
    }
    ```

    Давайте посмотрим, что делает этот код.

    - Он инициализирует клиент Graph в конструкторе для службы.
    - Он реализует `getCalendarView` функцию, использующую клиент Graph следующим образом:
      - Вызывается URL-адрес `/me/calendarview`.
      - Метод включает заглавную, из-за чего время начала и окончания возвращаемых событий должно быть в предпочтительном часовом `header` `Prefer: outlook.timezone` поясе пользователя.
      - Метод добавляет параметры и добавляет `query` окно времени для представления `startDateTime` `endDateTime` календаря.
      - Метод ограничивает поля, возвращенные для каждого события, только теми, которые будут `select` фактически использовать представление.
      - Метод `orderby` сортировать результаты по времени начала.

1. Создайте компонент Angular для вызова этого нового метода и отображения результатов вызова. Запустите следующую команду в CLI.

    ```Shell
    ng generate component calendar
    ```

1. После завершения команды добавьте компонент в массив `routes` **в ./src/app/app-routing.module.ts**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. Откройте **./tsconfig.jsи** добавьте в объект следующее `compilerOptions` свойство.

    ```json
    "resolveJsonModule": true
    ```

1. Откройте **./src/app/calendar/calendar.component.ts** и замените содержимое на следующее.

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findIana } from 'windows-iana';
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

      public events?: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findIana(this.authService.user?.timeZone ?? 'UTC');
        const timeZone = ianaName![0].valueOf() || this.authService.user?.timeZone || 'UTC';

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user?.timeZone ?? 'UTC')
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

Пока это только отрисовка массива событий в JSON на странице. Сохраните изменения и перезапустите приложение. Войдите и щелкните **ссылку Календарь** в панели nav. Если все работает надлежащим образом, в календаре пользователя должен появиться дамп событий в формате JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете обновить компонент, чтобы отображать события более удобным `CalendarComponent` для пользователя образом.

1. Удалите временный код, который добавляет оповещение из `ngOnInit` функции. Обновленная функция должна выглядеть так.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Добавьте функцию в `CalendarComponent` класс для формата `DateTimeTimeZone` объекта в строку ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Откройте **./src/app/calendar/calendar.component.html** и замените его содержимое следующим.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Это циклы через коллекцию событий и добавляет строку таблицы для каждого из них. Сохраните изменения и перезапустите приложение. Щелкните **ссылку Календарь,** и теперь приложение должно отрисовки таблицы событий.

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
