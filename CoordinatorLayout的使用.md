# CoordinatorLayout

### CoordinatorLayout与AppBarLayout

通过AppBarLayout的子视图的属性控制。观察AppBarLayout的子布局，Toobar有app:layout_scrollFlags属性，这就是控制滑动时视图效果的属性。

##### app:layout_scrollFlags有四个值：

- scroll:所有想滚动出屏幕的view都需要设置这个flag， 没有设置这个flag的view将被固定在屏幕顶部。例如，TabLayout 没有设置这个值，将会停留在屏幕顶部。
- enterAlways:设置这个flag时，向下的滚动都会导致该view变为可见，启用快速“返回模式”。
- enterAlwaysCollapsed:当你的视图已经设置minHeight属性又使用此标志时，你的视图只能已最小高度进入，只有当滚动视图到达顶部时才扩大到完整高度。
- exitUntilCollapsed:滚动退出屏幕，最后折叠在顶端。

为了达到Toobar可以滚动，CoordinatorLayout里面,放一个带有可滚动的View。

为了使得Toolbar有滑动效果，必须做到如下三点: 

- CoordinatorLayout作为布局的父布局容器。 
- 给需要滑动的组件设置 app:layout_scrollFlags=”scroll|enterAlways” 属性。 
- 给滑动的组件设置app:layout_behavior属性

### CoordinatorLayout与CollapsingToolbarLayout

CollapsingToolbarLayout可实现Toolbar的折叠效果。CollapsingToolbarLayout的子视图类似与LinearLayout垂直方向排放。

CollapsingToolbarLayout 提供以下属性和方法是用： 


- Collapsing title：ToolBar的标题，当CollapsingToolbarLayout全屏没有折叠时，title显示的是大字体，在折叠的过程中，title不断变小到一定大小的效果。你可以调用setTitle(CharSequence)方法设置title。 
2. Content scrim：ToolBar被折叠到顶部固定时候的背景，你可以调用setContentScrim(Drawable)方法改变背景或者 在属性中使用 app:contentScrim=”?attr/colorPrimary”来改变背景。 
3. Status bar scrim：状态栏的背景，调用方法setStatusBarScrim(Drawable)。还没研究明白，不过这个只能在Android5.0以上系统有效果。 
4. Parallax scrolling children：CollapsingToolbarLayout滑动时，子视图的视觉差，可以通过属性app:layout_collapseParallaxMultiplier=”0.6”改变。值de的范围[0.0,1.0]，值越大视察越大。 
5. CollapseMode ：子视图的折叠模式，在子视图设置，有两种“pin”：固定模式，在折叠的时候最后固定在顶端；“parallax”：视差模式，在折叠的时候会有个视差折叠的效果。我们可以在布局中使用属性app:layout_collapseMode=”parallax”来改变。



