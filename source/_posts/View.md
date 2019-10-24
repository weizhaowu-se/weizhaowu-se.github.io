title: View
date: 2016-09-26
tags:
- android
 
---
## 追踪手指在滑动过程中的速度
	@Override
	    public boolean onTouchEvent(MotionEvent event) {
	        VelocityTracker velocityTracker = VelocityTracker.obtain();
	        velocityTracker.addMovement(event);
	        velocityTracker.computeCurrentVelocity(1000);
	        int x = (int)velocityTracker.getXVelocity();
	        int y = (int)velocityTracker.getYVelocity();
	
	        Log.d(W, "x:" + x + ",y:" + y);
	        return super.onTouchEvent(event);
	    }

## 检测手势行为

	implements GestureDetector.OnGestureListener

		@Override
	    public boolean onTouchEvent(MotionEvent event) {	
	        boolean consume = gestureDetector.onTouchEvent(event);
	        return consume;
	    }
## 滑动
### scrollTo&&scrollerBy
### 使用动画
### 改变布局参数


    public void onClick(View v) {
        ViewGroup.MarginLayoutParams marginLayoutParams =
                (ViewGroup.MarginLayoutParams)button.getLayoutParams();
        Log.d(W, "layout");
        marginLayoutParams.width += 100;
        marginLayoutParams.leftMargin += 100;
        button.requestLayout();
    }