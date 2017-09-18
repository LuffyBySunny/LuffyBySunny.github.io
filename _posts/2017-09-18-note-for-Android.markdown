---
layout:		post
title:		"RecycklerView基本用法"
subtitle:		"自定义Adapter的用法"
date:		2017-09-18 12:00:00
author:		"Droodsunny"
header-img: "img/in-post/Android.jpg"
---

> 最近参考着资料做了几个RecyclerView的项目实例。感觉照着书本敲代码没有什么收获 
> 
> 思路跟不上作者，对代码的结构还不是很了解，根本就不懂一些方法的重写到底有什么用。
> 
> 刚开始接触 Adapter的时候感觉很神奇，自定义Adapter可以让List的加载更加多样化。
><br/>
>
>ViewHolder的使用提高了ListView和RecyclerView的加载效率。
## 准备工作 
RecyclerView定义在了support库中，所以要在app.gradle中添加相应的依赖库，在dependencies添加
> compile 'com.android.support:recyclerview-v7:25.3.1'


## 自定View的布局
> 首先新建一个xml文件，作为加载的布局，例如新建fruits_item.xml  
> 包括两个控件，一个是图片，一个是文本
> 
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
	<ImageView
    android:id="@+id/fruit_image"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
    <TextView
      android:id="@+id/fruit_name"
      android:layout_marginLeft="10dp"
      android:layout_gravity="center_vertical"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content" />
	</LinearLayout>

><br>

**然后为RecyclerView准备一个适配器**,新建FruitAdapter类，继承RecyclerView.Adapter,将泛型指定为FruitAdapter.ViewHolder。ViewHolder为FruitAdapter的内部类

**ViewHolder类的构造**  

	static class ViewHolider extends RecyclerView.ViewHolder{
        View fruitview;
         ImageView fruitimage;
        TextView fruitname;
        public ViewHolider(View itemView) {
            super(itemView);
            fruitview=itemView;
            fruitimage=(ImageView)itemView.findViewById(R.id.fruit_image);
            fruitname=(TextView)itemView.findViewById(R.id.fruit_name);

        }
    }
ViewHolder的构造函数中要传入一个View参数，通常是RecyclerView子项的最外层布局，然后可以通过findviewById()方法获得布局中的控件，因此可以避免重复加载控件

> FruitAdapter的构造函数，传入一个List,指定泛型为Fruit,参数为获取的要显示的对象的List实例
>
    public FruitAdapter(List<Fruit> fruitList){
        mFruitList=fruitList;
    }

>重写onCreatViewHolder方法，用于创建ViewHolder实例，在这个方法中加载自定义布局，然后创建ViewHolder实例，并把加载出来的布局传入到构造函数,而且可以在这设置控件的监听事件,最后将ViewHolder的实例返回
>
>
	   @Override
>      public FruitAdapter.ViewHolider onCreateViewHolder(ViewGroup parent, int viewType) {
        View view= LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item,parent,false);
        final ViewHolider holider=new ViewHolider(view);
        holider.fruitview.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int position = holider.getAdapterPosition();
                Fruit fruit=mFruitList.get(position);
                Toast.makeText(v.getContext(),"You clicked view"+fruit.getName(),Toast.LENGTH_SHORT).show();
            }
        });
        holider.fruitimage.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int position=holider.getAdapterPosition();
                Fruit fruit=mFruitList.get(position);
                Toast.makeText(v.getContext(),"You clicked image"+fruit.getName(),Toast.LENGTH_SHORT).show();
            }
        });
        return holider;
    }

><br/>


重写onBindViewHolder方法，对RecyclerView的子项进行赋值，会在每个子项滚动到屏幕内部时执行,通过position参数得到当前项的实例，然后在ViewHolder的控件里设置数据。
> 
>
    @Override
    public void onBindViewHolder(FruitAdapter.ViewHolider holder, int position) {
    Fruit fruit=mFruitList.get(position);
        holder.fruitimage.setImageResource(fruit.getImageid());
        holder.fruitname.setText(fruit.getName());
    }

getItemCount 用于告诉RecyclerView有多少个子项，直接返回数据源的长度
> 
> 
    @Override
    public int getItemCount() {
        return mFruitList.size();
    }


# 运行结果如图
![](https://github.com/LuffyBySunny/LuffyBySunny.github.io/blob/master/img/in-post/result.png)



		






