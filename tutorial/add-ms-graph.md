<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="92a94-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="92a94-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="92a94-102">В этом приложении для совершения вызовов в Microsoft Graph будет использоваться клиентская библиотека [Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript) Graph.</span><span class="sxs-lookup"><span data-stu-id="92a94-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="92a94-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="92a94-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="92a94-104">Начните с создания `Event` класса, определяющего поля, которые будут отображаться в приложении.</span><span class="sxs-lookup"><span data-stu-id="92a94-104">Start by creating an `Event` class that defines the fields that the app will display.</span></span> <span data-ttu-id="92a94-105">Создайте новый файл в `./src/app` каталоге `event.ts` и добавьте приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="92a94-105">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

```TypeScript
// For a full list of fields, see
// https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0
export class Event {
  subject: string;
  organizer: Recipient;
  start: DateTimeTimeZone;
  end: DateTimeTimeZone;
}

// https://docs.microsoft.com/graph/api/resources/recipient?view=graph-rest-1.0
export class Recipient {
  emailAddress: EmailAddress;
}

// https://docs.microsoft.com/graph/api/resources/emailaddress?view=graph-rest-1.0
export class EmailAddress {
  name: string;
  address: string;
}

// https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0
export class DateTimeTimeZone {
  dateTime: string;
  timeZone: string;
}
```

<span data-ttu-id="92a94-106">Затем добавьте новую службу для хранения всех вызовов графов.</span><span class="sxs-lookup"><span data-stu-id="92a94-106">Next, add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="92a94-107">Как и при работе со службой проверки подлинности, созданной ранее, создание службы позволяет вставить ее в любой компонент, которому необходим доступ к Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="92a94-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span> <span data-ttu-id="92a94-108">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="92a94-108">Run the following command in your CLI.</span></span>

```Shell
ng generate service graph
```

<span data-ttu-id="92a94-109">После выполнения команды откройте `./src/app/graph.service.ts` файл и замените его содержимое на следующее:</span><span class="sxs-lookup"><span data-stu-id="92a94-109">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Client } from '@microsoft/microsoft-graph-client';

import { AuthService } from './auth.service';
import { Event } from './event';
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

  async getEvents(): Promise<Event[]> {
    try {
      let result =  await this.graphClient
        .api('/me/events')
        .select('subject,organizer,start,end')
        .orderby('createdDateTime DESC')
        .get();

      return result.value;
    } catch (error) {
      this.alertsService.add('Could not get events', JSON.stringify(error, null, 2));
    }
  }
}
```

<span data-ttu-id="92a94-110">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="92a94-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="92a94-111">Он инициализирует клиент Graph в конструкторе для службы.</span><span class="sxs-lookup"><span data-stu-id="92a94-111">It initializes a Graph client in the constructor for the service.</span></span>
- <span data-ttu-id="92a94-112">Он реализует `getEvents` функцию, использующую клиент Graph, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="92a94-112">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
  - <span data-ttu-id="92a94-113">URL-адрес, который будет вызываться — это `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="92a94-113">The URL that will be called is `/me/events`.</span></span>
  - <span data-ttu-id="92a94-114">`select` Метод ограничит поля, возвращаемые для каждого события, только теми, которые будут реально использоваться в представлении.</span><span class="sxs-lookup"><span data-stu-id="92a94-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
  - <span data-ttu-id="92a94-115">`orderby` Метод сортирует результаты по дате и времени создания, начиная с самого последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="92a94-115">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="92a94-116">Теперь создайте угловой компонент, чтобы вызвать этот новый метод и отобразить результаты вызова.</span><span class="sxs-lookup"><span data-stu-id="92a94-116">Now create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="92a94-117">Выполните следующую команду в командной панели CLI.</span><span class="sxs-lookup"><span data-stu-id="92a94-117">Run the following command in your CLI.</span></span>

```Shell
ng generate component calendar
```

<span data-ttu-id="92a94-118">После выполнения команды добавьте компонент в `routes` массив. `./src/app/app-routing.module.ts`</span><span class="sxs-lookup"><span data-stu-id="92a94-118">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

<span data-ttu-id="92a94-119">Откройте `./src/app/calendar/calendar.component.ts` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="92a94-119">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

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

  private events: Event[];

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

<span data-ttu-id="92a94-120">Теперь это просто отрисовывает массив событий в JSON на странице.</span><span class="sxs-lookup"><span data-stu-id="92a94-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="92a94-121">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="92a94-121">Save your changes and restart the app.</span></span> <span data-ttu-id="92a94-122">Войдите и щелкните ссылку **Календарь** на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="92a94-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="92a94-123">Если все работает, вы должны увидеть дамп событий JSON в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="92a94-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="92a94-124">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="92a94-124">Display the results</span></span>

<span data-ttu-id="92a94-125">Теперь вы можете обновить `CalendarComponent` компонент, чтобы отображать события более удобным для пользователя способом.</span><span class="sxs-lookup"><span data-stu-id="92a94-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="92a94-126">Сначала удалите временный код, который добавляет оповещение из `ngOnInit` функции.</span><span class="sxs-lookup"><span data-stu-id="92a94-126">First, remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="92a94-127">Обновленная функция должна выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="92a94-127">Your updated function should look like this.</span></span>

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

<span data-ttu-id="92a94-128">Теперь добавьте функцию в `CalendarComponent` класс, чтобы отформатировать `DateTimeTimeZone` объект в строку ISO.</span><span class="sxs-lookup"><span data-stu-id="92a94-128">Now add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

```TypeScript
formatDateTimeTimeZone(dateTime: DateTimeTimeZone): string {
  try {
    return moment.tz(dateTime.dateTime, dateTime.timeZone).format();
  }
  catch(error) {
    this.alertsService.add('DateTimeTimeZone conversion error', JSON.stringify(error));
  }
}
```

<span data-ttu-id="92a94-129">Наконец, откройте `./src/app/calendar/calendar.component.html` файл и замените его содержимое на приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="92a94-129">Finally, open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <th scope="col">Organizer</th>
    <th scope="col">Subject</th>
    <th scope="col">Start</th>
    <th scope="col">End</th>
  </thead>
  <tbody>
    <tr *ngFor="let event of events">
      <td>{{event.organizer.emailAddress.name}}</td>
      <td>{{event.subject}}</td>
      <td>{{formatDateTimeTimeZone(event.start) | date:'short' }}</td>
      <td>{{formatDateTimeTimeZone(event.end) | date: 'short' }}</td>
    </tr>
  </tbody>
</table>
```

<span data-ttu-id="92a94-130">В результате получается цикл по коллекции событий и добавляется строка таблицы для каждой из них.</span><span class="sxs-lookup"><span data-stu-id="92a94-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="92a94-131">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="92a94-131">Save the changes and restart the app.</span></span> <span data-ttu-id="92a94-132">Щелкните ссылку **Календарь** , после чего приложение должно отобразить таблицу событий.</span><span class="sxs-lookup"><span data-stu-id="92a94-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Снимок экрана С таблицей событий](./images/add-msgraph-01.png)