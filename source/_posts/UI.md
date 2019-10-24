title: UI
date: 2016-09-26
tags:
- android
 
---
<font style="font-family:微软雅黑">
## TextView
* android:gravity
> top bottom left right等等，指定对齐方向

* match\_parent
* fill\_parent 
* wrap_content

## EditText
maxLine
## ImageView
                imageView.setImageResource(R.mipmap.abc);
## visible invisible gone属性
* visible 可见
* invisible 不可见但是还存在
* gone 不可见不存在
* View setVisibilty() View.gone ...
## AlertDialog
	AlertDialog.Builder builder = new AlertDialog.Builder(UIActivity.this);
                builder.setTitle("AlertDialogTitle");
                builder.setCancelable(false);
                builder.setMessage("some message");
                builder.setPositiveButton("Yes", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {

                    }
                });
                builder.setNegativeButton("No", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {

                    }
                });
                builder.show();
## LinearLayout
* 指定方向，要注意如果是水平方向的话，则内部控件的宽度不能够是match_parent的不然会充满整个布局，同理竖直方向也是类似的。
* android:layout_gravity属性：正如TextView中gravity表示文本内容占据整个宽度的位置，这个属性指的是控件在LinearLayout中的相对位置，而且，当外层layout是竖直布局时在其上的关于竖直的设置是不会生效的。注意，这个属性亦是属于子布局里面的而不是属于LinearLayout（要指定的是每一个不同的子布局）。
* android:layout\_weight属性：系统会先把 LinearLayout 下所有控件指定的 layout\_weight 值相加， 得到一个总值，然后每个控件所占大小的比例就是用该控件的 layout_weight 值除以刚才算出的总值。
* 布局中的如果只有部分控件有weight属性，则有weight属性的控件共享剩下的宽度或者高度（按照比例分配）。

## RelativeLayout 相对布局

## dp和sp
在编写 Android 程序的时候，尽量将控件或布局的大小指定成 match_parent
或 wrap_content，如果必须要指定一个固定值，则使用 dp 来作为单位，指定文字大小的时候
使用 sp 作为单位。