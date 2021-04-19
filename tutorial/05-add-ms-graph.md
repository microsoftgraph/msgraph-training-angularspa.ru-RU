<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="70071-101">В этом упражнении вы будете включать Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="70071-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="70071-102">Для этого приложения вы будете использовать [библиотеку microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="70071-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="70071-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="70071-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="70071-104">Добавьте новую службу для проведения всех вызовов Graph.</span><span class="sxs-lookup"><span data-stu-id="70071-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="70071-105">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="70071-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="70071-106">Как и в созданной ранее службе проверки подлинности, создание службы позволяет вводить ее в любые компоненты, которые нуждаются в доступе к Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="70071-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="70071-107">После завершения команды откройте **./src/app/graph.service.ts** и замените содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="70071-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

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

    <span data-ttu-id="70071-108">Давайте посмотрим, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="70071-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="70071-109">Он инициализирует клиент Graph в конструкторе для службы.</span><span class="sxs-lookup"><span data-stu-id="70071-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="70071-110">Он реализует `getCalendarView` функцию, использующую клиент Graph следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70071-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="70071-111">Вызывается URL-адрес `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="70071-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="70071-112">Метод включает заглавную, из-за чего время начала и окончания возвращаемых событий должно быть в предпочтительном часовом `header` `Prefer: outlook.timezone` поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="70071-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="70071-113">Метод добавляет параметры и добавляет `query` окно времени для представления `startDateTime` `endDateTime` календаря.</span><span class="sxs-lookup"><span data-stu-id="70071-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="70071-114">Метод ограничивает поля, возвращенные для каждого события, только теми, которые будут `select` фактически использовать представление.</span><span class="sxs-lookup"><span data-stu-id="70071-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="70071-115">Метод `orderby` сортировать результаты по времени начала.</span><span class="sxs-lookup"><span data-stu-id="70071-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="70071-116">Создайте компонент Angular для вызова этого нового метода и отображения результатов вызова.</span><span class="sxs-lookup"><span data-stu-id="70071-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="70071-117">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="70071-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="70071-118">После завершения команды добавьте компонент в массив `routes` **в ./src/app/app-routing.module.ts**.</span><span class="sxs-lookup"><span data-stu-id="70071-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. <span data-ttu-id="70071-119">Откройте **./tsconfig.jsи** добавьте в объект следующее `compilerOptions` свойство.</span><span class="sxs-lookup"><span data-stu-id="70071-119">Open **./tsconfig.json** and add the following property to the `compilerOptions` object.</span></span>

    ```json
    "resolveJsonModule": true
    ```

1. <span data-ttu-id="70071-120">Откройте **./src/app/calendar/calendar.component.ts** и замените содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="70071-120">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

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

<span data-ttu-id="70071-121">Пока это только отрисовка массива событий в JSON на странице.</span><span class="sxs-lookup"><span data-stu-id="70071-121">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="70071-122">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="70071-122">Save your changes and restart the app.</span></span> <span data-ttu-id="70071-123">Войдите и щелкните **ссылку Календарь** в панели nav.</span><span class="sxs-lookup"><span data-stu-id="70071-123">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="70071-124">Если все работает надлежащим образом, в календаре пользователя должен появиться дамп событий в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="70071-124">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="70071-125">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="70071-125">Display the results</span></span>

<span data-ttu-id="70071-126">Теперь вы можете обновить компонент, чтобы отображать события более удобным `CalendarComponent` для пользователя образом.</span><span class="sxs-lookup"><span data-stu-id="70071-126">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="70071-127">Удалите временный код, который добавляет оповещение из `ngOnInit` функции.</span><span class="sxs-lookup"><span data-stu-id="70071-127">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="70071-128">Обновленная функция должна выглядеть так.</span><span class="sxs-lookup"><span data-stu-id="70071-128">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="70071-129">Добавьте функцию в `CalendarComponent` класс для формата `DateTimeTimeZone` объекта в строку ISO.</span><span class="sxs-lookup"><span data-stu-id="70071-129">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="70071-130">Откройте **./src/app/calendar/calendar.component.html** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="70071-130">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="70071-131">Это циклы через коллекцию событий и добавляет строку таблицы для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="70071-131">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="70071-132">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="70071-132">Save the changes and restart the app.</span></span> <span data-ttu-id="70071-133">Щелкните **ссылку Календарь,** и теперь приложение должно отрисовки таблицы событий.</span><span class="sxs-lookup"><span data-stu-id="70071-133">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Снимок экрана с таблицей событий](./images/add-msgraph-01.png)
