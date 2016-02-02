# Custom Views in XML
View, ImageView, WebView, TextView, and the list goes on. The different types of Views that Android provides are fundamental to building an app. But there comes a time when we need just a little more customization. Doing a little bit of research brings you to extending one of these classes and writing Java code. But we can do better.

We can go one step further and use our custom Views in XML. Not only can we use them, we can create and grab the custom View's XML attribute values from Java code!

## Steps
1. Create an XML file in the 'values' directory. Name it attrs.xml. Create a few custom XML attributes in attrs.xml
2. Create a Java class that extends View (and grab the XML attribute values from Java code)
3. Add the custom View into an XML layout and give values to the XML attributes from step 1
4. Customize away!

## Code
1. Create the following file in your project: main/res/values/attrs.xml

`<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="CustomView">
        <attr name="circle_fill_color" format="color" />
        <attr name="circle_degrees" format="integer" />
    </declare-styleable>
</resources>`



2. Create a custom Java class that extends View: CustomView.java

public class CustomView extends View {
    private int circleColor;
    private int circleRadius;
    private Paint paint;

    public CustomView(Context context) {
        super(context);
        init(context, null, 0);
    }

    // We need to include this constructors in order to use CustomView from xml
    public CustomView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs, 0);
    }

    // We need to include this constructor in order to use CustomView from xml
    public CustomView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr);
    }

    private void init(Context context, AttributeSet attrs, int defStyle) {
        this.context = context;
        if (attrs != null) {
            // This is where we will grab attribute values that are specified in XML
            TypedArray attrArray = 
                getContext().obtainStyledAttributes(attrs, R.styleable.CustomView, defStyle, 0);
            circleColor = 
                attrArray.getColor(R.styleable.CustomView_circle_fill_color, Color.RED);
            circleRadius = 
                attrArray.getInt(R.styleable.CustomView_circle_degrees, 90);
        }

        paint.setColor(circleColor);
        paint.setStyle(Style.FILL);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        // do custom drawing based on values obtained from XML attribute
        canvas.drawCircle(canvas.getWidth()/2, canvas.getHeight()/2, circleRadius, paint);
    }
}



3. Add CustomView to the xml layout. MainActivity's layout in this case: activity_main.xml

`<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical">
    <org.yourpackagename.CustomView
    android:id="@+id/custom_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:circle_fill_color="@android:color/holo_blue_dark"
    app:circle_radius="50dp"/>
</LinearLayout>`

## Final Words
When referencing an XML attribute name from the Java code (see init method in step 1), be sure to prefix it with the `declare-stylable` name and `_`. In our case, we need to use `R.styleable.CustomView_circle_fill_color` in the init method, since we used the following tag in attrs.xml `<declare-styleable name="CustomView">`. 

Please note that there are different methods for TypedArray for different data types (floats, ints, dimensions, colors).

Happy Coding :)!
