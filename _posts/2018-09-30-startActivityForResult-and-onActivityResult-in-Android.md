---
layout: post
title: "startActivityForResult() and onActivityResult() in Android"
author: "Eddy Hu"
categories: journal
tags: [android,java]
image: android.jpg
---


**First Activity**


    import android.app.Activity;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.view.View.OnClickListener;
    import android.widget.Button;
    import android.widget.EditText;
    
    public class MainActivity extends Activity {
    
        private Button button;
        private final static int REQUESTCODE = 1; 
        private EditText one, two, result;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            one = (EditText) findViewById(R.id.Text_one);
            two = (EditText) findViewById(R.id.Text_two);
            result = (EditText) findViewById(R.id.Text_result);
            button = (Button) findViewById(R.id.button);
            button.setOnClickListener(new OnClickListener() {
                @Override
                public void onClick(View v) {
                    // TODO Auto-generated method stub
                    // get input from user
                    int a = Integer.parseInt(one.getText().toString());
                    int b = Integer.parseInt(two.getText().toString());
    
                    Intent intent = new Intent(MainActivity.this,
                            OtherActivity.class);
                    intent.putExtra("a", a);
                    intent.putExtra("b", b);
                    
    
                    // startActivity(intent);this method cannot return data
                    startActivityForResult(intent, REQUESTCODE); //REQUESTCODE--->1
                }
            });
        }
    
        // get the return data
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            // RESULT_OK，Standard activity result:
            // operation succeeded. defaul value is -1
            if (resultCode == 2) {
                if (requestCode == REQUESTCODE) {
                    int three = data.getIntExtra("three", 0);
                    result.setText(String.valueOf(three));
                }
            }
        }
    
    }


**First Layout**

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity" >
    
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal" >
    
            <EditText
                android:id="@+id/Text_one"
                android:layout_width="80dp"
                android:layout_height="wrap_content" />
    
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="  +  " />
    
            <EditText
                android:id="@+id/Text_two"
                android:layout_width="80dp"
                android:layout_height="wrap_content" />
    
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="  =  " />
    
            <EditText
                android:id="@+id/Text_result"
                android:layout_width="80dp"
                android:layout_height="wrap_content" />
        </LinearLayout>
    
        <Button
            android:id="@+id/button"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Result" />
    
    </LinearLayout>

**Second Activity**

    import android.app.Activity;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.view.View.OnClickListener;
    import android.widget.Button;
    import android.widget.EditText;
    import android.widget.TextView;
    
    public class OtherActivity extends Activity {
    
        private Button button;
        private TextView textView;
        private EditText editText;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            // TODO Auto-generated method stub
            super.onCreate(savedInstanceState);
            setContentView(R.layout.other);
            button = (Button) findViewById(R.id.button2);
            textView = (TextView) findViewById(R.id.msg);
            editText = (EditText) findViewById(R.id.Text_three);
            // get the data
            Intent intent = getIntent();//Return the intent that started this activity. 
            int a = intent.getIntExtra("a", 0); //default is 0
            int b = intent.getIntExtra("b", 0); //default is 0
            textView.setText(a + " + " + b + " = " + " ? ");
    
            button.setOnClickListener(new OnClickListener() {
                @Override
                public void onClick(View v) {
                    // TODO Auto-generated method stub
                    Intent intent = new Intent();
                    // get the result
                    int three = Integer.parseInt(editText.getText().toString());
                    intent.putExtra("three", three); //return the data to previous activity
                    //setResult(resultCode, data);
                    setResult(2, intent);
                    
                    finish(); //finish this activity
                }
            });
        }
    }

**Second layout**

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical" >
    
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical" >
    
            <TextView
                android:id="@+id/msg"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />
    
            <EditText
                android:id="@+id/Text_three"
                android:layout_width="match_parent"
                android:layout_height="wrap_content" />
        </LinearLayout>
    
        <Button
            android:id="@+id/button2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="return result" />
    
    </LinearLayout>
