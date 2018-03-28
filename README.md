# Android动画深入分析
Android动画可以分为三种
- View动画
    - View动画通过对场景里的对象不断做图像变换（平移、缩放、旋转、透明度）从而产生动画效果
    - 是一种渐进式动画，并且View动画也支持自定义    
- 帧动画（帧动画其实也属于View动画的一种）
    - 帧动画通过顺序播放一系列图像从而产生动画效果，可以简单理解为图片切换动画
    - 如果图片过多过大会造成OOM
- 属性动画
    - 属性动画通过动态地改变对象的属性从而达到动画效果
    - 属性动画为API11的新属性，在低版本中无法直接使用属性动画，需要通过兼容库来支持
## 1 View动画
### 1 四种View动画
View动画的作用对象是View，支持四种动画效果，分别是平移动画、缩放动画、旋转动画和透明度动画
- 1 平移动画
    - 1 标签: translate
    - 2 子类: TranslateAnimation
    ```
    //在xml文件中创建，xml文件路径为res/anim/filename.xml
    <translate
        android:fromXDelta="float" //x的起始点
        android:toXDelta="float"   //x的终点值
        android:fromYDelta="float" //y的起始点 
        android:toYDelta="float"   //y的终点值
        />
    ```
- 2 缩放动画
    - 1 标签: scale
    - 2 子类: ScaleAnimation
    ```
    //在xml文件中创建，xml文件路径为res/anim/filename.xml
    <scale
         android:fromXScale="float" //水平方向缩放起始值
         android:toXScale="float" //水平方向缩放结束值
         android:fromYScale="float"//竖直方向的起始值
         android:toYScale="float" //水平方向的结束值
         android:pivotX="float" //缩放的轴点x坐标
         android:pivotY="float" //缩放的轴点y坐标
         />        
    ```    
- 3 旋转动画
    - 1 标签: rotate
    - 2 子类: RotateAnimation
    ```
    <rotate
        android:fromDegree="float" //旋转开始的角度
        android:toDegree="float" //旋转结束的角度
        andorid:pivotX="float" //旋转的轴点x坐标
        android:pivotY="float" //旋转的轴点y坐标
        />
    ```
- 4 透明度动画
    - 1 标签: alpha
    - 2 子类: AlphaAnimation
    ```
    <alpha
        android:fromAlpha="float"//其实透明度
        android:toAlpha="float"//结束透明度
        />
    ```
- 5 View动画还有一些通用的属性
    ```
    android:duration="1000" //动画持续的时间
    android:fillAfter="boolean"// 动画结束后VIew是否停留在结束位置
    ```
- 6 可以用java代码来实现动画
    ```
    AlphaAnimation alphaAnimation = new AlphaAnimation(0, 1);
    alphaAnimation.setDuration(300);
    mView.setAnimation(alphaAnimation)
    ```
### 2 iew动画的特殊使用场景
- 1 LayoutAnimation 
LayoutAnimation作用于ViewGroup,为ViewGroup指定一个动画，这样当它的子元素出场时都会具有这种动画效果。这种效果常常用于ListView上（ListView的每个item都以一定的动画形式出现）。    
    
    - 1 定义LayoutAnimation
    
    ```
    <LayoutAnimation
        xmls="http://schemas.android/com/apk/res/android"
        android:delay="0.5" 
        android:animationOrder="normal"
        android:animation="@anim/anim_item"
        />
    ```
    android:delay 表示子元素开始动画的延迟时间，如子元素入场动画的时间周期为300ms， 则每个子元素要延迟150ms才能播放入场动画
    
    android:animationOrder 表示子元素动画的顺序，normal:顺序播放(排在前面的子元素先开始播放入场动画)；reverse:逆向播放；random:随即播放入场动画 
    
    android:animation 为子元素指定入场动画
    
    - 2 为ViewGroup指定android:layoutAnimation属性
    ```
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layoutAnimation="@anim/anim_layout"
        />
    ```
      
## 2 属性动画    

