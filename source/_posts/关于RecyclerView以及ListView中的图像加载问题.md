---
title: 关于RecyclerView以及ListView中的图像加载问题
date: 2016-10-07 18:55:32
tags:
- android

---
<font style="font-family:微软雅黑">
### ListView 中出现错位，重复的图片
* 原因：为了性能的提升，我们一般会采取重复使用item的方式，应用仅仅需要inflate n（屏幕所能显示的item总数）个view而非数据的size个view，这样可以极大地提高效率，如下

        public View getView(int position, View convertView, ViewGroup parent) {
            ViewHolder holder = null;
            if (convertView != null) {
                holder = (ViewHolder)convertView.getTag();
            } else {
                convertView = mInflater.inflate(R.layout.item_list_view, parent, false);
                holder = new ViewHolder();
                holder.imageView = (ImageView)convertView.findViewById(R.id.image_view_list_view);
                holder.textView = (TextView)convertView.findViewById(R.id.text_view_list_view);
                convertView.setTag(holder);
            }
		......
		}
<!--more-->
但是，随之而来的问题是：图片异步加载时可能会导致的错乱问题。当第一个图片在后台线程进行加载时（可能是网络下载），在还没有下载完成的时候，我们对listview进行滚动，这个时候的item的view是复用的，所以当图片加载完成后，会将图片设置到后面的某个item里。
  
* 解决方法：通过给imageView设置tag（将tag设置为uri），在设置图片时，进行比较，不等的设置为默认值。

            ImageView imageView = holder.imageView;
            String tag = (String) imageView.getTag();
            String url = items.get(position).getUrl();
            if (!url.equals(tag)) {
                imageView.setImageDrawable(mDefaultBitmapDrawable);
            }
            imageView.setTag(url);
            imageLoader.bindBitmap(url, imageView, 100, 100);

### RecyclerView
类似的

        public void onBindViewHolder(ViewHolder holder, int position) {
            String tag = (String) holder.imageView.getTag();
            if (!items.get(position).getUrl().equals(tag)) {

            }
            holder.imageView.setTag(items.get(position).getUrl());
            imageLoader.bindBitmap(items.get(position).getUrl(), holder.imageView, 500, 500);
        }