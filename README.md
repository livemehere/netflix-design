# HTML,CSS, JS 만으로 넷플릭스 페이지 똑같이 만들어보기

## DEMO

![](./demo/pc.gif)

---

![](./demo/mobile.gif)

## 왜 했는가?

웹 공부를 한지가 2년이 다되간다.
나의 공부방식은 숲을 보는 방식으로, 처음부터 세세하게 공부해나가기 보다는 빠르게 전체 흐름을 파악하고, 해보고싶은거 조금씩 건들여본다.

그럼 계중에 쉽게 이해되거나 그렇지 않은게 생긴다.
또 흥미로워서 더 상세히 공부해보고싶은 것도 생긴다.
그리고 무지성으로 라이브러리를 가져다 쓰는 행위를 거부하고 직접 만들어서 사용하고싶은 욕구가 강해진다.

> 스스로 풀스택이라 자부할 수 있을만큼, 완벽하진않지만, 내가원하는 서비스는 스스로 프론트부터 백엔드까지 만들 수 있는 자신감이 있다.
> 하지만 제일 자신없는 부분이 font-end를 꾸미는 일이었다. 특히나, 반응형에서 골머리를 해맸다. 그래서 지난 몇달간은 반응형과 CSS에 대해서 집중했다

## CSS를 굳이 잘해야되나?

개인적으로 프론트엔드 개발자라면 CSS는 자유자제로 다룰 줄 알아야 한다고 생각한다.
개발이라는 것이 무에서 유를 만들어내고, 머리속에만 있던 상상을 실현해 내는 일이라 생각하는데,
아무리 JS가 뛰어나거나, React와 같은 라이브러리를 잘써도, 디자인감각과, 디자인시안이 있는데도 이쁘게 만들지 못하면 말짱 도로묵이라는 생각이들었다.
간단한 예시로, 네이버 사이트에서 css를 빼면 쌩 HTML인데, 그렇게해놓으면 누가 쓰겠는가??..

극단적이지만, 그만큼 CSS가 차지하는 비중이 크다고 생각한다.
즉, UI,UX가 중요하다는 소리이다.

그리고 React를 하다가 해매고있는 나를 다시 돌아보면, 원하는 Layout을 만들지못해서 시간을 허비하는 경우도 잦았다.

## 왜 Netflix 인가?

개인적으로 생각했을 때 UI,UX가 훌륭하고, 사용자가 많은 사이트들 중에서 선정했다.
그리고 지금 slide와 scroll 에 관심이 많아서, 슬라이드가 들어간 것을 먼저 찾았다.

## 핵심 구현사항

### 슬라이드

슬라이드는 처음엔 slideJS를 사용하려고 가져와서 썻는데, CSS 커스텀하는게 귀찮아서 그냥 슬라이드도 직접 구현해보았다.
어차피 원래 직접하려는게 목표였지만, slideJS 데모를 보니, 애니메이션이 너무 괜찮아보여 써봤는데, 효과는 좋았지만, 커스텀하는 일이 생각보다 귀찮아서,
남에 라이브러리 커스텀하는거에 골머리 아플빠엔 그냥 내가 처음부터 만들지뭐.. 라는 마인드였다.

#### html

```html
 <div class="content-list">
    <h1>한국이 만든 콘텐츠</h1>
    <div class="slider">
    </div>
    <div class="prev"><i class="fa-solid fa-angle-right prev-arrow"></i></div>
    <div class="next"><i class="fa-solid fa-angle-right"></i></div>
</div>
```

#### scss

```scss
    display: flex;
    gap: .5rem;
    overflow-x: scroll;
    scroll-behavior: smooth;
    &::-webkit-scrollbar{
    display: none;
    }
```

- 사실 슬라이드에서 css는 볼게없다. 간단히 flex 주고, gap을 주었다.
- flex는 자동으로 shrink 속성이 들어가 있기때문에, shrink를 없애주던지, min-width를 넣어주면 되는데, 나는 후자를 선택해서 각 슬라이드 아이템들이 flex속성때문에 압축되는걸 방지했다
- 그리고 부드러운 스크롤을 위해서 smooth속성과, 스크롤바를 없앴다(이건 미관상)

#### javascript

```js
const next = document.querySelectorAll('.next');
const prev = document.querySelectorAll('.prev');
const slider = document.querySelectorAll('.slider')
for(let i =0;i<slider.length;i++){
    makeSlider(slider[i],prev[i],next[i]);
}
function makeSlider(element,prev,next){
    next.addEventListener('click',()=>{
        const offsetX = element.offsetWidth;
        element.scrollBy(offsetX,0)
    })
    prev.addEventListener('click',()=>{
        const offsetX = element.offsetWidth;
        element.scrollBy(-offsetX,0)
    })
}
```

> 프론트엔드에서 어떤 인터렉션을 구현할때 css 선에서 해결할지, js로 넘어갈지는 나는 아직 클릭을 기준으로 나눈다. css에서 toggle정도야 checkbox  와 label을 이용하면되지만, 클릭해서 뭔가가 변해야한다면 Js로 작업하는 수 밖에 없는거 같다.

- makeSlider()라는 함수를 만들었는데, 슬라이드를 여러개 만드려고 함수로 뺏다. 인자3개를 주면, 몸통과, 그 몸통을 슬라이드처럼 움직이게하는 버튼 두개에 이벤트를 달아준다.
- 간단히 내가 설계한 것은 슬라이드의 몸통을 flex박스로하고, overflow를 scroll로 해놓았으니, 버튼으로 스크롤만 이동시켜주면 슬라이드 완성이라고 생각했다.
- 버튼을 누르면 element의 스크롤을 x축으로 이동해주도록 했다. scrollBy(x,y)를 이용하면, 픽셀단위로 해당요소의 스크롤을 이동할 수 있다.
- 스크롤 한번에 모든 요소들이 솨라락 시원하게 바꼈으면 좋겠기 때문에, 요소의 viewport에 보이는 width값만큼 스크롤 시켰다.

#### ⭐️ DOM 요소의 다양한 크기 구하는 법

```css
offsetWidth, offsetHeight : 화면에보이는 크기 (스크롤바,패딩,보더 포함)

clientWidth, clientHeight : 화면에보이는 크기 (위사항 제외)

scrollWidth, scrollHeight : 화면에보이지 않는곳을 포함한 실제크기

getBoundingClientRect() : offsetWidth와 동일한 값을 반환하지만, 차이점은 랜더링된 크기를 반환한다는 것이다.
예를들어서 transform:scale(.5)가 적용되어있다면 그만한 크기를 반환한다.
```

- 위 사항들 처럼 슬라이드가 되는 몸체에서 스크롤 하는 픽셀값은 화면에 보이는만큼만 이동해주어야하기 때문에 scrollWidth or offsetWidth를 사용하면된다.


## 나머지

나머지 사항들은 크게 설명할 건 없는데, 방식만 설명하자면

### 동영상

```html
<main>
        <div class="video">
            <video src="./video/doctor.mp4" autoplay loop></video>
        </div>
        <div class="description">
            <h1>Doctor Strange 2</h1>
            <h3>매주 새로운 트레일러 공개</h3>
            <p>
                5월, 차원의 경계가 무너지고 닥터 스트레인지가 온다
                전 세계를 뒤흔들 역대급 멀티버스 전쟁의 시작!
                [닥터 스트레인지: 대혼돈의 멀티버스] 티저 예고편 공개!
            </p>
            <div class="buttons">
                <button class="play"><i class="fa-solid fa-play"></i><span>재생</span></button>
                <button class="detail"><i class="fa-solid fa-circle-info"></i>상세 정보</button>
            </div>
        </div>
        <div class="age-info">
            <i class="fa-solid fa-rotate-right"></i>
            <div class="age">15+</div>
        </div>
    </main>
```

- 그냥 부모요소에 자식으로 비디오랑, 설명란 두개를 놓고, 비디오는 부모에 가득차게 해두고, 설명란들과 버튼들은 absolute로 배치를했다.
- 비디오는 속성으로 autoplay loop 를 주었다.(제일 넷플릭스같은 느낌을 준다.)

### navbar

- flex로 space-between 속성줘서 구성했다.
- media query속성으로 어색한 분기점마다 조금씩 수정해주고, 모바일에서는 메뉴버튼들을 absolute로 바꾸고, 메뉴라는 모바일 전용 버튼을 만들고 hover하면 보이도록했다.(만들다보니 내가 왜 hover로했나 싶다.. 모바일은 클릭해야하는데...그냥 냅둔다...)

## 하단 footer

![](./demo/footer.png)

- 난 grid를 은근 좋아하고, 그리드가 딱맞는 곧이면 바로써버리고싶은 충동이 있다. 대부분 flex로 해결되지만, grid가 딱맞을거같은 레이아웃들이 있다. 인스타그램이라던지..
- 넷플릭스가 사진과 같이 구현해놨길래 크게는 flex로 한번감싸고, flex-direction을 column으로 준뒤, 하단 벜튼들은 그냥 grid로 해결했다.

## 썸네일들

https://yts.mx/ 오픈API를 사용하였고
비디오는 유튜브영상 하나 다운했다...

## 모바일 환경에서의 문제

- scroll-behavior 은 안먹는다.
- slide에서 overflow-y를 hidden으로 설정해주지 않아서, 터치로 슬라이드할때 위아래도 움직인다.
- scale 금지해야되는데, 안해서 중간중간 터치시 확대되는 경우가있는데 불편하다.
- 폰트설정을 안해서 사파리 폰트로보니 못생겼다.
- 동영상 자동재생이 안된다.

> 모바일은 역시그냥 간단하게하고, 차라리 webview나 앱으로 하는게.....좋지않을까...
