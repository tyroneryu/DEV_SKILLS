# JS
## Ajax와 서버
- Ajax(Asynchronous JavaScript ans XML): 비동기적인 웹 애플리케이션 개발에 사용하는 기술

### Ajax with follow
#### 사전준비
1. M:N까지 진행한 Django 프로젝트
2. 가상환경 생성 및 활성화, 패키지 설치

#### Ajax 적용
- 프로필 페이지에 axios CDN 작성
```html
<!-- accounts/profile.html -->

<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
  
</script>
```
- `form` 요소 선택을 위해 `id` 속성 지정 및 선택
- `action`과 `method` 속성은 삭제
- 요청은 axios로 대체되기 때문
```html
<!-- accounts/profile.html -->

<form id="follow-form">
  {% csrf_token %}
  {% if request.user in person.followers.all %}
    <input type="submit" value="언팔로우">
  {% else %}
    <input type="submit" value="팔로우">
  {% endif %}
</form>
```
```js
const formTag = document.querySelector('#follow-form')
```
- `form` 요소에 이벤트 핸들러 할당
- `submit` 이벤트의 기본 동작 취소
```js
// accounts/profile.html

formTag.addEventListener('submit', function(event) {
  event.preventDefault()
})
```
- axios 요청 작성
1. url에 작성할 `user pk`는 어떻게 작성해야 하는가
2. `csrftoken`은 어떻게 보내야 하는가
```js
// accounts/profile.html

formTag.addEventListener('submit', function(event) {
  event.preventDefault()
  axios({
    method: 'post',
    url: `/accounts/${}/follow/`,
  })
})
```
- url에 작성할 `user pk` 가져오기
```html
<!-- accounts/profile.html -->

<form id="follow-form" data-user-id="{{ person.pk }}">
  {% csrf_token %}
  {% if request.user in person.followers.all %}
    <input type="submit" value="언팔로우">
  {% else %}
    <input type="submit" value="팔로우">
  {% endif %}
</form>
```
```js
// accounts/profile.html

formTag.addEventListener('submit', function(event) {
  event.preventDefault()
  const userId = event.currentTarget.dataset.userId
  axios({
    method: 'post',
    url: `/accounts/${}/follow/`,
  })
})
```

#### `data-*` 속성
- 사용자 지정 데이터 특성을 만들어 임의의 데이터를 HTML과 DOM 사이에서 교환할 수 있는 방법
```html
<div data-my-id="my-data"></div>

<script>
  const myId = event.target.dataset.myId
</script>
```
- 모든 사용자 지정 데이터는 JS에서 `dataset` 속성을 통해 사용
- 주의 사항
    1. 대소문자 여부에 상관없이 `xml`문자로 시작 불가
    2. 세미콜론 포함 불가
    3. 대문자 포함 불가

#### Ajax 적용 이어서..
- 요청 url 작성 마무리
```js
// accounts/profile.html

formTag.addEventListener('submit', function(event) {
  event.preventDefault()
  const userId = event.currentTarget.dataset.userId
  axios({
    method: 'post',
    url: `/accounts/${userId}/follow/`,
  })
})
```
- 문서상 `input hidden` 타입으로 존재하는 `csrf token` 데이터를 이제는 axios로 전송해야 함
- `csrf` 값을 가진 input 요소를 직접 선택 후 axios에 작성
```js
// accounts/profile.html
const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

formTag.addEventListener('submit', function(event) {
  event.preventDefault()
  const userId = event.currentTarget.dataset.userId
  axios({
    method: 'post',
    url: `/accounts/${userId}/follow/`,
    headers: {'X-CSRFToken': csrftoken},
  })
})
```
- 팔로우 버튼을 토글하기 위해서는 현재 팔로우 상태인지 언팔로우 상태인지에 대한 상태 확인이 필요
- Django의 view 함수에서 팔로우 여부를 파악할 수 있는 변수를 추가로 생성해 JSON 타입으로 응답하기
- 팔로우 상태 여부를 JS에게 전달할 데이터 작성
- 응답은 더이상 HTML 문서가 아닌 JSON 데이터로 응답
```python
# accounts/views.py

from django.http import JsonResponse

@login_required
def follow(request, user_pk):
    me = request.user
    you = get_user_model().objects.get(pk=user_pk)

    if me != you:
        if me in you.followers.all():
            you.followers.remove(me)
            is_followed = False
        else:
            you.followers.add(me)
            is_followed = True
        context = {
            'is_followed': is_followed
        }
        return JsonResponse(context)
    return redirect('accounts:profile', you.username)
```
- 팔로우 요청 후 Django 서버로부터 받은 데이터 확인하기
- 응답 데이터 `is_followed`에 따라 팔로우 버튼을 토글하기
```js
// accounts/profile.html

axios({
  method: 'post',
  url: `/accounts/${userId}/follow/`,
  headers: {'X-CSRFToken': csrftoken},
})
  .then((response) => {
    const isFollowed = response.data.is_followed
    const followBtn = document.querySelector('input[type=submit]')
    if (isFollowed === true) {
      followBtn.value = 'Unfollow'
    } else {
      followBtn.value = 'Follow'
    }
})
```
- 클라이언트와 서버 간 XHR 객체를 주고 받는 것을 확인하기
- 개발자도구 - Network
![JS43](./asset/JS43.PNG)
![JS44](./asset/JS44.PNG)
- 팔로잉 수와 팔로워 수 비동기 적용
- 해당 요소를 선택할 수 있도록 `span` 태그와 `id` 속성 작성
```html
<!-- accounts/profile.html -->

<div>
  팔로잉 : <span id="followings-count">{{ person.followings.all|length }}</span> /
  팔로워 : <span id="followers-count">{{ person.followers.all|length }}</span>
</div>
```
- 각 `span` 태그를 선택
```js
// accounts/profile.html

.then((response) => {
  const isFollowed = response.data.is_followed
  const followBtn = document.querySelector('input[type=submit]')
  if (isFollowed === true) {
    followBtn.value = 'Unfollow'
  } else {
    followBtn.value = 'Follow'
  }
  // 14. 팔로잉/팔로워 수를 출력하는 span 태그를 선택
  const followingsCountTag = document.querySelector('#followings-count')
  const followersCountTag = dictsort.querySelector('#followers-count')
})
```
- Django view 함수에서 팔로워, 팔로잉 인원 수 연산을 진행하여 결과를 응답 데이터로 전달
```python
# accounts/views.py

@login_required
def follow(request, user_pk):
    me = request.user
    you = get_user_model().objects.get(pk=user_pk)

    if me != you:
        if me in you.followers.all():
            you.followers.remove(me)
            is_followed = False
        else:
            you.followers.add(me)
            is_followed = True
        context = {
            'is_followed': is_followed,
            'followings_count': you.followings.count(),
            'followers_count': you.followers.count(),
        }
        return JsonResponse(context)
    return redirect('accounts:profile', you.username)
```
- 응답 데이터를 받아 각 태그의 인원 수 값 변경에 적용
```js
// accounts/profile.html

.then((response) => {
  const isFollowed = response.data.is_followed
  const followBtn = document.querySelector('input[type=submit]')
  if (isFollowed === true) {
    followBtn.value = 'Unfollow'
  } else {
    followBtn.value = 'Follow'
  }
  // 14. 팔로잉/팔로워 수를 출력하는 span 태그를 선택
  const followingsCountTag = document.querySelector('#followings-count')
  const followersCountTag = dictsort.querySelector('#followers-count')

  followingsCountTag.textContent = response.data.followings_count
  followersCountTag.textContent = response.data.followers_count
})
```

### Ajax with likes
### Ajax 좋아요 적용 시 유의 사항
- 전반적인 Ajax 적용은 팔로우 구현 과정과 모두 동일
- 단, 팔로우와 달리 좋아요 버튼은 한 페이지에 여러 개가 존재
- 모든 좋아요 버튼에 이벤트 리스너를 할당...?

### 버블링
- 한 요소에 이벤트가 발생하면 이 요소에 할당된 핸들러가 동작하고 이어서 부모 요소의 핸들러가 동작하는 현상
- 가장 최상단의 조상 요소(`document`)를 만날 떄까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작

### 버블링이 필요한 이유
- 만약 각자 다른 동작을 수행하는 버튼이 여러개 있다고 가정
- 각 버튼마다 이벤트 핸들러 할당...?
- 각 버튼의 공통 조상인 `div` 요소에 이벤트 핸들러 단 하나만 할당

### Ajax 적용
- 모든 좋아요 `form` 요소를 포함하는 최상위 요소 작성
```html
<!-- articles/index.html-->

<article class="article-container">
  {% for article in articles %}
    ...
  {% endfor %}
</article>
```
- 최상위 요소 선택, 이벤트 핸들러 할당
- 하위 요소들의 `submit` 이벤트를 감지하고 `submit` 기본 이벤트를 취소
```js
// articles/index.html

const articleContainer = document.querySelector('.article-container')

articleContainer.addEventListener('submit', function (event) {
  event.preventDefault()
})
```
- axios 작성
- url에 작성해야 하는 `article pk`는 어떻게??
- 각 좋아요 `form`에 `article.pk`를 부여 후 HTML의 `article.pk` 값을 JS에서 참조
```html
<!-- articles/index.html -->

<form data-article-id="{{ article.pk }}">
  {% csrf_token %}
  {% if request.user in article.like_users.all %}
    <input type="submit" value="좋아요 취소">
  {% else %}
    <input type="submit" value="좋아요">
  {% endif %}
</form>
```
```js
// articles/index.html

const articleContainer = document.querySelector('.article-container')
const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

articleContainer.addEventListener('submit', function (event) {
  event.preventDefault()
  const articleId = event.target.dataset.articleId
  axios({
    method: 'post',
    url: `/articles/${articleId}/likes`,
    headers: {'X-CSRFToken': csrftoken},
  })
})
```

#### `currentTarget` & `target`
- `currentTarget` 속성
    - 현재 요소
    - 항상 이벤트 핸들러가 연결된 요소만을 참조하는 속성
    - `this`와 같음
- `target` 속성
    - 이벤트가 발생한 가장 안쪽의 요소를 참조하는 속성
    - 실제 이벤트가 시작된 요소
    - 버블링이 진행되어도 변하지 않음

### Ajax 적용 이어서...
- url 완성 후 요청 및 응답
```js
// articles/index.html

articleContainer.addEventListener('submit', function (event) {
  event.preventDefault()

  const articleId = event.target.dataset.articleId
  axios({
    method: 'post',
    url: `/articles/${articleId}/likes`,
    headers: {'X-CSRFToken': csrftoken},
  })
    .then((response) => {
      
    })
    .catch((error) => {
      
    })
})
```
- 좋아요 버튼을 토글하기 위해서는 현재 사용자가 좋아요를 누른 상태인지 좋아요를 누르지 않은 상태인지에 대한 상태 확인이 필요
- Django의 view 함수에서 좋아요 여부를 파악할 수 있는 변수 추가 생성
- JSON 타입으로 응답하기
```python
# articles/views.py

from django.http import JsonResponse

@login_required
def likes(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    if request.user in article.like_users.all():
        article.like_users.remove(request.user)
        is_liked = False
    else:
        article.like_users.add(request.user)
        is_liked = True
    context = {
        'is_liked': is_liked
    }
    return JsonResponse(context)
```
- 응답 데이터 `is_liked`를 받아 `isLiked` 변수에 할당
```js
// articles/index.html

axios({
  method: 'post',
  url: `/articles/${articleId}/likes`,
  headers: {'X-CSRFToken': csrftoken},
})
  .then((response) => {
    const isLiked = response.data.is_liked
  })
  .catch((error) => {
    
  })
```
- `isLiked`에 따라 좋아요 버튼을 토글하기
- 문자와 `article`의 `pk` 값을 혼합하여 `id` 속성 값을 설정
```html
<!-- articles/index.html -->

{% if request.user in article.like_users.all %}
  <input type="submit" value="좋아요 취소" id="like-{{ article.pk }}">
{% else %}
  <input type="submit" value="좋아요" id="like-{{ article.pk }}">
{% endif %}
```
- 각 좋아요 버튼을 선택 후 `isLiked`에 따라 버튼을 토글
```js
// articles/index.html

.then((response) => {
  const isLiked = response.data.is_liked
  const likeBtn = document.querySelector(`#like-${articleId}`)
  if (isLiked === true) {
    likeBtn.value = '좋아요 취소'
  } else {
    likeBtn.value = '좋아요'
  }
})
```

#### 버블링을 활용하지 않은 경우
1. `querySelectorAll()`을 사용해 전체 좋아요 버튼을 선택
2. `forEach()`를 사용해 전체 좋아요 버튼을 순회하면서 진행
