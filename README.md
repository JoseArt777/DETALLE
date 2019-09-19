# DETALLE
package com.example.contactos;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

import android.Manifest;
import android.app.Activity;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

public class Detalle extends AppCompatActivity {

    private TextView tvNombre;
    private TextView tvTelefono;
    private TextView tvemail;
    private TextView tvdescripcion;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detalle);

        Bundle parametros = getIntent().getExtras();
        String nombre = parametros.getString(getResources().getString(R.string.pnombre));
        String telefono = parametros.getString(getResources().getString(R.string.ptelefono));
        String email = parametros.getString(getResources().getString(R.string.pemail));
        String descripcion = parametros.getString(getResources().getString(R.string.pdescripcion));

        tvNombre = (TextView) findViewById(R.id.tvnombre);
        tvTelefono = (TextView) findViewById(R.id.tvtelefono);
        tvemail = (TextView) findViewById(R.id.tvemail);
        tvdescripcion = (TextView) findViewById(R.id.tvdescripcion);

        tvNombre.setText(nombre);
        tvTelefono.setText(telefono);
        tvemail.setText(email);
        tvdescripcion.setText(descripcion);

    }

    public void llamar(View v) {
        String telefono = tvTelefono.getText().toString();


        if (ActivityCompat.checkSelfPermission (this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    Activity#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for Activity#requestPermissions for more details.
            return;
        }
        startActivity(new Intent(Intent.ACTION_CALL, Uri.parse("tel:" + telefono)));
    }
    public void enviaremail(View v){

        String name=tvemail.getText().toString() ;
        Intent emailIntent=new Intent((Intent.ACTION_SEND ));
        emailIntent .setData(Uri.parse("mailto:") );
        emailIntent .putExtra(Intent.EXTRA_EMAIL,emailIntent);
        emailIntent.setType("message/rfc822");
        startActivity(Intent.createChooser(emailIntent,"Email") ) ;
    }
}





package com.example.contactos;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import androidx.appcompat.app.AppCompatActivity;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    ArrayList <contacto > contactos ;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        contactos= new ArrayList<contacto>();

        contactos.add(new contacto("Alex Lopez", "57388652", "lopez2@gmail.com", "Bachiller" ) ) ;
        contactos.add(new contacto("Mayda Chavez", "35672312", "chavezmayda@gmail.com", "Perito Contador" ) ) ;
        contactos.add(new contacto("Brenda Rivera", "35698741", "brenda@gmail.com", "Secretaria" ) ) ;
        contactos.add(new contacto("Enyelber Zamora", "34737803", "Zamora456@gmail.com", "amigo" ) ) ;
        contactos.add(new contacto("Anderson Cuellar", "62315482", "cuellar5789@gmail.com", "Perito Contador" ) ) ;
        contactos.add(new contacto("Cristian Urias", "78521643", "urias@gmail.com", "Bachiller" ) ) ;


        ArrayList <String>  nombresContacto=new ArrayList<>() ;
        for (contacto contacto: contactos){
                nombresContacto.add(contacto.getNombre());


        }

        ListView lstcontactos=(ListView ) findViewById(R.id.lstcontactos);
        lstcontactos .setAdapter(new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, nombresContacto   ) ) ;

    lstcontactos.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            Intent intent=new Intent(MainActivity.this, Detalle.class) ;
            intent.putExtra(getResources().getString(R.string.pnombre),contactos.get(position).getNombre() );
            intent.putExtra(getResources().getString(R.string.ptelefono),contactos.get(position).getTelefono() );
            intent.putExtra(getResources().getString(R.string.pemail),contactos.get(position).getEmail() );
            intent.putExtra(getResources().getString(R.string.pdescripcion),contactos.get(position).getDescripcion() );
            startActivity(intent) ;





        }
    });


    }
}


package com.example.contactos;

public class contacto {

    private String nombre;
    private String telefono;
    private String email;
    private String Descripcion;
    private int foto;
    

    public contacto(String nombre, String telefono, String email, String descripcion  ) {
        this.nombre=nombre;
        this.telefono=telefono;
        this.email =email;
        this.Descripcion = descripcion;

    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getDescripcion() {
        return Descripcion;
    }

    public void setDescripcion(String descripcion) {
        Descripcion = descripcion;
    }

    public String getTelefono() {
        return telefono;
    }

    public void setTelefono(String telefono) {
        this.telefono = telefono;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }
}
