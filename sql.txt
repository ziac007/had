<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/uname"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPersonName"
        android:hint="Username"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.512"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.123" />

    <EditText
        android:id="@+id/pwd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPassword"
        android:hint="Password"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.512"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.287" />

    <Button
        android:id="@+id/ins"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Insert"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.428" />

    <Button
        android:id="@+id/del"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Delete"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.673" />

    <Button
        android:id="@+id/update"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.565" />

    <Button
        android:id="@+id/dis"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Display"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.784" />

    <TextView
        android:id="@+id/txt"
        android:layout_width="261dp"
        android:layout_height="49dp"
        android:hint="Display"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.566"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.884" />
</androidx.constraintlayout.widget.ConstraintLayout>




Main.java 

package com.example.sqlite;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    EditText uname,pass;
    TextView txt;
    Button ins,del,dis,upd;
    dbhelper helper;
    private static final String dbname="comp";
    private static final String tbname = "employ";
    private static final int dbversion =1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        uname=findViewById(R.id.uname);
        pass=findViewById(R.id.pwd);
        ins=findViewById(R.id.ins);
        del=findViewById(R.id.del);
        dis=findViewById(R.id.dis);
        upd=findViewById(R.id.update);
        txt=findViewById(R.id.txt);

        ins.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                helper=new dbhelper(MainActivity.this,dbname,null,dbversion);

                long lo=helper.adduser(uname.getText().toString(),pass.getText().toString());
                if(lo==-1)
                    Toast.makeText(MainActivity.this, "Error", Toast.LENGTH_SHORT).show();
                else
                    Toast.makeText(MainActivity.this, "Inserted", Toast.LENGTH_SHORT).show();
            }
        });
        del.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                helper=new dbhelper(MainActivity.this,dbname,null,dbversion);
                helper.delete(uname.getText().toString());
            }
        });
        upd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                helper=new dbhelper(MainActivity.this,dbname,null,dbversion);
                helper.update(uname.getText().toString(),pass.getText().toString());
            }
        });
        dis.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                helper=new dbhelper(MainActivity.this,dbname,null,dbversion);
                String res= helper.display(MainActivity.this);
                txt.setText(res);
            }
        });
    }
}




dbhelper.java


package com.example.sqlite;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

public class dbhelper extends SQLiteOpenHelper {

    private static final String dbname="comp";
    private static final String tbname = "employ";
    private static final int dbversion =1;
    public dbhelper(@Nullable Context context, @Nullable String name, @Nullable SQLiteDatabase.CursorFactory factory, int version) {
        super(context, dbname,null, dbversion);
    }

    @Override
    public void onCreate(SQLiteDatabase sqLiteDatabase) {
        sqLiteDatabase.execSQL("CREATE TABLE "+tbname+"(uname VARCHAR(10),passw VARCHAR(10))"+";");
    }

    @Override
    public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
        sqLiteDatabase.execSQL("DROP TABLE IF EXISTS "+tbname);
        onCreate(sqLiteDatabase);
    }
    public long adduser(String name, String pass){
        SQLiteDatabase sqLiteDatabase = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("uname",name);
        cv.put("passw",pass);
        long result = sqLiteDatabase.insert(tbname,null,cv);
        sqLiteDatabase.close();
        return result;
    }
    public void update(String name, String pass){
        SQLiteDatabase sqLiteDatabase = this.getWritableDatabase();
        sqLiteDatabase.execSQL("UPDATE "+tbname+" SET passw='"+pass+"' "+" WHERE uname='"+name+"'");
        sqLiteDatabase.close();
    }
    public void delete(String name)
    {
        SQLiteDatabase sqLiteDatabase = this.getWritableDatabase();
        sqLiteDatabase.execSQL("DELETE FROM " + tbname + " WHERE uname='" + name+"'");
        sqLiteDatabase.close();
    }
    public String display(Context ctx)
    {
        SQLiteDatabase sqLiteDatabase = this.getReadableDatabase();
        Cursor cursor = sqLiteDatabase.rawQuery("SELECT * FROM "+tbname,null);
        String finalres = " ";
        while(cursor.moveToNext()) {
            finalres += cursor.getString(0) + ":" + cursor.getString(1);
            System.out.println("");
        }
        return finalres;
    }
}
