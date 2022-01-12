package com.bhaveshspapps.ddai;

import android.content.Intent;
import android.net.Uri;
import android.os.Build;
import android.os.Bundle;
import android.speech.RecognitionListener;
import android.speech.RecognizerIntent;
import android.speech.SpeechRecognizer;
import android.speech.tts.TextToSpeech;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.text.format.DateUtils;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Button;
import android.widget.Toast;

import java.util.Date;
import java.util.List;
import java.util.Locale;

public class MainActivity extends AppCompatActivity {
    private TextToSpeech myTTS;
    private SpeechRecognizer mySpeechRecognizer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton mic = (FloatingActionButton) findViewById(R.id.mic);
        mic.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent=new Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH);
                intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE_MODEL,RecognizerIntent.LANGUAGE_MODEL_FREE_FORM);
                intent.putExtra(RecognizerIntent.EXTRA_MAX_RESULTS,1);
             mySpeechRecognizer.startListening(intent);

            }
        });
        initializeTextToSpeech();
        initializemySpeechRecognizer();



    }
    ///////////TESTING BUTTONS/////////////////////

    public void butn2(View view){
        Intent ib=getPackageManager().getLaunchIntentForPackage("com.mxtech.videoplayer.ad");

        startActivity(ib);


    }


    ////////////////////////////////////////////////////////




    private void initializemySpeechRecognizer() {
        if(SpeechRecognizer.isRecognitionAvailable(this)){
            mySpeechRecognizer = SpeechRecognizer.createSpeechRecognizer(this);
            mySpeechRecognizer.setRecognitionListener(new RecognitionListener() {
                @Override
                public void onReadyForSpeech(Bundle bundle) {

                }

                @Override
                public void onBeginningOfSpeech() {

                }

                @Override
                public void onRmsChanged(float v) {

                }

                @Override
                public void onBufferReceived(byte[] bytes) {

                }

                @Override
                public void onEndOfSpeech() {

                }

                @Override
                public void onError(int i) {

                }

                @Override
                public void onResults(Bundle bundle) {
                    List<String> results= bundle.getStringArrayList(
                      SpeechRecognizer.RESULTS_RECOGNITION
                    );
                    processResult(results.get(0));
                }

                @Override
                public void onPartialResults(Bundle bundle) {

                }

                @Override
                public void onEvent(int i, Bundle bundle) {

                }
            });
        }
    }

    private void processResult(String command) {
        command= command.toLowerCase();
        if(command.indexOf("what") !=-1){
            if(command.indexOf("your name") !=-1)
            {
                speak("My Name is DD");
            }

           else if(command.indexOf("time") !=-1){
                Date now= new Date();
                String time = DateUtils.formatDateTime(this,now.getTime(),DateUtils.FORMAT_SHOW_TIME);
                speak("The Time is"+ time + "    Anything Else");
            }
        }
     else if(command.indexOf("Open") !=-1){
            if(command.indexOf("Browser") !=-1){
                Intent i=getPackageManager().getLaunchIntentForPackage("com.UCMobile.intl");
                Uri.parse("http://www.youtube.com");
                startActivity(i);
            }
            else if (command.indexOf("Player")!=-1){
                Intent i=getPackageManager().getLaunchIntentForPackage("com.mxtech.videoplayer.ad");

                startActivity(i);



            }

       }
        else if(command.indexOf("Call ") !=-1){
             if(command.indexOf("Papa") !=-1){
                Uri number = Uri.parse("tel:8862018624");
                Intent call = new Intent(Intent.ACTION_CALL, number);
                startActivity(call);
            }
             else if(command.indexOf("Tanu") !=-1){

                 Uri number = Uri.parse("tel:8862018624");
                 Intent call = new Intent(Intent.ACTION_CALL, number);
                 startActivity(call);

             }

        }



    }

    private void initializeTextToSpeech() {
        myTTS =new TextToSpeech(this, new TextToSpeech.OnInitListener() {
            @Override
            public void onInit(int i) {
                if(myTTS.getEngines().size()==0)
                {
                    Toast.makeText(MainActivity.this,"No tts Engine",Toast.LENGTH_LONG).show();
                    finish();


                }else{
                    myTTS.setLanguage(Locale.ENGLISH);
                    speak("Hello, This is DDAI!");

                }
            }
        });
    }

    private void speak(String message) {
        if(Build.VERSION.SDK_INT>=21)
        {
            myTTS.speak(message,TextToSpeech.QUEUE_FLUSH,null,null );
        }
        else{
            myTTS.speak(message,TextToSpeech.QUEUE_FLUSH,null);

        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }

    @Override
    protected void onPause() {
        super.onPause();
        myTTS.shutdown();
    }
}
