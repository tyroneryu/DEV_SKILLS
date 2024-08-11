# Responsive Web
## Bootstrap Grid system
- 웹 페이지의 레이아웃을 조정하는데 사용되는 12개의 컬럼으로 구성된 시스템
- Grid system의 목적
    - 반응형 디자인을 지원해 웹 페이지를 모바일, 태블릿, 데스크탑 등 다양한 기기에서 적절하게 표시할 수 있도록 도움
- 반응형 웹 디자인 (Responsive Web Design)
    - 디바이스 종류나 화면 크기에 상관없이, 어디서든 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술

### Grid system 구조
#### 1. container
- column 들을 담고 있는 공간        
![Grid01](./asset/Grid01.PNG)

#### 2. Column
- 실제 컨텐츠를 포함는 부분         
![Grid02](./asset/Grid02.PNG)

#### 3. Gutter
- 컬럼과 컬럼 사이의 여백 영역
![Grid03](./asset/Grid03.PNG)

- 1개의 row 안에 12개의 column 영역이 구성
- 각 요소는 12개 중 몇 개를 차지할 것인지 지정됨        
![Grid04](./asset/Grid04.PNG)

### Grid system 실습
#### Basic         
```html
  <h2 class="text-center">Basic</h2>
  <div class="container">
    <div class="row">
      <div class="box col">col</div>
      <div class="box col">col</div>
      <div class="box col">col</div>
    </div>
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-4">col-4</div>
      <div class="box col-4">col-4</div>
    </div>
    <div class="row">
      <div class="box col-2">col-2</div>
      <div class="box col-8">col-8</div>
      <div class="box col-2">col-2</div>
    </div>
  </div>
```     
![Grid05](./asset/Grid05.PNG)

#### Nesting(중첩)
```html
  <h2 class="text-center">Nesting</h2>
  <div class="container">
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-8">
        <div class="row">
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
        </div>
      </div>
    </div>
  </div>
```     
![Grid06](./asset/Grid06.PNG)

#### Offset(상쇄)
```html
  <h2 class="text-center">Offset</h2>
  <div class="container">
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-4 offset-4">col-4 offset-4</div>
    </div>
    <div class="row">
      <div class="box col-3 offset-3">col-3 offset-3</div>
      <div class="box col-3 offset-3">col-3 offset-3</div>
    </div>
    <div class="row">
      <div class="box col-6 offset-3">col-6 offset-3</div>
    </div>
  </div>
```     
![Grid07](./asset/Grid07.PNG)

#### Gutters
- Grid system에서 column 사이에 여백 영역
- x축: padding, y축: margin으로 여백 생성
```html
  <h2 class="text-center">Gutters(gx-0)</h2>
  <div class="container">
    <div class="row gx-0">
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>

  <br>

  <h2 class="text-center">Gutters(gy-5)</h2>
  <div class="container">
    <div class="row gy-5">
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>


  <br>

  <h2 class="text-center">Gutters(g-5)</h2>
  <div class="container">
    <div class="row g-5">
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>
```     
![Grid08](./asset/Grid08.PNG)

### 참고
#### The Grid system
- CSS가 아닌 편집 디자인에서 나온 개념으로, 구성 요소를 잘 배치하여 시각적으로 좋은 결과물을 만들기 위함
- 기본적으로 안쪽에 있는 요소들의 오와 열을 맞추는 것에서 기인
- 정보 구조와 배열을 채계적으로 작성하여 정보의 질서를 부여하는 시스템      
![Grid09](./asset/Grid09.PNG)

## Grid system for responsive web
### Responsive Web Design
- 디바이스 종류나 화면 크기에 상관없이, 어디서든 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술
- Bootstrap grid system에서는 12개 column과 6개 breakpoin를 사용하여 반응형 웹 디자인을 구현

### Grid system breakpoints
- 웹 페이지를 다양한 화면 크기에서 적절하게 배치하기 위한 분기점
- 화면 너비에 다라 6개의 분기점 제공 (xs, sm, md, lg, xl, xxl)

| |xs (<576px>)|sm (>=576px)|md (>=768px)|lg (>=992px)|xl (>=1200px)|xxl (>=1400px)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Container `max-width`|None (auto)|540px|720px|960px|1140px|1320px|
|Class prefix|`.col-`|`.col-sm-`|`.col-md-`|`.col-lg-`|`.col-xl-`|`.col-xxl-`|
- 각 breakpoints마다 설정된 최대 너비 값 이상으로 화면이 커지면 grid system 동작이 변경됨

#### breakpoints 실습
```html
  <h2 class="text-center">Breakpoints</h2>
  <div class="container">
    <div class="row">
      <div class="col-12 col-sm-6 col-md-2 col-lg-3 col-xl-4 box">
        col
      </div>
      <div class="col-12 col-sm-6 col-md-8 col-lg-3 col-xl-4 box">
        col
      </div>
      <div class="col-12 col-sm-6 col-md-2 col-lg-3 col-xl-4 box">
        col
      </div>
      <div class="col-12 col-sm-6 col-md-12 col-lg-3 col-xl-12 box">
        col
      </div>
    </div>
```     
![Grid10](./asset/Grid10.PNG)

#### breakpoints + offset 실습
```html
    <h2 class="text-center">Breakpoints + offset</h2>
    <div class="row g-4">
      <div class="col-12 col-sm-4 col-md-6 box">
        col
      </div>
      <div class="col-12 col-sm-4 col-md-6 box">
        col
      </div>
      <div class="col-12 col-sm-4 col-md-6 box">
        col
      </div>
      <div class="col-12 col-sm-4 col-md-6 offset-sm-4 offset-md-0 box">
        col
      </div>
    </div>
```     
![Grid11](./asset/Grid11.PNG)

#### Media Query로 작성된 Grid system의 breakpoints
```css
/* small devices (landscape phones, 576px and up) */
@media (min-width: 576px) {
    ...
}
/* medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
    ...
}
/* large devices (desktops, 992px and up) */
@media (min-width: 992px) {
    ...
}
/* X-large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
    ...
}
/* XX-large devices (larger desktops, 1400px and up) */
@media (min-width: 1400px) {
    ...
}
```

### 참고
#### Grid cards
- row-cols 클래스를 사용하여 행당 표시할 열(카드) 수를 손쉽게 제어할 수 있음        
![Grid12](./asset/Grid12.PNG)
```html
  <h2 class="text-center">Grid Cards</h2>
  <div class="container">
    <div class="row row-cols-1 row-cols-sm-3 row-cols-md-2 gy-3">
      <div class="col">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">This is a longer card with supporting text below as a natural lead-in to additional
              content. This content is a little bit longer.</p>
          </div>
        </div>
      </div>
      <div class="">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">This is a longer card with supporting text below as a natural lead-in to additional
              content. This content is a little bit longer.</p>
          </div>
        </div>
      </div>
      <div class="">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">This is a longer card with supporting text below as a natural lead-in to additional
              content.</p>
          </div>
        </div>
      </div>
      <div class="">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">This is a longer card with supporting text below as a natural lead-in to additional
              content. This content is a little bit longer.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
```
