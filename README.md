
# 这两天没有上传笔记，在解决图床的问题，主打一个白嫖，所以要费点心思，先是用了gitee的图床好不容易配好后发现居然加了防盗链，后面又转了github的咱目前来说github也是最稳定且免费的，现在搞好了图床以后上传笔记就很快了


# 1\.轨道控制器


## 1 控制物体移动


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155882101-110d7057-c4f7-495a-8570-9a085e24ac.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155882101-110d7057-c4f7-495a-8570-9a085e24ac.gif)


前面我们创建了物体，为了让物体移动起来。我们可以设置它的position属性进行位置的设置。


相机和立方体都是物体。每个物体都是1个对象。


在官方文档里，我们可以看到相机camera和物体mesh都继承Object3D类。所以camera、mesh都属于3d对象。从3d对象的官方文档里，我们可以找到position属性，并且该属性一个vector3对象。因此通过官方vector3类的文档，我们可以简单使用下面2种方式来修改position位置，当然后面还会讲解更多的方式。



```
//设置该向量的x、y 和 z 分量。
mesh.position.set(x,y,z);
//直接设置position的x,y,z属性
mesh.position.x = x;
mesh.position.y = y;
mesh.position.z = z;
```

官方文档：[https://threejs.org/docs/index.html?q\=vect\#api/zh/math/Vector3](https://github.com)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659154999473-f1613254-0a32-423d-ab06-5ec845115743.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659154999473-f1613254-0a32-423d-ab06-5ec845115743.png)


[![](02Threejs%E5%BC%80%E5%8F%91%E5%85%A5%E9%97%A8%E4%B8%8E%E8%B0%83%E8%AF%95.assets/1659155075799-68500c3f-c4f2-4778-bd74-92fb4758377e.png)](02Threejs%E5%BC%80%E5%8F%91%E5%85%A5%E9%97%A8%E4%B8%8E%E8%B0%83%E8%AF%95.assets/1659155075799-68500c3f-c4f2-4778-bd74-92fb4758377e.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155075799-68500c3f-c4f2-4778-bd74-92fb4758377e.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155075799-68500c3f-c4f2-4778-bd74-92fb4758377e.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155259015-18d131a4-4024-412f-8475-c789e90770f7.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155259015-18d131a4-4024-412f-8475-c789e90770f7.png)


### 1\.1 每一帧修改一点位置形成动画


例如，每一帧让立方体向右移动0\.01，并且当位置大于5时，从0开始。那么可以这么设置。



```
function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

## 2 综合上述代码


1、在前面创建的项目中的main.js文件写入代码



```
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：控制3d物体移动

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
cube.position.x = 3;

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器  
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();

```

效果演示：


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155886712-2ca27601-d200-4faa-8c4f-c1be7528ff95.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659155886712-2ca27601-d200-4faa-8c4f-c1be7528ff95.gif)


## 3\.总结


就能放大缩小画面了


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115205706667.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115205706667.png)


自动在中心原点处


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115205843624.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115205843624.png)


设置位置，还可以父子级网格，子级是相对于父级来设置位置


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210215338.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210215338.png)


子级相较于父级的正3位置所以还是原点


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210246725.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210246725.png)


# 2\.物体缩放与旋转


物体的缩放与旋转是我们经常要操作的方式。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659251307199-f9a66659-bb4b-4f84-bfc1-8cd1070e0b80.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659251307199-f9a66659-bb4b-4f84-bfc1-8cd1070e0b80.gif)


## 1 scale设置缩放


因为物体的scale属性是vector3对象，因此按照vector的属性和方法，设置x/y/z轴方向的缩放大小。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659166498104-cf107704-d042-4121-a8af-08956886becc.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659166498104-cf107704-d042-4121-a8af-08956886becc.png)



```
//例如设置x轴放大3倍、y轴方向放大2倍、z轴方向不变
cube.scale.set(3, 2, 1);
//单独设置某个轴的缩放
cube.scale.x = 3

```

## 2 rotation设置旋转


因为的旋转通过设置rotation属性，该属性是Euler类的实例，因此可以通过Euler类的方法进行设置旋转角度。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659250390242-8f980762-3974-4beb-aa7a-7e00e0500b5d.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659250390242-8f980762-3974-4beb-aa7a-7e00e0500b5d.png)


因此可以通过以下方式设置旋转物体



```
//直接设置旋转属性，例如围绕x轴旋转90度
cube.rotation.x = -Math.PI/2

//围绕x轴旋转45度
cube.rotation.set(-Math.PI / 4, 0, 0, "XZY");
```

set方法，每个参数具体定义


.set ( x : Float, y : Float, z : Float, order : String ) : this


x \- 用弧度表示x轴旋转量。
y \- 用弧度表示y轴旋转量。
z \- 用弧度表示z轴旋转量。
order \- (optional) 表示旋转顺序的字符串。


### 2\.1 旋转动画


每一帧旋转弧度制的0\.01角度，实现动画代码



```
function render() {
  cube.position.x += 0.01;
  cube.rotation.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}
```

## 3 综合上述代码


1、在前面创建的项目中的main.js文件写入代码



```
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：控制3d物体移动

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
cube.position.x = 3;

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();

```

效果演示：


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659251307199-f9a66659-bb4b-4f84-bfc1-8cd1070e0b80.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1659251307199-f9a66659-bb4b-4f84-bfc1-8cd1070e0b80.gif)


## 4\.总结


缩放也是较于父级，父级变大，子级跟着大


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210609890.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115210609890.png)


# 3\.自适应屏幕大小


## 1\.1 自适应屏幕大小


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1671291087832-61b6524e-17f2-4106-a458-988c6dc400d0.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1671291087832-61b6524e-17f2-4106-a458-988c6dc400d0.gif)


你会发现，我们前面写好的代码，在页面尺寸发生改变的时候，并不能自适应的改变尺寸，而出现空白或者滚动条突出的情况。所以监听屏幕大小的改变，来重新设置相机的宽高比例和渲染器的尺寸大小，代码如下：



```
// 监听画面变化，更新渲染画面
window.addEventListener("resize", () => {
  //   console.log("画面变化了");
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight;
  //   更新摄像机的投影矩阵
  camera.updateProjectionMatrix();

  //   更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight);
  //   设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio);
});
```

aspect属性是设置摄像机视锥体的长宽比，通常是使用画布的宽/画布的高。camera.updateProjectionMatrix()用于更新摄像机投影矩阵，相机任何参数被改变以后必须被调用


## 1\.2 控制场景全屏


经常我们需要全屏的展示三维场景。例如，我们想要双击，实现全屏效果，代码如下：



```
window.addEventListener("dblclick", () => {
  const fullScreenElement = document.fullscreenElement;
  if (!fullScreenElement) {
    //   双击控制屏幕进入全屏，退出全屏
    // 让画布对象全屏
    renderer.domElement.requestFullscreen();
  } else {
    //   退出全屏，使用document对象
    document.exitFullscreen();
  }
});
```

**fullscreenElement**只读属性返回当前在此文档中以全屏模式显示的元素。


如果文档当前未使用全屏模式，则返回值为null。


使用[element.requestFullscreen()](https://github.com)方法以全屏模式查看元素，exitFullscreen方法退出全屏。


## 2 综合上述代码


1、在前面创建的项目中的main.js文件写入代码



```
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
import gsap from "gsap";

// console.log(THREE);

// 目标：js控制画面全屏

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
// cube.position.x = 3;
// 缩放
// cube.scale.set(3, 2, 1);
// cube.scale.x = 5;
// 旋转
cube.rotation.set(Math.PI / 4, 0, 0, "XZY");

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 设置控制器阻尼，让控制器更有真实效果,必须在动画循环里调用.update()。
controls.enableDamping = true;

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);
// 设置时钟
const clock = new THREE.Clock();

window.addEventListener("dblclick", () => {
  const fullScreenElement = document.fullscreenElement;
  if (!fullScreenElement) {
    //   双击控制屏幕进入全屏，退出全屏
    // 让画布对象全屏
    renderer.domElement.requestFullscreen();
  } else {
    //   退出全屏，使用document对象
    document.exitFullscreen();
  }
  //   console.log(fullScreenElement);
});

function render() {
  controls.update();
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();

// 监听画面变化，更新渲染画面
window.addEventListener("resize", () => {
  //   console.log("画面变化了");
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight;
  //   更新摄像机的投影矩阵
  camera.updateProjectionMatrix();

  //   更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight);
  //   设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio);
});
```

效果演示：


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1671291087809-3db938f0-b222-4f1c-92e8-2ea8b0cdb7b3.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1671291087809-3db938f0-b222-4f1c-92e8-2ea8b0cdb7b3.gif)


## 应对HD\-DPI显示器


HD\-DPI代表每英寸高密度点显示器(视网膜显示器)。它指的是当今大多数的Mac和windows机器以及几乎所有的智能手机。


浏览器中的工作方式是不管屏幕的分辨率有多高使用CSS像素设置尺寸会被认为是一样的。 同样的物理尺寸浏览器会渲染出字体的更多细节。


使用three.js有多种方法来应对HD\-DPI。


第一种就是不做任何特别的事情。这可以说是最常见的。 渲染三维图形需要大量的GPU处理能力。移动端的GPU能力比桌面端的要弱。至少截止到2018年, 手机都有非常高的分辨率显示器。 目前最好的手机的HD\-DPI比例为3x，意思是非高密度点显示器上的一个像素在高密度显示器上是9个像素。 意味着需要9倍的渲染。


计算9倍的像素是个大工程所以如果保持代码不变我们将计算一个像素然后浏览器将以三倍大小绘制(3x3\=9像素)。


对于大型的three.js应用来说上面就是你想要的否侧你的帧速率会很低。


尽管如此如果你确实想用设备的分辨率来渲染，three.js中有两种方法来实现。


一种是使用renderer.setPixelRatio来告诉three.js分辨率的倍数。 访问浏览器从CSS像素到设备像素的倍数然后传给three.js。


renderer.setPixelRatio(window.devicePixelRatio);


之后任何对renderer.setSize的调用都会神奇地使用您请求的大小乘以您传入的像素比例. 强烈不建议这样。 看下面。


另一种方法是在调整canvas的大小时自己处理。



```
function resizeRendererToDisplaySize(renderer) {
      const canvas = renderer.domElement;
      const pixelRatio = window.devicePixelRatio;
      const width = canvas.clientWidth * pixelRatio | 0;
      const height = canvas.clientHeight * pixelRatio | 0;
      const needResize = canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    }
```

第二章方法从客观上来说更好。为什么？因为我拿到了我想要的。 在使用three.js时有很多种情况下我们需要知道canvas的绘图缓冲区的确切尺寸。 比如制作后期处理滤镜或者我们在操作着色器需要访问gl\_FragCoord变量，如果我们截屏或者给GPU 读取像素，绘制到二维的canvas等等。 通过我们自己处理我们会一直知道使用的尺寸是不是我们需要的。 幕后并没有什么特殊的魔法发生。


## 3\.其他补充


### 3\.1 gui调试工具


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115220219132.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115220219132.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115221840993.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115221840993.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115221832251.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115221832251.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF1.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF1.gif)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115222155709.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241115222155709.png)


### 3\.2 创建顶点，形成三角形


three里面所有材质平面都是由三角形组成[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118205745969.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118205745969.png)


一个面顶点的数量，因为设置了共用顶点


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118205809786.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118205809786.png)


### 3\.3 顶点组


可以一个网格的多个材质拆分开，分别设置，需要设置顶点组，材质所以在的顶点为一个组


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210130874.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210130874.png)


创建几何体，在分别创建不同的材质，注意材质用数组


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210342341.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210342341.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210401796.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210401796.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210446710.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118210446710.png)


### 3\.4 快速创建几何体


刚才看到的是用材质创建


官网可以快速创建


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118211811954.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118211811954.png)


## 4\.基础材质


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111821332047.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111821332047.png)基础材质不受光照影响


贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213405522.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213405522.png)


加上透明度


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213422445.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213422445.png)


透明度贴图，黑色部分完全透明，只剩下白色部分


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213502815.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213502815.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111821350991.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111821350991.png)加上背景，环境以及环境贴图都为同一张鱼眼图后，可让材质如同在这个场景中


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213629041.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213629041.png)


高光贴图，越亮的区域反射光线越明显


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213716547.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213716547.png)


光照贴图，让材质能够有此贴图的光照效果


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213748077.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213748077.png)


代码实现


创建几何体，创建材质，几何体和材质形成物体


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213912347.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118213912347.png)


加载纹理贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214011902.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214011902.png)


允许透明度


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214058364.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214058364.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214043065.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214043065.png)


加载ao贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214343414.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214343414.png)


透明贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214505728.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214505728.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214802119.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214802119.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214807540.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214807540.png)


光照贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214844829.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214844829.png)


环境贴图，需要hdr加载器


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214927473.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214927473.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214950629.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118214950629.png)


想让环境包裹场景


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215026319.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215026319.png)


还可以在里面设置环境贴图，让材质可以在这个环境反射，透明等更为真实


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215331467.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215331467.png)


光照贴图，高光贴图


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215207611.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118215207611.png)


### 4\.1 纹理属性


如果感觉有色差，那么看下是不是纹理属性的色彩原因，默认是线性，和 srgb，同在50%，srbg分布的更均匀一点，从暗到白


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220122048.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220122048.png)


切换方法


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220145911.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220145911.png)


### 4\.2 雾


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220250959.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220250959.png)


两种，一种线性（慢慢开始有雾），一种指数（很快就被雾包裹）


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220413328.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220413328.png)


[![](02Threejs%E5%BC%80%E5%8F%91%E5%85%A5%E9%97%A8%E4%B8%8E%E8%B0%83%E8%AF%95.assets/image-20241118220425318.png)](02Threejs%E5%BC%80%E5%8F%91%E5%85%A5%E9%97%A8%E4%B8%8E%E8%B0%83%E8%AF%95.assets/image-20241118220425318.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220506312.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220506312.png)


# 4\.gltf模型加载


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF-173211151865866.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF-173211151865866.gif)


**真正的three场景，比如下面这样，并不是一个一个考代码写出来会很累，也很复杂，一般都是用建模软件建好之后，用gitf加载进来**


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220817312.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118220817312.png)


GLTFLoader是Three.js中用于加载glTF 2\.0资源的加载器。glTF是一种开放格式规范，旨在更高效地传输和加载3D内容。该格式文件以JSON或二进制(.glb)格式提供，可以包含外部文件存储贴图(.jpg、.png)和额外的二进制数据(.bin)。


一个glTF文件可以包含一个或多个场景，每个场景包括网格、材质、贴图、蒙皮、骨架、变形目标、动画、灯光以及摄像机等组件。这些组件可以通过GLTFLoader进行加载和解析，并在Three.js中使用。


下面是一个简单的案例，演示如何使用GLTFLoader加载模型并在Three.js中进行渲染：



```
// 创建场景
const scene = new THREE.Scene();

// 创建相机
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5;

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// 创建GLTFLoader对象并加载模型
const loader = new THREE.GLTFLoader();
loader.load('model.gltf', function (gltf) {
  // 将模型添加到场景中
  scene.add(gltf.scene);

  // 设置相机位置和渲染目标
  camera.position.x += (Math.sin(Date.now() * 0.001) * 500);
  camera.position.y += (Math.sin(Date.now() * 0.001) * 500);
  camera.lookAt(scene.position);

  // 在每一帧更新时更新相机位置和渲染目标
  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
  animate();
}, undefined, function (error) {
  console.error(error);
});
```

在这个案例中，我们首先创建了一个场景、一个相机和一个渲染器。然后创建了一个GLTFLoader对象并调用`load()`方法来加载glTF资源。当加载完成时，我们将模型添加到场景中，并设置相机的位置和渲染目标。最后在每一帧更新时调用渲染器的`render()`方法来渲染整个场景。如果加载过程中出现错误，会输出错误信息到控制台。


导进来这个物体也是个组对象，这也是个3D对象，可以去设置属性


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221449090.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221449090.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221501119.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221501119.png)


要让这个物体的材质有颜色并且可以反射光，可以去设置一个环境贴图，并且球体贴图，让四周都能够染色


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221636555.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221636555.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221644927.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221644927.png)


## 解压缩


如果一个场景物体过大需要解压缩


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221807216.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221807216.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221826066.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221826066.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221845083.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221845083.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221921365.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221921365.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221932865.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241118221932865.png)


# 5\.补件动画tween


## 光线投射实现场景交互


要拿到three里面的材质，不能通过事件，只能通过从摄像机射出一个光投射过可以拿到物体


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119203821979.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119203821979.png)


创建三个球


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119203909379.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119203909379.png)


创建射线和鼠标向量


在全局点击事件里面，将当前点击的鼠标坐标转换成three的向量坐标，\-1—1的范围，可以直接记公式


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119204509570.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119204509570.png)


在全局点击事件里面，通过摄像机和鼠标点击位置更新射线，同时计算射线穿过去的物体，**括号里面可以写一个数组为你创建的物体，也可以直接scene.children**会检测到所有穿过去的物体为一个数组


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205111936.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205111936.png)


拿到这个物体后就可以去做一些事情，比如让点击到的第一个物体为一个颜色，再次点击回到原来颜色


注意：getHex方法获取到颜色的十六进制


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111920523954.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-2024111920523954.png):[楚门加速器p](https://tianchuang88.com)


## tween 是什么？如何使用？你为什么想用它？


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1686246326861-519b0cc5-7c8a-48e3-86ec-d7ee3ddb36ee.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/1686246326861-519b0cc5-7c8a-48e3-86ec-d7ee3ddb36ee.png)


补间(动画)（来自 [in\-between](https://github.com)）是一个概念，允许你以平滑的方式更改对象的属性。你只需告诉它哪些属性要更改，当补间结束运行时它们应该具有哪些最终值，以及这需要多长时间，补间引擎将负责计算从起始点到结束点的值。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205448782.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205448782.png)


创建实例，什么轴到哪个位置，多少毫秒内完成


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205707702.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119205707702.png)


例如，`position`对象拥有`x`和`y`两个坐标:



```
var position = {x: 100, y: 0}
```

如果你想将`x`坐标的值从`100`变成`200`，你应该这么做：



```
// 首先为位置创建一个补间(tween)
var tween = new TWEEN.Tween(position)

// 然后告诉 tween 我们想要在1000毫秒内以动画的形式移动 x 的位置
tween.to({x: 200}, 1000)
```

一般来说这样还不够，tween 已经被创建了，但是它还没被激活(使用)，你需要这样启动：



```
// 启动
tween.start()
```

最后，想要成功的完成这种效果，你需要在主函数中调用`TWEEN.update`，如下使用：



```
animate()

function animate() {
	requestAnimationFrame(animate)
	// [...]
	TWEEN.update()
	// [...]
}
```

这样在更新每帧的时候都会运行补间动画；经过 1 秒后 (1000 毫秒) `position.x`将会变成 `200`。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119210307197.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119210307197.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119210447052.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119210447052.png)


除非你在控制台中打印出 `x` 的值，不然你看不到它的变化。你可能想要使用 `onUpdate` 回调:



```
tween.onUpdate(function (object) {
	console.log(object.x)
})
```


> tips:你可能在这里获取不到 `object.x` ,具体的见我提的这个 [issue](https://github.com)


这个函数将会在动画每次更新的时候被调用；这种情况发生的频率取决于很多因素 \- 例如，计算机或设备的速度有多快（以及如何繁忙）。


到目前为止，我们只使用补间动画向控制台输出值，但是您可以将它与 three.js 对象结合：



```
var tween = new TWEEN.Tween(cube.position).to({x: 100, y: 100, z: 100}, 10000).start()

animate()

function animate() {
	requestAnimationFrame(animate)
	TWEEN.update()

	threeRenderer.render(scene, camera)
}
```

在这种情况下，因为 three.js 渲染器将在渲染之前查看对象的位置，所以不需要使用明确的`onUpdate`回调。


你可能也注意到了一些不同的地方：tween.js 可以链式调用！ 每个`tween`函数都会返回`tween`实例，所以你可以重写下面的代码：



```
var tween = new TWEEN.Tween(position)
tween.to({x: 200}, 1000)
tween.start()
```

改成这样：



```
var tween = new TWEEN.Tween(position).to({x: 200}, 1000).start()
```

在将会看到很多例子，所以熟悉它是很好的！比如 [04\-simplest](https://github.com) 这个例子。


## tween.js 的动画


Tween.js 不会自行运行。你需要显式的调用 `update` 方法来告诉它何时运行。推荐的方法是在主动画循环中执行这个操作。使用 `requestAnimationFrame` 调用此循环以获得最佳的图形性能。


比如之前这个例子：



```
animate()

function animate() {
	requestAnimationFrame(animate)
	// [...]
	TWEEN.update()
	// [...]
}
```

如果调用的时候不传入参数，`update` 将会判断当前时间点以确定自上次运行以来已经有多久。


当然你也可以传递一个明确的时间参数给 `update`



```
TWEEN.update(100)
```

意思是"更新时间 \= 100 毫秒"。你可以使用它来确保代码中的所有时间相关函数都使用相同的时间值。例如，假设你有一个播放器，并希望同步运行补间。 你的 `animate` 函数可能看起来像这样：



```
var currentTime = player.currentTime
TWEEN.update(currentTime)
```

我们使用明确的时间值进行单元测试。你可以看下 [tests.ts](https://github.com) 这个例子，看看我们如何用不同的值调用`TWEEN.update()` 来模拟时间传递。


## 控制一个补间


### start 和 stop


到目前为止，我们已经了解了`Tween.start`方法，但是还有更多的方法来控制单个补间。 也许最重要的一个是 `start` 对应的方法：`停止` 。 如果你想取消一个补间，只要调用这个方法通过一个单独的补间:



```
tween.stop()
```

停止一个从未开始或已经停止的补间没有任何效果。 没有错误被抛出。


`start` 方法接受一个参数 `time`。如果你使用它，那么补间不会立即开始，直到特定时刻，否则会尽快启动（i.e 即在下次调用 `TWEEN.update`）。


### update


补间也有一个更新的方法\-\-\-这实际上是由 `TWEEN.update` 调用的。 你通常不需要直接调用它，除非你是个 疯狂的 hacker。


### chain


当你顺序排列不同的补间时，事情会变得有趣，例如在上一个补间结束的时候立即启动另外一个补间。我们称这为链式补间，这使用 `chain` 方法去做。因此，为了使 `tweenB` 在 `tewwnA` 启动：


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119211332110.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119211332110.png)


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF-17321115628107.gif)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/GIF-17321115628107.gif)



```
tweenA.chain(tweenB)
```

或者，对于一个无限的链式，设置 tweenA 一旦 tweenB 完成就开始：



```
tweenA.chain(tweenB)
tweenB.chain(tweenA)
```

关于无限的链式查看 [Hello world](https://github.com) 。


在其他情况下，您可能需要将多个补间链接到另一个补间，以使它们（链接的补间）同时开始动画：



```
tweenA.chain(tweenB, tweenC)
```


> 警告：调用 `tweenA.chain（tweenB）` 实际上修改了 tweenA，所以 tweenA 总是在 tweenA 完成时启动。 `chain` 的返回值只是 tweenA，不是一个新的 tween。


### repeat


如果你想让一个补间永远重复，你可以链接到自己，但更好的方法是使用 `repeat` 方法。 它接受一个参数，描述第一个补间完成后需要多少次重复



```
tween.repeat(10) // 循环10次
tween.repeat(Infinity) // 无限循环
```

补间的总次数将是重复参数加上一个初始补间。查看 [Repeat](https://github.com)。


### yoyo


这个功能只有在独自使用 `repeat` 时才有效果。 活跃时，补间的行为将像 yoyo 一样，i.e 它会从起始值和结束值之间跳出，而不是从头开始重复相同的顺序。


### delay


更复杂的安排可能需要在实际开始运行之前延迟补间。 你可以使用 `delay` 方法来做到这一点



```
tween.delay(1000)
tween.start()
```

将在调用启动方法后的 1 秒钟后开始执行。


## 控制所有补间


在 TWEEN 全局对象中可以找到以下方法，除了 `update` 之外，通常不需要使用其中的大部分对象。


### TWEEN.update(time)


我们已经讨论过这种方法。 它用于更新所有活动的补间。
如果 `time` 不指定，它将使用当前时间。


### TWEEN.getAll and TWEEN.removeAll


用于获取对活动 `tweens` 数组的引用，并分别仅从一个调用中将它们全部从数组中删除


### TWEEN.add(tween) and TWEEN.remove(tween)


用于将补间添加到活动补间的列表，或者分别从列表中删除特定的补间。


这些方法通常只在内部使用，但是如果您想要做一些有趣的事情，则会被暴露。


## 控制补间组


使用 `TWEEN` 单例来管理补间可能会导致包含许多组件的大型应用程序出现问题。 在这些情况下，您可能希望创建自己的更小的补间组。


### 示例：交叉组件冲突


如果使用 `TWEEN` 有多个组件，并且每个组件都想管理自己的一组补间，则可能发生冲突。 如果一个组件调用 `TWEEN.update（）` 或 `TWEEN.removeAll（）`，则其他组件的补间也将被更新或删除。


### 创建你自己的补间组


为了解决这个问题，每个组件都可以创建自己的 `TWEEN.Group` 实例（这是全局的 `TWEEN` 对象在内部使用的）。 实例化新的补间时，可以将这些组作为第二个可选参数传入：



```
var groupA = new TWEEN.Group()
var groupB = new TWEEN.Group()

var tweenA = new TWEEN.Tween({x: 1}, groupA).to({x: 10}, 100).start()

var tweenB = new TWEEN.Tween({x: 1}, groupB).to({x: 10}, 100).start()

var tweenC = new TWEEN.Tween({x: 1}).to({x: 10}, 100).start()

groupA.update() // 只更新tweenA
groupB.update() // 只更新tweenB
TWEEN.update() // 只更新tweenC

groupA.removeAll() // 只移除tweenA
groupB.removeAll() // 只移除tweenB
TWEEN.removeAll() // 只移除tweenC
```

通过这种方式，每个组件都可以处理创建，更新和销毁自己的一组补间。


## 改变缓动功能


Tween.js 将以线性方式执行值之间的插值（即缓动），所以变化将与流逝的时间成正比。 这是可以预见的，但在视觉上也是相当无趣的。 不要担心 \- 使用缓动方法可以轻松更改此行为。 例如：



```
tween.easing(TWEEN.Easing.Quadratic.In)
```

这将导致缓慢地开始向最终值变化，向中间加速，然后迅速达到其最终值，相反，`TWEEN.Easing.Quadratic.Out` 一开始会加速，但随着值的接近最终放缓。


### 可用的缓动函数：TWEEN.Easing


tween.js 提供了一些现有的缓动功能。它们按照它们表示的方程式进行分组：线性，二次，三次，四次，五次，正弦，指数，圆形，弹性，背部和弹跳，然后是缓动型：In，Out 和 InOut。


除非您已经熟悉这些概念，否则这些名称可能不会对您说什么，所以您可能需要查看 [Graphs](https://github.com) 示例，该示例将一个页面中的所有曲线进行图形化，以便比较它们如何看待一瞥。


这些功能是从 Robert Penner 慷慨地提供几年前作为自由软件提供的原始方程派生而来的，但是已经被优化以便与 JavaScript 很好地发挥作用。


[![](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119211028024.png)](https://raw.githubusercontent.com/HeymarJR/blog-pic/main/image-20241119211028024.png)


### 使用自定义缓动功能


您不仅可以使用任何现有的功能，还可以提供您自己的功能，只要遵循一些约定即可：


* 它必须接受一个参数：
	+ `k`: 缓动过程，或我们的补间所处的时间有多长。允许的值在\[0，1]的范围内。
* 它必须根据输入参数返回一个值。


不管要修改多少个属性，easing 函数在每次更新时只调用一次。 然后将结果与初始值以及这个值和最终值之间的差值（delta）一起使用，就像这个伪代码一样：



```
easedElapsed = easing(k);
for each property:
	newPropertyValue = initialPropertyValue + propertyDelta * easedElapsed;
```

对于更注重性能表现的人来说：只有在补间上调用 `start()` 时才会计算增量值。


因此，让我们假设您想使用一个缓解值的自定义缓动函数，但是将 Math.floor 应用于输出，所以只返回整数部分，从而产生一种梯级输出:



```
function tenStepEasing(k) {
	return Math.floor(k * 10) / 10
}
```

你可以通过简单地调用它的缓动方法来使用它，就像我们之前看到的那样：



```
tween.easing(tenStepEasing)
```

查看 [graphs for custom easing functions](https://github.com) 示例，以查看这个动作（还有一些用于生成步进函数的元编程）。


## 回调函数


另一个强大的特性是能够在每个补间的生命周期的特定时间运行自己的功能。 当更改属性不够时，通常需要这样做。


例如，假设你正在试图给一些不能直接访问属性的对象设置动画，但是需要你调用 setter。 您可以使用 `update` 回调来读取新的更新值，然后手动调用 setters。 所有的回调函数都将补间对象作为唯一的参数。



```
var trickyObjTween = new TWEEN.Tween({
	propertyA: trickyObj.getPropertyA(),
	propertyB: trickyObj.getPropertyB(),
})
	.to({propertyA: 100, propertyB: 200})
	.onUpdate(function (object) {
		object.setA(object.propertyA)
		object.setB(object.propertyB)
	})
```

或者想象一下，当一个补间开始时，你想播放声音。你可以使用 `start` 回调：



```
var tween = new TWEEN.Tween(obj).to({x: 100}).onStart(function () {
	sound.play()
})
```

每个回调的范围是补间对象\-\-在这种情况下，是 `obj`。


### onStart


在补间开始之前执行\-\-i.e. 在计算之前。每个补间只能执行一次，i.e. 当通过 `repeat()` 重复补间时，它将不会运行。


同步到其他事件或触发您要在补间启动时发生的操作是非常好的。


补间对象作为第一个参数传入。


### onStop


当通过 `stop()` 显式停止补间时执行，但在正常完成时并且在停止任何可能的链补间之前执行补间。


补间对象作为第一个参数传入。


### onUpdate


每次补间更新时执行，实际更新后的值。


补间对象作为第一个参数传入。


### onComplete


当补间正常完成（即不停止）时执行。


补间对象作为第一个参数传入。


  * [这两天没有上传笔记，在解决图床的问题，主打一个白嫖，所以要费点心思，先是用了gitee的图床好不容易配好后发现居然加了防盗链，后面又转了github的咱目前来说github也是最稳定且免费的，现在搞好了图床以后上传笔记就很快了](#%E8%BF%99%E4%B8%A4%E5%A4%A9%E6%B2%A1%E6%9C%89%E4%B8%8A%E4%BC%A0%E7%AC%94%E8%AE%B0%E5%9C%A8%E8%A7%A3%E5%86%B3%E5%9B%BE%E5%BA%8A%E7%9A%84%E9%97%AE%E9%A2%98%E4%B8%BB%E6%89%93%E4%B8%80%E4%B8%AA%E7%99%BD%E5%AB%96%E6%89%80%E4%BB%A5%E8%A6%81%E8%B4%B9%E7%82%B9%E5%BF%83%E6%80%9D%E5%85%88%E6%98%AF%E7%94%A8%E4%BA%86gitee%E7%9A%84%E5%9B%BE%E5%BA%8A%E5%A5%BD%E4%B8%8D%E5%AE%B9%E6%98%93%E9%85%8D%E5%A5%BD%E5%90%8E%E5%8F%91%E7%8E%B0%E5%B1%85%E7%84%B6%E5%8A%A0%E4%BA%86%E9%98%B2%E7%9B%97%E9%93%BE%E5%90%8E%E9%9D%A2%E5%8F%88%E8%BD%AC%E4%BA%86github%E7%9A%84%E5%92%B1%E7%9B%AE%E5%89%8D%E6%9D%A5%E8%AF%B4github%E4%B9%9F%E6%98%AF%E6%9C%80%E7%A8%B3%E5%AE%9A%E4%B8%94%E5%85%8D%E8%B4%B9%E7%9A%84%E7%8E%B0%E5%9C%A8%E6%90%9E%E5%A5%BD%E4%BA%86%E5%9B%BE%E5%BA%8A%E4%BB%A5%E5%90%8E%E4%B8%8A%E4%BC%A0%E7%AC%94%E8%AE%B0%E5%B0%B1%E5%BE%88%E5%BF%AB%E4%BA%86)
* [1\.轨道控制器](#tid-64BSNa)
* [1 控制物体移动](#tid-SPziWX)
* [1\.1 每一帧修改一点位置形成动画](#tid-rP54T8)
* [2 综合上述代码](#tid-5CB6cQ)
* [3\.总结](#tid-6zdc8c)
* [2\.物体缩放与旋转](#tid-QmMWrE)
* [1 scale设置缩放](#tid-nBsHXY)
* [2 rotation设置旋转](#tid-sj6TRw)
* [2\.1 旋转动画](#tid-PwzANf)
* [3 综合上述代码](#tid-CiFAf4)
* [4\.总结](#tid-tFZDwJ)
* [3\.自适应屏幕大小](#tid-XYiin3)
* [1\.1 自适应屏幕大小](#tid-B83P72)
* [1\.2 控制场景全屏](#tid-nmrCZ4)
* [2 综合上述代码](#tid-Wj2xR6)
* [应对HD\-DPI显示器](#%E5%BA%94%E5%AF%B9hd-dpi%E6%98%BE%E7%A4%BA%E5%99%A8)
* [3\.其他补充](#tid-KrkYGh)
* [3\.1 gui调试工具](#tid-2Htpjz)
* [3\.2 创建顶点，形成三角形](#tid-K4xJp5)
* [3\.3 顶点组](#tid-FD2aCi)
* [3\.4 快速创建几何体](#tid-xFwyiM)
* [4\.基础材质](#tid-JJbB4n)
* [4\.1 纹理属性](#tid-3FpYEd)
* [4\.2 雾](#tid-wyfQnc)
* [4\.gltf模型加载](#tid-BHnG4R)
* [解压缩](#%E8%A7%A3%E5%8E%8B%E7%BC%A9)
* [5\.补件动画tween](#tid-CJ4xcc)
* [光线投射实现场景交互](#%E5%85%89%E7%BA%BF%E6%8A%95%E5%B0%84%E5%AE%9E%E7%8E%B0%E5%9C%BA%E6%99%AF%E4%BA%A4%E4%BA%92)
* [tween 是什么？如何使用？你为什么想用它？](#tween-%E6%98%AF%E4%BB%80%E4%B9%88%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%E4%BD%A0%E4%B8%BA%E4%BB%80%E4%B9%88%E6%83%B3%E7%94%A8%E5%AE%83)
* [tween.js 的动画](#tweenjs-%E7%9A%84%E5%8A%A8%E7%94%BB)
* [控制一个补间](#%E6%8E%A7%E5%88%B6%E4%B8%80%E4%B8%AA%E8%A1%A5%E9%97%B4)
* [start 和 stop](#start-%E5%92%8C-stop)
* [update](#update)
* [chain](#chain)
* [repeat](#repeat)
* [yoyo](#yoyo)
* [delay](#delay)
* [控制所有补间](#%E6%8E%A7%E5%88%B6%E6%89%80%E6%9C%89%E8%A1%A5%E9%97%B4)
* [TWEEN.update(time)](#tweenupdatetime)
* [TWEEN.getAll and TWEEN.removeAll](#tweengetall-and-tweenremoveall)
* [TWEEN.add(tween) and TWEEN.remove(tween)](#tweenaddtween-and-tweenremovetween)
* [控制补间组](#%E6%8E%A7%E5%88%B6%E8%A1%A5%E9%97%B4%E7%BB%84)
* [示例：交叉组件冲突](#%E7%A4%BA%E4%BE%8B%E4%BA%A4%E5%8F%89%E7%BB%84%E4%BB%B6%E5%86%B2%E7%AA%81)
* [创建你自己的补间组](#%E5%88%9B%E5%BB%BA%E4%BD%A0%E8%87%AA%E5%B7%B1%E7%9A%84%E8%A1%A5%E9%97%B4%E7%BB%84)
* [改变缓动功能](#%E6%94%B9%E5%8F%98%E7%BC%93%E5%8A%A8%E5%8A%9F%E8%83%BD)
* [可用的缓动函数：TWEEN.Easing](#%E5%8F%AF%E7%94%A8%E7%9A%84%E7%BC%93%E5%8A%A8%E5%87%BD%E6%95%B0tweeneasing)
* [使用自定义缓动功能](#%E4%BD%BF%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BC%93%E5%8A%A8%E5%8A%9F%E8%83%BD)
* [回调函数](#%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)
* [onStart](#onstart)
* [onStop](#onstop)
* [onUpdate](#onupdate)
* [onComplete](#oncomplete)

   \_\_EOF\_\_

   ![](https://github.com/heymar)Heymar  - **本文链接：** [https://github.com/heymar/p/18561711](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
