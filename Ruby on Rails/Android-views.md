# 안드로이드 Veiw

## View
 - 화면 그 자체를 나타냄
 - Activity에 씌우는 껍데기
 - XML, Java 둘 중 하나로 작성가능 (보통 XML)
 - Widget, Adapter, Layout 계열로 나눌 수 있음

## Widget
 - TextView, ImageView 등
 - 사용자와 작용하는 목적이 뚜렷한 View

## Adapter
 - ListView, GridView, RecyclerView 등
 - 많은 정보를 스크롤 하며 나열할 때  많이 쓰임

## Layout
 - LinearLayout, RelativeLayout, FrameLayout 등
 - Widget, Adapter, Layout 등을 담을 수 있는 틀
 - 화면 공간을 분할할 때 많아 쓰임

## XML
 - 부모 자식관계를 가질 수 있음
 - HTML 문법과 유사함
~~~
<View
  android:layout_width="math_parent"
  android:layout_width="wrap_content"
  android:background="#ff0000"
  >
  
  <TextView ... />
  <ImageView ... />
</View>
~~~