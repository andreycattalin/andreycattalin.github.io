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
{% endhighlight %}

Después en la clase principal: MainActivity.java

{% highlight java %}
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);

        // Obtiene el ViewPager y establece su PagerAdapter para que pueda mostrar los elementos
        ViewPager viewPager = (ViewPager) findViewById(R.id.viewpager);
        viewPager.setAdapter(new Tabs_Bar(this.getSupportFragmentManager(), this));

        // Pasa el TabLayout al ViewPager
        TabLayout tabLayout = (TabLayout) findViewById(R.id.sliding_tabs);
        tabLayout.setupWithViewPager(viewPager);

    }
}
{% endhighlight %}