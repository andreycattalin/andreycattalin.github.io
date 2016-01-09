---
published: true
title: Android Tabs – Con Fragments y ViewPager
layout: post
tags: [Android, Tabs, Fragments, ViewPager, TabLayout, FragmentPagerAdapter]
categories: [Android]
---
Este código es para crear Tabs en Android, utilizando Fragments y el ViewPager.

{% highlight java %}
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);

        // Get the ViewPager and set it's PagerAdapter so that it can display items
        ViewPager viewPager = (ViewPager) findViewById(R.id.viewpager);
        viewPager.setAdapter(new Tabs_Bar(this.getSupportFragmentManager(), this));

        // Give the TabLayout the ViewPager
        TabLayout tabLayout = (TabLayout) findViewById(R.id.sliding_tabs);
        tabLayout.setupWithViewPager(viewPager);

    }
}
{% endhighlight %}