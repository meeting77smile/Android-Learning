# Android基础

## 学习前的声明

### 1. 语言要求

- 本次学习用到的语言为<mark>Java</mark>。（<mark>Kotlin</mark>虽然为Android官方推荐语言，但Java与Kotlin的关系类似于C与C++的关系，学好了Java，便能比较快地上手Kotlin）

### 2. 设置主界面

- 在AndroidManifest.xml文件中，在如图所示的位置改为想要作为主界面的的java文件名（注意别忘了名称开头要加点）
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-04%20231000.png?raw=true)

### 3. java实例代码的说明

- 学习过程中的java实例代码里的"chapter03"指的是文件夹名称，实际学习过程中应该<mark>改为自己的文件夹名称<mark>：
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-05%20202508.png?raw=true)

## 1.简单控件

### 1.1 文本显示

#### 1.1.1 设置文本内容

有两种方式：

1. **在XML文件中通过属性android:text设置文本**：
   
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
   
       <TextView
           android:id="@+id/tv_test"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="设置文本内容"/>
   </LinearLayout>
   ```

(其中<mark>android:id="@+id/tv_test</mark>" 是为第二种方法做准备)

2. **在Java代码中调用文本视图对象(TextView)的setText方法设置文本**：
   
   ```java
   public class TestViewActivity extends AppCompatActivity {
       @Override
       protected void onCreate(@Nullable Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_test_view);
           TextView tv_test=findViewById(R.id.tv_test);
           tv_test.setText(R.string.test);
       }
   }
   ```

（其中，<mark>View tv</mark>为文本试图为视图对象；R.string.test可用"设置文本内容"代替。但为了方便同时修改一个值，还是用R.string.test方法合适，并在strings.xml文件中加上：

```xml
    <string name="test">设置文本内容"</string>
```

#### 1.1.2 设置字体大小

类似的，也有两种方式：

 1.**在XML文件中通过属性android:textSize设置文本（要带单位）**：

```xml
    <TextView
        android:textSize="30dp"/>
```

其中，字号单位有三种：

- **px**(PiXel像素)：是手机屏幕的最小显示单位，与设备的显示屏有关。

- **dp**：指的是与设备无关的显示单位，只与屏幕的尺寸有关。对于相同分辨率的手机，屏幕越大，相同DP的组件占用的屏幕比例越小；对于**相同尺寸**的手机，即使分辨率不同，**相同DP**的组件**占用的屏幕比例也相同**。

- **sp**：原理与dp类似，但它是<mark>专门用于设置字体大小</mark>的。当在<mark>手机系统调整字体大小</mark>时，<mark>只有以sp</mark>为单位的字体的<mark>大小会变化</mark>，<mark>px和dp</mark>则<mark>不会改变</mark>。
2. **在Java代码中调用文本视图对象的setTextSize方法设置**：
   
   ```java
   TextView tv_test=findViewById(R.id.tv_test);
   tv_test.setTextSize(30);
   ```
   
   此时设置字体大小不用带单位，默认以sp为单位(说明官方推荐以sp作为字体的单位)

#### 1.1.3 设置字体颜色

也有两种方式：

1. **在XML文件中通过属性android:textColor设置文本**：

```xml
<TextView
    android:textColor="#FF000000"/>
```

或者：

```xml
<TextView
    android:textColor="@color/black"/>
```

2. **在Java代码中调用文本视图对象(TextView)的setBackgroundColor方法或setBackgroundResource方法设置文本**:
   
   - **setBackgroundColor方法(使用系统自带的颜色)：**
     
     ```java
     TextView tv_test=findViewById(R.id.tv_test);
     tv_test.setBackgroundColor(Color.GREEN);
     ```
   
   - **setBackgroundResource方法(使用xml文件中自定义的颜色)：**
     
     先在" <mark>colors.xml</mark> "文件中定义"green"：
     
     ```xml
     <color name="green">#4CAF50</color>
     ```
     
     接着在java文件中：
     
     ```java
     TextView tv_test=findViewById(R.id.tv_test);
     tv_test.setBackgroundResource(R.color.green);
     ```

3. 补充：**使用八位十六进制数表示颜色**的注意事项：
   
   - **含义**：以八位编码#FFEEDDCC为例，FF表示透明度，EE表示红色的浓度，DD表示绿色的浓度，CC表示蓝色的浓度。
   
   - **数值**含义：透明度从FF到00，透明度逐渐增大；其他的RGB三色数值越大，表示颜色越浓，也就是越亮。
   
   - 也可以用省略前两位数字，<mark>用六位十六进制数表示</mark>颜色。在<mark>xml文件中默认前两位为FF</mark>，也就是完全不透明；在<mark>java文件中默认前两位为00</mark>，也就是完全透明。

### 1.2 视图基础

#### 1.2.1 设置视图的宽高

1. **在XML文件中通过属性android:layout设置**
   
   其中，宽高的取值主要有以下三种：
   
   - **match_parent**：表示与上级试图一致。
   
   - **warp_content**：表示与内容自适应。
   
   - **以dp为单位**的具体尺寸。

2. **在Java代码中调用文本视图对象(TextView)的getLayoutParams方法和setLayoutParams方法设置文本**：
   
   首先确保XML文件的宽高属性值为**wrap_content**：
   
   ```xml
       <TextView
           android:id="@+id/tv_code"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="通过代码指定试图宽度"
           android:textSize="30dp"
           android:textColor="#000000"
           android:background="#00ffff"/>
   ```
   
   接着在JAVA文件中依次执行以下三个步骤：
   
   - 调用控件对象的getLayoutParams方法，获取该控件的布局参数。
     
     ```java
     TextView tv_code =findViewById(R.id.tv_code);
     //获取tv_code的布局参数(含宽度和高度)
     ViewGroup.LayoutParams params = tv_code.getLayoutParams();
     ```
   
   - 修改布局参数的<mark>width属性值</mark>。
     
     - 首先在原先的java文件所在的com.example文件夹中新建一个java类，并输入以下代码完成dp与px的换算。示例中以"Utils"命名。
       
       ```java
       public class Utils {
           public  static  int dip2px(Context context, float dpvalue){
               //获取当前手机的像素密度(1个dp对应几个px)
               float scale =context.getResources().getDisplayMetrics().density;
               //四舍五入取整
               return (int)(dpvalue * scale +0.5f);
           }
       }
       ```
     
     - 接着在原来的java文件中（如此时将宽度设置为300dp）：
       
       ```java
       //此处params.width默认的单位是像素px，故此时需要将单位dp利用公式换算成px：
       params.width=Utils.dip2px(this,300);
       ```
     
     - 最后调用控件对象的setLayoutParams方法，填入修改后的布局参数即可生效。
       
       ```java
       //设置tv_code的布局参数
       tv_code.setLayoutParams(params);
       ```

#### 1.2.2 设置视图的间距

可在**XML文件**中改变两种属性：

- **layout_margin** 属性（设置外间距），它指定了当前视图与周围<mark>同级</mark>视图之间的距离，包括**layout_margin** , **layout_marginLeft** , **layout_marginRight** , **layout_marginTop** , **layout_marginBottom**等属性。

- **padding** 属性（设置内间距），它指定了当前视图与内部<mark>下级</mark>视图之间的距离 , 包括**padding** , **paddingLeft** , **paddingTop** , **paddingRight** , **paddingBottom**等属性。
  
  代码示例：
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!--最外层的布局背景为蓝色-->
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="300dp"
      android:orientation="vertical"
      android:background="#00AAFF">
  
      <!--中间层的布局背景为黄色-->
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:background="#FFFF99"
          android:layout_margin="20dp"
          android:padding="60dp">
  
          <!--最内层的视图背景为红色和绿色-->
          <View
              android:layout_width="50dp"
              android:layout_height="50dp"
              android:background="#FF0000"/>
          <View
              android:layout_width="50dp"
              android:layout_height="50dp"
              android:background="#00FF22"
              android:layout_marginLeft="100dp"/>
      </LinearLayout>
   </LinearLayout>
  ```
  
  其中，中间层的<mark>Linearout</mark>与内层的<mark>View</mark>是**上级和下级视图**的关系，故应该在Linearout里设置<mark>padding属性</mark> ；内层的两个<mark>view</mark>是**同级视图**的关系，故可在其中一个view中设置<mark>layout_margin属性</mark>。
  
  **注意：** 当设置的视图大小与间距属性<mark>冲突</mark>时（如设置padding属性时，内部视图的大小超过了外部视图，则会<mark>优先考虑视图间距属性</mark>。)
  
  最终效果：
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/20240921212601.png?raw=true)

#### 1.2.3 设置视图的对齐方式

类似的，在<mark>XML文件</mark>中也有两种属性：

- **layout_gravity** 属性，它指定了<mark>当前视图相对于上级视图</mark>的对齐方式。

- **gravity** 属性，它指定了<mark>下级视图相对于当前视图</mark>的对齐方式。
  
  二者的取值包括：<mark>left</mark> , <mark>right</mark> , <mark>bottom</mark>  , <mark>top </mark>。
  
  还可以用<mark>竖线 “ | ” 同时取值</mark>，例如 **letf|top** 表示既靠左又靠上，也就是朝左上角对齐。此外，此方式<mark>没有顺序要求</mark>，即此时 top|left也有相同的效果。
  
  代码示例：
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!--最外层的布局背景为蓝色-->
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="600dp"
      android:orientation="horizontal"
      android:background="#FFFF99">
  
      <!--第一个子布局背景为红色，它在上级视图中朝下对齐，它的下级视图则靠左上对齐-->
      <LinearLayout
          android:layout_width="100dp"
          android:layout_height="200dp"
          android:layout_margin="10dp"
          android:background="#F44336"
          android:layout_gravity="bottom"
          android:padding="10dp"
          android:layout_weight="1"
          android:gravity="left|top">
  
          <View
              android:layout_width="200dp"
              android:layout_height="100dp"
              android:background="#00BCD4"
      </LinearLayout>
      <!--第二个子布局背景为红色，它在上级视图中朝上对齐，它的下级视图则靠右下对齐-->
          <LinearLayout
              android:layout_width="100dp"
              android:layout_height="200dp"
              android:layout_margin="10dp"
              android:background="#F44336"
              android:layout_gravity="top"
              android:padding="10dp"
              android:layout_weight="1"
              android:gravity="right|bottom">
  
              <View
                  android:layout_width="200dp"
                  android:layout_height="100dp"
                  android:background="#00BCD4"
      </LinearLayout>
   </LinearLayout>
  ```
  
  最终效果：
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-21%20225822.png?raw=true)

### 1.3 常用布局

#### 1.3.1 线性布局LinearLayout

- **线性布局的排列方式(orientation)**
  
  线性布局有两种排列方式，可通过改变LinearLayout中的<mark>orientation</mark>的属性值（不指定时默认为水平排列）：
  
  - **水平排列**：设置属性值为horizontal ，使得内部视图从左往右排列。
  
  - **垂直排列**：设置属性值为vertical ，使得内部视图从上往下排列。

- **线性布局的权重(layout_weight)**
  
  指的是线性布局的下级视图各自拥有<mark>多大比例的宽高</mark>（当个视图所占的比例=其权重/同级视图的权重和）。
  
  权重属性<mark>layout_weight</mark> 不在**layout_weight** 节点设置，而在线性布局的直接下级视图设置。
  
  - 当**layout_width**为<mark>0dp</mark> 时，layout_weight表示<mark>水平方向</mark>的宽度比例。
  
  - 当**layout_height**为0dp 时，layout_weight表示<mark>垂直方向</mark>的宽度比例。

- 代码示例：
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:orientation="horizontal">
  
          <TextView
              android:layout_width="0dp"
              android:layout_height="wrap_content"
              android:text="横排第一个"
              android:textSize="30dp"
              android:background="#00BCD4"
              android:layout_weight="1"/>
          <TextView
              android:layout_width="0dp"
              android:layout_height="wrap_content"
              android:text="横排第二个"
              android:textSize="30dp"
              android:background="#E91E63"
              android:layout_weight="1"/>
          <TextView
              android:layout_width="0dp"
              android:layout_height="wrap_content"
              android:text="横排第三个"
              android:textSize="30dp"
              android:background="#8BC34A"
              android:layout_weight="1"/>
      </LinearLayout>
  
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="0dp"
          android:text="竖排第一个"
          android:textSize="30dp"
          android:background="#FFC107"
          android:layout_weight="2"/>
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="0dp"
          android:text="竖排第二个"
          android:textSize="30dp"
          android:background="#FF5722"
          android:layout_weight="1"/>
  
  </LinearLayout>
  ```
  
  此时横排的<mark>TextView</mark> 视图各占<mark>三分之一</mark>，竖排的分别占<mark>三分之二</mark>和<mark>三分之一</mark>。
  
  横排的<mark>TextView</mark> 视图由于均为<mark>水平排列</mark>，且均把<mark>layout_width设置为0dp</mark>，故此时layout_weight表示<mark>水平方向的宽度比例</mark>；竖排同理。
  
  最终效果：
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20182310.png?raw=true)

#### 1.3.2 相对布局RelativeLayout

- **RelativeLayout**的<mark>下级视图的位置</mark>可由其他视图的位置决定（若不设定则<mark>默认</mark>显示在RelativeLayout内部的<mark>左上角</mark>）。用于确定下级视图位置的<mark>参照物</mark>有两种：
  
  - 与该下级视图平级的视图。
  
  - 该视图的上级视图（即它归属的RelativeLayout）。

- **RelativeLayout的属性**
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20190511.png?raw=true)

- **代码示例**
  
  - **以上级视图为参照物(属性值为true或false)**：
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <TextView
            android:id="@+id/tv_center"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="我在中间"
            android:textSize="16dp"
            android:layout_centerInParent="true"/>
        <TextView
            android:id="@+id/tv_topleft"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="我在左上角"
            android:textSize="16dp"
            android:layout_alignParentLeft="true"
            android:layout_alignParentTop="true"/>
        <TextView
            android:id="@+id/tv_bottomright"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="我在右下角"
            android:textSize="16dp"
            android:layout_alignParentRight="true"
            android:layout_alignParentBottom="true"/>
    
    </RelativeLayout>
    ```
    
    效果图：
    
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20191223.png?raw=true)
  
  - **与该下级视图平级的视图为参照物(属性值为作为参照物的同级视图的id)**
    
    - 易错：如向将一个视图设置在其<mark>平级视图的左边</mark>时，<mark>不应只设置layout_toLeftOf</mark>属性，否则会出现这种情况：
      
      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent">
          <TextView
              android:id="@+id/tv_center"
              android:layout_width="200dp"
              android:layout_height="100dp"
              android:layout_centerInParent="true"
              android:text="我在中间"
              android:textSize="16dp" />
          <TextView
              android:id="@+id/tv_left"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="同级视图左边"
              android:textSize="16dp"
              android:layout_toLeftOf="@id/tv_center"/>
      </RelativeLayout>
      ```
      
      ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20192524.png?raw=true)
      
      所以应该再设置<mark>layout_above(在参照物的左上方)</mark>或<mark>layout_below(在参照物的右上方)</mark>属性:
      
      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent">
          <TextView
              android:id="@+id/tv_center"
              android:layout_width="200dp"
              android:layout_height="100dp"
              android:layout_centerInParent="true"
              android:text="我在中间"
              android:textSize="16dp" />
          <TextView
              android:id="@+id/tv_left"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="同级视图左边"
              android:textSize="16dp"
              android:layout_toLeftOf="@id/tv_center"
              android:layout_above="@id/tv_center"/>
      </RelativeLayout>
      ```
      
      ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20192944.png?raw=true)

#### 1.3.3 网格布局GridLayout

- 网格布局支持多行多列的表格排列。

- 网格布局默认从左往右、从上到下排列，其主要有两个属性：
  
  - **columnCount属性**：它指定了<mark>网格的列数</mark>，即<mark>每行能放多少个视图</mark>。
  
  - **rowCount属性**：它指定了<mark>网格的行数</mark>，即<mark>每列能放多少个视图</mark>。

- 代码示例：
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:columnCount="2"
      android:rowCount="2">
  
      <TextView
          android:layout_width="0dp"
          android:layout_height="60dp"
          android:background="#F6E0E0"
          android:text="粉色"
          android:textSize="16dp"
          android:textColor="#000000"
          android:gravity="center"
          android:layout_columnWeight="1"/>
      <TextView
          android:layout_width="0dp"
          android:layout_height="60dp"
          android:background="#FF9800"
          android:text="橙色"
          android:textSize="16dp"
          android:textColor="#000000"
          android:gravity="center"
          android:layout_columnWeight="1"/>
      <TextView
          android:layout_width="0dp"
          android:layout_height="60dp"
          android:background="#08FF00"
          android:text="绿色"
          android:textSize="16dp"
          android:textColor="#000000"
          android:gravity="center"
          android:layout_columnWeight="1"/>
      <TextView
          android:layout_width="0dp"
          android:layout_height="60dp"
          android:background="#8000FF"
          android:text="深紫色"
          android:textSize="16dp"
          android:textColor="#000000"
          android:gravity="center"
          android:layout_columnWeight="1"/>\
  
  </GridLayout>
  ```
  
  注意：
  
  1. 为了使<mark>字体在视图中间</mark>，需要用到先前学的<mark>gravity属性</mark>(字体text为当前视图TextView组件的<mark>下级视图</mark>)。
  
  2. 为了使得<mark>每列</mark>的TextView视图均占GridLayou<mark>t视图的一半</mark>，需要用到先前学的<mark>权重属性</mark>，但GridLayout里的权重名称变成了<mark>layout_columnWeight</mark>和<mark>layout_rowWeight</mark>，分别修改<mark>列和行</mark>的视图的权重。

- 最终结果
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-23%20202259.png?raw=true)

#### 1.3.4 滚动视图ScrollView

  当下级视图的大小超出屏幕或当前视图时，可以使用滚动视图来滑动。有两种滚动视图：

- **ScrollView** 
  
  它是<mark>垂直方向</mark>的滚动视图；当需要垂直方向的滚动时，它的<mark>layout_width</mark>的属性值需要设置为<mark>match_parent</mark>，<mark>layout_height</mark>属性值需要设置为<mark>wrap_content</mark>。

- **HorizontalScrollView**
  
  它是<mark>水平方向</mark>的滚动视图；当需要水平方向的滚动时，它的<mark>layout_width</mark>的属性值需要设置为<mark>wrap_parent</mark>，<mark>layout_height</mark>属性值需要设置为<mark>match_content</mark>。

- 代码示例
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <HorizontalScrollView
          android:layout_width="wrap_content"
          android:layout_height="200dp">
  
          <LinearLayout
              android:layout_width="wrap_content"
              android:layout_height="match_parent"
              android:orientation="horizontal">
  
              <View
                  android:layout_width="300dp"
                  android:layout_height="match_parent"
                  android:background="#aaffff"/>
              <View
                  android:layout_width="300dp"
                  android:layout_height="match_parent"
                  android:background="#ffff00"/>
  
          </LinearLayout>
  
      </HorizontalScrollView>
  
      <ScrollView
          android:layout_width="match_parent"
          android:layout_height="wrap_content">
  
      <LinearLayout
          android:layout_width="wrap_content"
          android:layout_height="match_parent"
          android:orientation="vertical">
  
          <View
              android:layout_width="match_parent"
              android:layout_height="400dp"
              android:background="#2196F3"/>
          <View
              android:layout_width="match_parent"
              android:layout_height="400dp"
              android:background="#FF5722"/>
  
      </LinearLayout>
  
      </ScrollView>
  
  </LinearLayout>
  ```
  
  此时HorizontalScrollView里的两个TextView的<mark>宽度</mark>加起来<mark>太大</mark>了，故<mark>需要用到HorizontalScrollView</mark>；ScrollView里的两个TextView的<mark>高度加起来太大</mark>了，故<mark>需要用到ScrollView</mark>；
  
  最终效果：
  
  ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-24%20135648.png?raw=true)

### 1.4 按钮控件Button

- <mark>Button是TextView的子类</mark>，因此它<mark>也是一种视图</mark>。(而<mark>TextView是View的子类</mark>)。Button与TextView的<mark>区别</mark>主要有：
  
  - <mark>Button有默认的按钮背景颜色</mark>，而TextView则没有。
  
  - <mark>Button的内部文本默认居中对齐</mark>，而TextView的内部文本默认靠左上角对齐。

#### 1.4.1 新增属性

- 与TextView相比，Button新增了两个属性：
  
  - **textAllCaps属性**
    
    它指定了<mark>是否将英文字母全部转为大写</mark>，为<mark>true</mark>表示转为<mark>大写</mark>，为<mark>false</mark>则<mark>不做大写转换</mark>。
  
  - **onClick属性**
    
    它用来接管用户的点击动作，其<mark>属性值</mark>是<mark>点击按钮时触发的方法</mark>。
  
  - 代码示例：
    
    - 首先，在xml文件中：
      
      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="vertical"
          android:padding="5dp">
          <TextView
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:text="下面的按钮默认英文为原样："
              android:textColor="@color/black"
              android:textSize="17sp"
              android:gravity="center"/>
          <Button
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:text="Hello World!"
              android:textColor="@color/black"
              android:textSize="17sp"/>
      
          <TextView
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:text="下面的按钮设置英文为大写："
              android:textColor="@color/black"
              android:textSize="17sp"
              android:gravity="center"/>
          <Button
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:text="Hello World!"
              android:textColor="@color/black"
              android:textSize="17sp"
              android:textAllCaps="true"/>
      
          <LinearLayout
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical"
              android:layout_marginTop="50sp">
              <Button
                  android:layout_width="match_parent"
                  android:layout_height="wrap_content"
                  android:text="直接指定点击方法"
                  android:textColor="@color/black"
                  android:textSize="17sp"
                  android:textAllCaps="true"
                  android:onClick="doclick"/>
              <TextView
                  android:id="@+id/tv_result"
                  android:layout_width="match_parent"
                  android:layout_height="wrap_content"
                  android:text="这里显示按钮的点击结果"
                  android:textSize="17sp"
                  android:textColor="@color/black"/>
      
          </LinearLayout>
      
      </LinearLayout>
      ```
    
    - 为了实现”直接指定直接方法“按钮的点击按钮则出现语句的功能，还需要在java文件中写上:
      
      ```java
      package com.example.chapter03;
      
      import android.os.Bundle;
      import android.view.View;
      import android.widget.Button;
      import android.widget.TextView;
      
      import androidx.annotation.Nullable;
      import androidx.appcompat.app.AppCompatActivity;
      import androidx.compose.ui.text.TextRange;
      
      import com.example.chapter03.util.DateUtil;
      
      public class ButtonStyleActivity extends AppCompatActivity {
      
          private TextView tv_result;
      
          @Override
          protected void onCreate(@Nullable Bundle savedInstanceState) {
              super.onCreate(savedInstanceState);
              setContentView(R.layout.activity_button_style);
              //（涉及生命周期知识）选择在中创建tv_result：tv_result创建在onCreat中,findViewById（该语句）只需要执行一次；若创建在doclick中，则是每点击一次执行一次，太麻烦了
              tv_result = findViewById(R.id.tv_result);//按下Ctrl+Alt+f之后，再按下回车键，将tv_result从局部变量设置为全局变量
          }
      
          public void doclick(View v){
              String desc =String.format("您在%s时点击了按钮：”%s“", DateUtil.getNowTime(),((Button) v).getText());
              tv_result.setText(desc);//显示的时间不是现实时间，而是虚拟机里的时间
          }
      }
      ```
    
    - 接着再新建一个名为DateUtil（名字可任意）的java文件，在里面写上：
      
      ```java
      package com.example.chapter03.util;
      
      import java.text.SimpleDateFormat;
      import java.util.Date;
      
      public class DateUtil {
      
          public static String getNowTime(){
              SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
              return sdf.format(new Date());
          }
      }
      ```
      
      最终效果：
      
      点击前：
      
      ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-24%20164522.png?raw=true)
      
      点击后：
      
      ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-24%20164655.png?raw=true)

#### 1.4.2 点击事件

- 补充知识点：**监听器**是专门<mark>监听控件的动作行为</mark>，只有控件发生了指定的动作<mark>，监听器</mark>才会<mark>触发开关去执行</mark>对应的代码逻辑。

- 点击监听器（<mark>OnClickListener</mark>）通过<mark>setOnClickListenrt方法(View的一个内部接口)</mark>设置。按钮<mark>被按住少于500毫秒</mark>时<mark>才会触发</mark>点击事件。

- **实现**点击事件有**两种常用方法** 
  
  - 首先现在xml文件中：
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    
        <Button
            android:id="@+id/btn_click_single"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="设置单独的点击监听器"
            android:textColor="#000000"
            android:textSize="15sp"/>
        <Button
            android:id="@+id/btn_click_public"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="设置公共的点击监听器"
            android:textColor="#000000"
            android:textSize="15sp"/>
    
        <TextView
            android:id="@+id/tv_result"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="5dp"
            android:gravity="center"
            android:textColor="#000000"
            android:textSize="15sp"
            android:text="这里查看按钮的点击结果"/>
    
    </LinearLayout>
    ```
  
  接着在java文件中：
  
  - **法一**：<mark>新建一个类</mark>MyOnClickListener（名称可自定义）来<mark>实例化OnClickListener</mark> 
    
    ```java
    package com.example.chapter03;
    
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    
    import androidx.annotation.Nullable;
    import androidx.appcompat.app.AppCompatActivity;
    
    import com.example.chapter03.util.DateUtil;
    
    public class ButtonClickActivity extends AppCompatActivity {
    
        private static TextView tv_result;
    
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_button_click);
            findViewById(R.id.btn_click_single);
            tv_result = findViewById(R.id.tv_result);
            Button btn_click_single = findViewById(R.id.btn_click_single);
            btn_click_single.setOnClickListener(new MyOnClickListener());
    
        }
    
        static class MyOnClickListener implements View.OnClickListener{//1.当监听到按钮被点击
            //2.就执行该回调方法
            @Override
            public void onClick(View v) {//注意别漏了实现该接口的抽象方法
                String desc =String.format("您在%s时点击了按钮：”%s“", DateUtil.getNowTime(),((Button) v).getText());
                tv_result.setText(desc);//在My0nClickListener类里使用tv_result有两点要注意
                //1.要在My0nClickListener类对着tv_result按下Ctrl+Alt+f，将其设置全局变量
                //2.由于My0nClickListener类是static（静态）的，而tv_result不是静态的，因此需要将tv_result也设置为静态的：private static TextView tv_result
            }
        }
    }
    ```
    
    最终效果（当点击第一个按钮时有反应，点击第二个没反应）
    
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-24%20210939.png?raw=true)
  
  - **法二**：用<mark>当前activity的类</mark>(此时为ButtonClickActivity)来实例化<mark>OnClickListener</mark>
    
    ```java
    package com.example.chapter03;
    
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    
    import androidx.annotation.Nullable;
    import androidx.appcompat.app.AppCompatActivity;
    
    import com.example.chapter03.util.DateUtil;
    
    public class ButtonClickActivity extends AppCompatActivity implements View.OnClickListener{
    
        private static TextView tv_result;
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_button_click);
            findViewById(R.id.btn_click_public);
            tv_result = findViewById(R.id.tv_result);
            Button btn_click_public = findViewById(R.id.btn_click_public);
            btn_click_public.setOnClickListener(this);//this指当前activity(ButtonClickActivity)的实例
        }
        @Override
        public void onClick(View view) {
            if(view.getId()==R.id.btn_click_public){
                String desc =String.format("您在%s时点击了按钮：”%s“，并通过当前的类ButtonClickActivity实现", DateUtil.getNowTime(),((Button) view).getText());
                tv_result.setText(desc);
            }
        }
    }
    ```
    
    最终效果（当点击第二个按钮时有反应，点击第一个没反应）
    
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-09-24%20211312.png?raw=true)

#### 1.4.3 长按点击事件

- 长按监听器（<mark>OnLongClickListener</mark>）通过<mark>setOnLongClickListenrt方法</mark>设置。按钮被按住<mark>大于等于500毫秒</mark>时<mark>才会触发</mark> 长按点击事件。

- 实现长按点击事件（包括上述的点击事件），除了上述两种方法之外，还有第三种方法，那就用匿名内部类的方式：
  
  - 首先在xml文件中：
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    
        <Button
            android:id="@+id/btn_long_click"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="设置长按点击监听器"
            android:textColor="#000000"
            android:textSize="15sp"/>
    
        <TextView
            android:id="@+id/tv_result"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="5dp"
            android:gravity="center"
            android:textColor="#000000"
            android:textSize="15sp"
            android:text="这里查看按钮的点击结果"/>
    
    </LinearLayout>
    ```
  
  - 接着在java文件中：
    
    ```java
    package com.example.chapter03;
    
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    
    import androidx.annotation.Nullable;
    import androidx.appcompat.app.AppCompatActivity;
    
    import com.example.chapter03.util.DateUtil;
    
    public class ButtonLongClickActivity extends AppCompatActivity {
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_button_long_click);
            TextView tv_result=findViewById(R.id.tv_result);
            Button btn_long_click=findViewById(R.id.btn_long_click);
            //用匿名内部类的方式：
            btn_long_click.setOnLongClickListener(new View.OnLongClickListener() {
                @Override
                public boolean onLongClick(View view) {
                    String desc =String.format("您在%s时点击了按钮：”%s“，并通过匿名类实现", DateUtil.getNowTime(),((Button) view).getText());
                    tv_result.setText(desc);
                    return true;
                }
            });
        }
    }
    ```
    
      <mark>onLongClick函数的返回值</mark>的说明：若<mark>当前长按按钮控件</mark>之外还<mark>有一个带有长按按钮的父容器</mark>时，若返回值为<mark>true</mark>，则<mark>当前</mark>长按按钮控件会<mark>消耗（触发）点击事件</mark>而<mark>不会往外传</mark>给父容器；若返回值为<mark>false</mark>时，则<mark>当前</mark>长按按钮控件自身<mark>不会消耗</mark>（触发）点击事件，而是<mark>传给父容器</mark>，由<mark>父容器来消耗</mark>（涉及到<mark>事件冒泡</mark>知识，感兴趣可自行了解）
    
    此外，jdk 8版本以上还可以将new View.OnLongClickListener()方法用<mark>lambda表达式</mark>来替换：
    
    ```java
    btn_long_click.setOnLongClickListener(view -> {
        String desc = String.format("您在%s时点击了按钮：”%s“，并通过匿名类实现", DateUtil.getNowTime(), ((Button) view).getText());
        tv_result.setText(desc);
        return true;
    ```
    
    最终效果：当点击按钮超过300毫秒时才有反应
    
       ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-03%20214517.png?raw=true)

#### 1.4.4 禁用与恢复按钮

- 在实际开发过程中（如点击按钮发送验证码给手机后60s内，无法再次点击按钮来发送验证码），<mark>按钮</mark>通常拥有<mark>两种状态</mark>，即<mark>不可用状态</mark>与<mark>可用状态</mark>，它们在<mark>外观和功能</mark>上的<mark>区别</mark>如下：
  
  - **不可用按钮**：按钮不允许点击，即按钮<mark>点击也没反应</mark>，且按钮<mark>文字为灰色</mark>。
  
  - **可用按钮**：按钮允许点击，<mark>点击</mark>按钮会<mark>触发点击事件</mark>，同时按钮<mark>文字为正常的黑色</mark>。

- 按钮是否允许点击由<mark>enabled属性</mark>控制，属性值为<mark>true</mark>时表示<mark>允许点击</mark>，为<mark>false</mark>时表示<mark>不允许点击</mark>。

- 实例：
  
  - 首先，在xml文件中：
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
    
            <Button
                android:id="@+id/btn_enable"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="启用测试按钮"
                android:textSize="17sp"/>
            <Button
                android:id="@+id/btn_disable"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="禁用测试按钮"
                android:textSize="17sp"/>
        </LinearLayout>
        <Button
            android:id="@+id/btn_test"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="测试按钮"
            android:textColor="#888888"
            android:textSize="17sp"
            android:enabled="false"/>
        <TextView
            android:id="@+id/tv_result"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="这里查看测试按钮的点击结果"/>
    
    </LinearLayout>
    ```
  
  - 接着，在java文件中：
    
    ```java
    package com.example.chapter03;
    
    import android.annotation.SuppressLint;
    import android.graphics.Color;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    
    import androidx.annotation.Nullable;
    import androidx.appcompat.app.AppCompatActivity;
    
    import com.example.chapter03.util.DateUtil;
    
    public class ButtonEnableActivity extends AppCompatActivity implements View.OnClickListener {
    
        private TextView btn_test;
        private TextView tv_result;
        private TextView tvResult;
        private Button btnTest;
    
        @SuppressLint("WrongViewCast")
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_bitton_enable);
            Button btn_enable =findViewById(R.id.btn_enable);
            Button btn_disable =findViewById(R.id.btn_disable);
            btn_test = findViewById(R.id.btn_test);//设置为全局变量：鼠标对着该变量名”btn_test“，再按下ctrl+alt+f，最后按下回车
            tv_result = findViewById(R.id.tv_result);//设置为全局变量
            //设置点击事件
            btn_enable.setOnClickListener(this);//设置公共的点击事件方法：将鼠标对着“this”,再按下alt+回车，最后再选择“mark...”选项
            btn_disable.setOnClickListener(this);
            btn_test.setOnClickListener(this);
        }
        //设置公共的点击事件
        @Override
        public void onClick(View view) {
            //由于在此处还需使用btn_test与tv_result，故需要将二者设置为全局变量
            if(view.getId()==R.id.btn_enable){
                btn_test.setEnabled(true);//启用当前控件
                btn_test.setTextColor(Color.BLACK);//设置按钮的文字颜色
            } if (view.getId()==R.id.btn_disable) {
                btn_test.setEnabled(false);
                btn_test.setTextColor(Color.GRAY);
            } if (view.getId()==R.id.btn_test) {
                String desc =String.format("您在%s时点击了按钮：”%s“", DateUtil.getNowTime(),((Button) view).getText());
                tv_result.setText(desc);
            }
        }
    }
    ```

- 最终效果：
  
    刚开始时：
  
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-04%20235912.png?raw=true)
  
    按下”启用按钮“按钮后：
  
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-04%20235927.png?raw=true)
  
    按下”测试按钮“后：
  
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-04%20235938.png?raw=true)
  
    按下”禁用测试按钮“后：
  
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-04%20235949.png?raw=true)

### 1.5 图像显示

#### 1.5.1 图像视图ImageView

- 图像视图展示的图片通常位于<mark>res/drawable目录</mark>。

- <mark>设置图像视图</mark>的显示图片有<mark>两种方式</mark>（<mark>例子</mark>中的图片文件名为”<mark>img_test01</mark>“，且位于res/drawable目录）：
  
  - 在<mark>xml文件</mark>中，通过属性<mark>android:src</mark>设置图片资源，属性值格式形如”<mark>@drawable/不含拓展名的图片名称</mark>“。
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <ImageView
            android:id="@+id/iv_scale"
            android:layout_width="match_parent"
            android:layout_height="200dp" />
    </LinearLayout>
    ```
  
  - 在<mark>java代码</mark>中，使用<mark>setImageResource方法</mark>设置图片资源，方法参数格式形如”<mark>R.drawable.不含拓展名的图片名称</mark>“。
    
    ```java
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <ImageView
            android:id="@+id/iv_scale"
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:scaleType="center"/>
    </LinearLayout>
    ```

- **图像视图的缩放**
  
  - ImageView本身默认图片居中显示（<mark>默认为fitCenter/FIT_CENTER</mark>类型)，若要改变图片的显示方式，可通过<mark>scaleType属性</mark>设定，该属性的取值说明如下：
    
    ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-05%20210831.png?raw=true)
  
  - 该部分可自行去试验。
  
  - <mark>center</mark>、centerInside、fitCenter、centerCrop的<mark>区别</mark>
    
    - 它们之间的区别在于：<mark>fitCenter</mark>既允许缩小图片，也允许放大图片；<mark>centerInside</mark>只允许缩小图片；<mark>center</mark>始终保持图片的原始尺寸（如果图片太大，则视图中只能显示图片的一部分），既不允许缩小图片，也不允许放大图片；<mark>centerCrope</mark>既可能缩小图片，也可能放大图片，最终结果是使得图片充满视图，可能使得图片宽高比例被改变。因此，当<mark>图片尺寸大于视图</mark>时，centerInside与fitCenter都会缩小图片，此时它俩的显示效果相同；当<mark>图片尺寸小于视图</mark>时，centerInside与center都会保持图片大小不变，此时它们的显示效果相同。

#### 1.5.2 图像按钮ImageButton

- <mark>ImageButton</mark> 是显示图片的图像按钮，它<mark>继承于ImageView</mark>，而非Button。

- **ImageButton**与**ImageView**之间的**区别**有：
  
  - <mark>ImageButton</mark>上的图像可<mark>按比例缩放</mark>，而<mark>Button</mark>通过背景设置的图像会<mark>拉伸变形</mark>，故<mark>通常</mark>图片按钮是通过<mark>ImageButton来实现</mark>。
  
  - <mark>Button</mark>只能靠背景<mark>显示一张图片</mark>，而<mark>ImageButton</mark>可分别在前景和背景显示图片，从而实现<mark>两张图片叠加</mark>的效果。
  
  - ImageButton<mark>有默认的按钮背景</mark>，ImageView<mark>默认无背景</mark>。
  
  - ImageButton<mark>默认</mark>的缩放类型为<mark>center</mark>，而ImageView<mark>默认</mark>的缩放类型为<mark>fitCenter</mark>
  
  - 示例代码
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    
        <ImageButton
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:src="@drawable/img_test01"
            android:scaleType="fitCenter"/>
    </LinearLayout>
    ```

#### 1.5.3 同时展示文本与图像

- 同时展示文本与图像主要有**两种方式**：
  
  - 利用<mark>LinearLayout</mark>对I<mark>mageView和TextView</mark>进行<mark>组合布局</mark>。
  
  - 通过<mark>按钮控件Button</mark>的<mark>drawablexx属性</mark>来<mark>设置</mark>文本周围的<mark>图标</mark>。
    
    - ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-06%20223710.png?raw=true)
    
    - 注意：通常<mark>按钮控件Button</mark>的背景默认为紫色且<mark>无法覆盖</mark>，所以<mark>需要进行以下操作</mark>：
      
      - 首先在"AndroidMainfest.xml"文件中，按下ctrl键后用鼠标点击如图圈出的位置：
        
        ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-06%20224304.png?raw=true)
      
      - 接着会进入“themes.xml"文件中，将划线的位置改为图片中划线部分的代码：
        
        ![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-06%20224606.png?raw=true)

### 1.6 项目实践：简单计算器

<mark>以下是示例代码：</mark>

#### 1.6.1 values文件夹中

- **strings . xml文件**
  
  ```xml
  <resources>
      <string name="app_name">MyApplication01</string>
      <string name="test">设置文本内容"</string>
      <string name="title_activity_linear_layout">LinearLayoutActivity</string>
      <string name="simple_calculator">简单计算器</string>
      <string name="cancle">CE</string>
      <string name="clear">C</string>
      <string name="plus">+</string>
      <string name="minus">-</string>
      <string name="divide">÷</string>
      <string name="multiply">×</string>
      <string name="one">1</string>
      <string name="two">2</string>
      <string name="three">3</string>
      <string name="four">4</string>
      <string name="five">5</string>
      <string name="six">6</string>
      <string name="seven">7</string>
      <string name="eight">8</string>
      <string name="nine">9</string>
      <string name="zero">0</string>
      <string name="dot">.</string>
      <string name="equal">=</string>
      <string name="sqrt">sqrt</string>
      <string name="reciprocal">1/x</string>
  </resources>
  ```
  
  

- 在<mark>values文件夹中</mark>再<mark>新建</mark>一个名为<mark>dimens . xml</mark>的文件
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
      
      <dimen name="button_font_size">30sp</dimen>
      <dimen name="button_height">80dp</dimen>
  </resources>
  ```



#### 1.6.2 layout文件夹中

- <mark>新建</mark>一个名为<mark>activity_calculator . xml</mark>的文件
  
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical"
      android:background="#EEEEEE"
      android:padding="5dp">
  
  <!--用滚动视图可以避免某些型号手机的尺寸太小-->
      <ScrollView
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:orientation="vertical">
  
  <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
  
      <!--计算器的名称-->
      <TextView
          android:layout_width="match_parent"
          android:layout_height="40dp"
          android:gravity="center"
          android:text="@string/simple_calculator"
          android:textColor="@color/white"
          android:textSize="20sp"
          android:background="#03A9F4"/>
  
      <!--显示的数字，这里最后加上的lines指高度为几行-->
      <TextView
          android:id="@+id/tv_result"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:background="@color/white"
          android:gravity="bottom|right"
          android:text="@string/zero"
          android:textSize="25sp"
          android:textColor="@color/black"
          android:lines="3"/>
  
      <GridLayout
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:rowCount="5"
          android:columnCount="4">
  
          <Button
              android:id="@+id/btn_cancle"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/cancle"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_divide"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/divide"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_mutiply"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/multiply"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_clear"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/clear"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_seven"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/seven"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_eight"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/eight"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_nine"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/nine"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_plus"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/plus"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_four"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/four"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_five"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/five"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_six"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/six"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_minus"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/minus"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_one"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/one"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_two"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/two"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_three"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/three"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"/>
          <Button
              android:id="@+id/btn_sqrt"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/sqrt"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"
              android:textAllCaps="false"/>
          <Button
              android:id="@+id/btn_reciprocal"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/reciprocal"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"
              android:textAllCaps="false"/>
          <Button
              android:id="@+id/btn_zero"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/zero"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"
              android:textAllCaps="false"/>
          <Button
              android:id="@+id/btn_dot"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/dot"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"
              android:textAllCaps="false"/>
          <Button
              android:id="@+id/btn_equal"
              android:layout_width="0dp"
              android:layout_columnWeight="1"
              android:layout_height="@dimen/button_height"
              android:text="@string/equal"
              android:textSize="@dimen/button_font_size"
              android:textColor="@color/black"
              android:textAllCaps="false"/>
  
      </GridLayout>
  </LinearLayout>
  
      </ScrollView>
  </LinearLayout>
  ```



#### 1.6.3 java文件

- <mark>新建</mark>一个名为<mark>CalculatorActivity . java</mark>的java文件
  
  ```java
  package com.example.chapter03;
  
  import android.os.Bundle;
  import android.view.View;
  import android.widget.TextView;
  
  import androidx.annotation.Nullable;
  import androidx.appcompat.app.AppCompatActivity;
  
  import org.w3c.dom.Text;
  
  import javax.sql.StatementEvent;
  
  public class CalculatorActivity extends AppCompatActivity implements View.OnClickListener {
  
      private TextView tv_result;//设置为类中的全局变量
      private String firstNum = "";//第一个操作数
      private String operator = "";//运算符
      private String secondNum = "";//第二个操作数
      private String result = "";//当前的计算结果
      private String showText = "0";//显示在计算器上的文本内容，不一定是单个数字或符号，可以是数字与符号组成的一大串
  
      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_calculator);
          tv_result = findViewById(R.id.tv_result);
          //下面给每个按钮控件都注册监听器（由于之后用不到按钮的变量，故无需再创建新的按钮控件变量）
          findViewById(R.id.btn_cancle).setOnClickListener(this);
          findViewById(R.id.btn_divide).setOnClickListener(this);
          findViewById(R.id.btn_mutiply).setOnClickListener(this);
          findViewById(R.id.btn_clear).setOnClickListener(this);
          findViewById(R.id.btn_nine).setOnClickListener(this);
          findViewById(R.id.btn_eight).setOnClickListener(this);
          findViewById(R.id.btn_seven).setOnClickListener(this);
          findViewById(R.id.btn_six).setOnClickListener(this);
          findViewById(R.id.btn_five).setOnClickListener(this);
          findViewById(R.id.btn_four).setOnClickListener(this);
          findViewById(R.id.btn_three).setOnClickListener(this);
          findViewById(R.id.btn_two).setOnClickListener(this);
          findViewById(R.id.btn_one).setOnClickListener(this);
          findViewById(R.id.btn_plus).setOnClickListener(this);
          findViewById(R.id.btn_minus).setOnClickListener(this);
          findViewById(R.id.btn_sqrt).setOnClickListener(this);
          findViewById(R.id.btn_reciprocal).setOnClickListener(this);
          findViewById(R.id.btn_zero).setOnClickListener(this);
          findViewById(R.id.btn_dot).setOnClickListener(this);
          findViewById(R.id.btn_equal).setOnClickListener(this);
      }
  
      @Override
      public void onClick(View view) {
          String inputText;//按下按钮后输入的内容
          //将按钮对应的文本转化为字符串类型并赋值给inputText
          inputText = ((TextView) view).getText().toString();
              //用于未给按钮创建相应的变量，故需要R.id的方式
          if(view.getId()==R.id.btn_clear){//点击了清除按钮
              clear();
              }
          else if (view.getId()==R.id.btn_cancle) {//点击了回退按钮
              if (!showText.equals("") && !showText.equals("0")){
                  if(operator.equals("")){//只有操作数1
                      firstNum=firstNum.substring(0,firstNum.length()-1);
                  }
                  else {
                      if(!secondNum.equals("")){
                          secondNum=secondNum.substring(0,secondNum.length()-1);
                      }
                  }
                  refreshText(showText.substring(0,showText.length()-1));
                  }
          }
          else if (view.getId()==R.id.btn_sqrt) {//点击了开平方根按钮
              if (!firstNum.equals("")) {
                  double calculate_result = Math.sqrt(Double.parseDouble(firstNum));
                  refreshOperate(String.valueOf(calculate_result));
                  refreshText(showText + "√=" + result);
              }
          }
          else if (view.getId()==R.id.btn_plus || view.getId()==R.id.btn_minus || view.getId()==R.id.btn_mutiply ||view.getId()==R.id.btn_divide) {
              //点击了加减乘除按钮
              if (!showText.equals("")) {
                  operator = inputText;
                  refreshText(showText + operator);
              }
          }
          else if(view.getId()==R.id.btn_reciprocal) {//点击了求倒数按钮
              if (!firstNum.equals("")) {
                  double calculate_result = 1.0 / Double.parseDouble(firstNum);
                  refreshOperate(String.valueOf(calculate_result));
                  refreshText("1" + "/" + showText + "=" + result);
              }
          }
          else if (view.getId()==R.id.btn_equal) {//点击了等号按钮
              if (!showText.equals("")) {
                  if(operator.equals("")){
                      refreshText(showText+"="+showText);
                  }
                  else {
                      double calculate_result = calculateFour();
                      refreshOperate(String.valueOf(calculate_result));
                      refreshText(showText + "=" + result);
                  }
              }
          }
          else{//点击了数字或小数点
                  if (operator.equals("")) {//无运算符，则继续拼接第一个操作数（可连续拼接）
                      if((firstNum.equals(".") ||firstNum.equals("0."))&& inputText.equals(".")){//避免同时输入多个小数点
                          inputText="";
                      }
                      else{
                          firstNum = firstNum + inputText;
                      }
                  } else {//有运算符，继续拼接第二个操作数（可连续拼接）
                      if((secondNum.equals(".") ||secondNum.equals("0.")) && inputText.equals(".")){//避免同时输入多个小数点
                          inputText="";
                      }
                      else{
                          secondNum = secondNum + inputText;
                      }
                  }
                  if (showText.equals("0") && !inputText.equals(".")) {//若当前显示的数字只有0，则输入的数字应该覆盖此0；但如果输入的是小数点，
                      //则用户的目的可能是输入小数，此时不覆盖
                      refreshText(inputText);
                  }
                  else if (showText.equals("0") && inputText.equals(".")) {//一开始时就按下小数点
                      firstNum="0"+".";
                      refreshText(firstNum);
                  } else {
                      refreshText(showText + inputText);//写成”showText + “才能实现拼接的效果：是因为前面可能已经有数据了，即showtext（为该类中的全局变量，可存储值）可能已经有值了
                  }
          }
      }
  
      //清除文本显示
      private void clear() {
          refreshOperate("");//顺便借助一下该函数，能把几个参与运算的变量同时清零
          refreshText("");
      }
  
      //刷新运算结果，实现运算完之后的结果可以继续参与新的运算
      private void refreshOperate(String new_result) {
          result = new_result;
          firstNum=result;
          secondNum="";//待输入，故为空
          operator="";//待输入，故为空
      }
  
      //更新文本显示
      private void refreshText(String text){
          showText = text;
          tv_result.setText(showText);
      }
  
      //进行四则运算
      private double calculateFour(){
          if(operator.equals("+")){//parseDouble:将字符串类型转化为double类型
              return Double.parseDouble(firstNum)+Double.parseDouble(secondNum);
          } else if (operator.equals("-")) {
              return Double.parseDouble(firstNum)-Double.parseDouble(secondNum);
          } else if (operator.equals("×")) {
              return Double.parseDouble(firstNum)*Double.parseDouble(secondNum);
          } else if (operator.equals("÷")) {
              return Double.parseDouble(firstNum)/Double.parseDouble(secondNum);
          }
          return 0;
      }
  }
  
  ```



#### 1.6.4 最终效果

![](https://github.com/meeting77smile/Android-Learning/blob/main/note/images/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-09%20194406.png?raw=true)
