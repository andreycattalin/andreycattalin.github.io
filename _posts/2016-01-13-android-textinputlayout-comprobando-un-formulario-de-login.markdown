---
published: true
title: Android TextInputLayout - Comprobando un formulario de login
layout: post
tags: [TextInputLayout]
categories: [Android]
---
Cuando creamos un formulario en android, el usuario puede cometer errores o dejarlo en blanco, para eso tenemos que indicarle que necesitamos esos datos obligatorios y que no ha completado.

Para mostrar un ejemplo, haremos un login, funcional a nivel de vista.

Primero creamos un layout "activity_login.xml":

{% highlight xml %}
<LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="?attr/actionBarSize"
        android:orientation="vertical"
        android:paddingLeft="20dp"
        android:paddingRight="20dp"
        android:paddingTop="60dp">

        <android.support.design.widget.TextInputLayout
            android:id="@+id/input_layout_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp">

            <EditText
                android:id="@+id/in_nombre"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Nombre"
                android:singleLine="true" />
        </android.support.design.widget.TextInputLayout>

        <android.support.design.widget.TextInputLayout
            android:id="@+id/input_layout_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <EditText
                android:id="@+id/in_password"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Password"
                android:inputType="textPassword" />
        </android.support.design.widget.TextInputLayout>

        <CheckBox
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="Recordarme"
            android:id="@+id/check_recordarme"
            android:layout_marginTop="15dp"
            android:checked="false" />

        <Button
            android:id="@+id/btn_entrar"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:background="@drawable/btn0"
            android:layout_marginTop="40dp"
            android:text="Entrar"
            android:textColor="@android:color/white" />
  </LinearLayout>
{% endhighlight %}

A continuación se hace lo siguiente para realizar la comprobación, el codigo es simple y está comentado.

{% highlight java %}
public class LoginActivity extends AppCompatActivity {

    private String PREFS_NAME = "AppOnFire";
    private static final String Name = "nameKey";
    private static final String Email = "passKey";

    private Button btn_entrar;
    private TextView in_nombre, in_password;
    private TextInputLayout inputLayoutName, inputLayoutPassword;
    private CheckBox check_recordarme;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        init_Login();
    }

    /***
     * Iinicializa las vistas y comprueba el login
     */
    public void init_Login() {
        inputLayoutName = (TextInputLayout) findViewById(R.id.input_layout_name);
        inputLayoutPassword = (TextInputLayout) findViewById(R.id.input_layout_password);

        in_nombre = (TextView) findViewById(R.id.in_nombre);
        in_password = (TextView) findViewById(R.id.in_password);

        in_nombre.addTextChangedListener(new checkLogin(in_nombre));
        in_password.addTextChangedListener(new checkLogin(in_password));

        check_recordarme = (CheckBox) findViewById(R.id.check_recordarme);

        btn_entrar = (Button) findViewById(R.id.btn_entrar);
        btn_entrar.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                doLogin(); // metodo para hacer el login
            }
        });
    }

    /**
     * Validar Loginm comprueba datos y actua según el checkbox
     */
    private void doLogin() {
        if (!validateName()) {
            return;
        } else if (!validatePassword()) {
            return;
        } else {
            if (check_recordarme.isChecked()) {
                Toast.makeText(getApplicationContext(), "Has entrado en modo recuerdame.", Toast.LENGTH_SHORT).show();

                final SharedPreferences settings = getSharedPreferences(PREFS_NAME, 0);
                SharedPreferences.Editor editor = settings.edit();

                String name = in_nombre.getText().toString();
                String pass = in_password.getText().toString();

                editor.putString(Name, name);
                editor.putString(Email, pass);
                editor.commit();
            } else {
                Log.v("Login", "Entrado");
                Toast.makeText(getApplicationContext(), "Has entrado en modo olvidame.", Toast.LENGTH_SHORT).show();
            }
        }
    }

    /**
     * Solicita el foco en la vista
     *
     * @param view
     */
    private void requestFocus(View view) {
        if (view.requestFocus()) {
            getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_VISIBLE);
        }
    }

    /**
     * Valida el nombre
     *
     * @return si es valido o no
     */
    private boolean validateName() {
        if (in_nombre.getText().toString().trim().isEmpty()) {
            inputLayoutName.setError(getString(R.string.err_msg_name));
            requestFocus(in_nombre);
            return false;
        } else if(in_nombre.getText().toString().length() < 3){
            inputLayoutName.setError(getString(R.string.err_msg_name2));
            requestFocus(in_nombre);
            return false;
        } else {
            inputLayoutName.setErrorEnabled(false);
        }

        return true;
    }

    /**
     * Valida el password
     * @return si es valido o no
     */
    private boolean validatePassword() {
        if (in_password.getText().toString().trim().isEmpty()) {
            inputLayoutPassword.setError(getString(R.string.err_msg_password));
            requestFocus(in_password);
            return false;
        } else {
            inputLayoutPassword.setErrorEnabled(false);
        }

        return true;
    }

    private class checkLogin implements TextWatcher {
        private View view;

        private checkLogin(View view) {
            this.view = view;
        }

        public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {
        }

        public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
        }

        public void afterTextChanged(Editable editable) {
            switch (view.getId()) {
                case R.id.in_nombre:
                    validateName();
                    break;
                case R.id.in_password:
                    validatePassword();
                    break;
            }
        }
    }
}
{% endhighlight %}

Y así nos quedaría:

![](http://i.imgur.com/9MaFp9T.png)