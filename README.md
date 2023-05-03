<h1 align="center">Admiral</h1>

## 🖥 Introduction

Ниже приведена демонстрация работы админки:

_Здесь видео и картинки, влюбляющие в пакет с первого взгляда_

## 📖 Table of Contents

-   [ℹ️ About](#ℹ️about)
-   [✨ Features](#✨-features)
-   [🚨 Какие проблемы решаем](#🚨-Какие-проблемы-решаем)
-   [🔨 Installation](#🔨-installation)
-   [📦 Usage](#📦-usage)
    -   [📦 Interaction with API](#📦-interaction-with-api)
        -   [📦 Auth](#auth)
        -   [📦 CRUD](#crud)
    -   [📦 Menu](#menu)
    -   [📦 Themes](#📦-themes)
    -   [📦 Icons](#📦-icons)
-   [🤝 Contributing](#contributing)
-   [© License](#license)
-   [📚 Contact](#contact)
-   [📚 Authors](#authors)

## ℹ️About

Admiral - это фронтенд фреймворк для создания административных интерфейсов на React. Он предоставляет готовые компоненты и инструменты, которые помогают упростить и ускорить процесс разработки административных интерфейсов.

## ✨ Features

Некоторые из возможностей, предоставляемых Admiral:

-   📀 Набор высококачественных компонентов React из коробки.
-   🛡 Написано на TypeScript с встроенными статическими типами.
-   👨‍💻 Адаптивный дизайн: интерфейс библиотеки адаптируется к любому размеру экрана, что обеспечивает удобство использования на мобильных устройствах.
-   🌍 Интернационализация: поддержка разных языков и локализация интерфейса.
-   👨‍🎨 Потрясающий дизайн: интерфейс библиотеки имеет современный дизайн, который обеспечивает удобство использования.
-   🎨 Темы оформления: библиотека поддерживает темы оформления, которые позволяют изменять цветовую схему интерфейса.

## 🚨 Какие проблемы решаем

Admiral решает ряд проблем, связанных с созданием административных интерфейсов на React. Вот некоторые из этих проблем:

-   **Сложность разработки.** Создание административного интерфейса может быть сложной задачей, особенно если вы только начинаете работать с React. Admiral предоставляет готовые компоненты и инструменты, которые помогают упростить этот процесс и ускорить разработку.
-   **Несогласованность дизайна.** Часто разные разработчики создают разные компоненты, что может привести к несогласованности дизайна. Admiral решает эту проблему, предоставляя единый дизайн и стиль для всех компонентов.
-   **Сложность поддержки.** Поддержка административного интерфейса может быть сложной задачей, особенно если вы используете различные библиотеки и компоненты. Admiral решает эту проблему, предоставляя все необходимые компоненты и инструменты в одной библиотеке.
-   **Низкая производительность.** Некоторые административные интерфейсы могут работать медленно и тормозить из-за большого количества запросов к серверу и сложных операций на клиенте. Admiral решает эту проблему, используя современные технологии, такие как React Hooks, Redux и GraphQL, что позволяет достичь высокой производительности и быстрой отзывчивости интерфейса.
-   **CRUD.** Admiral также предоставляет инструменты для быстрого создания CRUD (Create, Read, Update, Delete) интерфейсов. Благодаря готовым компонентам и функциональности, вы можете быстро создавать таблицы с данными, формы для создания и редактирования объектов, а также компоненты для удаления объектов. Мы также подготовили [DEMO](examples/express-server/README.md) для быстрого старта Вашего проекта.

В общем и целом, использование Admiral позволяет упростить и ускорить процесс разработки административных интерфейсов на React, а также повысить их качество и производительность.

## 🔨 Installation

Вы можете воспользоваться несколькими способами установки Admiral:

### 📦 NPX

```bash
npx create-admiral-app

# or

npx create-admiral-app my-app
```

### 📦 Использовать один из наших examples

Подробная инструкция по установке и запуску находится в каждом из примеров.

-   [Express Server](examples/express-server/README.md)

Затем откройте ваш браузер и посетите страницу http://localhost:3000.

## 📦 Usage

Файл App.tsx является точкой входа в приложение. В нем происходит инициализация библиотеки и рендеринг компонента `Admin`.

```tsx
import React from 'react'
import { Admin, createRoutesFrom, OAuthProvidersEnum } from '../admiral'
import Menu from './config/menu'
import dataProvider from './dataProvider'
import authProvider from './authProvider'

const apiUrl = '/api'
// import all pages from pages folder and create routes
const Routes = createRoutesFrom(import.meta.globEager('../pages/**/*'))

function App() {
    return (
        <Admin
            dataProvider={dataProvider(apiUrl)}
            authProvider={authProvider(apiUrl)}
            menu={Menu}
            oauthProviders={[
                OAuthProvidersEnum.Google,
                OAuthProvidersEnum.Github,
                OAuthProvidersEnum.Jira,
            ]}
        >
            <Routes />
        </Admin>
    )
}

export default App
```

### Взаимодействие с API

#### Auth - [AuthProvider](src/authProvider.ts)

Основной контракт для авторизации в системе представляет интерфейс `AuthProvider`.

```ts
export interface AuthProvider {
    login: (params: any) => Promise<any>
    logout: (params: any) => Promise<void | false | string>
    checkAuth: (params: any) => Promise<void>
    getIdentity: () => Promise<UserIdentity>
    oauthLogin?: (provider: OAuthProvidersEnum) => Promise<{ redirect: string }>
    oauthCallback?: (provider: OAuthProvidersEnum, data: string) => Promise<any>

    [key: string]: any
}
```

Пример реализации можно посмотреть в [authProvider.ts](src/authProvider.ts).

Разберем основные методы реализации в таблице:

| Метод         | Название                          | Описание                                                                                                                                                | Параметры                                                                                            | Возвращаемое значение                                                          |
| ------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| login         | Авторизация пользователя          | Делает POST запрос на `/api/login` и сохраняет поле `token` в localStorage, которое используется в дальнейших запросах                                  | `params` - объект с полями `username` и `password`                                                   | Объект с полем `token` и объектом `user`, в котором есть поля `email` и `name` |
| logout        | Logout пользователя               | Делает POST запрос на `/api/logout` и удаляет поле `token` из localStorage                                                                              |                                                                                                      | `void`                                                                         |
| checkAuth     | Проверка авторизации пользователя | Делает GET запрос на `/api/checkAuth` и проверяет валидность токена, ожидает код статуса - 200. Используется при каждом использовании API               | Bearer `token`                                                                                       | Status code 200                                                                |
| getIdentity   | Получение данных пользователя     | Делает GET запрос на `/api/getIdentity` и возвращает объект с данными пользователя                                                                      | Bearer `token`                                                                                       | Объект `user` с полями `email` и `name`                                        |
| oauthLogin    | Авторизация через OAuth           | Делает GET запрос на `/api/auth/social-login/${provider}` и возвращает объект с полем `redirect`, который используется для редиректа                    | `provider` - OAuth провайдер                                                                         | Объект с полем `redirect`                                                      |
| oauthCallback | Callback авторизации через OAuth  | Делает POST запрос на `/api/auth/social-login/${provider}/callback` и сохраняет поле `token` в localStorage, который используется в дальнейших запросах | `provider` - OAuth провайдер, `data` - данные, полученные от OAuth провайдера, где есть поле `token` | Объект с полем `token`                                                         |

#### CRUD [DataProvider](src/dataProvider.ts)

Основной контракт для работы с данными представляет интерфейс `DataProvider`.

```ts
export interface DataProvider {
    getList: (resource: string, params: any) => Promise<any>
    getOne: (resource: string, params: any) => Promise<any>
    getMany: (resource: string, params: any) => Promise<any>
    getManyReference: (resource: string, params: any) => Promise<any>
    create: (resource: string, params: any) => Promise<any>
    update: (resource: string, params: any) => Promise<any>
    updateMany: (resource: string, params: any) => Promise<any>
    delete: (resource: string, params: any) => Promise<any>
    deleteMany: (resource: string, params: any) => Promise<any>

    [key: string]: any
}
```

Разберем основные методы реализации в таблице:

| Метод   | Название                   | Описание                                                                                                                           | Параметры                                                                                          |
| ------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| getList | Получение списка сущностей | Делает GET запрос на `/api/${resource}` и возвращает объект с данными, которые будут использоваться в компоненте `List`            | `resource` - название ресурса, `params` - объект с параметрами запроса                             |
| getOne  | Получение сущности         | Делает GET запрос на `/api/${resource}/${id}` и возвращает объект с данными, которые будут использоваться в компоненте `Show`      | `resource` - название ресурса, `id` - идентификатор сущности                                       |
| create  | Создание сущности          | Делает POST запрос на `/api/${resource}` и возвращает объект с данными, которые будут использоваться в компоненте `Create`         | `resource` - название ресурса, `params` - объект с данными сущности                                |
| update  | Обновление сущности        | Делает PUT запрос на `/api/${resource}/${id}` и возвращает объект с данными, которые будут использоваться в компоненте `Edit`      | `resource` - название ресурса, `id` - идентификатор сущности, `params` - объект с данными сущности |
| delete  | Удаление сущности          | Делает DELETE запрос на `/api/${resource}/${id}` и возвращает объект с данными, которые будут использоваться в компоненте `Delete` | `resource` - название ресурса, `id` - идентификатор сущности                                       |

### Menu

Меню представляет собой массив объектов, которые имеют следующую структуру:

```tsx
import { Menu, SubMenu, MenuItemLink } from '../../admiral'

const CustomMenu = () => {
    return (
        <Menu>
            <MenuItemLink icon="FiCircle" name="First Menu Item" to="/first" />
            <SubMenu icon="FiCircle" name="Second Menu Item">
                <MenuItemLink icon="FiCircle" name="Sub Menu Item" to="/second" />
            </SubMenu>
        </Menu>
    )
}

export default CustomMenu
```

### Icons

Icons used in Admiral are from [React Icons](https://react-icons.github.io/react-icons/).

## 🤝 Contributing

Если вы хотите внести свой вклад в развитие Admiral, то просто сделайте форк репозитория, внесите свои изменения и отправьте pull request. Мы будем рады рассмотреть ваши предложения!

## ©️ License

Эта библиотека распространяется под лицензией MIT. Подробности можно узнать в файле LICENSE.\_

## 📚 Contact

Если у вас есть какие-либо вопросы, пожалуйста, свяжитесь с нами по адресу: <a href="mailto:admiral@dev.family">admiral@dev.family</a>

## 📚 Authors

-   **Alexandra Kashina** - _Initial work_ - <a href="https://github.com/alexandrakashina">alexandrakashina</a>
