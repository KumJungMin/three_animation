# three.js

# 2. what is three.js ? (2)

## 1) three.js는 webGL과 같다?

- three.js는 3D객체를 랜더링할 때 webGL을 사용한다.
- webGL은 점, 선, 삼각형만을 그리며 상당히 많은 양의 코드를 짜야한다.
- 하지만 three.js는 3D요소들의 처리를 도와, 직관적인 코드를 짤 수 있다!

## 2) three.js앱의 구조

### (1) three.js는 트리구조이다.

![three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(1).png](three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(1).png)

- Renderer

: scene, camera 객체를 받아, 카메라의 절두체(사물을 랜더링하는 영역) 안 3D 씬 일부를 이미지로 랜더링한다.

- scene graph

 : scene이 최상위 노드로서, 배경색과 안개 등의 요소를 포함한다. 부모 자식 트리구조이다.

: 자동차(부모)의 방향이 변하면 바퀴(자식)의 방향도 같이 움직인다.

- Texture : 이미지나 canvas로 생성한 이미지 또는 scene객체에서 랜더링한 결과물이다.
- Light : 여러 종류의 광원을 말한다.

### (2) Mesh, Geometry, Material로 객체를 그린다.

![three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(2).png](three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(2).png)

- Mesh : Metrial로 하나의 Geometry를 그리는 객체이다.
- Geometry : 기하학 객체의 정점 데이터로서, 구, 정육면체, 면, 나무 등 다양하다. (기하학 객체들은 정점들의 모음이라고 생각하자)
- Meterial : 기하학 객체를 그리는데 사용하는 표면 속성으로, 하나의 Material는 여러개의 texure를 사용할 수 있다.

### (3) 여러 개의 Mesh가 하나의 Material, Geometry를 동시 참조할 수 있다.

- Material, Geometry는 재사용이 가능하다.
- 아래는 주황색 정육면체 2개를 그리는 경우이다.

    ![three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(4).png](three%20js%20b821a16253954afcafebad4f6545977a/Untitled_Diagram_(4).png)

- 두 정육면체의 위치가 달라야 하니, 2개의 mesh를 준비한다.
- 각 mesh는 똑같은, 정점데이터와 색을 사용하므로 같은 Geometry, Material를 참조한다.

---

## 3) Hello Cube!

### (1) three.js 로드

- `type="module"`을 꼭 사용해야 한다.

```html
<script type="module">
import * as THREE from './resources/threejs/r122/build/three.module.js';
</script>
```

### (2) canvas 태그 작성

- html에 canvas 태그를 작성한다.
- 만약 Three.js에 canvas 요소를 넘겨주지 않으면, Three.js는 canvas 요소를 자동으로 생성한다.

```html
<body>
  <canvas id="c"></canvas>
</body>
```

### (3) three.js, 랜더링

- `WebGLRenderer`은 입력한 데이터를 실제로  canvas에 그려준다.
- renderer종류는 다양한데 WebGLRenderer은 3차원 세상을 canvas에 그려준다.

```html
<script type="module">
import * as THREE from './resources/threejs/r122/build/three.module.js';
 
function main() {
  const canvas = document.querySelector('#c');          //canvas 참조
  const renderer = new THREE.WebGLRenderer({canvas});   //canvas에 랜더링 객체를 뿌릴거야
  ...
</script>
```

### (4) 카메라 준비

- `PerspectiveCamera(원근 카메라)`를 준비한다.

![three%20js%20b821a16253954afcafebad4f6545977a/_2020-11-06__2.11.46.png](three%20js%20b821a16253954afcafebad4f6545977a/_2020-11-06__2.11.46.png)

- fov

: 시야각으로  아래 예제는 수직면 75도로 설정했다. 

: Three.js의 대부분이 각도 단위로 호도(radians)를 사용한다.

: 하지만, 원근 카메라만 특이하게 도(degrees)를 인자로 받는다.

- aspect

: canvas의 가로 세로 비율이다. 

: 기본 설정으로 canvas의 크기는 300x150이다.

- near, far

: 는 카메라 앞에 렌더링되는 공간 범위를 지정하는 요소이다. 

: 이 공간 바깥에 있는 요소는 화면에서 잘려나가며, 렌더링되지 않는다.

- 절두체 (camera ,fov, near, far)

: *"절두체"*는 끝부분이 잘려나간 피라미드처럼 생긴 3차원 모양이다.

: 절두체 안에 있는 요소만 렌더링되며, 바깥에 있는 요소는 렌더링되지 않는다.

```jsx
const fov = 75;
const aspect = 2;  // the canvas default
const near = 0.1;
const far = 5;
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
```

### (5) 카메라 각도 조절

- 카메라는 -Z 축 +Y 축, 즉 아래를 바라본다.
- 정육면체를 원점에 놓을 예정이니 카메라를 조금 뒤로 움직여 화면 안에 들어오게 한다.

![three%20js%20b821a16253954afcafebad4f6545977a/_2020-11-06__2.19.46.png](three%20js%20b821a16253954afcafebad4f6545977a/_2020-11-06__2.19.46.png)

```jsx
camera.position.z = 2;
```

### (6)Scene 만들기

- 뭔가를 화면에 렌더링하고 싶다면 먼저 Scene에 추가해야 한다.

```jsx
const scene = new THREE.Scene();
```

### (7) 정육면체 객체 생성하기

- BoxGeometry 생성자를 호출한다.

```jsx
const boxWidth = 1;
const boxHeight = 1;
const boxDepth = 1;
const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
```

### (8) 정육면체 색 지정하기

- Material을 만들고 색을 지정한다.
- 색을 지정할 때는 CSS처럼 숫자 형태의 hex 코드를 이용한다.(css의 string형태로 사용도 가능)

```jsx
const material = new THREE.MeshBasicMaterial({color: 0x44aa88});
```

### (9) Mesh 만들기

- Geometry(물체의 형태) + Material(물체의 색, 밝기, 질감 등)을 이용해 Mesh를 만든다.
- Mesh는 Geometry, Material 외에도 물체의 위치, 방향, 크기 등을 담은 객체이다.

### (10) Scene에 Mesh 넣기

- 완성된 정육면체 Mesh를 Scene에 넣어준다.

```jsx
scene.add(cube);
```

### (11) render에게 scene와 camera 넘겨주기

- renderer의 render 메서드에 Scene과 Camera를 넘겨주면 화면을 렌더링한다.

```jsx
renderer.render(scene, camera);
```

### (12) 객체에 애니메이션 주기

- 애니메이션을 구현할 때는 `requestAnimationFrame`를 사용한다.
- `requestAnimationFrame` 함수는 브라우저에 애니메이션 프레임을 요청하는 함수이다.
- 우리가 보는 동영상은 여러 프레임의 모음이다.(종이를 촤르르 넘기면서 움직이는 기법과 유사)
- 그래서 우리는 `requestAnimationFrame`함수를 재귀적으로 호출하여 프레임을 계속 호출해야 움직임을 볼 수 있다.

아니 재귀적으로 호출하면 제 컴퓨터가 터지잖아요??

아닙니다! three.js는 사용자가 해당 브라우저를 보고 있을 때만 실행된답니다!(와, 효율적이야)

```jsx
function render(time) {
	time *= 0.001; //time -> seconds로 변환

	cube.rotation.x = time;  //time마다 cube가 회전
	cube.rotation.y = time;

	requestAnimationFrame(render);
}
requestAnimationFrame(render);
```