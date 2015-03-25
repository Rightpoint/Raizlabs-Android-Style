## [Raizlabs Android Style Guide](id:tableOfContents)<a name="tableOfContents"></a>

[Whitespace](#whitespace)

[In-Line Comments](#inlinecomments)

[Naming](#naming)

[Constants](#constants)

[Method signatures](#methods)

[Getters, Setters, and Member Variables](#getters)

[Operators](#operators)

[If statements](#ifelse)

[Switch statements](#switches)

[Imports](#imports)

[Class Organization](#organization)
***

### [Whitespace](id:whitespace)<a name="whitespace"></a>

- Newlines
    - Never more than one consecutive newline of whitespace.
    - Use one newline of whitespace to separate out conceptually separate bits of methods.
    - Never use a newline before opening braces. Closing braces go on a new line.
			
			public void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);

				// set up foo object
				Foo foo = new Foo();
				foo.setProperty(value);

				// set up bar object
				Bar bar = new Bar(foo);
			}
			
		
- Indentation
	- Always use tabs, not spaces.

[back to top](#tableOfContents)

### [In-Line Comments](id:inlinecomments)<a name="inlinecomments"></a> 
- Comment whenever you write code that might appear weird or intimidating to a new developer.
- Comment any nontrivial or unintuitive code.
- Prefer well named methods and variables over comments. Well named, self-explanitory code does not require in-line comments.
- Comment whenever you are mitigating a known bug in any external code such as the OS or libraries. (Include the code revision and when it may be able to be removed, if you know) 
- Never use your name in comments or code.
	- It isn't a good idea to send that info to clients
- Never reference bug numbers from an external bug tracker (Jira, Github) in code.

- Double slash comments (`//`)
	- One space always immediately after slashes
	- You _may_ comment "trivial" code if it aids readability in some way (eg. visually distinguishing multiple tasks in a long method)
	- You _may_ comment in-line where appropriate. Eg. to identify the closing brace of a nested code block.
	- In general, put on the line before the code being explained. One newline should come before the comment and after the code fragment being explained to avoid confusion with following code unrelated to comment:

		
			...preceding code...
			...preceding code...
			
			// Longer explanatory comment
			...code being explained...
			
			...other code unrelated to comment...
			...other code unrelated to comment...

			
- Special comment identifiers

		- `// !!!:` (Not standard Java. Use anyway?)

			- Use to comment code that mitigates OS bugs, code that could in the future be eliminated or changed when something out of our control is fixed
		
		
		- `// TODO:` 
		
			- Use when code is committed but is intentionally left incomplete, i.e. empty method bodies whose implementation is part of another sprint-planned issue
		
- `/* */`

	- Very long comments


[back to top](#tableOfContents)

### [Naming](id:naming)<a name="naming"></a>

- Variables and methods always use camel case

		// like this
		private TextView titleTextView;
		
		
		// and this
		public void performLongRunningOperation();
- Names should always be descriptive and may be verbose. Long method names are acceptable (within reason).
	- Don't give members or methods generic names (unless the purpose is generic)
			
			// BAD!
			public TextView getTextView() {
				return textView;
			}
	- Instead, prefix the variable name with a relevant descriptor

			// GOOD!
			public TextView getTitleTextView() {
				return titleTextView;
			}
- Identifiers in names should be ordered from most general to most specific. This keeps identifiers in the same order as package and class naming conventions. For example:

		public static final String TAG_FRAGMENT_PRODUCT_DETAILS = "ProductDetailsFragment";

[back to top](#tableOfContents)	

### [Constants](id:constants)<a name="constants"></a>
- Stand alone constants should be defined at the top of the relevant class file and defined in all caps, delimited by underscores. These should be moved into shared common files/classes if they are consumed by multiple classes.

		public static final String TAG_FRAGMENT_PRODUCT_DETAILS = "ProductDetailsFragment";

- Constants which are related or are to be a set of choices should be broken into their own enums or static classes of constants.

			public class Foo {
				public static class Action {
					public static final String Add = "add";
					public static final String Edit = "edit";
				}
				
				...
			}


[back to top](#tableOfContents)



### [Method signatures](id:methods)<a name="methods"></a>

- Descriptive method names
- Descriptive parameter names
- Listeners and delegates always on tail of method parameters

	- Preferred:
	
			public Object methodName(Object parameter, Delegate otherParameter)
	- Not preferred:
		
			public void setT(Object obj, Bitmap b)
			public void methodName(Delegate delegate, Object obj)
	- Tab-align parameters if the declaration spans multiple lines
:	
			public void someMethodFoo(	Object param,
										int otherParam,
										float anotherParam) {

[back to top](#tableOfContents)

#### [Getters, Setters, and Member Variables](id:getters)<a name="getters"></a>
- Member variables should _NOT_ be prefixed with an m or anything else.
- Trivial, single-line getters and setters should be written inline to keep the code neat and consolidated.

		private int clickCount;
		public int getClickCount() { return this.clickCount; }
		public void setClickCount(int count) { this.clickCount = count; }

- Boolean getters should be prefixed with "is" or "has" instead of "get"
		
		private boolean awesome;
		public boolean isAwesome() { return awesome; }
		public void setAwesome(boolean awesome) { this.awesome = awesome; }
		
		public boolean hasAwesomeAnimations() { return true; }
		
- **DO NOT** access members directly if getters/setters have been defined.
 	- Unless you have a very good reason (Parsers/Loaders)
- **DO NOT** expose members directly.
	- Unless you're writing a "struct-like" class which just wraps other values. When doing so, capitalize member names.
							 
			public class ViewHolder {
				public View Title;
				public View Subtitle;
				public ImageView Icon;
			}

[back to top](#tableOfContents)

### [Operators](id:operators)<a name="operators"></a>
- Unary operators stick to the number they modify:

		int x = -10;
		int y = (x * -3);

- Use spaces between all binary and ternary mathematical operators:
- Fully parenthesize mathematical expressions and any logical expression with 1+ operator

		int x = ((1 + 1) / 1);
	
- Conditional expressions must be enclosed in parentheses:

		String largeString = (x > 200) ? "large" : "small";
		boolean isLargeString = (x > 200);
		
- Non-expressions in ternary conditional operators do not need parentheses:
		
		String loadingString = this.isLoading() ? true : false;

		
- No nesting of ternary expressions
	
- Don't even think about it
	
		boolean dontDoThis = this.otherBoolean ? ((this.dont) ? this.do : this.it) : this.please;

[back to top](#tableOfContents)


### [If statements](id:ifelse)<a name="ifelse"></a>

- **NEVER** forgo the brackets for one-line if statements ([#gotofail](https://www.imperialviolet.org/2014/02/22/applebug.html) anyone?)
	- Very simple inline ifs are acceptable
	
			if (param == null) return;
- One space between the control keyword and opening parentheses 
- Opening brace same line as conditional-if/else, one space 
- Continuing keywords (else if/else) on the same line as closing braces
- All braces and keywords are flush left and code within braces are indented with a tab

	- Preferred:
		
			if (expression) {
				// if code
			} else if (other expression) {
				// else if code
			} else {
				// else code
			}	
			
	- Not Preferred:
	
			if (expression) 
			{ // shouldn't be on next line
				// if code
			} 
			else if (expression) // else shouldn't start on new line
			{
				// else if code
			}
			else 
				// else code // NEVER forgo brackets

[back to top](#tableOfContents)

### [Switch statements](id:switches)<a name="switches"></a>

-  Braces should be on same line as case
-  If a case has more than one line code (other than the break), surround that case's body with braces
-  One space between the switch keyword and the open parenthesis, one space between the close parenthesis and the opening brace.
	
		switch (expression) {
		case 1:
			// code
			break;
		case 2:
			// code
			// code
			break;
		default:
			// default code
			break;
		}

- We strongly encourage you to put comments at the **end** of fall-through statements
	
		switch ( expression ) {
		case 1:
			// case 1 code
			break;
		case 2: // fall-through
		case 3:
		    // code executed for values 2 and 3
		   	break;
		default: 
			// default code
		    break;
		}
		
- Do not use a default if there isn't any handling for the default case

	Preferred: 
	
		switch ( expression ) {
		case 1:
			// case 1 code
			break;
		default:
		    // default code
		    // more default code
		    break;
		}
		
	Not Preferred:
	
		switch ( expression ) {
			case 1: {
				// case 1 code
				break;
			}
		    default: // nothing here, no need for default!
		    	break;
		}


[back to top](#tableOfContents)

### [Imports](id:imports)<a name="imports"></a>

- Imports should be broken into groups in the following order, delineated by a new line. Genearlly, your application package imports should be last.
	- java
	- javax
	- org
	- android
	- com
	
			package com.raizlabs.MyApp;
		
			import java.io.IOException;
			import java.io.InputStream;
		
			import javax.crypto.Cipher;

			import org.apache.http.client.HttpClient;
		
			import android.content.Context;
			import android.util.AttributeSet;
			import android.widget.ScrollView; 

			import com.raizlabs.functions.Delegate;

			public class MyScrollView extends ScrollView {
				...
			}

[back to top](#tableOfContents)

### [Class Orginaization](id:organization)<a name="organization"></a>

- Class files should generally have their members defined in the following order:
	- Constants (public, then private)
	- Public interface declarations
	- Static methods (public, then private)
	- Private members, along with related trivial getters/setters
	- Any other trivial getters/setters
	- Constructors
	- Life cycle methods
	- Inherited / Interface methods
	- Any other methods
	- Anonymous class members
	- Inner class definitions
- Example:
		
		public class MyView extends ViewGroup {

			// Constants
			public static String EXTRA_BAR = "Bar";

			private static String TAG_FOO = "Foo";
	
			// Public interfaces
			public interface ActionListener {
				public void onActionPerformed(Object action);
			}
	
			// Static methods
			public static Object getThing() {
				return null;
			}
	
			// Private members and getters/setters
			private int textColor;
			public int getTextColor() { return textColor; }
			public void setTextColor(int color) { this.textColor = color; }
	
			private LinearLayout itemLayout;
	
			// Other trivial getters
			public int getItemCount() { return itemLayout.getChildCount(); }

			// Constructors
			public MyView(Context context, AttributeSet attrs, int defStyle) {
				super(context, attrs, defStyle);
			}
	
			// Lifecycle methods
			@Override
			protected void onAttachedToWindow() {
				super.onAttachedToWindow();
			}
	
			@Override
			protected void onDetachedFromWindow() {
				super.onDetachedFromWindow();
			}
	
			// Inherited methods
			@Override
			protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
				...
			}
	
			// Other methods
			public void sortChildren() {
				...
			}
	
			// Anonymous class members
			private OnClickListener childClickListener = new OnClickListener() {
				@Override
				public void onClick(View v) {
					...
				}
			};
	
			// Inner class definitions
			private static class StateHelper {
				...
			}
		}
	
- These rules may be circumvented if the replacement organization makes more sense

		public MyView(Context context) {
			super(context);
			init();
		}
	
		public MyView(Context context, AttributeSet attrs) {
			super(context, attrs);
			init();
		}
	
		public MyView(Context context, AttributeSet attrs, int defStyle) {
			super(context, attrs, defStyle);
			init();
		}
	
		private void init() {
			// Shared constructor logic
			...
		} 

[back to top](#tableOfContents)

### [XML Conventions](id:xmlconventions)<a name="xmlconventions"></a>
- Where applicable, related resources should be combined into one file
	- Ex: Text sizes, colors, styles, etc should all be consolidated into a values/text.xml 
- Names should generally follow a pattern of most general to most specific.
	- Per Android, some identifiers may not contain capitals. Use camel case and periods where you can, but fall back to underscores where they cannot be used.
	- Ex: text_product_details.xml, R.style.Text_ProductDetails_Header
- Layout resources should be prepended with the intended usage
	- Common use identifiers:
		- activity : activity layout
		- fragment : fragment layout
		- view : custom view layout
		- list_item : list view cell
	- Examples:
		- layout/activity_main.xml
		- layout/view_hours.xml
		- layout/list_item_product.xml
- IDs should start with their file name to avoid naming collisions.
	- Exception: IDs or views which are intentionally shared between files
	- For brevity, camel case should be used where underscores were used in place of spaces.
		- layout/list_item_product.xml -> android:id="@+id/listItem_product_name"
		- layout/view_product_details.xml -> android:id="@+id/view_productDetails_name"
- Value IDs should be period delimited

		