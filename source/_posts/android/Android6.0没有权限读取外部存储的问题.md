---
date: 2016-12-23 11:35
status: public
title: Android 权限管理
tags:
  - Android
---

摘要:Android6.0没有权限读取外部存储的问题
<!--more-->
最近在做一个电子商城APP，本人负责Android客户端，在项目尾期遇到一个问题，经测试发现，Android6.0以上的手机都没有权限直接读取外部存储，即使在AndroidManifest.xml加上
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

也没有任何效果，最后查询国外论坛发现6.0以后很多权限都需要主动请求后才能使用，以下为解决方案：
		public class AuthorityUtil {
	    public static int ExternalRW_PERMISSIONS_CODE = 124;
	    //读取手机存储的权限
	    public static boolean isGrantExternalRW(Activity activity) {
	        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
	            if (PackageManager.PERMISSION_GRANTED == ContextCompat.checkSelfPermission(activity, android.Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
	                return true;
	            } else {
	                Toast.makeText(activity, "请到手机设置允许读取手机存储权限，不然会更新失败哦~", Toast.LENGTH_LONG).show();
	                String[] mPermissionList = new String[]{
	                        Manifest.permission.READ_EXTERNAL_STORAGE,
	                        Manifest.permission.WRITE_EXTERNAL_STORAGE};
	                ActivityCompat.requestPermissions(activity, mPermissionList, ExternalRW_PERMISSIONS_CODE);
	                return false;
	            }
	        }
	        return true;
	    }
		}
调用
		if(AuthorityUtil.isGrantExternalRW(MainActivity.this)) {
		}
回调
		@Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        if (requestCode == AuthorityUtil.ExternalRW_PERMISSIONS_CODE) {
            for (int i = 0; i < permissions.length; i++) {
                String permission = permissions[i];
                int grantResult = grantResults[i];
                if (permission.equals(Manifest.permission.READ_EXTERNAL_STORAGE)) {
                    if (grantResult == PackageManager.PERMISSION_GRANTED) {
                        //授权成功后的逻辑
                        if (!BaseTools.isServiceWork(getApplicationContext(), "com.sun0769.wirelessdongguan.service.UpdataService")) {
                            Log.e("info", "启动服务！！");
                            Intent intent = new Intent(MainActivity.this, UpdataService.class);
                            startService(intent);
                        } else {
                            Log.e("info", "服务已经启动了！！");
                        }
                    }
                }
            }
        }
    }