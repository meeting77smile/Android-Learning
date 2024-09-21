# Android基础

## 1.控件

### 1.1 简单控件

 **<font color=red>文本显示</font>**

- **设置文本内容**
  
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

- **设置字体大小**
  
  类似的，也有两种方式：
  
   1.**在XML文件中通过属性android:textSize设置文本（要带单位）**：
  
  ```xml
      <TextView
          android:textSize="30dp"/>
  ```
  
  其中，字号单位有三种：
  
  - **px**(PiXel像素)：是手机屏幕的最小显示单位，与设备的显示屏有关。
  
  - **dp**：指的是与设备无关的显示单位，只与屏幕的尺寸有关。对于相同分辨率的手机，屏幕越大，相同DP的组件占用的屏幕比例越小；对于**相同尺寸**的手机，即使分辨率不同，**相同DP**的组件**占用的屏幕比例也相同**。
  
  - **sp**：原理与dp类似，但它是<mark>专门用于设置字体大小</mark>的。当在手机系统调整字体大小时，只有以sp为单位的字体的大小会变化，px和dp则不会改变。
  2. **在Java代码中调用文本视图对象的setTextSize方法设置**：
     
     ```java
     TextView tv_test=findViewById(R.id.tv_test);
     tv_test.setTextSize(30);
     ```
     
     此时设置字体大小不用带单位，默认以sp为单位(说明官方推荐以sp作为字体的单位)

- **设置字体颜色**
  
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

- **设置视图的宽高**
  
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

- **设置视图的间距**
  
  可在**XML文件**中改变两种属性：
  
  - **layout_margin** 属性（设置外间距），它指定了当前视图与周围<mark>同级</mark>视图之间的距离，包括**layout_margin** , **layout_marginLeft** , **layout_marginRight** , **layout_marginTop** , **layout_marginBottom**等属性。
  
  - **padding** 属性（设置内间距），它指定了当前视图与内部<mark>下级</mark>视图之间的距离 , 包括**padding** , **paddingLeft** , **paddingTop** , **paddingRight** , **paddingBottom**等属性。
    
    示例：
    
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

- **设置视图的对齐方式**
  
  类似的，在<mark>XML文件</mark>中也有两种属性：
  
  - **layout_gravity** 属性，它指定了<mark>当前视图相对于上级视图</mark>的对齐方式。
  
  - **gravity** 属性，它指定了<mark>下级视图相对于当前视图</mark>的对齐方式。
    
    
    
    二者的取值包括：<mark>left</mark> , <mark>right</mark> , <mark>bottom</mark>  , <mark>top </mark>。
    
    还可以用<mark>竖线 “ | ” 同时取值</mark>，例如 **letf|top** 表示既靠左又靠上，也就是朝左上角对齐。此外，此方式<mark>没有顺序要求</mark>，即此时 top|left也有相同的效果。
    
    示例：
    
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
