# retrofit
package com.example.retrofit;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.List;

import retrofit2.Call;
import retrofit2.Callback;
import retrofit2.Response;
import retrofit2.Retrofit;
import retrofit2.converter.gson.GsonConverterFactory;

public class MainActivity extends AppCompatActivity {
    List<Zrobienia>zrobione;
    TextView textViewZrobienia;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textViewZrobienia = findViewById(R.id.textViewDoZrealizowania);
        EditText editText = findViewById(R.id.editTextTextPersonName);
        Button button = findViewById(R.id.button);
        ListView listView = findViewById(R.id.listView);




        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://my-json-server.typicode.com/matii26/Proba/")
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        JsonPlaceHolderApi jsonPlaceHolderApi = retrofit.create(JsonPlaceHolderApi.class);
        Call<List<Zrobienia>> call = jsonPlaceHolderApi.getZrobienia();
        call.enqueue(new Callback<List<Zrobienia>>() {
            @Override
            public void onResponse(Call<List<Zrobienia>> call, Response<List<Zrobienia>> response) {
                zrobione = response.body();
                String raz = zrobione.get(0).getNazwaRzeczy();
                String dwa = zrobione.get(1).getNazwaRzeczy();
                String trzy = zrobione.get(2).getNazwaRzeczy();
                String cztery = zrobione.get(3).getNazwaRzeczy();
                ArrayList<String>todoList = new ArrayList<>();
                todoList.add(raz);
                todoList.add(dwa);
                todoList.add(trzy);
                todoList.add(cztery);


                button.setOnClickListener(
                        new View.OnClickListener() {
                            @Override
                            public void onClick(View view) {
                                String wartosc = editText.getText().toString();
                                int FinalnaWartosc = Integer.parseInt(wartosc);
                                if (FinalnaWartosc == 2){
                                   ArrayList new_list = new ArrayList<>(todoList.subList(0,2));
                                    ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this,
                                    android.R.layout.simple_list_item_1,new_list);
                                    listView.setAdapter(adapter);
                                }
                                if (FinalnaWartosc == 1){
                                    ArrayList new_list = new ArrayList<>(todoList.subList(0,3));
                                    ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this,
                                            android.R.layout.simple_list_item_1,new_list);
                                    listView.setAdapter(adapter);
                                }
                                if (FinalnaWartosc == 0){
                                    ArrayList new_list = new ArrayList<>(todoList.subList(0,4));
                                    ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this,
                                            android.R.layout.simple_list_item_1,new_list);
                                    listView.setAdapter(adapter);
                                }

                                //listView.setAdapter(adapter);
                            }
                        }
                );

            }

            @Override
            public void onFailure(Call<List<Zrobienia>> call, Throwable t) {

            }
        });


    }
}

#Zrobienia.java
package com.example.retrofit;

import  com.google.gson.annotations.SerializedName;

public class Zrobienia {
    private String nazwaRzeczy;
    private int priorytet;
    private String data;
    private String zrealizowane;

    public String getNazwaRzeczy() {
        return nazwaRzeczy;
    }

    public int getPriorytet() {
        return priorytet;
    }

    public String getData() {
        return data;
    }

    public String getZrealizowane() {
        return zrealizowane;
    }

    @Override
    public String toString() {
        return "Zrobienia{" +
                "nazwaRzeczy='" + nazwaRzeczy + '\'' +
                ", priorytet=" + priorytet +
                ", data='" + data + '\'' +
                ", zrealizowane='" + zrealizowane + '\'' +
                '}';
    }

    public void setNazwaRzeczy(String nazwaRzeczy) {
        this.nazwaRzeczy = nazwaRzeczy;
    }

    public void setPriorytet(int priorytet) {
        this.priorytet = priorytet;
    }

    public void setData(String data) {
        this.data = data;
    }

    public void setZrealizowane(String zrealizowane) {
        this.zrealizowane = zrealizowane;
    }
}

#JsonPlaceHolderApi.java
package com.example.retrofit;

import java.util.List;

import retrofit2.Call;
import retrofit2.http.GET;

public interface JsonPlaceHolderApi {

    @GET("DoZrobienia")
    public Call<List<Zrobienia>> getZrobienia();
}
