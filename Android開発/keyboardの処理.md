## キーボードの処理6/21



1. EditTextの前にフォーカス用のダミーを作成する。
   ```focusable```と```focusableInTouchMode```と```clickable```は```true```にしておく。

2. EditTextのimeOptionsを```actionDone```にしておく。これはソフトウェアキーボードのenterボタンが押されたときの処理を上書きするため。（今回だとフォーカスをeditTextからフォーカスを外す。）

3. editTextのフォーカスが外れたときはキーボードをしまう。

   ```kotlin
   binding.editText1.setOnFocusChangeListener { v, hasFocus -> 
    if (!hasFocus) {
        val manager = this.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
        manager.hideSoftInputFromWindow(v.windowToken, 0)
     }
   }
   ```

   

4. キーボードが押された時の処理
   2のimeOptionsで指定したキーを指定する。

   ここで1で指定したダミーにフォーカスを合わせればキーボードが隠れる。

   ```kotlin
   binding.editText1.setOnEditorActionListener { v, actionId, event ->
   	return@setOnEditorActionListener when (actionId) {
   	EditorInfo.IME_ACTION_DONE-> {
   		binding.closeKey.requestFocus()
   		true
   	}
   	else -> false
   	}
   }
```
   
5. 





```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <androidx.constraintlayout.widget.ConstraintLayout

        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/constraint"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        
        <TextView
            android:id="@+id/closeKey"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            android:focusable="true"
            android:focusableInTouchMode="true"
            android:clickable="true"
            />

        <EditText
            android:id="@+id/editText1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            android:inputType="text"
            android:imeOptions="actionDone"
            app:layout_constraintTop_toBottomOf="@id/guideline10"/>

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```