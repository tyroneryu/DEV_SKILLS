# Vue
## DRF 프로젝트
### django 프로젝트 안내
- 스켈레톤 프로젝트 django-pjt 제공
- 외부 패키지 및 라이브러리는 requirements.txt에 작성되어있음
- DRF 프로젝트는 주석을 해제하며 진행

#### Skeleton code 살펴보기
- Model 클래스 확인
```py
# articles/models.py

from django.db import models
from django.conf import settings


class Article(models.Model):
    # user = models.ForeignKey(
    #     settings.AUTH_USER_MODEL, on_delete=models.CASCADE
    # )
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```
```py
# accounts/models.py

from django.db import models
from django.contrib.auth.models import AbstractUser


# Create your models here.
class User(AbstractUser):
    pass
```
- URl 확인
```py
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
    # path('accounts/', include('dj_rest_auth.urls')),
    # path('accounts/signup/', include('dj_rest_auth.registration.urls')),
]
```
```py
# articles/urls.py

urlpatterns = [
    path('articles/', views.article_list),
    path('articles/<int:article_pk>/', views.article_detail),
]
```
- Serializers 확인
```py
# articles/serializers.py

from rest_framework import serializers
from .models import Article


class ArticleListSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = ('id', 'title', 'content')


class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = '__all__'
        # read_only_fields = ('user',)
```
- views.py의 import 부분 확인
```py
# articles/views.py

from rest_framework.response import Response
from rest_framework.decorators import api_view
from rest_framework import status

# permission Decorators
# from rest_framework.decorators import permission_classes
# from rest_framework.permissions import IsAuthenticated

from django.shortcuts import get_object_or_404, get_list_or_404

from .serializers import ArticleListSerializer, ArticleSerializer
from .models import Article
```
- view 함수 확인
```py
# articles/views.py

@api_view(['GET', 'POST'])
# @permission_classes([IsAuthenticated])
def article_list(request):
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            # serializer.save(user=request.user)
            return Response(serializer.data, status=status.HTTP_201_CREATED)

@api_view(['GET'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)

    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        print(serializer.data)
        return Response(serializer.data)
```
- settings.py 확인
```py
# settings.py

INSTALLED_APPS = [
    'articles',
    'accounts',
    'rest_framework',
    # 'rest_framework.authtoken',
    # 'dj_rest_auth',
    # 'corsheaders',
    # 'django.contrib.sites',
    # 'allauth',
    # 'allauth.account',
    # 'allauth.socialaccount',
    # 'dj_rest_auth.registration',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# REST_FRAMEWORK = {
#     # Authentication
#     'DEFAULT_AUTHENTICATION_CLASSES': [
#         'rest_framework.authentication.TokenAuthentication',
#     ],
#     # permission
#     'DEFAULT_PERMISSION_CLASSES': [
#         'rest_framework.permissions.AllowAny',
#     ],
# }
```
- fixtures 확인
- 가상환경 생성 후 아래 코드 실행
```
$ python -m venv venv
$ source venv/Scripts/activate
$ pip install -r requirements.txt
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py loaddata articles.json
```
- 서버 실행 후 전체 게시글 조회 요청해보기

### Vue 프로젝트 안내
- 스켈레톤 프로젝트 vue-pjt 제공
- Vite를 사용해 Pinia 및 Vue router가 추가 되어 있음
- pinia-plugin-persistedstate가 설치 및 등록 되어있음
- Vue 프로젝트는 직접 코드를 작성하며 진행
- 컴포넌트 구조
![Vue47](./asset/Vue47.PNG)

#### Skeleton code 살펴보기
- App 컴포넌트
```vue
<!-- App.vue -->

<template>
  <header>
    <nav>
    </nav>
  </header>
  <RouterView />
</template>

<script setup>
import { RouterView } from 'vue-router'
</script>

<style scoped>
</style>
```
- route에 등록된 컴포넌트(Article, Create, Detail, LogIn, SignUp)
- ArticleList 컴포넌트
```vue
<!-- components/ArticleList.vue -->

<template>
  <div>
    <h3>Article List</h3>
    <ArticleListItem />
  </div>
</template>

<script setup>
import ArticleListItem from '@/components/ArticleListItem.vue'
</script>
```
- ArticleListItem 컴포넌트
```vue
<!-- components/ArticleListItem.vue -->

<template>
  <div>
  </div>
</template>

<script setup>
</script>
```
- routes 현황
```js
// router/index.js

import { createRouter, createWebHistory } from 'vue-router'
// import ArticleView from '@/views/ArticleView.vue'
// import DetailView from '@/views/DetailView.vue'
// import CreateView from '@/views/CreateView.vue'
// import SignUpView from '@/views/SignUpView.vue'
// import LogInView from '@/views/LogInView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    // {
    //   path: '/',
    //   name: 'ArticleView',
    //   component: ArticleView
    // },
```
- store 현황
```js
// store/counter.js

import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  return {  }
}, { persist: true })
```
- main.js 현황
```js
// src/main.js

import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'

const app = createApp(App)
const pinia = createPinia()

pinia.use(piniaPluginPersistedstate)
// app.use(createPinia())
app.use(pinia)
app.use(router)

app.mount('#app')
```
- 패키지 설치 후 서버 실행
```
$ npm install
$ npm run dev
```

## 메인 페이지 구현
### 시작하기 전에..
- 무결점의 프로젝트를 만드는 것이 아님
- frontend 프레임워크와 backend 프레임워크 간의 요청과 응답, 그 과정에서 등장하는 새로운 개념과 문제를 해결하면서 하나의 웹 애플리케이션 서비스를 구현하는 과정에 집중

### 게시글 목록 출력
- ArticleView 컴포넌트에 ArticleList 컴포넌트와 ArticleListItem 컴포넌트 등록 및 출력하기
- AtticleList와 ArticleListItem은 각각 게시글 출력을 담당
- ArticleView의 route 관련 코드 주석 해제
```js
// router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import ArticleView from '@/views/ArticleView.vue'
// import DetailView from '@/views/DetailView.vue'
// import CreateView from '@/views/CreateView.vue'
// import SignUpView from '@/views/SignUpView.vue'
// import LogInView from '@/views/LogInView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'ArticleView',
      component: ArticleView
    },
  ]
})
```
- App 컴포넌트에 ArticleView 컴포넌트로 이동하는 RouterLink 작성
```vue
<!-- App.vue -->

<template>
  <header>
    <nav>
      <RouterLink :to="{ name: 'ArticleView' }">Articles</RouterLink>
    </nav>
  </header>
  <RouterView />
</template>

<script setup>
import { RouterView, RouterLink } from 'vue-router'
</script>
```
- ArticleView 컴포넌트에 ArticleList 컴포넌트 등록
```vue
<!-- views/ArticleView.vue -->

<template>
  <div>
    <h1>Article Page</h1>
    <ArticleList />
  </div>
</template>

<script setup>
import ArticleList from '@/components/ArticleList.vue'
</script>
```
- store에 임시 데이터 articles 배열 작성하기
```js
// store/counter.js

export const useCounterStore = defineStore('counter', () => {
  const articles = ref([
    { id: 1, title:'Article 1', content: 'Content 1' },
    { id: 2, title:'Article 2', content: 'Content 2' },
  ])
  return { articles }
}, { persist: true })
```
- ArticleList 컴포넌트에서 게시글 목록 출력
- store의 articles 데이터 참조
- v-for를 활용하여 하위 컴포넌트에서 사용할 article 단일 객체 정보를 props로 전달
```vue
<!-- components/ArticleList.vue -->

<template>
  <div>
    <h3>Article List</h3>
    <ArticleListItem
      v-for="article in store.articles"
      :key="article.id"
      :article="article"
    />
  </div>
</template>

<script setup>
import { useCounterStore } from '@/stores/counter';
import ArticleListItem from '@/components/ArticleListItem.vue'

const store = useCounterStore()
</script>
```
- ArticleListItem 컴포넌트는 내려받은 props를 정의 후 출력
```vue
<!-- components/ArticleListItem.vue -->

<template>
  <div>
    <h5>{{ article.id }}</h5>
    <p>{{ article.title }}</p>
    <p>{{ article.content }}</p>
  </div>
</template>

<script setup>
defineProps({
  article: Object
})
</script>
```

### DRF와의 요청과 응답
- 이제는 임시 데이터가 아닌 DRF 서버에 요청하여 데이터를 응답 받아 store에 저장 후 출력하기
- DRF 서버로의 AJAX 요청을 위한 axios 설치 및 관련 코드 작성
- `$ npm install axios`
- DRF 서버로 요청을 보내고 응답 데이터를 처리하는 getArticles 함수
```js
// store/counter.js

import { ref, computed } from 'vue'
import { defineStore } from 'pinia'
import axios from 'axios'

export const useCounterStore = defineStore('counter', () => {
  const articles = ref([])
  const API_URL = 'http://127.0.0.1:8000'
  const getArticles = function() {
    axios({
      method: 'get',
      url: `${API_URL}/api/v1/articles/`
    })
    .then(response => {
      console.log(response)
    })
    .catch(error => {
      console.log(error)
    })
  }
  return { articles, API_URL, getArticles }
}, { persist: true })
```
- ArticleView 컴포넌트가 마운트 될 때 getArticles 함수가 실행되도록 함
- 해당 컴포넌트가 렌더링 될 때 항상 최신 게시글 목록을 불러오기 위함
```vue
<!-- views/ArticleView.vue -->

<template>
  <div>
    <h1>Article Page</h1>
    <ArticleList />
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useCounterStore } from '@/stores/counter'
import ArticleList from '@/components/ArticleList.vue'

const store = useCounterStore()

onMounted(() => {
  store.getArticles()
})
</script>
```
- Vue와 DRF 서버를 모두 실행한 후 응답 데이터 확인
- 에러 발생
- 그런데 DRF 서버 측에서는 문제없이 응답했음(200 OK)
- 서버는 응답했으나 브라우저 측에서 거절한 것
- 브라우저가 거절한 이유: XMLHttpRequest에 대한 접근이 CORS policy에 의해 차단되었다.

## CORS Policy
### SOP
- SOP(Same origin policy): 동일 출처 정책
- 어떤 출처(Origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호 작용하는 것을 제한하는 보안 방식
- 다른 곳에서 가져온 자료는 일단 막는다
- 웹 애플리케이션의 도메인이 다른 도메인의 리소스에 접근하는 것을 제어하여 사용자의 개인 정보와 데이터의 보안을 보호하고, 잠재적인 보안 위협을 방지
- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임

#### Origin (출처)
- URL의 Protocol, Host, Port를 모두 포함하여 출처라고 부름
- Same Origin 예시
  - 아래 세 영역이 일치하는 경우에만 동일 출처(Same-origin)으로 인정
![Vue48](./asset/Vue48.PNG)
  - http://localhost:3000/articles/3/ 을 기준으로 동일 출처 여부를 비교

|URL|결과|이유|
|:---:|:---:|:---:|
|http://localhost:3000/articles/3/|성공|Path만 다름|
|http://localhost:3000/comments/3/|성공|Path만 다름|
|https://localhost:3000/articles/3/|실패|Protocol 다름|
|http://localhost:80/articles/3/|실페|Port 다름|
|http://yahuua:3000/articles/3/|실패|Host 다름|

#### CORS Policy의 등장
- 기본적으로 웹 브라우저는 같은 출처에서만 요청하는 것을 허용하며, 다른 출처로의 요청은 보안상의 이유로 차단됨
  - SOP에 의해 다른 출처의 리소스와 상호작용하는 것이 기본적으로 제한되기 때문
- 하지만 현대 웹 애플리케이션은 다양한 출처로부터 리소스를 요청하는 경우가 많기 때문에 CORS 정책이 필요하게 되었음
- CORS는 웹 서버가 리소스에 대한 서로 다른 출처 간 접근을 허용하도록 선택할 수 있는 기능을 제공

### CORS
- CORS (Cross-Origin Resource Sharing): 교차 출처 리소스 공유
- 특정 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
- 만약 다른 출처의 리소스를 가져오기 위해서는 이를 제공하는 서버가 브라우저에게 다른 출처지만 접근해도 된다는 사실을 알려야 함
- CORS policy (교차 출처 리소스 공유 정책)

#### CORS Policy
- CORS Policy (Cross-Origin Resource Sharing Policy): 교차 출처 리소스 공유 정책
- 다른 출처에서 온 리소스를 공유하는 것에 대한 정책
- 서버에서 설정되며, 브라우저가 해당 정책을 확인하여 요청이 허용되는지 여부를 결정
- 다른 출처의 리소스를 불러오려면 그 다른 출처에서 올바른 CORS header를 퐘한 응답을 반환해야 함

#### CORS 적용 방법
![Vue49](./asset/Vue49.PNG)

#### CORS Policy 정리
- 웹 애플리케이션이 다른 도메인에 있는 리소스에 안전하게 접근할 수 있도록 허용 또는 차단하는 보안 메커니즘
- 서버가 약속된 CORS Header를 포함하여 응답한다면 브라우저는 해당 요청을 허용
- 서버에서 CORS Header를 만들어야 함

### CORS Headers 설정
- Django에서는 django-cors-headers 라이브러리 활용
- 손쉽게 응답 객체에 CORS header를 추가해주는 라이브러리

#### django-cors-headers 사용하기
- 설치 (requirements.txt로 인해 사전에 설치되어 있음)
- `$ pip install django-cors-headers`
- 관련 코드 주석 해제
- CORS를 허용할 Vue 프로젝트의 Domain 등록
```py
# settings.py

INSTALLED_APPS = [
    'articles',
    'accounts',
    'rest_framework',
    # 'rest_framework.authtoken',
    # 'dj_rest_auth',
    'corsheaders',
    # 'django.contrib.sites',
    # 'allauth',
    # 'allauth.account',
    # 'allauth.socialaccount',
    # 'dj_rest_auth.registration',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    # 'allauth.account.middleware.AccountMiddleware',
]

CORS_ALLOWED_ORIGINS = [
    'http://127.0.0.1:5173',
    'http://localhost:5173',
]
```
- 메인 페이지에서 DRF 응답 데이터 확인

## Article CR 구현
### 전체 게시글 목록 저장 및 출력
- 응답 받은 데이터에서 각 게시글의 데이터 구성 확인 (id, title, content)
- store에 게시글 목록 데이터 저장
```js
// store/counter.js

import { ref, computed } from 'vue'
import { defineStore } from 'pinia'
import axios from 'axios'

export const useCounterStore = defineStore('counter', () => {
  const articles = ref([])
  const API_URL = 'http://127.0.0.1:8000'
  const getArticles = function() {
    axios({
      method: 'get',
      url: `${API_URL}/api/v1/articles/`
    })
    .then(response => {
      console.log(response.data)
      articles.value = response.data
    })
    .catch(error => {
      console.log(error)
    })
  }
  return { articles, API_URL, getArticles }
}, { persist: true })
```
- store에 저장된 게시글 목록 출력 확인
- pinia-plugin-persistedstate에 의해 브라우저 Local Storage에 저장됨

### 단일 게시글 데이터 출력
- DetailView 관련 route 주석 해제
```js
// router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import ArticleView from '@/views/ArticleView.vue'
import DetailView from '@/views/DetailView.vue'
// import CreateView from '@/views/CreateView.vue'
// import SignUpView from '@/views/SignUpView.vue'
// import LogInView from '@/views/LogInView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'ArticleView',
      component: ArticleView
    },
    {
      path: '/articles/:id',
      name: 'DetailView',
      component: DetailView
    },
```
- ArticleListItem에 DetailView 컴포넌트로 가기 위한 RouterLink 작성
```vue
<!-- components/ArticleListItem.vue -->

<template>
  <div>
    <h5>{{ article.id }}</h5>
    <p>{{ article.title }}</p>
    <p>{{ article.content }}</p>
    <RouterLink
      :to="{ name: 'DetailView', params: { id: article.id } }"
    >
      [Detail]
    </RouterLink>
    <hr>
  </div>
</template>

<script setup>
import { RouterLink } from 'vue-router'

defineProps({
  article: Object
})
</script>
```
- DetailView가 마운트 될 때 특정 게시글을 조회하는 AJAX 요청 진행
```vue
<!-- views/DetailView.vue -->

<script setup>
import axios from 'axios'
import { onMounted } from 'vue'
import { useCounterStore } from '@/stores/counter'
import { useRoute } from 'vue-router'
import { ref } from 'vue'

const store = useCounterStore()
const route = useRoute()

onMounted(() => {
  axios({
    method: 'get',
    url: `${store.API_URL}/api/v1/articles/${route.params.id}/`,
  })
  .then((response) => {
    console.log(response.data)
  })
  .catch((error) => {
    console.log(error)
  })
})
</script>
```
- 응답 데이터 확인, 저장 후 출력
```vue
<!-- views/DetailView.vue -->

<template>
  <div>
    <h1>Detail</h1>
    <div v-if="article">
      <p>글 번호 : {{ article.id }}</p>
      <p>제목 : {{ article.title }}</p>
      <p>내용 : {{ article.content }}</p>
      <p>작성시간 : {{ article.created_at }}</p>
      <p>수정시간 : {{ article.updated_at }}</p>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { onMounted } from 'vue'
import { useCounterStore } from '@/stores/counter'
import { useRoute } from 'vue-router'
import { ref } from 'vue'

const store = useCounterStore()
const route = useRoute()
const article = ref(null)

onMounted(() => {
  axios({
    method: 'get',
    url: `${store.API_URL}/api/v1/articles/${route.params.id}/`,
  })
  .then((response) => {
    console.log(response.data)
    article.value = response.data
  })
  .catch((error) => {
    console.log(error)
  })
})
</script>
```
- 결과 확인

### 게시글 작성
- CreateView 관련 route 주석 해제
```js
// router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import ArticleView from '@/views/ArticleView.vue'
import DetailView from '@/views/DetailView.vue'
import CreateView from '@/views/CreateView.vue'
// import SignUpView from '@/views/SignUpView.vue'
// import LogInView from '@/views/LogInView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'ArticleView',
      component: ArticleView
    },
    {
      path: '/articles/:id',
      name: 'DetailView',
      component: DetailView
    },
    {
      path: '/create',
      name: 'CreateView',
      component: CreateView
    },
```
- ArticleView에 CreateView 컴포넌트로 가기 위한 RouterLink 작성
```vue
<!-- views/ArticleView.vue -->

<template>
  <div>
    <h1>Article Page</h1>
    <RouterLink :to="{ name: 'CreateView' }">
      [CREATE]
    </RouterLink>
    <ArticleList />
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useCounterStore } from '@/stores/counter'
import { RouterLink } from 'vue-router'
import ArticleList from '@/components/ArticleList.vue'

const store = useCounterStore()

onMounted(() => {
  store.getArticles()
})
</script>
```
- v-model을 사용해 사용자 입력 데이터를 양방향 바인딩
- v-model의 trim 수식어를 사용해 사용자 입력 데이터의 공백 제거
```vue
<!-- views/CreateView.vue -->

<template>
  <div>
    <h1>게시글 작성</h1>
    <form>
      <input type="text" v-model.trim="title">
      <textarea v-model.trim="content"></textarea>
      <input type="submit">
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const title = ref(null)
const content = ref(null)
</script>
```
- 게시글 생성 요청을 담당하는 createArticle 함수 작성
- 게시글 생성이 성공한다면 ArticleView 컴포넌트로 이동
```vue
<!-- views/CreateView.vue -->

<script setup>
import axios from 'axios'
import { useCounterStore } from '@/stores/counter'
import { useRouter } from 'vue-router'

const store = useCounterStore()
const title = ref(null)
const content = ref(null)
const router = useRouter()
const createArticle = function() {
  axios({
    method: 'post',
    url: `${store.API_URL}/api/v1/articles/`,
    data: {
      title: title.value,
      content: content.value,
    },
  })
  .then(() => {
    router.push({ name: 'ArticleView' })
  })
  .catch((error) => {
    console.log(error)
  })
}
</script>
```
- submit 이벤트가 발생하면 createArticle 함수를 호출
- v-on의 prevent 수식어를 사용해 submit 이벤트의 기본 동작 취소
```vue
<!-- views/CreateView.vue -->

<template>
  <div>
    <h1>게시글 작성</h1>
    <form @submit.prevent="createArticle">
      <input type="text" v-model.trim="title">
      <textarea v-model.trim="content"></textarea>
      <input type="submit">
    </form>
  </div>
</template>
```
