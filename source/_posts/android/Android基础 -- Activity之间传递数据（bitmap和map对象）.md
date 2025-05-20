---
title: 安卓传递数据
date: 2016-012-23 22:37:23
categories:
  - 安卓
tags:
  - 其他
---

摘要:安卓传递数据
<!--more-->
正文:面试的时候遇到这个，记下来以后可以用，之前没有用过

第一个：传递bitmap

  这个问题非常奇葩（可能我Android水平还不够），居然不会报错，我是直接用bundle或Intent的extral域直接存放bitmap，结果运行时各种宕机，各种界面乱窜（我非常的纳闷）。。。搜索之后看大家都说不能直接传递大于40k的图片，然后在德问论坛上找到了解法。就是把bitmap存储为byte数组，然后再通过Intent传递。

的 

 代码如下所示：

Bitmap bmp=((BitmapDrawable)order_con_pic.getDrawable()).getBitmap();  
Intent intent=new Intent(OrderConfirm.this,ShowWebImageActivity.class);  
ByteArrayOutputStream baos=new ByteArrayOutputStream();  
bmp.compress(Bitmap.CompressFormat.PNG, 100, baos);  
byte [] bitmapByte =baos.toByteArray();  
intent.putExtra("bitmap", bitmapByte);  
startActivity(intent);  

其中 第一行代码就是如何从一个imageview中获得其图片，这个问题也倒腾了下，貌似用setDrawingCacheEnabled也行，因为开始用的这个方法，但是直接在activity之间传递bitmap，所以导致运行时错误，后来改正之后没有再尝试。
先new一个ByteArrayOutputStream流，然后使用Bitmap中的compress方法，把数据压缩到一个byte中，传输就可以了。

在另一个activity中取出来的方法是：

imageView = (ZoomableImageView) findViewById(R.id.show_webimage_imageview);  
        Intent intent=getIntent();  
        if(intent !=null)  
        {  
            byte [] bis=intent.getByteArrayExtra("bitmap");  
            Bitmap bitmap=BitmapFactory.decodeByteArray(bis, 0, bis.length);  
            imageView.setImageBitmap(bitmap);  
        }  
取出来字节数组之后，用BitmapFactory中的decodeByteArray方法组合成一个bitmap就可以了。
再加上一个存储的代码：

public void saveMyBitmap(String bitName,Bitmap mBitmap) throws IOException {  
        File f = new File("/sdcard/Note/" + bitName);  
        if(!f.exists())  
            f.mkdirs();//如果没有这个文件夹的话，会报file not found错误  
        f=new File("/sdcard/Note/"+bitName+".png");  
        f.createNewFile();  
        try {  
            FileOutputStream out = new FileOutputStream(f);  
             mBitmap.compress(Bitmap.CompressFormat.PNG, 100, out);  
             out.flush();  
             out.close();  
        } catch (FileNotFoundException e) {  
                Log.i(TAG,e.toString());  
        }  
         
    }  


2.传递map对象：

封装到bundle中：

Map<String,Object> data=orderlist.get(arg2-1);  
            SerializableMap tmpmap=new SerializableMap();  
            tmpmap.setMap(data);  
            bundle.putSerializable("orderinfo", tmpmap);  
            intent.putExtras(bundle);  

这个SeralizableMap是自己封装的一个实现了Serializable接口的类：
public class SerializableMap implements Serializable {  
    private Map<String,Object> map;  
    public Map<String,Object> getMap()  
    {  
        return map;  
    }  
    public void setMap(Map<String,Object> map)  
    {  
        this.map=map;  
    }  
}  
这样才能把map对象扔到bundle中去，
取出来的方法是：

Bundle bundle = getIntent().getExtras();  
        SerializableMap serializableMap = (SerializableMap) bundle  
                .get("orderinfo");  