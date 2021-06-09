# vue-test

> test task for roistat

Работает добавление отображение по уровням, подгрузка из LS. Реактивно не работает скрытие и отображение уровней. Только после перезагрузки страницы. Ошибка из-за вложенности объектов, при их мутации другие обьекты вложенные в массив так же мутируют. Надо было делать отображение в верстке вставлять еще раз в саму верстку компонент tableform
v-if:item.child.lenght>0 to передаем дочерние элементы в компонент и рендерим снова его же <table v-for="item in items"> что бы рекурсивно генерировать дерево элементов, и оперировать только им.   


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
