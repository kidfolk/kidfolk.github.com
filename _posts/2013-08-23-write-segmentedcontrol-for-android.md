---
layout: post
title: "Write A UISegmentedControl For Android"
categories: Android

---

当我在看Stanford cs193p(iOS开发课程)的时候，作为一名Android工程师，我会情不自禁地将iOS和Android拿来比较。随着iOS学习的不断深入，我想我一定会写一些有关两者对比的文章。iOS里有一个原生的控件叫做UISegmentedControl，简单来说这个控件提供一组单选按钮，在很多的iOS应用里通常被用来作为内容切换的按钮。我知道Android原生的控件库没有类似的控件，但是直到最近当我在翻看api文档的时候无意中发现了下面两个东西：

- [Holo_SegmentedButton](http://developer.android.com/reference/android/R.style.html#Holo_SegmentedButton)
- [Theme_segmentedButtonStyle](http://developer.android.com/reference/android/R.styleable.html#Theme_segmentedButtonStyle)

我不想放过这个无意的发现，一个想法突然出现在我脑里，也许Android中有类似的UISegmentedControl。

于是我开始寻找Android中UISegmentedControl的蛛丝马迹。但是除了一个style和一个attr之外，再没有其他有关SegmentedControl的内容了,以下是这两个东西的内容：

{% highlight xml linenos %}
<!-- Style you can use with a container (typically a horizontal
     LinearLayout) to get a "segmented button" background and spacing. -->
<style name="SegmentedButton">
    <item name="android:background">@android:drawable/btn_default</item>
    <item name="android:divider">?android:attr/dividerVertical</item>
    <item name="android:showDividers">middle</item>
</style>

<!-- Style for segmented buttons - a container that houses several buttons
     with the appearance of a singel button broken into segments. -->
<attr name="segmentedButtonStyle" format="reference" />
{% endhighlight %}

无论如何我准备尝试一下这个SegmentedButton style,看看它能带来什么效果。于是我按照上面style的注释写了下面这些代码：

{% highlight xml linenos %}
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:paddingLeft="@dimen/activity_horizontal_margin"
                android:paddingRight="@dimen/activity_horizontal_margin"
                android:paddingTop="@dimen/activity_vertical_margin"
                android:paddingBottom="@dimen/activity_vertical_margin"
                tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        style="?android:segmentedButtonStyle">

        <Button
            android:id="@+id/segment2"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="2"/>

        <Button
            android:id="@+id/segment3"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:text="3"/>
    </LinearLayout>
</RelativeLayout>
{% endhighlight %}

上面的代码没什么特别之处，我只是将LinearLayout的style设置成segmentedButtonStyle，从style的内容来看它就是将LinearLayout的背景设置成btn_default,并且显示子控件之间的分割线。上面这一组控件的组合并不能满足类似UISegmentedControl单选的需求。Android里面的单选按钮是一组被RadioGroup包裹着的RadioButton，很明显，RadioGroup的功能与UISegmentedControl很像，并且RadioGroup有着非常清晰的定义和api。但是它的样子与UISegmentedControl相差的十万八千里。倒是segmentedButtonStyle跟UISegmentedControl有几分相似。于是我准备将segmentedButtonStyle应用于RadioGroup。

{% highlight xml linenos %}
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:paddingLeft="@dimen/activity_horizontal_margin"
                android:paddingRight="@dimen/activity_horizontal_margin"
                android:paddingTop="@dimen/activity_vertical_margin"
                android:paddingBottom="@dimen/activity_vertical_margin"
                tools:context=".MainActivity">

    <RadioGroup
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:checkedButton="@+id/segment2"
        style="?android:segmentedButtonStyle">

        <RadioButton
            android:id="@+id/segment2"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:button="@null"
            android:background="@drawable/item_background_holo_light"
            android:gravity="center"
            android:text="2"/>

        <RadioButton
            android:id="@+id/segment3"
            android:layout_width="0dp"
            android:layout_height="48dp"
            android:layout_weight="1"
            android:button="@null"
            android:background="@drawable/item_background_holo_light"
            android:gravity="center"
            android:text="3"/>
    </RadioGroup>

</RelativeLayout>
{% endhighlight %}

上面的代码我除了将segmentedButtonStyle应用于RadioGroup，还将RadioButton的样式改成传统按钮的样子，并且为它的background设置一个selector，在selector中为state_checked设置一个合适的图片，这样SegmentedControl就算完成了。

最后我可以说Android中同样有UISegmentedControl，只不过这个UISegmentedControl有点隐蔽。来一张最后的效果图：

![UISegmentedControl](http://ww4.sinaimg.cn/mw205/655257b8gw1e7x2g3jivpj20u01hcq4h)
