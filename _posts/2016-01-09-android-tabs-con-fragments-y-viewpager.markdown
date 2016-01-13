---
published: true
title: Android Tabs – Con Fragments y ViewPager
layout: post
tags: [Android, Tabs, Fragments, ViewPager, TabLayout, FragmentPagerAdapter]
categories: [Android]
---
Este código es para crear Tabs en Android, utilizando Fragments y ViewPager. Antes de empezar hay que configurar el build.gradle de Android Studio como se indica en la página de <a href="https://goo.gl/C6a8EA">CodePath</a>.

Primero de todo, creamos en el xml principal <b>"activity_main.xml"</b> el TabLayout que serán las pestañas y el ViewPager que nos servirá para mostrar otros layouts según cambiemos de pestaña.

{% highlight xml %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/colorPrimaryDark"
        android:layout_weight="1"
        android:orientation="vertical"
        android:gravity="center">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Instant"
            android:id="@+id/textView"
            android:textColor="#FFF"
            android:textSize="90sp"
            android:layout_gravity="center" />
    </LinearLayout>

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="250dp">

        <android.support.design.widget.TabLayout
            android:id="@+id/sliding_tabs"
            style="@style/StartTabLayout"
            app:tabTextColor="@color/whiteTransparent"
            app:tabSelectedTextColor="#FFF"
            android:background="@color/colorPrimaryDark"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabIndicatorColor="@color/colorSand"
            app:tabGravity="fill"
            app:tabMode="fixed" />

        <android.support.v4.view.ViewPager
            android:id="@+id/viewpager"
            android:layout_width="match_parent"
            android:layout_height="0px"
            android:layout_weight="1"
            android:background="@color/colorWhite" />

    </LinearLayout>

</LinearLayout>
{% endhighlight %}

Después en la clase principal <b>"MainActivity.java"</b> referenciamos el TabLayout y el ViewPager.

{% highlight java %}
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);

        // Obtiene el ViewPager y establece su PagerAdapter para que pueda mostrar los elementos
        ViewPager viewPager = (ViewPager) findViewById(R.id.viewpager);
        viewPager.setAdapter(new Tabs_Bar(this.getSupportFragmentManager()));

        // Pasa el TabLayout al ViewPager
        TabLayout tabLayout = (TabLayout) findViewById(R.id.sliding_tabs);
        tabLayout.setupWithViewPager(viewPager);

    }
}
{% endhighlight %}

Ahora creamos el contenido de cada layout a mostrar con el ViewPager. Voy a poner uno de ejemplo, pero como haremos 2 pestañas este paso habrá que duplicarlo. Mi ejemplo será <b>"tab_empezar.xml"</b> y el código dentro de un LinearLayout es el siguiente:


{% highlight xml %}
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:orientation="vertical"
        android:paddingLeft="30dp"
        android:paddingRight="30dp">

        <Button
            android:id="@+id/btn_sesion"
            android:layout_width="fill_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:layout_marginBottom="10dp"
            android:background="@drawable/start_btn_main"
            android:paddingBottom="19dp"
            android:paddingTop="19dp"
            android:text="@string/iniciar_sesion"
            android:textAllCaps="false"
            android:textSize="20sp" />

        <Button
            android:id="@+id/btn_resetPassword"
            style="?android:attr/borderlessButtonStyle"
            android:layout_width="fill_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:background="@drawable/start_btn_grey"
            android:paddingBottom="15dp"
            android:paddingTop="15dp"
            android:text="@string/recuperar_contrasena"
            android:textAllCaps="false"
            android:textColor="@android:color/white"
            android:textSize="14sp" />
</LinearLayout>
{% endhighlight %}

Y luego creamos <b>"TabEmpezar.java"</b>, heredando de fragment e implementando el evento OnClickListener. Como he dicho antes, esto también se haría dos veces, una vez por cada pestaña que creamos.

{% highlight java %}
public class TabEmpezar extends Fragment implements View.OnClickListener {
    private Button btn_sesion, btn_resetPassword;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {

        View view = inflater.inflate(R.layout.tab_empezar, container, false);

        btn_sesion = (Button) view.findViewById(R.id.btn_sesion);
        btn_resetPassword = (Button) view.findViewById(R.id.btn_resetPassword);

        btn_sesion.setOnClickListener(this);
        btn_resetPassword.setOnClickListener(this);

        return view;
    }

    @Override
    public void onClick(View v) {
        if (v.getId() == R.id.btn_sesion) {
            Intent intent = new Intent(getActivity(), InvitacionSolicitar.class);
            startActivity(intent);
        } else if (v.getId() == R.id.btn_resetPassword) {
            // Abrir otro Activity
        }
    }
}
{% endhighlight %}

Nos queda el último paso, darle nombre a las pestañas y según la pestaña nos muestre un layout u otro. Herendando de FragmentPagerAdapter creamos <b>"Tabs_Bar.java"</b> completamos el paso que hicimos al principio en el MainActivity.java y escribimos esto:

{% highlight java %}
public class Tabs_Bar extends FragmentPagerAdapter {

    private String tabTitles[] = {"Invitación", "Empezar"};

    public Tabs_Bar(FragmentManager fm) {
        super(fm);
    }

    @Override
    public int getCount() {
        return tabTitles.length;
    }

    @Override
    public Fragment getItem(int position) {
        switch (position) {
            case 0:
                TabInvitacion tab1 = new TabInvitacion();
                return tab1;
            case 1:
                TabEmpezar tab2 = new TabEmpezar();
                return tab2;
            default:
                return new Fragment();
        }
    }

    @Override
    public CharSequence getPageTitle(int position) {
        // Genera el titulo basado en la posicion
        return tabTitles[position];
    }
}
{% endhighlight %}

Ya está. Con esto terminaríamos, y quedaría de esta manera:

![](https://i.imgsafe.org/4360afe.png)