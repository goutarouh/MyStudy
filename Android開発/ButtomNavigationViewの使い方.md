1. 適当に3つほどFragmentを準備する。Fragment1~3とする。

   ```kotlin
   // app\src\main\java\com\example\deleteafterthis\Fragment1.kt
   class Fragment1: Fragment() {
       override fun onCreateView(
           inflater: LayoutInflater,
           container: ViewGroup?,
           savedInstanceState: Bundle?
       ): View? {
           val root = inflater.inflate(R.layout.fragment_1, container, false)
           return root
       }
   }
   ```

   

   ```xml
   <!-- app\src\main\res\layout\fragment1 -->
   <?xml version="1.0" encoding="utf-8"?>
   <androidx.constraintlayout.widget.ConstraintLayout
       xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:app="http://schemas.android.com/apk/res-auto"
       xmlns:tools="http://schemas.android.com/tools"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
   
       <TextView
           android:id="@+id/textView1"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="Fragment1だよ"
           android:textSize="48sp"
           app:layout_constraintBottom_toBottomOf="parent"
           app:layout_constraintEnd_toEndOf="parent"
           app:layout_constraintStart_toStartOf="parent"
           app:layout_constraintTop_toTopOf="parent" />
   </androidx.constraintlayout.widget.ConstraintLayout>
   ```

   

2. axtivity_main.xmlを編集

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:app="http://schemas.android.com/apk/res-auto"
       xmlns:tools="http://schemas.android.com/tools"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       tools:context=".MainActivity">
   
       <fragment
           android:id="@+id/main_fragment"
           android:name="androidx.navigation.fragment.NavHostFragment"
           android:layout_width="match_parent"
           android:layout_height="0dp"
           app:layout_constraintTop_toTopOf="parent"
           app:layout_constraintBottom_toTopOf="@+id/bottom_navigation_view"
           app:defaultNavHost="true"
           app:navGraph="@navigation/nam_main" />
   
       <com.google.android.material.bottomnavigation.BottomNavigationView
           android:id="@+id/bottom_navigation_view"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:background="#FFEB3B"
           app:layout_constraintBottom_toBottomOf="parent"
           app:menu="@menu/bottom_navigation_menu" />
   
   </androidx.constraintlayout.widget.ConstraintLayout>
   ```

   

3. navigationファイルはこんな感じ

   ```xml
   <!-- app\src\main\res\navigation\nam_main.xml -->
   <?xml version="1.0" encoding="utf-8"?>
   <navigation xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:app="http://schemas.android.com/apk/res-auto" android:id="@+id/nam_main"
       app:startDestination="@id/fragment1">
   
       <fragment
           android:id="@+id/fragment1"
           android:name="com.example.deleteafterthis.Fragment1"
           android:label="Fragment1" />
       <fragment
           android:id="@+id/fragment2"
           android:name="com.example.deleteafterthis.Fragment2"
           android:label="Fragment2" />
       <fragment
           android:id="@+id/fragment3"
           android:name="com.example.deleteafterthis.Fragment3"
           android:label="Fragment3" />
   </navigation>
   ```

   

4. メニューファイルはこんな感じ

   ```xml
   <!-- app\src\main\res\menu\bottom_navigation_menu.xml -->
   <?xml version="1.0" encoding="utf-8"?>
   <menu xmlns:android="http://schemas.android.com/apk/res/android">
   
       <item
           android:id="@+id/fragment1"
           android:icon="@drawable/ic_android_black_24dp"
           android:enabled="true"
           android:title="Frag1"/>
       <item
           android:id="@+id/fragment2"
           android:icon="@drawable/ic_android_black_24dp"
           android:enabled="true"
           android:title="Frag2"/>
       <item
           android:id="@+id/fragment3"
           android:icon="@drawable/ic_android_black_24dp"
           android:enabled="true"
           android:title="Frag3"/>
   
   </menu>
   ```

   

5. MainActivity.ktはこんな感じ

   ```kotlin
   package com.example.deleteafterthis
   
   import androidx.appcompat.app.AppCompatActivity
   import android.os.Bundle
   import androidx.navigation.findNavController
   import androidx.navigation.ui.setupWithNavController
   import com.google.android.material.bottomnavigation.BottomNavigationView
   
   class MainActivity : AppCompatActivity() {
   
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)
   
           val navController = findNavController(R.id.main_fragment)
           findViewById<BottomNavigationView>(R.id.bottom_navigation_view)
               .setupWithNavController(navController)
       }
   }
   
   ```



## 結果

<img src="C:\Users\gouta\AppData\Roaming\Typora\typora-user-images\image-20200601200329996.png" width="200pm">

## 注意点

- Navigationファイルで指定したIDとMenuファイルで指定するIDを対応させる。
- findNavControllerはNavHostFragmentの要素を指定する。
  - findNavControllerは```import androidx.navigation.findNavController```を使用する。(他のパッケージにもいくつかある。。。)