1. パーミッションの追加
   AndroidManifest.xmlの<application>タグの手前

   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```

   

2. okhttpの追加
   build.gradle(app)のdependency

   ```groovy
   implementation  "com.squareup.okhttp3:okhttp:4.3.1"
   ```

   

3. coroutinesの追加

   build.gradle(app)のdependency

   ```groovy
   def coroutines_version = "1.3.3"
   implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines_version"
   implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:$coroutines_version"
   ```

   

4. Java8の機能の有効可
   build.gradle(app)のandroid

   ```groovy
   compileOptions {
   	sourceCompatibility JavaVersion.VERSION_1_8
       targetCompatibility JavaVersion.VERSION_1_8
   }
   ```

   

5. kotlinのバージョンの変更

   - build.gradle(project)のbuildscript
   - koroutinesを使用するため
   - 1.3.71→1.3.70(2020/6/4)

   ```groovy
   ext.kotlin_version = '1.3.70'
   ```





追加

すべての通信許可 manifest.xmlのapplication

```xml
android:usesCleartextTraffic="true"

tools:targetApi="m"もついでに追加すると成功することがある。
```



アプリのアンインストール



### ここまででlivedoorのお天気データは持ってこれたがjsonサーバーはなぜかだめ

```kotlin
val URL = "http://weather.livedoor.com/forecast/webservice/json/v1?city=400040"
```



androidではエミュレータからみたlocal machineは127.0.0ではなく10.0.2.2です。

```kotlin
val URL = "http://10.0.2.2:3000/test"
```

---



これはたぶん必要ない

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

