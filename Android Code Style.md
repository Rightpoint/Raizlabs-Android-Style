## [Raizlabs Android Style Guide](id:tableOfContents)<a name="tableOfContents"></a>

[Gradle](#gradle)

[Class Organization](#organization)

[Whitespace](#whitespace)

[Line Length](#linelength)

[Comments](#inlinecomments)

[Naming](#naming)

[Constants](#constants)

[Method signatures](#methods)

[Getters, Setters, and Member Variables](#getters)

[Privacy](#privacy)

[Operators](#operators)

[If statements](#ifelse)

[Switch statements](#switches)

[Imports](#imports)

***

### [Gradle](id:gradle)<a name="gradle"></a>

#### Project build.gradle format

- the format and organization of the project's `build.gradle` must follow:
  - plugin declarations
  - `apply from:` file declarations
  - project property initialization
  - local variable initialization
  - `android{}` configuration enclosure
    - compileSDK  and build tools version
    - `defaultConfig` enclosure with versionCode, versionName, and minSdkVersion
      - these should _never_ go in the `AndroidManifest.xml`
    - All project should use Java 7 so define a `compileOptions{}`
    - packaging options to exclude any duplicate files from java libraries
    - `flavorDimensions` and `productFlavors{}`
    - `buildTypes{}`
  - `dependencies` block
    - group dependencies of logical significance such as by google/android,
      remote RZ libraries, third party libraries, local projects, jars, etc.)
  - any custom `Task` or local `build.gradle` methods defined.

```groovy

// plugin declaration
apply plugin: 'com.android.application'

// apply from file
apply from: '../keystorePassword.gradle`

// project property
version = 1.0.0

// android enclosure
android {
    compileSdkVersion 22
    buildToolsVersion "22"

    defaultConfig {
        versionCode 2203
        versionName "2.2.3"
        minSdkVersion 14
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }

    packagingOptions {
        exclude 'META-INF/LICENSE.txt'
        exclude 'META-INF/NOTICE.txt'
    }

    flavorDimensions "country"

    productFlavors {

        us {
            flavorDimension "country"
        }

        ca {
            applicationId "com.raizlabs.anotherapplicationid"
            flavorDimension "country"
        }
    }

    buildTypes {

        debug {
            debuggable true
        }

	// always use release for signing
        release {
            signingConfig signingConfigs.release
        }
    }

}

dependencies {

    // google android dependencies
    compile 'com.android.support:support-v4:22.0.0'

    // third party remote libraries
    compile 'com.actionbarsherlock:actionbarsherlock:4.4.0@aar'

    // third party local jars should reference EXACT files
    compile files('libs/mylib.jar', 'libs/anotherjar.jar') 
    
    // third party local projects
    compile project(':Libraries:StickyListHeaders')
    
    // Raizlabs remote libraries
    compile 'com.raizlabs.android:CoreUtils:1.0.0'
    
    // Raizlabs local projects
    compile project(':Libraries:CoreUtils:CoreUtils')
}


```

[back to top](#tableOfContents)

#### Product Flavors and Build Types

- You _should_ expect to use multiple `productFlavors` in a project
  - These should be named relative to the project's api or app type
  - define a `flavorDimension` to enable (if needed) multiple dimensions

  - Do:

```groovy

flavorDimensions "endpoint"

productFlavors {
  staging {
    flavorDimension "endpoint"
  }

  live {
    flavorDimension "endpoint"
  }
}

```

  - Don't:

```groovy

productFlavors {
  signed {

  }

  unsigned {

  }
}


```

- We will only ever need two kinds of `buildTypes`: `debug` and `release`
  - `debug` is signed with the `debug.keystore` file on your local machine
  - the `release` type is signed with your application's keystore for release to the Play Store.

```groovy

buildTypes {

    debug {
        debuggable true
    }

    release {
        minifyEnabled false
        signingConfig signingConfigs.release
    }
}

```

#### Release Signing

- git repos for projects should _never_ store the valid `storePassword` and `keyPassword`
  - rather we place dummy `keystorePassword.gradle` and `keystoreInfo.gradle` files in our project

- `keystoreInfo.gradle`

```groovy

android {
    signingConfigs {
        release {
            storeFile file("../release_key.keystore")
            keyAlias "alias name"
        }
    }
}

```

- `keystorePassword.gradle`

```groovy

android {
    signingConfigs {
        release {
            storePassword "KEYSTORE_PASSWORD"
            keyPassword "ALIAS_PASSWORD"
        }
    }
}

```

- apply these to your project's build.gradle


[back to top](#tableOfContents)


### [Class Organization](id:organization)<a name="organization"></a>

- Class files should generally have their members defined in the following order:
	- Constants (public, then private)
	- Public interface declarations
	- Static variables with any associated methods (public, then private)
	- Private members, along with related trivial getters/setters
	- Any other trivial getters/setters
	- Constructors
	- Life cycle methods
	- Inherited / Interface methods
	- Any other methods
	- Anonymous class members
	- Inner class definitions

- Example:

```java

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
```

- These rules may be circumvented if the replacement organization makes more sense

```java

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

```

[back to top](#tableOfContents)


### [Whitespace](id:whitespace)<a name="whitespace"></a>

- Newlines
    - _Never_ more than one consecutive newline of whitespace.
    - Use **one** newline of whitespace to separate out conceptually separate bits of methods.
    - _Never_ use a newline before opening braces. Closing braces go on a new line.

```java

	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		// set up foo object
		Foo foo = new Foo();
		foo.setProperty(value);

		// set up bar object
		Bar bar = new Bar(foo);
	}

  ```


- Indentation
	- Always use tabs, not spaces.

[back to top](#tableOfContents)


### [Line Length](id:linelength)<a name="linelength"></a>

Single lines should not exceed **80** characters in length (Studio will show a vertical bar
where the text should end).

- For lines longer than *80* characters, break:
 - before a method call
 - after a comma
 - after an operator
 - Indent newline by 1 tab.

This is not a hard limit but generally, when possible, code lines should _not_ exceed it.

Take this:

```java

private String getDepartmentName() {
       return getIntent().hasExtra(INTENT_DEPARTMENT_NAME) ? getIntent().getStringExtra(INTENT_DEPARTMENT_NAME) : SomeFragment.ALL_DEPARTMENTS;
}

```

Break on the `?` operator:

```java
private String getDepartmentName() {
       return getIntent().hasExtra(INTENT_DEPARTMENT_NAME) ?
               getIntent().getStringExtra(INTENT_DEPARTMENT_NAME) : SomeFragment.ALL_DEPARTMENTS;
}

```


For classes that use the **builder** notation, break on _each_ call to the builder class.


Do _NOT_ do this

```java

AlertDialog.Builder builder = new AlertDialog.Builder(context).setTitle("MyTitle").setMessage("MyMessage").setView(customView);

```

Do _this_:

```java

AlertDialog.Builder builder = new AlertDialog.Builder(context)
                                .setTitle("MyTitle")
                                .setMessage("MyMessage")
                                .setView(customView);

```

[back to top](#tableOfContents)

### [Comments](id:inlinecomments)<a name="inlinecomments"></a>

- Comment whenever you write code that might appear weird or intimidating to a new developer.
- Comment any nontrivial or unintuitive code.
- Prefer well named methods and variables over comments. Well named, self-explanitory code does not require in-line comments.
- Comment whenever you are mitigating a known bug in any external code such as the OS or libraries. (Include the code revision and when it may be able to be removed, if you know)
- Never use your name in comments or code.
	- It isn't a good idea to send that info to clients
- Never reference bug numbers from an external bug tracker (Jira, Github) in code.

#### Double slash comments (```//```)

	- One space always immediately after slashes
	- You _may_ comment "trivial" code if it aids readability in some way (eg. visually distinguishing multiple tasks in a long method)
	- You _may_ comment in-line where appropriate. Eg. to identify the closing brace of a nested code block.
	- In general, put on the line before the code being explained. One newline should come before the comment and after the code fragment being explained to avoid confusion with following code unrelated to comment:

```java

		...preceding code...
		...preceding code...

		// Longer explanatory comment
		...code being explained...

		...other code unrelated to comment...
		...other code unrelated to comment...
```


#### Special comment identifiers

```// !!!:```

    (Not standard Java. Use anyway?)

	   - Use to comment code that mitigates OS bugs, code that could in the future be eliminated or changed when something out of our control is fixed


```// TODO:```

			- Use when code is committed but is intentionally left incomplete, i.e. empty method bodies whose implementation is part of another sprint-planned issue

```/* */```
      - generally these should _never_ be used

#### Block Comments

  - Block comments should _only_ apply to class, interface, variable or method declarations.
  Do **not** use them inside of methods.

  - For class/interface headers:

```java

/**
 * This is a well though-out description for this class file.
 */
public class MyClass {

}

```

  - For methods:

```java

/*
 * This method description tells me any quirks or non-obvious things about it.

 * @param   paramName Any parameter that is worth explaining please document, otherwise leave out!
 * @return  Describe what we return. Omit the description when the return explains the method.
 */
public String myMethod(String paramName) {

}

```


#### Projects

- Comments for project must at least go on
  - All public & protected methods (when methods aren't self-documenting)
  - Custom interfaces and their corresponding methods
  - Any `newInstance()` or creator for an Android component such as a `Fragment` ,
`Activity`, or `View`

#### Open Source

Comments for open source projects _must_ go on:

  - All public elements such as class files, methods, variables, etc.
  - Inside methods with explanation for any tricky logic or not-easily readable logic
  - Private variables and their basic usage

#### Comments Never When...

  - Commented code must _never_ be left over when placing a pull request. It's
ok to check it in before that.
  - Getter and setters when they are simply java-bean-esque

```java

public void setData(String data) {
      this.data = data;
}

```
  - There is nothing to say about a `@param` or javadoc element


[back to top](#tableOfContents)

### [Naming](id:naming)<a name="naming"></a>

- We do _not_ follow the Android standard variable names rather

```java

public class MyClass {
  public static final int SOME_CONSTANT = 42;
  public int publicField;
  private static MyClass singleton;
  int packagePrivate;
  private int private;
  protected int protected;
}

```

- Variables and methods always use camel case

```java

// like this
private TextView titleTextView;


// and this
public void performLongRunningOperation();

// not this
private CheckBox mCheckBox;

```

- Names should always be descriptive and may be verbose. Long method names are acceptable (within reason).
	- Don't give members or methods generic names (unless the purpose is generic)

```java

// BAD!
public TextView getTextView() {
	return textView;
}

```

	- Instead, prefix the variable name with a relevant descriptor

```java

// GOOD!
public TextView getTitleTextView() {
	return titleTextView;
}

```

- Identifiers in names should be ordered from most general to most specific. This keeps identifiers in the same order as package and class naming conventions. For example:

```java

public static final String TAG_FRAGMENT_PRODUCT_DETAILS = "ProductDetailsFragment";

```

[back to top](#tableOfContents)

### [Constants](id:constants)<a name="constants"></a>
- Stand alone constants should be defined at the top of the relevant class file and defined in all caps, delimited by underscores. These should be moved into shared common files/classes if they are consumed by multiple classes.

```java

public static final String TAG_FRAGMENT_PRODUCT_DETAILS = "ProductDetailsFragment";

```

- Constants which are related or are to be a set of choices should be broken into their own enums or static classes of constants.

```java

	public class Foo {
		public static class Action {
			public static final String ADD = "add";
			public static final String EDIT = "edit";
		}

		...
	}
```


[back to top](#tableOfContents)


### [Method signatures](id:methods)<a name="methods"></a>

- Descriptive method names
- Descriptive parameter names
- Listeners and delegates always on tail of method parameters

	- Preferred:

```java

public String getRunningStateText(boolean isApplicationRunning, int stringResourceId)

```

	- Not preferred:


```java

public void setT(Object o, Bitmap b)
public void methodName(Delegate delegate, Object obj)

```

	- Tab-align parameters if the declaration spans multiple lines:

```java

public void someMethodFoo(Object param,
							int otherParam,
							float anotherParam) {
```

[back to top](#tableOfContents)

#### [Getters, Setters, and Member Variables](id:getters)<a name="getters"></a>
- Member variables should _NOT_ be prefixed with an m or anything else.
- Getters and Setters should be regioned off

```java

// region Getters and Setters

public int getClickCount() {
  return this.clickCount;
}
public void setClickCount(int count) {
  this.clickCount = count;
}

// endregion

```

- Boolean getters should be prefixed with "is" or "has" instead of "get"

```java

private boolean isAwesome;

public boolean isAwesome() {
  return isAwesome;
}
public void setAwesome(boolean isAwesome) {
  this.isAwesome = isAwesome;
}

public boolean hasAwesomeAnimations() {
  return true;
}

```

### [Privacy](id:privacy)<a name="privacy"></a>

#### Variable Privacy

- **DO NOT** access members directly if getters/setters have been defined.
 	- Unless you have a very good reason (Parsers/Loaders/Annotation Processing libraries)
- **DO NOT** expose members directly.
- Unless you're writing a "struct-like" class which just wraps other values.

```java

public class ViewHolder {
	public View title;
	public View subtitle;
	public ImageView icon;
}
```
- Strongly encourage `private` variables above all else as it encourages encapsulation
for getters and setters.
- Use `protected` to members you want to expose to a subclass only
- Use package local only for:
  - instances where `private` is not an option such
as annotation processors
  -  ViewHolder classes
  -  ViewModel / struct-like classes
  -  static fields local to a specific package
- In general, avoid `public` except when defining constant values or when it makes sense.

[back to top](#tableOfContents)

### [Operators](id:operators)<a name="operators"></a>
- Unary operators stick to the number they modify:

```java

int x = -10;
int y = (x * -3);

```

- Use spaces between all binary and ternary mathematical operators:
- Fully parenthesize mathematical expressions and any logical expression with 1+ operator

```java

int x = ((1 + 1) / 1);

```

- Conditional expressions must be enclosed in parentheses:

```java

String largeString = (x > 200) ? "large" : "small";
boolean isLargeString = (x > 200);

```

- Multiple, longer expressions _must_ break on the operator:

```java

if (condition == null ||
        condition.isTrue() ||
        someotherCondition.looksBad()) {

}

```

- Non-expressions in ternary conditional operators do not need parentheses:

```java

String loadingString = this.isLoading() ? true : false;

```

- **No** nesting of ternary expressions

  - Don't even think about it

```java

boolean dontDoThis = this.otherBoolean ? ((this.dont) ? this.do : this.it) : this.please;

```

[back to top](#tableOfContents)


### [If statements](id:ifelse)<a name="ifelse"></a>

- **NEVER** forgo the brackets for one-line if statements ([#gotofail](https://www.imperialviolet.org/2014/02/22/applebug.html) anyone?)
- One space between the control keyword and opening parentheses
- Opening brace same line as conditional-if/else, one space
- Continuing keywords (else if/else) on the same line as closing braces
- All braces and keywords are flush left and code within braces are indented with a tab

	- Do:

```java

if (expression) {
	// if code
} else if (other expression) {
	// else if code
} else {
	// else code
}

```

	- Don't do:

```java

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

```

[back to top](#tableOfContents)

### [Switch statements](id:switches)<a name="switches"></a>

-  Braces should be on same line as case
-  If a case has more than one line code (other than the break), surround that case's body with braces
-  One space between the switch keyword and the open parenthesis, one space between the close parenthesis and the opening brace.

```java

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
```

- We strongly encourage you to put comments at the **end** of fall-through statements

```java

switch (expression) {
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

```

- Do not use a default if there isn't any handling for the default case

	- Do:

```java

switch (expression) {
  case 1:
  	// case 1 code
  	break;
  default:
      // default code
      // more default code
      break;
}

```

	- Don't do:

```java

switch (expression) {
	case 1: {
		// case 1 code
		break;
	}
  default: // nothing here, no need for default!
  	break;
}
```

[back to top](#tableOfContents)

### [Imports](id:imports)<a name="imports"></a>

- In general, use the Android Studio **organize imports** feature

[back to top](#tableOfContents)
