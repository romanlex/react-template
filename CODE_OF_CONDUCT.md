<!--
При `npm i` пакет ставится в `node_modules` и любые изменения в нем не попадают в дев сборку
Т.к. вебпак тащит пакет из `node_modules`
### Абсолютные импорты
> **Абсолютный** путь не зависит от текущего местоположения и позволяет обращаться к папкам и файлам по коротким понятным путям, минуя сложную вложенность, вроде `"../../../../src/components"`.

Для обеспечения работоспособности абсолютных импортов во всех системах и любых IDE, лучше всего воспользоваться нативным механизмом разрешения путей - линковкой файла \ каталога в `package.json`. На примере папки `components` из данного репозитория, в ней должен находится файл `package.json` с двумя обязательными полями `version` и `name`: `{ "version": "1.0.0", "name": "components" }`. После создания этого файла остается лишь в корне проекта выполнить команду c указанием пути к новому модулю `npm i -S src/components` или `yarn add file:src/components`. Теперь в проекте ко всем вложенным в `components` структурам можно обращаться по короткому пути:
```javascript
import { App } from "components/App";
``` -->

### Структура кода компонента в файле
Последовательно:
1. Импорты из сторонних библиотек
2. `\n` Локальные импорты по абсолютным путям
3. `\n` Локальные импорты по относительным путям путям
> При этом каждая группа импортов должна соблюдать внутреннюю последовательность:
> 1. Типы
> 2. Константы
> 3. Функции
> 4. Компоненты
> 5. Стили
4. Объявление типов
5. Объявление констант
6. Объявление функций
7. Объявление стилизованных компонентов (`styled-components`)
    > Следует объявлять от вложенных к верхнеуровневым компонентам, т.к. в последних вложенные компоненты могуть использоваться как селекторы
8. Объявление и экспорт основного (и единственного) класса компонента с приставкой `Raw`
    > `Raw` - сырой компонент необходим для модульного тестирования
    
    Наполнение компонента:
    1. Объявление типов `static propTypes = {}`
    2. Объявление свойств `state = {}`
    3. Объявление методов `getState() { return this.state }`
    4. Объявление методов-обработчиков `handleClick = event => {}`
    5. `render() {}`
9. Объявление и экспорт основного класса компонента без приставки `Raw`, с присвоением `Raw` класса, при необходимости, обернутого в HOC.
    > `export const App = connect(mstp)(AppRaw)`

При этом файл не должен быть более **200** строк. Когда количество кода в файле переваливает за 100 - 150 строк - стоит задуматься о разбиение его на модули.
<!--
  если в файле связанный друг с другом код (например, Компонент), то он должен быть не больше 2000 строк;
  если в коде не связанный друг с другом код (например, АПИ), то его длинна не ограничена;
-->

## IDE

### Снипеты

#### Создание компонента

##### vscode

```json
{
  "Create React Component": {
    "prefix": "crc",
    "body": [
      "import * as React from 'react';",
      "import styled from 'styled-components';",
      "",
      "const Container = styled.div`",
      "\tdisplay: flex;",
      "`;",
      "",
      "export class ${1:App}Raw extends React.Component {",
      "\trender() {",
      "\t\tconst { children } = this.props;",
      "",
      "\t\treturn <Container>{children}</Container>;",
      "\t}",
      "}",
      "",
      "export const ${1:App} = ${1:App}Raw;",
      "",
    ]
  }
}
```