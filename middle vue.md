# 1. Жизненные циклы компонента Vue. Отличие created от mounted.
* beforeCreate Хук запускается в начале инициализации компонента. Данные не сделаны реактивными, а события еще не настроены:

* created Вы можете получить доступ к реактивным данным и активным событиям с помощью хука created. Шаблоны и виртуальная модель DOM еще не смонтированы, и их рендеринг не выполнен:

Хуки монтирования используются чаще всего. Они обеспечивают мгновенный доступ к компоненту до и после первого рендеринг. Однако они не выполняются во время рендеринга на стороне сервера. Используйте хуки монтирования, если вам требуется получить доступ или изменить DOM вашего компонента непосредственно до или после начального рендеринга. Не используйте хуки монтирования, если вам требуется доставить данные компонента при инициализации. Используйте для этой цели created (или created и activated для компонентов keep-alive). В частности, если вам нужны эти данные при рендеринге на стороне сервера.

* beforeMount запускается до начального рендеринга и после компиляции шаблона или функций рендеринга:
* mounted hook дает вам полный доступ к реактивному компоненту, шаблонам и модели DOM после рендеринга (через this.$el).

Хуки обновления вызываются, когда изменяется реактивное свойство, используемое вашим компонентом, или когда что-то еще вызывает его повторный рендеринг. Это позволяет использовать хук в цикле watch-compute-render для вашего компонента. Используйте хуки обновления, если вам нужно знать, когда ваш компонент выполняет повторный рендеринг, возможно для целей отладки или профилирования. Не используйте хуки обновления, если вам нужно знать, когда изменяется реактивное свойство вашего компонента. Используйте для этой цели computed properties или watchers.

* beforeUpdate запускается после изменения данных вашего компонента и начала цикла обновления и до исправления и повторного рендеринга модели DOM.
* updated запускается после изменения данных вашего компонента и повторного рендеринга DOM.

Хуки уничтожения позволяют выполнять действия во время уничтожения компонента, например, при очистке или отправке аналитических данных. Они срабатывают, когда ваш компонент уничтожается и удаляется из DOM.

* beforeDestroy срабатывает прямо перед уничтожением. Ваш компонент все еще присутствует и полностью функционален.
* Когда вы достигнете хука destroyed, от вашего компонента уже практически ничего не останется. Все, что было к нему прикреплено, будет уничтожено.
# 2.Что такое директива во VueJs, примеры директив
Помимо встроенных директив (таких как v-model и v-show), Vue позволяет использовать ваши собственные. При этом важно понимать, что основным механизмом создания повторно используемого кода во Vue 2.0 всё-таки являются компоненты. 

Регистрируем глобальную пользовательскую директиву `v-focus`
```
Vue.directive('focus', {
  // Когда привязанный элемент вставлен в DOM...
  inserted: function (el) {
    // Переключаем фокус на элемент
    el.focus()
  }
})
```
Чтобы зарегистрировать директиву локально, можно передать опцию directives при определении компонента:
```
directives: {
  focus: {
    // определение директивы
    inserted: function (el) {
      el.focus()
    }
  }
}

```
Теперь в шаблонах можно использовать новый атрибут v-focus:
```
<input v-focus>
```
# 3. Плагины во vue
Плагины позволяют добавить во Vue некоторую глобальную функциональность. Область применения плагинов явно не определена. Можно разделить плагины на нескольких типов:
* Добавляют глобальные методы и/или свойства, например vue-custom-element
* Добавляют глобальные объекты: директивы/фильтры/переходы и т.д., например vue-touch
* Добавляют опции компонентам через глобальную примесь, например vue-router
* Добавляют методы экземпляра Vue через Vue.prototype.
* Библиотеки, предоставляющие собственные API и комбинирующие вышеперечисленные возможности, например vue-router

Плагин Vue.js должен содержать метод install. Этот метод будет вызываться с конструктором Vue в качестве первого аргумента, и с дополнительными опциями плагина в качестве второго (если передавались)

# 4. Глобальные компоненты
Во многих проектах, глобальные компоненты определяются посредством Vue.component, с последующим new Vue({ el: '#container' }) для указания элемента-контейнера в теле каждой страницы.
Для проектов малого и среднего размера, в которых JavaScript используется лишь для некоторых страниц, этот подход может прекрасно работать. В более же сложных проектах или в случаях, когда весь ваш фронтенд управляется JavaScript, явными становятся следующие недостатки:
Глобальное определение заставляет давать уникальное имя каждому компоненту
Строковым шаблонам не хватает подсветки синтаксиса. Кроме того, приходится использовать уродливые слэши для многострочного HTML.
Нет модульной поддержки CSS — в то время как HTML и JavaScript разбиваются на модули-компоненты, CSS оказывается за бортом.
Отсутствие шага сборки ограничивает нас только HTML и ES5 JavaScript, не позволяя использовать препроцессоры вроде Pug (бывший Jade) и Babel
Все эти недостатки решаются однофайловыми компонентами с расширением .vue, использование которых позволяют такие инструменты как Webpack и Browserify.
# 5. Что такое VUEX?
Vuex - это шаблон управления состоянием и библиотека для Vue.js приложения. Он предназначен для хранения основных данных для всех компонентов приложения и гарантирует предсказуемость реактивных изменений данных.
# 6. Как сделать условный рендеринг компонента? Отличие if от show
Используйте директивы v-if и v-else, компонент будет удален из dom, если вы передадите ему ложное условие. Для сохранения элемента в директиве DOM v-show может быть использовано  css свойство display
```
<p v-if="true">Visible</p>
<p v-else>Not visible</p>
```

# 7.Ести ли в Vue.js поддержка data binding? Если да, как его использовать?
Да Vue.js поддержывает data binding, для связывания инпута и состояния следует использовать директиву v-model.
```
<input v-model="name" placeholder="What is your name?"><p>Hello, {{ name }}!</p>
```
# 8. Как реализовать маршрутизацию на стороне клиента  в Vue?

Рекомендуемый способ сделать SPA - использовать Vue Router, который является официальной библиотекой для маршрутизации, но не включен в основной фреймворк.

# 9. Как програмно сделать редирект в Vue Router?
Чтобы перейти на другую страницу програмно используйте router.push(location, onComplete?, onAbort?)
```
logOut() {
    userService.removeCurrentSession();
    this.$router.push('/login');
}
```
Также можно вернуться к какой-то точке стека истории с помощью  метода  go(n)
# 10. Как защитить какой-то маршрут от несанкционированного доступа?
Его можно сделать внутри компонента или в глобальных guards.
```
const router = new VueRouter({
    ...
}) router.beforeEach((to, from, next) ={
    if (to.isProtected() && !haveAccess(user)) {
        next(false)
    }
    next()
})
```
# 11. Обращение к элементу (рефы)
Хотя модель декларативного рендеринга Vue абстрагирует от вас большинство прямых операций DOM, все же могут быть случаи, когда нам нужен прямой доступ к базовым элементам DOM. Для этого мы можем использовать специальный refатрибут:
```
<input ref="input">
```
ref — это специальный атрибут, который позволяет нам получить прямую ссылку на конкретный элемент DOM или экземпляр дочернего компонента после его монтирования. Это может быть полезно, когда вы хотите, например, программно сфокусировать ввод на монтировании компонента или инициализировать стороннюю библиотеку для элемента.
