<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f02c5-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="f02c5-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="f02c5-102">В этом приложении для совершения вызовов в Microsoft Graph будет использоваться [Клиентская библиотека Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.</span><span class="sxs-lookup"><span data-stu-id="f02c5-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="f02c5-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="f02c5-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="f02c5-104">Добавление новой службы для хранения всех вызовов графов.</span><span class="sxs-lookup"><span data-stu-id="f02c5-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="f02c5-105">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="f02c5-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="f02c5-106">Как и при работе со службой проверки подлинности, созданной ранее, создание службы позволяет вставить ее в любой компонент, которому необходим доступ к Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f02c5-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="f02c5-107">После выполнения команды откройте **./СРК/АПП/граф.сервице.ТС** и замените ее содержимое на следующий.</span><span class="sxs-lookup"><span data-stu-id="f02c5-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

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

    <span data-ttu-id="f02c5-108">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="f02c5-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="f02c5-109">Он инициализирует клиент Graph в конструкторе для службы.</span><span class="sxs-lookup"><span data-stu-id="f02c5-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="f02c5-110">Он реализует `getCalendarView` функцию, использующую клиент Graph, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f02c5-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="f02c5-111">URL-адрес, который будет вызываться — это `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="f02c5-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="f02c5-112">`header`Метод включает `Prefer: outlook.timezone` заголовок, который приводит к тому, что время начала и окончания возвращаемых событий будет в предпочтительном часовом поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="f02c5-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="f02c5-113">`query`Метод добавляет `startDateTime` `endDateTime` Параметры и определяет окно представления календаря.</span><span class="sxs-lookup"><span data-stu-id="f02c5-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="f02c5-114">`select`Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.</span><span class="sxs-lookup"><span data-stu-id="f02c5-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="f02c5-115">`orderby`Метод сортирует результаты по времени начала.</span><span class="sxs-lookup"><span data-stu-id="f02c5-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="f02c5-116">Создайте угловой компонент, чтобы вызвать этот новый метод и отобразить результаты вызова.</span><span class="sxs-lookup"><span data-stu-id="f02c5-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="f02c5-117">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="f02c5-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="f02c5-118">После завершения команды добавьте компонент в `routes` массив в файле **./СРК/АПП/АПП-раутинг.модуле.ТС**.</span><span class="sxs-lookup"><span data-stu-id="f02c5-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="f02c5-119">Откройте **/СРК/АПП/календар/календар.компонент.ТС** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="f02c5-119">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

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

<span data-ttu-id="f02c5-120">Теперь это просто отрисовывает массив событий в JSON на странице.</span><span class="sxs-lookup"><span data-stu-id="f02c5-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="f02c5-121">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f02c5-121">Save your changes and restart the app.</span></span> <span data-ttu-id="f02c5-122">Войдите и щелкните ссылку **Календарь** на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="f02c5-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="f02c5-123">Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="f02c5-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="f02c5-124">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="f02c5-124">Display the results</span></span>

<span data-ttu-id="f02c5-125">Теперь вы можете обновить `CalendarComponent` компонент, чтобы отображать события более удобным для пользователя способом.</span><span class="sxs-lookup"><span data-stu-id="f02c5-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="f02c5-126">Удалите временный код, который добавляет оповещение из `ngOnInit` функции.</span><span class="sxs-lookup"><span data-stu-id="f02c5-126">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="f02c5-127">Обновленная функция должна выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f02c5-127">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="f02c5-128">Добавьте функцию в класс, `CalendarComponent` чтобы отформатировать `DateTimeTimeZone` объект в строку ISO.</span><span class="sxs-lookup"><span data-stu-id="f02c5-128">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="f02c5-129">Откройте **./срк/апп/календар/calendar.component.html** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="f02c5-129">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="f02c5-130">В результате получается цикл по коллекции событий и добавляется строка таблицы для каждой из них.</span><span class="sxs-lookup"><span data-stu-id="f02c5-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="f02c5-131">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f02c5-131">Save the changes and restart the app.</span></span> <span data-ttu-id="f02c5-132">Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.</span><span class="sxs-lookup"><span data-stu-id="f02c5-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
