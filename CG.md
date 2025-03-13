# 图形学记录

## 1. 回调函数是什么

回调函数是一种由系统或库在**特定事件发生时自动调用的函数**。在 GLUT 中，回调函数用于处理用户输入、窗口事件、绘制任务等。通过注册回调函数，你可以告诉 GLUT 在特定事件发生时执行你定义的逻辑。

### **GLUT 中的主要回调函数**

GLUT 提供了多种回调函数，用于处理不同的事件。以下是一些常用的回调函数：

#### 1. **显示回调函数（Display Callback）**

- 用于绘制场景。当窗口**需要重新绘制**时（如窗口首次显示、窗口大小改变或被覆盖后恢复），GLUT 会调用此函数。

- 注册函数：`glutDisplayFunc(void (*func)(void))`

- 示例：

  ```cpp
  void display() {
      glClear(GL_COLOR_BUFFER_BIT); // 清空颜色缓冲区
      // 绘制代码
      glFlush(); // 刷新缓冲区
  }
  glutDisplayFunc(display);
  ```

#### 2. **窗口大小回调函数（Reshape Callback）**

- 当**窗口大小改变时**调用。通常用于调整视口和投影矩阵。

- 注册函数：`glutReshapeFunc(void (*func)(int width, int height))`

- 示例：

  ```cpp
  void reshape(int width, int height) {
      glViewport(0, 0, width, height); // 设置视口
      glMatrixMode(GL_PROJECTION);    // 切换到投影矩阵
      glLoadIdentity();               // 重置投影矩阵
      gluOrtho2D(0, width, 0, height); // 设置正交投影
  }
  glutReshapeFunc(reshape);
  ```

#### 3. **键盘回调函数（Keyboard Callback）**

- 当用户**按下键盘键时**调用。

- 注册函数：`glutKeyboardFunc(void (*func)(unsigned char key, int x, int y))`

- 示例：

  cpp

  ```cpp
  void keyboard(unsigned char key, int x, int y) {
      if (key == 'q') { // 按下 'q' 键退出程序
          exit(0);
      }
  }
  glutKeyboardFunc(keyboard);
  ```

#### 4. **特殊键回调函数（Special Key Callback）**

- 用于**处理功能键**（如方向键、F1-F12 等）。

- 注册函数：`glutSpecialFunc(void (*func)(int key, int x, int y))`

- 示例：

  ```cpp
  void specialKeys(int key, int x, int y) {
      if (key == GLUT_KEY_UP) { // 按下上方向键
          // 处理逻辑
      }
  }
  glutSpecialFunc(specialKeys);
  ```

#### 5. **鼠标回调函数（Mouse Callback）**

- 当用户**点击或移动鼠标时**调用。

- 注册函数：`glutMouseFunc(void (*func)(int button, int state, int x, int y))`

- 示例：

  ```cpp
  void mouse(int button, int state, int x, int y) {
      if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) { // 左键按下
          // 处理逻辑
      }
  }
  glutMouseFunc(mouse);
  ```

#### 6. **鼠标移动回调函数（Motion Callback）**

- 当鼠标在按下状态下移动时调用。

- 注册函数：`glutMotionFunc(void (*func)(int x, int y))`

- 示例：

  ```cpp
  void motion(int x, int y) {
      // 处理鼠标移动逻辑
  }
  glutMotionFunc(motion);
  ```

#### 7. **空闲回调函数（Idle Callback）**

- 当**系统空闲时**调用。通常用于动画或持续更新场景。

- 注册函数：`glutIdleFunc(void (*func)(void))`

- 示例：

  cpp

  ```cpp
  void idle() {
      // 更新动画或逻辑
      glutPostRedisplay(); // 请求重绘
  }
  glutIdleFunc(idle);
  ```

#### 8. **定时器回调函数（Timer Callback）**

- 用于**定时执行任务**。与空闲回调不同，定时器回调是**精确**的。

- 注册函数：`glutTimerFunc(unsigned int millis, void (*func)(int value), int value)`

- 示例：

  ```cpp
  void timer(int value) {
      // 定时任务
      glutTimerFunc(1000, timer, 0); // 每 1000 毫秒调用一次
  }
  glutTimerFunc(1000, timer, 0);
  ```

### **回调函数的使用流程**

1. **定义回调函数**：编写处理特定事件的函数。
2. **注册回调函数**：使用 GLUT 提供的注册函数（如 `glutDisplayFunc`、`glutKeyboardFunc` 等）将回调函数与事件关联。
3. **进入主循环**：调用 `glutMainLoop()`，GLUT 会自动监听事件并调用相应的回调函数。

## 2.常见函数记录

```c++
void glClearColor(0.0f, 0.0f, 0.0f, 1.0f); // 设置为黑色
void glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
//清楚缓冲区
常见的缓冲区包括：
​颜色缓冲区（GL_COLOR_BUFFER_BIT）​：存储像素的颜色值。
​深度缓冲区（GL_DEPTH_BUFFER_BIT）​：存储像素的深度值（用于深度测试）。
​模板缓冲区（GL_STENCIL_BUFFER_BIT）​：存储模板值（用于模板测试）。

void glLoadIdentity();
//用于将当前矩阵（通常是模型视图矩阵或投影矩阵）重置为单位矩阵
//glMatrixMode 可以切换当前矩阵模式：
glMatrixMode(GL_MODELVIEW); // 切换到模型视图矩阵
glMatrixMode(GL_PROJECTION); // 切换到投影矩阵
glMatrixMode(GL_TEXTURE);    // 切换到纹理矩阵

void glTranslatef(GLfloat x, GLfloat y, GLfloat z);
//用于对当前矩阵（通常是模型视图矩阵）进行平移变换。它会将物体在三维空间中移动到指定的位置。将当前的模型变换矩阵乘以
[ 1  0  0  x ]
[ 0  1  0  y ]
[ 0  0  1  z ]
[ 0  0  0  1 ]

void glBegin(mode);
    其中mode=
    GL_POINTS
    GL_LINES前后两个顶点之间是线/GL_LINE_STRIPS一直连除了最后一个和第一个不会连上去/GL_LINE_LOOP最后一个点还将和第一个连在一起
    多边型图元较多，填充图元包含以下几个
    GL_POLYGON多边形/GL_TRIANGLE每三个点位连续的一组，多余的顶点被忽略/GL_TRIANGLE_STRIP每个新的顶点都将与之前两个顶点形成新的三角形/GL_TRIANGLE_FAN前三个点定义了一个三角形，后续的顶点都将与第一个顶点和他前一个顶点构成一个新的三角形/GL_SQUADS每连续四个点定义一个四边形/GL_QUAD_STRIP每个新的2个顶点都与之前的一对顶点构成一个新的四边形
        
        
void glEnd();

void glColor<34><bifd…><v>(略);
//4代表rgba(a是透明) 可以选用实数或者整数，实数是0-1，整数是0-255;

//影响取景 用于设置正交投影矩阵，它的参数指定了投影空间的左右、下上边界。
void gluOrtho2D(左下的左，右上的右，左下的下，右上的上);

//影响视口，宽度为w，高度为h，左下角为x,y 单位均为像素
void glViewport(GLint x,GLint y,GLsizei w,GLsizei h);

void glEnable(GLenum feature);
void glDisable(GLenum feature);
//用来启用或者禁用一些必须专门调整的特性
//目前特性包含：GL_LINE_STIPPLE线点划模式/GL_POLYGON_STIPPLE多边形点划模式

-----以下是有关绘制的函数-----
void glVertex<234><sifd>(坐标);//"基本的集合图元透视若干顶点定义的"

void glPointSize(GLfloat size);//默认值为1.0 
//该函数不可以放在begin和end中间;

void glLineWidth(GLfloat width);//默认值为1.0

void glLineStipple(GLint factor,GLushort pattern);//具体见下面“对于比较复杂函数的说明”

void glRect<sifd><v>(x1,y1,x2,y2)or(v1*,v2*);//用左上角和右下角的x和y坐标画一个矩形

void glShadeModel(GLenum Mode);
//将明暗模型设定为平滑模式(GK_SMOOTH)或者平面模式(GL_FLAT)，默认前者

-----以下是与表面相关的函数-----

void glPolygonMode(GLenum face,GL enum mode);
//绘制表面的方式
//face包含了GL_front/GL_BACK/GL_FRONT_AND_BACK
void glCullFace(GL enum mode);
//将某个面朝向的多边形或者全部多边形进行裁剪
//mode包含了GL_front/GL_BACK/GL_FRONT_AND_BACK
voidglFrontFace(GL enum mode);
//允许将逆时针(GL_CCW)或顺时针(GL_CN)方向定义为正面朝向


```

### 对于比较复杂的函数的说明

![427418976bf10c6827be62223d02607](https://raw.githubusercontent.com/YijieLiao/MyImgHost/main/img/427418976bf10c6827be62223d02607.jpg)



## 3.一些规范

