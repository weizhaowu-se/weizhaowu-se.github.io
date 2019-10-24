title: RecycleView
date: 2016-09-26
tags:
- android

---
<font style="font-family:微软雅黑">
### 创建RecycleView.Adapter的继承类
* 其中存在内部静态类：继承 RecyclerView.ViewHolder.
* 内部静态类：

		public static class ViewHolder extends RecyclerView.ViewHolder{
	        public Button button;
	        public ViewHolder(View t) {
	            super(t);
	            button = (Button) t.findViewById(R.id.item_text_view);
	        }}
#### 重载函数 onCreateViewHolder

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        final View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.my_text_view, parent, false);
		ViewHolder viewHolder = new ViewHolder(view);
        return viewHolder;
    }
#### 重载onBindViewHolder，设置每一个item的内容
    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
		//每个item的具体设置
        holder.button.setText(dataSet.get(position));
    }

### 实现按钮监听的方法
1. 声明接口

		public interface IOnItemClick {
	        void onItemClick(View view);
	    }
2. 在Activity中实现接口

		MenuAdapter.IOnItemClick iOnItemClick = new MenuAdapter.IOnItemClick() {
            @Override
            public void onItemClick(View view) {
				//此处的view为每个item的View而非里面的按钮之类的
		        int itemPosition = recyclerView.getChildLayoutPosition(view);
        	}
		}
3. 在初始化Adapter中，将接口的实现类传进去。
4. 在Adapter中。

		    @Override
		    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
		        final View view = LayoutInflater.from(parent.getContext())
		                .inflate(R.layout.my_text_view, parent, false);
		        ViewHolder viewHolder = new ViewHolder(view);
		        if(iOnItemClick != null) {
		            viewHolder.button.setOnClickListener(new View.OnClickListener() {
		                @Override
		                public void onClick(View v) {
		                    iOnItemClick.onItemClick(view);
							//此处调用的view为上面所述的view而非按钮v
		                }
		            });
		        }
		        return viewHolder;
		    }