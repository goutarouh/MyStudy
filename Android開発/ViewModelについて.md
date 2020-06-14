### ViewModel



1. ユーザー情報を保持する。

   ```kotlin
   class MyViewModel : ViewModel() {
           private val users: MutableLiveData<List<User>> by lazy {
               MutableLiveData().also {
                   loadUsers()
               }
           }
   
           fun getUsers(): LiveData<List<User>> {
               return users
           }
   
           private fun loadUsers() {
               // Do an asynchronous operation to fetch users.
           }
       }
       
   ```

2. アクティビティからのアクセス

   ```kotlin
   class MyActivity : AppCompatActivity() {
   
           override fun onCreate(savedInstanceState: Bundle?) {
               // Create a ViewModel the first time the system 
               //calls an activity's onCreate() method.
               // Re-created activities receive the same MyViewModel
               //instance created by the first activity.
   
               // Use the 'by viewModels()' Kotlin property delegate
               // from the activity-ktx artifact
               val model: MyViewModel by viewModels()
               model.getUsers().observe(this, Observer<List<User>>{ users ->
                   // update UI
               })
           }
       }
   ```

   



### ViewModelのライフサイクルについて

- ViewModelオブジェクトのスコープは、ViewModelProviderに渡されるLifecycleで設定される。





# 実装

- #### 参考

https://qiita.com/takahirom/items/3f012d46e15a1666fa33

- ViewModelProviderがエラーになる原因はライブラリを最新にしていなかったため。

  ### 動いたコードがこれ

  ```kotlin
  class MyViewModel : ViewModel() {
      var count: Int = 0
  }
  
  class MainActivity : AppCompatActivity() {
  
      lateinit var textView: TextView
      lateinit var myViewModel: MyViewModel
  
      override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.activity_main)
  
          textView = findViewById<TextView>(R.id.textView)
   Failed to call observer method
  
          textView.text = myViewModel.count.toString()
          textView.setOnClickListener {
              myViewModel.count++
              textView.text = myViewModel.count.toString()
          }
      }
  ```

  

