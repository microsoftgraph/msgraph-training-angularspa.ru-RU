<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a0d80-101">В этом разделе вы создадим новый проект Angular.</span><span class="sxs-lookup"><span data-stu-id="a0d80-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="a0d80-102">Откройте интерфейс командной строки (CLI), перейдите в каталог, в котором у вас есть права на создание файлов, и запустите следующие команды для установки инструмента [Angular CLI](https://www.npmjs.com/package/@angular/cli) и создания нового приложения Angular.</span><span class="sxs-lookup"><span data-stu-id="a0d80-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. <span data-ttu-id="a0d80-103">Angular CLI будет подсказок для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="a0d80-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="a0d80-104">Ответьте на запросы следующим образом.</span><span class="sxs-lookup"><span data-stu-id="a0d80-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="a0d80-105">После завершения команды измените каталог в CLI и запустите следующую команду, `graph-tutorial` чтобы запустить локальный веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="a0d80-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="a0d80-106">Браузер по умолчанию открывается [https://localhost:4200/](https://localhost:4200) на странице Angular по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a0d80-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="a0d80-107">Если браузер не открыт, откройте его и просмотрите, чтобы убедиться, [https://localhost:4200/](https://localhost:4200) что новое приложение работает.</span><span class="sxs-lookup"><span data-stu-id="a0d80-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="a0d80-108">Добавление пакетов node</span><span class="sxs-lookup"><span data-stu-id="a0d80-108">Add Node packages</span></span>

<span data-ttu-id="a0d80-109">Прежде чем двигаться дальше, установите дополнительные пакеты, которые вы будете использовать позже:</span><span class="sxs-lookup"><span data-stu-id="a0d80-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="a0d80-110">[bootstrap](https://github.com/twbs/bootstrap) для укладки и общих компонентов.</span><span class="sxs-lookup"><span data-stu-id="a0d80-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="a0d80-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) для использования компонентов Bootstrap из Angular.</span><span class="sxs-lookup"><span data-stu-id="a0d80-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="a0d80-112">[время](https://github.com/moment/moment) форматирования дат и времени.</span><span class="sxs-lookup"><span data-stu-id="a0d80-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="a0d80-113">Windows-iana</span><span class="sxs-lookup"><span data-stu-id="a0d80-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="a0d80-114">[msal-angular для](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) проверки подлинности в Azure Active Directory и получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="a0d80-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="a0d80-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a0d80-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="a0d80-116">Запустите следующие команды в CLI.</span><span class="sxs-lookup"><span data-stu-id="a0d80-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.1
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. <span data-ttu-id="a0d80-117">Запустите следующую команду в CLI, чтобы добавить пакет локализации Angular (необходимый ng-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="a0d80-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="a0d80-118">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="a0d80-118">Design the app</span></span>

<span data-ttu-id="a0d80-119">В этом разделе вы создадим пользовательский интерфейс для приложения.</span><span class="sxs-lookup"><span data-stu-id="a0d80-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="a0d80-120">Откройте **./src/styles.css и** добавьте следующие строки.</span><span class="sxs-lookup"><span data-stu-id="a0d80-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="a0d80-121">Добавьте модуль Bootstrap в приложение.</span><span class="sxs-lookup"><span data-stu-id="a0d80-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="a0d80-122">Откройте **./src/app/app.module.ts** и замените содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="a0d80-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. <span data-ttu-id="a0d80-123">Создайте новый файл в **папке ./src/app** с именем **user.ts** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="a0d80-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. <span data-ttu-id="a0d80-124">Создание компонента Angular для верхней навигации на странице.</span><span class="sxs-lookup"><span data-stu-id="a0d80-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="a0d80-125">В CLI запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a0d80-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="a0d80-126">После завершения команды откройте **./src/app/nav-bar/nav-bar.component.ts** и замените содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="a0d80-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean = false;
      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = undefined;
      }
    }
    ```

1. <span data-ttu-id="a0d80-127">Откройте **./src/app/nav-bar/nav-bar.component.html и замените** его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="a0d80-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="a0d80-128">Создание домашней страницы для приложения.</span><span class="sxs-lookup"><span data-stu-id="a0d80-128">Create a home page for the app.</span></span> <span data-ttu-id="a0d80-129">Запустите следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="a0d80-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="a0d80-130">После завершения команды откройте **./src/app/home.component.ts** и замените содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="a0d80-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }
    }
    ```

1. <span data-ttu-id="a0d80-131">Откройте **./src/app/home/home.component.html** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="a0d80-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="a0d80-132">Создайте простой `Alert` класс.</span><span class="sxs-lookup"><span data-stu-id="a0d80-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="a0d80-133">Создайте новый файл в **каталоге ./src/app** с именем **alert.ts** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="a0d80-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. <span data-ttu-id="a0d80-134">Создайте службу оповещения, которую приложение может использовать для отображения сообщений пользователю.</span><span class="sxs-lookup"><span data-stu-id="a0d80-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="a0d80-135">В CLI запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a0d80-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="a0d80-136">Откройте **./src/app/alerts.service.ts** и замените содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="a0d80-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="a0d80-137">Создание компонента оповещений для отображения оповещений.</span><span class="sxs-lookup"><span data-stu-id="a0d80-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="a0d80-138">В CLI запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a0d80-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="a0d80-139">После завершения команды откройте **./src/app/alerts/alerts.component.ts** и замените содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="a0d80-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. <span data-ttu-id="a0d80-140">Откройте **./src/app/alerts/alerts.component.html** и замените его содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="a0d80-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. <span data-ttu-id="a0d80-141">Откройте **./src/app/app-routing.module.ts** и замените `const routes: Routes = [];` строку следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a0d80-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="a0d80-142">Откройте **./src/app/app.component.html** и замените все содержимое следующим.</span><span class="sxs-lookup"><span data-stu-id="a0d80-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. <span data-ttu-id="a0d80-143">Добавьте файл изображений выбора с именем **no-profile-photo.png** в **каталоге ./src/assets.**</span><span class="sxs-lookup"><span data-stu-id="a0d80-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="a0d80-144">Это изображение будет использоваться в качестве фотографии пользователя, если у пользователя нет фотографии в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a0d80-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="a0d80-145">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="a0d80-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="a0d80-146">Теперь приложение должно выглядеть совсем по-другому.</span><span class="sxs-lookup"><span data-stu-id="a0d80-146">Now, the app should look very different.</span></span>

![Снимок экрана: обновленная домашняя страница](images/create-app-01.png)
