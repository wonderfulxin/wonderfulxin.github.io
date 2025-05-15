---
date: 2017-7-13 11:35
status: public
title: 安卓嵌套listview gridview
tags:
  - 安卓
---

摘要:经常在做安卓的时候会有这样的需求，listview嵌套gridview来使用
<!--more-->

经常在做安卓的时候会有这样的需求，listview嵌套gridview来使用
但是实际试过之后就会发现这样做有一个问题，无论ListView的高度怎么设置，
都会只显示一行的高度，那是由于ListView的父容器测量模式为UNSPECIFIED的时候，gridviewView的高度默认为一个item的高度，这样我们就重写ListView的onMeasure方法，来自定义高度：

	public class FixedGridView extends GridView {
	public boolean onMeasure = false;//用于判断的布尔值，只有完成计算，即onLayout后才为false,在onMeasure时为true
	public FixedGridView(Context context)
	{
		super(context);
	}

	public FixedGridView(Context context, AttributeSet attrs)
	{
		super(context, attrs);
	}
	
	public FixedGridView(Context context, AttributeSet attrs, int defStyle)
	{
		super(context, attrs, defStyle);
	}
	//无论ListView的高度怎么设置，都会只显示一行的高度，那是由于ListView对于父容器测量模式为UNSPECIFIED时
	// ListView的父容器测量模式为UNSPECIFIED的时候，ListView的高度默认为一个item的高度
	//在measure里面重新计算一下高度 动态设置一下高度就可以了
	@Override       
    public void onMeasure(int widthMeasureSpec, int heightMeasureSpec)
	{       
		onMeasure = true;
        int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2, MeasureSpec.AT_MOST);       
        super.onMeasure(widthMeasureSpec, expandSpec + 4);       
    }  
	@Override
	protected void onLayout(boolean changed, int l, int t, int r, int b) {
		onMeasure = false;
		super.onLayout(changed, l, t, r, b);
	}
	好了 大功告成，这样子嵌套在listview就不会说只是显示一个item的高度啦 