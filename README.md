
# Ex.No:1 To create a database table and to display the database table field using SQLite Database in Android Studio.


## AIM:

To create a database table and to display the database table field using SQLite Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as HelloWorld and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the DatabaseTable using the SQLite”.
Developed by:D.Amarnath Reddy
Registeration Number :212221240012
*/
```
### MainActivity.java:
```
package com.example.dbms;
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.database.Cursor;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    EditText editUserID;
    EditText editUserName;
    EditText editUserPassword;

    DataBaseManager dbManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editUserID = findViewById(R.id.editTextID);
        editUserName = findViewById(R.id.editTextUserName);
        editUserPassword = findViewById(R.id.editTextPassword);

        dbManager = new DataBaseManager(this);
        try {
            dbManager.open();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void btnUpdatePressed(View v) {
        dbManager.update(Long.parseLong(editUserID.getText().toString()), editUserName.getText().toString(), editUserPassword.getText().toString());
    }

    public void btnInsertPressed(View v) {
        dbManager.insert(editUserName.getText().toString(), editUserPassword.getText().toString());
    }
}

```
### DataBaseHelper.java:
``` 
package com.example.dbms;
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;


public class DataBaseHelper extends SQLiteOpenHelper {
    static final String DATABASE_NAME="DataBASE.DB";
    static final int DATABASE_VERSION=1;

    static final String DATABASE_TABLE= "USERS";
    static final String USER_ID= "_ID";
    static final String USER_NAME="user_name";
    static final String USER_PASSWORD="password";

    private static final String CREATE_DB_QUERY = "CREATE TABLE "+DATABASE_TABLE +" ( "+ USER_ID+ " INTEGER PRIMARY KEY AUTOINCREMENT, "+USER_NAME+ " TEXT NOT NULL,   "+ USER_PASSWORD+ " TEXT NOT NULL );";
    public DataBaseHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_DB_QUERY);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int i, int i1) {
        db.execSQL("DROP TABLE IF EXISTS "+ DATABASE_TABLE);
    }
}
```
### DataBaseManager.java:
```
package com.example.dbms;
import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;

import java.sql.SQLDataException;

public class DataBaseManager {
    private DataBaseHelper dbHelper;
    private Context context;
    private SQLiteDatabase database;

    public DataBaseManager(Context ctx){

        context=ctx;
    }
    public DataBaseManager open() throws SQLDataException {
        dbHelper = new DataBaseHelper(context);
        database = dbHelper.getWritableDatabase();
        return this;
    }
    public void close(){

        dbHelper.close();
    }
    public void insert (String username, String password){
        ContentValues contentValues=new ContentValues();
        contentValues.put(DataBaseHelper.USER_NAME,username);
        contentValues.put(DataBaseHelper.USER_PASSWORD,password);
        database.insert(DataBaseHelper.DATABASE_TABLE,null,contentValues);
    }
    public int update(long _id,String username,String password){
        ContentValues contentValues=new ContentValues();
        contentValues.put(DataBaseHelper.USER_NAME,username);
        contentValues.put(DataBaseHelper.USER_PASSWORD,password);
        int ret = database.update(DataBaseHelper.DATABASE_TABLE,contentValues,DataBaseHelper.USER_ID+"="+_id,null);
        return ret;
    }
}
```
### activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/editTextUserName"
        android:layout_width="350dp"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="100dp"
        android:textAlignment="center"
        android:hint="Username"/>

    <EditText
        android:id="@+id/editTextID"
        android:layout_width="350dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:inputType="textPersonName"
        android:hint="User ID"
        android:textAlignment="center"
        android:layout_below="@+id/editTextUserName"
        android:layout_centerHorizontal="true"/>

    <EditText
        android:id="@+id/editTextPassword"
        android:layout_marginTop="20dp"
        android:layout_width="350dp"
        android:layout_height="wrap_content"
        android:layout_below="@+id/editTextID"
        android:hint="Password"
        android:inputType="textPassword"
        android:textAlignment="center"
        android:layout_centerHorizontal="true"/>

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextPassword"
        android:layout_marginTop="4dp"
        android:onClick="btnFetchPressed"
        android:text="Fetch" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="btnInsertPressed"
        android:text="Insert"
        android:layout_below="@id/button2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="btnUpdatePressed"
        android:text="Update"
        android:layout_below="@id/button" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="btnDeletePressed"
        android:text="Delete"
        android:layout_below="@id/button3" />
</RelativeLayout>
```

## OUTPUT
<img width="561" alt="image" src="https://github.com/Dhanireddy-Amarnthreddy/Advance-Android-Odd-/assets/94165103/a295d461-6a92-4c36-aa58-27a5b8eb6bfc">
<img width="569" alt="image" src="https://github.com/Dhanireddy-Amarnthreddy/Advance-Android-Odd-/assets/94165103/68c796fd-f72e-43b0-a732-3584a9aa6922">
<img width="560" alt="image" src="https://github.com/Dhanireddy-Amarnthreddy/Advance-Android-Odd-/assets/94165103/78c15836-a9ba-4693-8878-27877c346c86">
<img width="170" alt="image" src="https://github.com/Dhanireddy-Amarnthreddy/Advance-Android-Odd-/assets/94165103/372b694f-dd21-4826-840a-68019916a8f2">

## RESULT

Thus a Simple Android Application create a dtatabase table and to display the database table  using SQLite Database in Android Studio is developed and executed successfully.
