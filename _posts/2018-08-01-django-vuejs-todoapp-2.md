---
title: "django REST framework 과 Vue.js 를 이용한 Todo 앱 만들기 (2)"
description: "django 및 django REST framework 과 Vue.js 를 활용하여 Todo 앱을 만들어본다."
categories:
- Django
tags:
- python
- django
- djangorestframework
- vuejs
- todoapp
- 파이썬
photos:
- https://www.djangoproject.com/s/img/logos/django-logo-negative.png
- http://www.django-rest-framework.org/img/logo.png
- https://vuejs.org/images/logo.png
---

> <sub>본 글은 학습한 내용을 정리하기 위해 작성한 것으로 오류가 있을 수 있는 점 염두에 두고 읽어주시기 바랍니다.<br />
> 제가 잘못 알고 있는 부분에 대해 알려주시면 공부에 도움이 많이 될 것입니다. 감사합니다.</sub>

> 관련 글 목록
> - [django REST framework 과 Vue.js 를 이용한 Todo 앱 만들기 (1)](/_posts/2018-07-24-django-vuejs-todoapp-1.md)
> - django REST framework 과 Vue.js 를 이용한 Todo 앱 만들기 (2)

# [django REST framework](http://www.django-rest-framework.org/) 과 [Vue.js](https://kr.vuejs.org/) 를 이용한 Todo 앱 만들기 (2) #

![Vue.js 결과](/assets/images/post_img/vuejs1-index7.gif "Vue.js 결과")
*Vue.js를 활용한 예상 결과물*

[지난 글](/_posts/2018-07-24-django-vuejs-todoapp-1.md)에서 django-rest-framework를 이용하여 간단한 REST API를 만들었다. 이번 글에서는 Vue.js를 적용해 사용자 친화적인 todoapp 화면과 기능을 구성해보고자 한다. 이 과정에서 새로운 할 일을 추가하는 기능(**C**reate), 할 일 목록을 읽어와 화면에 표시해주는 기능(**R**ead 또는 **R**etrieve), 할 일의 상태를 업데이트 하는 기능(**U**pdate), 할 일을 삭제하는 기능(**D**elete 또는 **D**estroy)을 구현한다.

> 본 글은 다음 자료들을 참고하여 작성했습니다.
> - (Article) [CRUD App using Vue.js and Django](https://medium.com/quick-code/crud-app-using-vue-js-and-django-516edf4e4217)
> - (Book) [Vue.js 입문](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791188612789&orderClick=LEA&Kc=)

## 0. 실습 환경 ##

- windows 10
- python 3.7
- django 2.0.7
- djangorestframework
- vue.js
- vscode

## 1. 작업 환경 설정 ##

### 1.1 폴더 트리 구성하기 ###

먼저 django 앱에 Vue.js 를 적용시키기 위해 `todoapp` 폴더 아래 다음과 같이 새로운 폴더(`/templates`, `/static/todoapp/css`, `/static/todoapp/js`) 및 파일(`index.html`, `style.css`, `main.js`)을 추가한다.

```bash
└─todoapp
   ├─static
   │  └─todoapp
   │      ├─css
   │      │  └─style.css
   │      └─js
   │         └─main.js
   └─templates
      └─index.html
```

### 1.2 `/todos/urls.py` 파일 수정 ###

프로젝트 전체의 설정을 관리하는 `todos` 폴더 안에 있는 `urls.py` 파일을 수정하여 `http://localhost:8000/` 으로 접속 할 경우 `index.html` 로 구성된 **todoapp** 페이지가 보이도록 아래와 같이 설정한다.

```python
# /todos/urls.py
from django.urls import path, include
from django.views.generic import TemplateView                       # 추가

urlpatterns = [
    path('api/', include('todoapp.urls')),
    path('', TemplateView.as_view(template_name='index.html')),     # 추가
]
```

### 1.3 `index.html` 파일 작성 후 확인 ###

아래와 같이 `index.html` 의 기본 골격을 작성하자.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <title>Todos</title>

  </head>
  <body>
    <h1>TodoApp</h1>
  </body>
</html>
```

이후 프로젝트 폴더에서

```bash
> python manage.py runserver
```

명령어를 이용해 개발 서버를 켠 뒤 웹 브라우저를 열고 [http://localhost:8000](http://localhost:8000) 에 접속하면 아래와 같은 화면을 볼 수 있다.

![index 기본 화면](/assets/images/post_img/vuejs1-index1.png "index 기본 화면")
*index.html*

### 1.4 CSS, Javascript 파일 연결 ###

본 프로젝트에서는 django 의 템플릿 엔진을 사용하기 때문에 그에 맞게 CSS 및 Javascript 파일을 연결해야 한다. 또한 Vue.js를 비롯한 라이브러리들을 사용하기 위해 파일 설정을 해야 하는데, 이번 프로젝트에서는 간단히 CDN 방식을 이용하여 외부 라이브러리들을 사용하도록 한다.

먼저 `style.css` 와 `main.js` 파일을 `index.html` 파일에 추가하자.

```
...
<head>
...
  <!-- head의 마지막 부분에 style.css의 link를 추가한다. -->
  {% load staticfiles %}
  <link rel="stylesheet" type="text/css" href="{% static 'todoapp/css/style.css' %}" />
</head>
<body>
...
  <!-- body의 마지막 부분에 main.js의 script 태그를 추가한다. -->
  {% load staticfiles %}
  <script src="{% static 'todoapp/js/main.js' %}"></script>
</body>
...
```

외부 라이브러리는 다음의 라이브러리를 사용한다.

- [Vue.js](https://kr.vuejs.org/index.html): 본 프로젝트에서 중점적으로 사용할 프론트엔드 프레임워크
- [Axios](https://github.com/axios/axios): Promise 기반의 HTTP 클라이언트, REST API와의 연결을 위해 사용하는 라이브러리
- [Font Awesome](https://fontawesome.com/): 화면 구성시 아이콘 사용을 위한 라이브러리

각각의 라이브러리는 다음과 같이 추가할 수 있다.

```html
...
<head>
...
  <!-- style.css link 태그 위에 아래 내용을 추가한다. -->
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css" integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ" crossorigin="anonymous">
...
</head>
<body>
...
  <!-- main.js script 태그 위에 아래 내용을 추가한다. -->
  <!-- production version, optimized for size and speed -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.16/vue.js"></script>
  <!-- axios cdn -->
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
...
</body>
...
```

여기까지 작성하면 Vue.js 를 프로젝트에 사용할 준비가 끝났다.

## 2. [Vue.js](https://kr.vuejs.org/) 적용 ##

### 2.1 Vue 인스턴스 생성 및 화면 연결 ###

Vue.js 프레임워크를 사용하기 위해 Vue 인스턴스를 만들어보자.

```javascript
// /todoapp/static/todoapp/js/main.js
var app = new Vue({
  el: '#app',
  data: {
    url: 'http://localhost:8000/api/todos/',
    todos: [],
    newTodoItem: ''
  }
})
```

위의 코드는 `app` 이라는 이름의 Vue 인스턴스를 만들고, `DOM` 요소 중 `#app` (`id` 가 `app`) 인 요소에 Vue 인스턴스를 연결시킨다.
`data` 항목에 추가한 `url` 은 API 의 엔드 포인트 중 중복되는 부분을 미리 하나의 변수에 할당한 것이고, `todos` 는 할 일 목록을 담을 리스트이며, `newTodoItem` 은 새로운 아이템을 추가할 때 이를 담는 역할을 한다.

이렇게 만든 Vue 인스턴스를 `index.html` 에 연결해보자.

```html
<!-- /todoapp/templates/index.html -->
...
  <!-- <h1>TodoApp</h1> 이후 아래 내용을 추가한다. -->
  {% verbatim %}
  <div id="app">
    {{ url }}
  </div>
  {% endverbatim %}
...
```

![Vue 인스턴스 연결 후 index.html](/assets/images/post_img/vuejs1-index2.png "Vue 인스턴스 연결 후 index.html")
*Vue 인스턴스 연결 후 index.html*

역시 django 개발 서버를 실행시키고 [http://localhost:8000](http://localhost:8000) 에 접속하면 위와 같은 화면이 나온다. 여기서 **TodoApp** 아래 나오는 주소는 Vue 인스턴스의 data 항목에 있는 `url` 값과 같음을 알 수 있다. (Vue 인스턴스와 연결이 잘 되었는지 확인하고자 `url` 을 출력하였고, 이후의 내용에선 해당 부분을 삭제한다.)

코드 중간에 `{% verbatim %}` 과 `{% endverbatim %}` 으로 감싸여진 부분이 있는데 이는 해당 부분이 Vue.js의 템플릿 문법을 따르므로 django의 템플릿 엔진이 해당 부분을 무시하도록 하는 태그이다.

Vue 인스턴스가 제대로 생성되고 `index.html` 에 연결된 것을 확인하였다면 이제 본격적으로 API와의 통신을 바탕으로 할 일 관리 어플리케이션에서 필요한 기능을 추가해보도록 하자.

이후의 과정은 **Axios** 라이브러리를 활용하며, **R**ead -> **C**reate -> **D**elete -> **U**pdate 순서로 진행한다.

### 2.2 axios.get 을 활용한 Read ###

먼저 API 서버에서 받은 데이터를 화면에 보여줄 수 있도록 `index.html` 에 필요한 부분을 추가하자.

```html
<!-- /todoapp/templates/index.html -->
<!-- <div id="app"></div> 안에 아래 내용을 추가한다. -->
<!-- {{ url }} 부분은 삭제한다. -->
    <ul id="todoList">
      <li class="todoItem" v-for="(todo, index) in todos">
        <p>{{ todo.title }}</p>
      </li>
    </ul>
```

Vue 인스턴스의 `todos` 리스트의 값들을 `for` 문으로 반복하며 `<li><p>{{ todo.title }}</p></li>` 문을 만들어주는 구문이다. 즉, `todos` 리스트의 길이만큼 `<li></li>` 태그가 만들어진다.

`todos` 리스트의 초기값은 빈 리스트이기 때문에 API 서버에서 데이터를 받아와 `todos` 리스트에 추가해줘야 `index.html` 의 변화가 화면에 나타난다. 기능을 구현하기 위해서는 Vue 인스턴스의 `methods` 에 기능을 수행하는 메소드를 작성하면 된다. 여기서는 `getData` 란 이름으로 메소드를 작성하고, 이를 `created` 에 추가하여 Vue 인스턴트가 생성될 때 데이터를 불러오도록 하자.

```javascript
// todoapp/static/todoapp/js/main.js
var app = new Vue({
  el: '#app',
  data: {
    ...                                         // 생략
  },
  // 아래 내용 추가
  methods: {
    getData: function() {
      axios.get(this.url)
      .then(response => {
        this.todos = response.data;
      })
      .catch(error => {
        console.log(error);
      });
    }
  },
  created() {
    this.getData();
  }
})
```

[http://localhost:8000](http://localhost:8000)에 접속해보면 여전히 아무런 내용이 뜨지 않는데, 아직 API에 데이터를 입력한 적이 없기 때문이다. [http://localhost:8000/api/todos/](http://localhost:8000/api/todos/) 페이지에 접속해서 페이지 하단의 문자 입력 창을 통해 몇 개의 예시를 입력해보자.

![API 서버에 예시 데이터 입력하기](/assets/images/post_img/vuejs1-api1.png "API 서버에 예시 데이터 입력하기")
*API 서버에 예시 데이터 입력하기*

위와 같이 예시 데이터를 몇 개 추가한 이후 [http://localhost:8000](http://localhost:8000)에 다시 접속해보면 아래와 같이 추가한 할 일 목록이 나오는 것을 확인할 수 있다.

![할 일 목록 출력 화면1](/assets/images/post_img/vuejs1-index3.png "할 일 목록 출력 화면1")
*할 일 목록 출력 화면1*

### 2.3 axios.post 를 활용한 Create ###

이제 API 페이지에 들어가지 않고도 새로운 할 일을 추가할 수 있도록 기능을 추가해보자.

데이터를 받아올 때와 마찬가지로 `index.html` 을 수정하는 일에서 시작하자.

```html
<!-- /todoapp/templates/index.html -->
...
    <div id="app">
      <div id="todoInput">
        <input type="text" v-model="newTodoItem" />
        <button v-on:click="addTodo">
          <i class="fas fa-plus"></i>
        </button>
      </div>
      <ul id="todoList">
...
```

위의 코드 추가로 문자 입력창을 만들고 `v-model` 속성으로 Vue 인스턴스의 `newTodoItem` 과 문자 입력창의 값을 연결했다. `button` 태그는 클릭시 `addTodo` 메소드를 호출하도록 하였고, Font Awesome 라이브러리의 플러스 기호를 버튼에 삽입하였다.

`index.html` 만 수정하고 [http://localhost:8000](http://localhost:8000)에 접속해보면 새로운 텍스트 입력창과 버튼이 하나 추가 되었지만 잘 나오던 목록이 더 이상 나오지 않는 것을 확인할 수 있다.

![할 일 목록 입력 화면1 - 에러 발생](/assets/images/post_img/vuejs1-index4.png "할 일 목록 입력 화면1 - 에러 발생")
*할 일 목록 입력 화면1 - 에러 발생*

크롬 개발자 도구를 열어서 확인하면 `v-on:click="addTodo"` 가 Vue 인스턴스에 정의되어 있지 않아서 에러가 발생했음을 알 수 있다. 문제를 해결하기 위해 Vue 인스턴스의 `getData` 메소드 아래에 `addTodo` 메소드를 추가하도록 하자.

```javascript
// todoapp/static/todoapp/js/main.js
// getData 아래에 다음 내용을 추가한다.
...
    addTodo: function() {
      if (this.newTodoItem !== "") {
        axios.post(this.url, {
          'title': this.newTodoItem
        })
        .then(result => {
          this.todos.push(result.data);
          this.clearInput();
        })
        .catch(error => {
          console.log(error);
        });
      }
    },
    clearInput: function() {
      this.newTodoItem = '';
    }
...
```

여기서 `newTodoItem` 은 문자 입력창의 값을 갖고 있으므로 `addTodo` 이 호출되면 이 문자 입력창이 비어있지 않은지 검사하고, 만약 빈 문자열이 아닐 때 `axios.post` 로 입력받은 문자열을 `title` 로 하는 **할 일**을 추가하게 된다. 이후 정상적으로 할 일이 추가되면 해당 할 일을 Vue 인스턴스의 `todos` 리스트에도 추가하고, 문자 입력창을 비우는 것으로 작업을 마무리한다. 문자 입력창을 비우는 작업은 추후 재활용성을 고려해 `clearInput` 이라는 별도의 메소드로 구성하였다.

위의 내용을 `main.js` 에 추가하고 다시 [http://localhost:8000](http://localhost:8000) 페이지에 접속하면 기존의 목록이 잘 출력됨을 확인 할 수 있고, 아래와 같이 새로운 할 일을 추가할 수 있다.

![할 일 목록 입력 화면2](/assets/images/post_img/vuejs1-index5.gif "할 일 목록 입력 화면2")
*할 일 목록 입력 화면2*

### 2.4 axios.delete 를 활용한 Delete ###

이번에는 기존의 할 일을 지우는 기능을 추가해보자. 역시 `index.html` 을 수정하는 작업부터 시작한다.

```html
<-- /todoapp/templates/index.html -->
<-- <p>{{ todo.title }}</p> 아래에 다음 내용을 추가한다. -->
...
    <span class="removeButton" v-on:click="removeTodo(todo, index)">
      <i class="fas fa-trash-alt"></i>
    </span>
...
```

`main.js` 의 `clearInput` 메소드 다음에 아래와 같이 `removeTodo(todo, index)` 메소드를 추가한다.

```javascript
// /todoapp/static/todoapp/js/main.js
// clearInput 메소드 아래에 다음 내용을 추가한다.
...
    removeTodo: function(todo, index) {
      axios.delete(this.url + todo.id)
      .then(result => {
        this.todos.splice(index, 1);
      })
      .catch(error => {
        console.log(error);
      });
    }
...
```

![할 일 목록 삭제 화면](/assets/images/post_img/vuejs1-index6.gif "할 일 목록 삭제 화면")
*할 일 목록 삭제 화면*

페이지를 열고 휴지통 아이콘을 클릭하면 위와 같이 해당 할 일이 사라지는 것을 확인할 수 있다. 이후 페이지를 새로고침 하거나 API 서버를 확인하면 해당 할 일이 제대로 지워졌음을 알 수 있다.

### 2.5 axios.put 을 활용한 Update ###

마지막으로 할 일의 `complete` 속성을 변경하는 기능을 추가해보자. 역시 `index.html` 을 먼저 수정한다. 이번에는 `<p>{{ todo.title }}</p>` 이 있던 자리를 아래 코드로 대체하자.

```html
<!-- /todoapp/templates/index.html -->
<!-- <p>{{ todo.title }}</p> 자리를 아래 코드로 대체한다. -->
...
    <span class="checkButton" v-on:click="toggleComplete(todo, index)">
      <i class="fas fa-check" v-bind:style="{ color: (todo.complete ? 'green' : 'red') }"></i>
      <p>{{ todo.title }}</p>
    </span>
...
```

기존의 `<p>{{ todo.title }}</p>` 부분까지 `<span></span>` 태그로 감싸면서 할 일 목록 전체의 어디를 클릭해도 `toggleComplete` 메소드를 호출 할 수 있도록 했다. 한편, `todo.complete` 의 값에 따라 아이콘의 색이 바뀔 수 있도록 `v-bind:style` 을 이용해 아이콘에 `attribute` 를 동적으로 연결했다.

이제 클릭시 호출할 `toggleComplete` 메소드를 추가하자.

```javascript
// /todoapp/static/todoapp/js/main.js
// removeTodo 메소드 아래에 다음 코드를 추가한다.
...
    toggleComplete: function(todo, index) {
      var updatedData = {
        'id': todo.id,
        'title': todo.title,
        'complete': !todo.complete,
        'created': todo.created
      };
      axios.put(this.url + todo.id + '/', updatedData)
      .then(result => {
        todo.complete = !todo.complete;
      })
      .catch(error => {
        console.log(error);
      });
    }
...
```

`toggleComplete` 메소드에서는 `updatedData` 를 만들어 `complete` 속성을 제외한 값을 기존 `todo` 의 값으로 하고, `complete` 속성을 기존과 반대되는 값으로 설정하여 `axios.put` 에 인자로 넘겨준다. 서버에 변경 사항이 잘 반영되면 Vue 인스턴스 내의 `todo` 값의 `complete` 속성 값을 변경한다.

`complete` 속성의 변화는 아래와 같이 아이콘의 색이 바뀌는 것으로 확인할 수 있다.

![complete 속성 변경 화면](/assets/images/post_img/vuejs1-index7.gif "complete 속성 변경 화면")
*complete 속성 변경 화면*

## 3. 다음 글 계획 ##

이어지는 글에서는 CSS 를 적용하여 화면을 조금 더 예쁘게 구성하고, 약간의 기능을 추가한다.

![CSS 적용 및 기능 추가 예상 결과물](/assets/images/post_img/todoapp1.gif "CSS 적용 및 기능 추가 예상 결과물")
*CSS 적용 및 기능 추가 예상 결과물*