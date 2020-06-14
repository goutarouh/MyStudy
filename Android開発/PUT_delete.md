### PUTメソッド

HttpUtilのput関数
formBodyは空?

- 追加はadd("String", "String")の形で追加できる

```kotlin
fun httpPut(url: String): String? {
    val formBody: RequestBody = FormBody.Builder()
    .build()
    val request = Request.Builder()
    .url(url)
    .put(formBody)
    .build()
    val response = HttpClient.instance.newCall(request).execute()
    val body = response.body?.string()
    return body
}
```

ViewModelの更新関数

```kotlin
fun change(user: User) {
    //        viewModelScope.launch(Dispatchers.Main) {
    //            val http = HttpUtil()
    //            async(Dispatchers.Default) { http.httpPut(  "http://10.0.2.2:3000/user/${user.id}") }.await().let {
    //            }
    //        }


    val position = _newInvitations.value?.indexOf(user)!!
    _newInvitations.value?.set(position, user.copy(s = user.s == false))
    _newInvitations.postValue(_newInvitations.value)

}
}
```



### LiveDataの削除

http://www.366service.com/jp/qa/0665ba3e8a46f9c0eff61a78c7185c1c このURL参照

安定版の1.1.0から1.2.0.alpha03に変更

```groovy
implementation 'androidx.recyclerview:recyclerview:1.2.0-alpha03'
```

HttpUtilのdelete関数

```kotlin
fun httpDelete(url: String): String? {
    val formBody: RequestBody = FormBody.Builder().build()
    val request = Request.Builder()
    .url(url)
    .delete()
    .build()
    val response = HttpClient.instance.newCall(request).execute()
    val body = response.body?.string()
    return body
}
```

ViewModelの削除関数

```kotlin
fun delete(user: User) {
    viewModelScope.launch(Dispatchers.Main) {
        val http = HttpUtil()
        async(Dispatchers.Default) { http.httpDelete(  "http://10.0.2.2:3000/user/${user.id}") }.await().let {
        }
    }

    _newInvitations.value?.remove(user)
    _newInvitations.postValue(_newInvitations.value)
}
```







### Adapter+DataBindin

オブザーバー

```kotlin
viewModel.newInvitations.observe(this, Observer { it ->
    adapter.submitList(null)
    adapter.submitList(it)
})
```



アダプター

```kotlin
package com.example.put_delete

import android.util.Log
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.ListAdapter
import androidx.recyclerview.widget.RecyclerView
import com.example.put_delete.databinding.ItemUserBinding

class UserAdapter(
    val viewModel: MainViewModel
):ListAdapter<User, UserAdapter.ViewHolder>(callbacks) {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val inflater = LayoutInflater.from(parent.context)

        //変更後
        val binding = ItemUserBinding.inflate(inflater, parent, false)
        return ViewHolder(binding)

        //変更前
        //val root = inflater.inflate(R.layout.item_user, parent, false)
        //return ViewHolder(root)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val user = getItem(position)

        //変更前
        //holder.nameText.text = user.name

        //変更後
        holder.binding.user = user
        holder.binding.vm = viewModel
    }

    //変更前
    //class ViewHolder(root: View): RecyclerView.ViewHolder(root) {
        //val nameText: TextView = root.findViewById(R.id.text)
    //}

    //変更後
    class ViewHolder(val binding: ItemUserBinding): RecyclerView.ViewHolder(binding.root) {
        //var position = binding.position
        //var user = binding.user
    }

    companion object {
        private val callbacks = object: DiffUtil.ItemCallback<User>() {
            override fun areItemsTheSame(oldItem: User, newItem: User): Boolean {
                return oldItem.id == newItem.id
            }

            override fun areContentsTheSame(oldItem: User, newItem: User): Boolean {
                return oldItem == newItem
            }

        }
    }

}
```

