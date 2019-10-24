---
title: android动画学习
date: 2016-10-19 19:47:37
tags:
- android

---
* 根据google官方的动画学习
* [https://developer.android.com/training/animation/index.html](https://developer.android.com/training/animation/index.html)
### Crossfading Two Views
* 两个view的切换：淡入淡出效果的实现
* 应用场景：进度条加载完成之后内容的显示
<!--more-->
#### 页面代码
	
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	
	    <ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
	        android:id="@+id/content"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	
	        <TextView style="?android:textAppearanceMedium"
	            android:lineSpacingMultiplier="1.2"
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:text="@string/lorem_ipsum"
	            android:padding="16dp" />
	
	    </ScrollView>
	
	    <ProgressBar android:id="@+id/loading_spinner"
	        style="?android:progressBarStyleLarge"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:layout_gravity="center" />
	
	</FrameLayout>

* 实现效果：在activity打开之后progressBar的alpha值逐渐下降到0直至可见性为GONE，而文本的内容的alpha值逐渐上升到1。
* 动画代码

	private void crossfade() {
	
	    // Set the content view to 0% opacity but visible, so that it is visible
	    // (but fully transparent) during the animation.
	    mContentView.setAlpha(0f);
	    mContentView.setVisibility(View.VISIBLE);
	
	    // Animate the content view to 100% opacity, and clear any animation
	    // listener set on the view.
	    mContentView.animate()
	            .alpha(1f)
	            .setDuration(mShortAnimationDuration)
	            .setListener(null);
	
	    // Animate the loading view to 0% opacity. After the animation ends,
	    // set its visibility to GONE as an optimization step (it won't
	    // participate in layout passes, etc.)
	    mLoadingView.animate()
	            .alpha(0f)
	            .setDuration(mShortAnimationDuration)
	            .setListener(new AnimatorListenerAdapter() {
	                @Override
	                public void onAnimationEnd(Animator animation) {
	                    mLoadingView.setVisibility(View.GONE);
	                }
	            });
		}
	
		
##### some tips
* > mShortAnimationDuration = getResources().getInteger(
                android.R.integer.config_shortAnimTime);  //200ms
* view.animate() 返回值为ViewPropertyAnimator,可以对其进行view上的某一个property的动画操作（例如alpha）
* 可以设置动画开始/结束等的监听事件，
#### ViewPager切换效果
* 重点在于Viewpager的函数
	>mPager.setPageTransformer(true, new DepthPageTransformer());
* 其中函数的第二个参数为PageTransformer接口
* 
	    public interface PageTransformer {
	        /**
	         * Apply a property transformation to the given page.
	         *
	         * @param page Apply the transformation to this page
	         * @param position Position of page relative to the current front-and-center
	         *                 position of the pager. 0 is front and center. 1 is one full
	         *                 page position to the right, and -1 is one page position to the left.
	         */
	        public void transformPage(View page, float position);
	    }
* position:
	* 0: 前台居中
	* 1: 前台右侧
	* -1:前台左侧
##### ZoomOutPageTransformer
	
				public class ZoomOutPageTransformer implements ViewPager.PageTransformer {
				    private static final float MIN_SCALE = 0.85f;
				    private static final float MIN_ALPHA = 0.5f;
				
				    @Override
				    public void transformPage(View view, float position) {
				        int pageWidth = view.getWidth();
				        int pageHeight = view.getHeight();
				
				        if (position < -1) { // [-Infinity,-1)
				            // This page is way off-screen to the left.
				            view.setAlpha(0);
				
				        } else if (position <= 1) { // [-1,1]
				            // Modify the default slide transition to shrink the page as well
				            float scaleFactor = Math.max(MIN_SCALE, 1 - Math.abs(position));
				            float vertMargin = pageHeight * (1 - scaleFactor) / 2;
				            float horzMargin = pageWidth * (1 - scaleFactor) / 2;
				            if (position < 0) {
				                view.setTranslationX(horzMargin - vertMargin / 2);
				            } else {
				                view.setTranslationX(-horzMargin + vertMargin / 2);
				            }
				
				            // Scale the page down (between MIN_SCALE and 1)
				            view.setScaleX(scaleFactor);
				            view.setScaleY(scaleFactor);
				
				            // Fade the page relative to its size.
				            view.setAlpha(MIN_ALPHA +
				                    (scaleFactor - MIN_SCALE) /
				                            (1 - MIN_SCALE) * (1 - MIN_ALPHA));
				
				        } else { // (1,+Infinity]
				            // This page is way off-screen to the right.
				            view.setAlpha(0);
				        }
				    }
				}
* 根据position对view进行位置的偏移(translation)以及大小的缩放(scale)以及透明度(alpha)的调整.
##### DepthPageTransformer 
		public class DepthPageTransformer implements ViewPager.PageTransformer {
		    private static final float MIN_SCALE = 0.75f;
		
		    public void transformPage(View view, float position) {
		        int pageWidth = view.getWidth();
		
		        if (position < -1) { // [-Infinity,-1)
		            // This page is way off-screen to the left.
		            view.setAlpha(0);
		
		        } else if (position <= 0) { // [-1,0]
		            // Use the default slide transition when moving to the left page
		            view.setAlpha(1);
		            view.setTranslationX(0);
		            view.setScaleX(1);
		            view.setScaleY(1);
		
		        } else if (position <= 1) { // (0,1]
		            // Fade the page out.
		            view.setAlpha(1 - position);
		
		            // Counteract the default slide transition
		            view.setTranslationX(pageWidth * -position);
		
		            // Scale the page down (between MIN_SCALE and 1)
		            float scaleFactor = MIN_SCALE
		                    + (1 - MIN_SCALE) * (1 - Math.abs(position));
		            view.setScaleX(scaleFactor);
		            view.setScaleY(scaleFactor);
		
		        } else { // (1,+Infinity]
		            // This page is way off-screen to the right.
		            view.setAlpha(0);
		        }
		    }
		}
#### Displaying Card Flip Animations
* 简单来讲,这个应该指的是一些页面切换时候的动画
		
		getFragmentManager()
		                .beginTransaction()
		
		                // Replace the default fragment animations with animator resources
		                // representing rotations when switching to the back of the card, as
		                // well as animator resources representing rotations when flipping
		                // back to the front (e.g. when the system Back button is pressed).
		                .setCustomAnimations(
		                        R.animator.card_flip_right_in,
		                        R.animator.card_flip_right_out,
		                        R.animator.card_flip_left_in,
		                        R.animator.card_flip_left_out)
		
		                // Replace any fragments currently in the container view with a
		                // fragment representing the next page (indicated by the
		                // just-incremented currentPage variable).
		                .replace(R.id.container, new CardBackFragment())
		
		                // Add this transaction to the back stack, allowing users to press
		                // Back to get to the front of the card.
		                .addToBackStack(null)
		
		                // Commit the transaction.
		                .commit();
* 核心方法为:setCustomeAnimations
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-19/36019666.jpg)
* 具体的代码可以参见[https://developer.android.com/training/animation/cardflip.html](https://developer.android.com/training/animation/cardflip.html)
#### Zooming a View
#### Animating Layout Changes
* layout布局变化的时候的动画是自带的,配置如下:

		<LinearLayout android:id="@+id/container"
		    android:animateLayoutChanges="true"
		    ...
		/>
* 接下来只需要直接在代码中addView中就可以了.

**下一篇要写的是属性动画的另外一些应用**